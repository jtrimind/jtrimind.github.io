---

title:  "Gstreamer 기본 튜토리얼 04: Time management"
---

## 목표
- pipeline의 stream position이나 duration과 같은 정보를 얻는 법
- stream의 다른 position으로 seek하는 법

## 소개
`GstQuery`는 element나 pad에게 정보를 얻어오는 수단이다.  
예제에서는 pipeline에게 seek이 가능한지 물어보고, 가능하다면 seek을 이용하여 다른 position으로 이동할 것이다.  

## Seeking example
```c++
#include <gst/gst.h>

/* Structure to contain all our information, so we can pass it around */
typedef struct _CustomData {
  GstElement *playbin;  /* Our one and only element */
  gboolean playing;      /* Are we in the PLAYING state? */
  gboolean terminate;    /* Should we terminate execution? */
  gboolean seek_enabled; /* Is seeking enabled for this media? */
  gboolean seek_done;    /* Have we performed the seek already? */
  gint64 duration;       /* How long does this media last, in nanoseconds */
} CustomData;

/* Forward definition of the message processing function */
static void handle_message (CustomData *data, GstMessage *msg);

int main(int argc, char *argv[]) {
  CustomData data;
  GstBus *bus;
  GstMessage *msg;
  GstStateChangeReturn ret;

  data.playing = FALSE;
  data.terminate = FALSE;
  data.seek_enabled = FALSE;
  data.seek_done = FALSE;
  data.duration = GST_CLOCK_TIME_NONE;

  /* Initialize GStreamer */
  gst_init (&argc, &argv);

  /* Create the elements */
  data.playbin = gst_element_factory_make ("playbin", "playbin");

  if (!data.playbin) {
    g_printerr ("Not all elements could be created.\n");
    return -1;
  }

  /* Set the URI to play */
  g_object_set (data.playbin, "uri", "https://www.freedesktop.org/software/gstreamer-sdk/data/media/sintel_trailer-480p.webm", NULL);

  /* Start playing */
  ret = gst_element_set_state (data.playbin, GST_STATE_PLAYING);
  if (ret == GST_STATE_CHANGE_FAILURE) {
    g_printerr ("Unable to set the pipeline to the playing state.\n");
    gst_object_unref (data.playbin);
    return -1;
  }

  /* Listen to the bus */
  bus = gst_element_get_bus (data.playbin);
  do {
    msg = gst_bus_timed_pop_filtered (bus, 100 * GST_MSECOND,
        GST_MESSAGE_STATE_CHANGED | GST_MESSAGE_ERROR | GST_MESSAGE_EOS | GST_MESSAGE_DURATION);

    /* Parse message */
    if (msg != NULL) {
      handle_message (&data, msg);
    } else {
      /* We got no message, this means the timeout expired */
      if (data.playing) {
        gint64 current = -1;

        /* Query the current position of the stream */
        if (!gst_element_query_position (data.playbin, GST_FORMAT_TIME, &current)) {
          g_printerr ("Could not query current position.\n");
        }

        /* If we didn't know it yet, query the stream duration */
        if (!GST_CLOCK_TIME_IS_VALID (data.duration)) {
          if (!gst_element_query_duration (data.playbin, GST_FORMAT_TIME, &data.duration)) {
            g_printerr ("Could not query current duration.\n");
          }
        }

        /* Print current position and total duration */
        g_print ("Position %" GST_TIME_FORMAT " / %" GST_TIME_FORMAT "\r",
            GST_TIME_ARGS (current), GST_TIME_ARGS (data.duration));

        /* If seeking is enabled, we have not done it yet, and the time is right, seek */
        if (data.seek_enabled && !data.seek_done && current > 10 * GST_SECOND) {
          g_print ("\nReached 10s, performing seek...\n");
          gst_element_seek_simple (data.playbin, GST_FORMAT_TIME,
              GST_SEEK_FLAG_FLUSH | GST_SEEK_FLAG_KEY_UNIT, 30 * GST_SECOND);
          data.seek_done = TRUE;
        }
      }
    }
  } while (!data.terminate);

  /* Free resources */
  gst_object_unref (bus);
  gst_element_set_state (data.playbin, GST_STATE_NULL);
  gst_object_unref (data.playbin);
  return 0;
}

static void handle_message (CustomData *data, GstMessage *msg) {
  GError *err;
  gchar *debug_info;

  switch (GST_MESSAGE_TYPE (msg)) {
    case GST_MESSAGE_ERROR:
      gst_message_parse_error (msg, &err, &debug_info);
      g_printerr ("Error received from element %s: %s\n", GST_OBJECT_NAME (msg->src), err->message);
      g_printerr ("Debugging information: %s\n", debug_info ? debug_info : "none");
      g_clear_error (&err);
      g_free (debug_info);
      data->terminate = TRUE;
      break;
    case GST_MESSAGE_EOS:
      g_print ("End-Of-Stream reached.\n");
      data->terminate = TRUE;
      break;
    case GST_MESSAGE_DURATION:
      /* The duration has changed, mark the current one as invalid */
      data->duration = GST_CLOCK_TIME_NONE;
      break;
    case GST_MESSAGE_STATE_CHANGED: {
      GstState old_state, new_state, pending_state;
      gst_message_parse_state_changed (msg, &old_state, &new_state, &pending_state);
      if (GST_MESSAGE_SRC (msg) == GST_OBJECT (data->playbin)) {
        g_print ("Pipeline state changed from %s to %s:\n",
            gst_element_state_get_name (old_state), gst_element_state_get_name (new_state));

        /* Remember whether we are in the PLAYING state or not */
        data->playing = (new_state == GST_STATE_PLAYING);

        if (data->playing) {
          /* We just moved to PLAYING. Check if seeking is possible */
          GstQuery *query;
          gint64 start, end;
          query = gst_query_new_seeking (GST_FORMAT_TIME);
          if (gst_element_query (data->playbin, query)) {
            gst_query_parse_seeking (query, NULL, &data->seek_enabled, &start, &end);
            if (data->seek_enabled) {
              g_print ("Seeking is ENABLED from %" GST_TIME_FORMAT " to %" GST_TIME_FORMAT "\n",
                  GST_TIME_ARGS (start), GST_TIME_ARGS (end));
            } else {
              g_print ("Seeking is DISABLED for this stream.\n");
            }
          }
          else {
            g_printerr ("Seeking query failed.");
          }
          gst_query_unref (query);
        }
      }
    } break;
    default:
      /* We should not reach here */
      g_printerr ("Unexpected message received.\n");
      break;
  }
  gst_message_unref (msg);
}
```

## 분석
```c++
/* Structure to contain all our information, so we can pass it around */
typedef struct _CustomData {
  GstElement *playbin;  /* Our one and only element */
  gboolean playing;      /* Are we in the PLAYING state? */
  gboolean terminate;    /* Should we terminate execution? */
  gboolean seek_enabled; /* Is seeking enabled for this media? */
  gboolean seek_done;    /* Have we performed the seek already? */
  gint64 duration;       /* How long does this media last, in nanoseconds */
} CustomData;

/* Forward definition of the message processing function */
static void handle_message (CustomData *data, GstMessage *msg);
```

모든 정보를 담고 있는 구조체를 정의하여 함수의 파라미터로 넘길 수 있게 한다.  
메시지 핸들링과 관련된 코드가 크기 때문에 `handle_message`로 옮겼다.  
[Basic tutorial 1: Hello world!]{% post_url /gstreamer/2020-06-04-basic-tutorial-01 %} 에서 본 것과 같이 `playnbin`을 사용하여 파이프라인을 만들었다.  
하지만 `playbin`은 그 자체로 파이프라인이고, 이 예시에서 파이프라인의 유일한 엘리먼트이므로, `playbin` 엘리먼트를 직접 사용하였다.  
그 뒤 `playbin`의 URI를 설정하고 파이프라인을 PLAYING 상태로 설정하였다.  

```c++
msg = gst_bus_timed_pop_filtered (bus, 100 * GST_MSECOND,
    GST_MESSAGE_STATE_CHANGED | GST_MESSAGE_ERROR | GST_MESSAGE_EOS | GST_MESSAGE_DURATION);
```

이전에는 `gst_bus_timed_pop_filtered()`에 타임아웃을 설정하지 않아서 메시지를 받을 때까지 멈추지 않았다.  
이제는 100 밀리초의 타임아웃을 설정하여, 100밀리초가 지나면 `gst_bus_timed_pop_filtered()`는 NULL을 반환할 것이다.  
이 로직은 UI를 업데이트하는데 사용될 것이다.  
타임아웃은 `GstClockTime`을 사용하여 나노초 단위를 쓴다.  
초나 밀리초와 같은 다른 시간 단위를 표현하려면 `GST_SECOND`나 `GST_MSECOND`와 같은 매크로를 곱한다.  

### UI 갱신
```c++
/* We got no message, this means the timeout expired */
if (data.playing) {
```

타임아웃으로 인해 `gst_bus_timed_pop_filtered()`이 NULL을 반환했고 파이프라인이 `PLAYING` 상태면 UI를 갱신한다.  
`PLAYING` 상태가 아니면 대부분의 쿼리가 실패할 것이기 때문에 아무것도 하지 않는다.  
UI 갱신 코드는 초당 10회 불리기 때문에 UI를 갱신하기 위해서 적절한 속도이다.  
현재 미디어의 position를 스크린에 출력할 것인데, 이는 파이프라인에 쿼리해서 알 수 있다.  
파이프라인에 쿼리하기 위해서는 몇가지 절차를 밟아야 하지만, position과 duration을 얻는 쿼리는 일반적으로 사용되므로 좀 더 쉬운 방법을 사용할 수 있다.

```c++
/* Query the current position of the stream */
if (!gst_element_query_position (data.pipeline, GST_FORMAT_TIME, &current)) {
  g_printerr ("Could not query current position.\n");
}
```

`gst_element_query_position()`은 쿼리 객체를 사용하지 않고 바로 position을 알 수 있다.  

```c++
/* If we didn't know it yet, query the stream duration */
if (!GST_CLOCK_TIME_IS_VALID (data.duration)) {
  if (!gst_element_query_duration (data.pipeline, GST_FORMAT_TIME, &data.duration)) {
     g_printerr ("Could not query current duration.\n");
  }
}
```

`gst_element_query_duration()`를 사용하면 duration을 알 수 있다.  


```c++
/* Print current position and total duration */
g_print ("Position %" GST_TIME_FORMAT " / %" GST_TIME_FORMAT "\r",
    GST_TIME_ARGS (current), GST_TIME_ARGS (data.duration));
```

`GST_TIME_FORMAT`과 `GST_TIME_FORMAT`은 GStreamer의 시간을 표현하기 위해 사용되는 매크로이다.  

```c++
/* If seeking is enabled, we have not done it yet, and the time is right, seek */
if (data.seek_enabled && !data.seek_done && current > 10 * GST_SECOND) {
  g_print ("\nReached 10s, performing seek...\n");
  gst_element_seek_simple (data.pipeline, GST_FORMAT_TIME,
      GST_SEEK_FLAG_FLUSH | GST_SEEK_FLAG_KEY_UNIT, 30 * GST_SECOND);
  data.seek_done = TRUE;
}
```

파이프라인에 `gst_element_seek_simple()`을 호출하여 쉽게 seek을 실행시켜본다.  
이 메서드 내부에는 seek을 하기 위한 복잡한 것들이 숨어있다.  
파라미터들에 대한 설명은 아래와 같다.  

- `GST_FORMAT_TIME`  
  seek의 도착 위치가 시간 단위임을 나타낸다.
  다른 seek-format은 다른 단위를 사용하는데, `GstSeekFlags`를 참고하면 된다.  
- `GST_SEEK_FLAG_FLUSH`  
  seek을 하기 전 파이프라인에 있는 모든 데이터를 버린다.  
  파이프라인이 리필되어 새로운 데이터가 나타나기 전까지 잠깐 멈출 수도 있지만, 어플리케이션의 반응성이 높아진다.  
  이 플래그가 없으면, 파이프라인의 끝에서 새로운 위치의 데이터가 나타날 때까지 오래된 데이터가 보여질 것이다.  
- `GST_SEEK_FLAG_KEY_UNIT`  
  인코딩된 비디오 스트림에서 임의의 위치를 seek하는 것은 불가능하고, 키 프레임이라고 불리는 특정 프레임을 seek할 수 있다.  
  이 플래그가 설정되면, seek은 가장 가까운 키 프레임으로 이동한 후 데이터를 생성하기 시작한다.  
  이 플래그가 설정되지 않으면, 파이프라인은 가장 가까운 키 프레임으로 이동하지만 요청한 position에 도달할 때까지 데이터는 보이지 않을 것이다.  
  이는 좀 더 정확하지만 시간이 더 걸리게 된다.  
- `GST_SEEK_FLAG_ACCURATE`  
  몇몇 미디어 클립은 인덱싱 정보를 충분히 제공하지 않아서 임의의 위치로 seek을 하는데 시간이 걸리게 된다.  
  이러한 경우, Gstreamer는 seek을 할 위치를 예측하는데 일반적으로 잘 작동한다.  
  정확한 시간을 찾아야해서 정밀도가 충분하지 않으면, 이 플래그를 사용한다.  
  seek하는 위치를 계산하는 것이 시간이 오래 걸릴 수도 있음에 유의한다.  
- `GST_FORMAT_TIME`의 형식으로 요청할 경우 나노초 단위로 파라미터를 받으므로, 쉽게 표현하기 위해 원하는 초 단위 시간에 `GST_SECOND`를 곱한다.  

### Message Pump
`handle_message` 함수는 파이프라인의 버스로부터 오는 모든 메시지를 처리한다.  
`ERROR`와 `EOS`의 처리는 이전 튜토리얼에 다루었으므로 생략한다.  

```c++
case GST_MESSAGE_DURATION:
  /* The duration has changed, mark the current one as invalid */
  data->duration = GST_CLOCK_TIME_NONE;
  break;
```

이 메시지는 스트림의 duration이 변경되었을 때 발생한다.  
여기에서는 단순히 현재 duration을 유효하지 않다고 표시하여, 나중에 다시 쿼리를 받을 때 설정될 것이다.  

```c++
case GST_MESSAGE_STATE_CHANGED: {
  GstState old_state, new_state, pending_state;
  gst_message_parse_state_changed (msg, &old_state, &new_state, &pending_state);
  if (GST_MESSAGE_SRC (msg) == GST_OBJECT (data->pipeline)) {
    g_print ("Pipeline state changed from %s to %s:\n",
        gst_element_state_get_name (old_state), gst_element_state_get_name (new_state));

    /* Remember whether we are in the PLAYING state or not */
    data->playing = (new_state == GST_STATE_PLAYING);
```
seek과 시간 관련 쿼리는 `PAUSED`나 `PLAYING` 상태일 때만 유효한 결과를 얻을 수 있다.  
`playing` 변수를 파이프라인을 `PLAYING` 상태인지 확인하는데 사용한다.  


```c++
if (data->playing) {
  /* We just moved to PLAYING. Check if seeking is possible */
  GstQuery *query;
  gint64 start, end;
  query = gst_query_new_seeking (GST_FORMAT_TIME);
  if (gst_element_query (data->pipeline, query)) {
    gst_query_parse_seeking (query, NULL, &data->seek_enabled, &start, &end);
    if (data->seek_enabled) {
      g_print ("Seeking is ENABLED from %" GST_TIME_FORMAT " to %" GST_TIME_FORMAT "\n",
          GST_TIME_ARGS (start), GST_TIME_ARGS (end));
    } else {
      g_print ("Seeking is DISABLED for this stream.\n");
    }
  }
  else {
    g_printerr ("Seeking query failed.");
  }
  gst_query_unref (query);
}
```

만약 `PLAYING` 상태에 들어섰다면, 첫번쨰 쿼리를 수행한다.  
파이프라인이 스트림에서 seek이 가능한지 요청한다.  
`gst_query_new_seeking()`은 `GST_FORMAT_TIME` 포맷의 "seeking" 타입의 쿼리 객체를 생성한다.  
이는 이동하고 싶은 위치를 시간으로 표시하여 seek하는 것에 관심이 있다는 것을 나타낸다.  
`GST_FORMAT_BYTES`를 사용하여 특정 바이트 위치로 seek하는 것을 요청할 수 있지만 일반적으로 덜 유용하다.  
쿼리 객체는 `gst_element_query()`을 사용하여 파이프라인으로 전달된다.  
결과는 동일한 쿼리에 저장되며 `gst_query_parse_seeking()`을 통해 얻을 수 있다.  
seek이 가능한가에 대한 boolean 값과 seek이 가능한 구간을 얻을 수 있다.  
쿼리 객체를 다 사용하였으면 unref하여 해제한다.  
이와 같은 정보가 있으면, 미디어플레이어는 주기적으로 현재 위치를 파악하여 슬라이더를 업데이트할 수 있고 슬라이더를 움직임으로써 seek을 할 수도 있다.  

## LICENSE

{% include_relative LICENSE %}
