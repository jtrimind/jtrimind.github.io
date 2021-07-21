---

title:  "Gstreamer 기본 튜토리얼 03: Dynamic pipelines"
---

## 목표
- pipeline을 즉석에서 만드는 법
- element들을 연결할 때 미세 조정하는 법
- 관심있는 event를 notify 받아 대응하는 법
- element가 가질 수 있는 상태

## 소개
Gstreamer element들이 서로 소통하기 위한 port를 pad(`GstPad`)라고 한다.  
element에서 data가 빠져나가는 곳은 src pad이고, element로 data가 입력받는 곳은 sink pad이다.  
source element는 src pad만 있는 element이고, sink element는 sink pad만 있는 element이다.  
filter element는 src pad와 sink pad가 있는 element이다.
  
![Gstreamer source element와 pad](https://gstreamer.freedesktop.org/documentation/tutorials/basic/images/src-element.png)  
![Gstreamer filter element와 pad](https://gstreamer.freedesktop.org/documentation/tutorials/basic/images/filter-element.png)  
![Gstreamer sink element와 pad](https://gstreamer.freedesktop.org/documentation/tutorials/basic/images/sink-element.png)

demux는 mux된 데이터가 입력되는 1개의 sink pad와 각각의 stream을 위한 여러 개의 src pad를 가지고 있다.  
![2개의 src pad를 가진 demuxer](https://gstreamer.freedesktop.org/documentation/tutorials/basic/images/filter-element-multi.png)

## Dynamic Hello World
```c++
#include <gst/gst.h>

/* Structure to contain all our information, so we can pass it to callbacks */
typedef struct _CustomData {
  GstElement *pipeline;
  GstElement *source;
  GstElement *convert;
  GstElement *resample;
  GstElement *sink;
} CustomData;

/* Handler for the pad-added signal */
static void pad_added_handler (GstElement *src, GstPad *pad, CustomData *data);

int main(int argc, char *argv[]) {
  CustomData data;
  GstBus *bus;
  GstMessage *msg;
  GstStateChangeReturn ret;
  gboolean terminate = FALSE;

  /* Initialize GStreamer */
  gst_init (&argc, &argv);

  /* Create the elements */
  data.source = gst_element_factory_make ("uridecodebin", "source");
  data.convert = gst_element_factory_make ("audioconvert", "convert");
  data.resample = gst_element_factory_make ("audioresample", "resample");
  data.sink = gst_element_factory_make ("autoaudiosink", "sink");

  /* Create the empty pipeline */
  data.pipeline = gst_pipeline_new ("test-pipeline");

  if (!data.pipeline || !data.source || !data.convert || !data.resample || !data.sink) {
    g_printerr ("Not all elements could be created.\n");
    return -1;
  }

  /* Build the pipeline. Note that we are NOT linking the source at this
   * point. We will do it later. */
  gst_bin_add_many (GST_BIN (data.pipeline), data.source, data.convert, data.resample, data.sink, NULL);
  if (!gst_element_link_many (data.convert, data.resample, data.sink, NULL)) {
    g_printerr ("Elements could not be linked.\n");
    gst_object_unref (data.pipeline);
    return -1;
  }

  /* Set the URI to play */
  g_object_set (data.source, "uri", "https://www.freedesktop.org/software/gstreamer-sdk/data/media/sintel_trailer-480p.webm", NULL);

  /* Connect to the pad-added signal */
  g_signal_connect (data.source, "pad-added", G_CALLBACK (pad_added_handler), &data);

  /* Start playing */
  ret = gst_element_set_state (data.pipeline, GST_STATE_PLAYING);
  if (ret == GST_STATE_CHANGE_FAILURE) {
    g_printerr ("Unable to set the pipeline to the playing state.\n");
    gst_object_unref (data.pipeline);
    return -1;
  }

  /* Listen to the bus */
  bus = gst_element_get_bus (data.pipeline);
  do {
    msg = gst_bus_timed_pop_filtered (bus, GST_CLOCK_TIME_NONE,
        GST_MESSAGE_STATE_CHANGED | GST_MESSAGE_ERROR | GST_MESSAGE_EOS);

    /* Parse message */
    if (msg != NULL) {
      GError *err;
      gchar *debug_info;

      switch (GST_MESSAGE_TYPE (msg)) {
        case GST_MESSAGE_ERROR:
          gst_message_parse_error (msg, &err, &debug_info);
          g_printerr ("Error received from element %s: %s\n", GST_OBJECT_NAME (msg->src), err->message);
          g_printerr ("Debugging information: %s\n", debug_info ? debug_info : "none");
          g_clear_error (&err);
          g_free (debug_info);
          terminate = TRUE;
          break;
        case GST_MESSAGE_EOS:
          g_print ("End-Of-Stream reached.\n");
          terminate = TRUE;
          break;
        case GST_MESSAGE_STATE_CHANGED:
          /* We are only interested in state-changed messages from the pipeline */
          if (GST_MESSAGE_SRC (msg) == GST_OBJECT (data.pipeline)) {
            GstState old_state, new_state, pending_state;
            gst_message_parse_state_changed (msg, &old_state, &new_state, &pending_state);
            g_print ("Pipeline state changed from %s to %s:\n",
                gst_element_state_get_name (old_state), gst_element_state_get_name (new_state));
          }
          break;
        default:
          /* We should not reach here */
          g_printerr ("Unexpected message received.\n");
          break;
      }
      gst_message_unref (msg);
    }
  } while (!terminate);

  /* Free resources */
  gst_object_unref (bus);
  gst_element_set_state (data.pipeline, GST_STATE_NULL);
  gst_object_unref (data.pipeline);
  return 0;
}

/* This function will be called by the pad-added signal */
static void pad_added_handler (GstElement *src, GstPad *new_pad, CustomData *data) {
  GstPad *sink_pad = gst_element_get_static_pad (data->convert, "sink");
  GstPadLinkReturn ret;
  GstCaps *new_pad_caps = NULL;
  GstStructure *new_pad_struct = NULL;
  const gchar *new_pad_type = NULL;

  g_print ("Received new pad '%s' from '%s':\n", GST_PAD_NAME (new_pad), GST_ELEMENT_NAME (src));

  /* If our converter is already linked, we have nothing to do here */
  if (gst_pad_is_linked (sink_pad)) {
    g_print ("We are already linked. Ignoring.\n");
    goto exit;
  }

  /* Check the new pad's type */
  new_pad_caps = gst_pad_get_current_caps (new_pad);
  new_pad_struct = gst_caps_get_structure (new_pad_caps, 0);
  new_pad_type = gst_structure_get_name (new_pad_struct);
  if (!g_str_has_prefix (new_pad_type, "audio/x-raw")) {
    g_print ("It has type '%s' which is not raw audio. Ignoring.\n", new_pad_type);
    goto exit;
  }

  /* Attempt the link */
  ret = gst_pad_link (new_pad, sink_pad);
  if (GST_PAD_LINK_FAILED (ret)) {
    g_print ("Type is '%s' but link failed.\n", new_pad_type);
  } else {
    g_print ("Link succeeded (type '%s').\n", new_pad_type);
  }

exit:
  /* Unreference the new pad's caps, if we got them */
  if (new_pad_caps != NULL)
    gst_caps_unref (new_pad_caps);

  /* Unreference the sink pad */
  gst_object_unref (sink_pad);
}
```

## 분석
```c++
/* Structure to contain all our information, so we can pass it to callbacks */
typedef struct _CustomData {
  GstElement *pipeline;
  GstElement *source;
  GstElement *convert;
  GstElement *sink;
} CustomData;
```
콜백에 사용하기 위한 정보들을 모은 구조체이다.  
  
```c++
/* Create the elements */
data.source = gst_element_factory_make ("uridecodebin", "source");
data.convert = gst_element_factory_make ("audioconvert", "convert");
data.resample = gst_element_factory_make ("audioresample", "resample");
data.sink = gst_element_factory_make ("autoaudiosink", "sink");
```
`uridecodebin`은 내부적으로 필요한 element들을 구성하여 uri를 raw audio나 video로 변환한다.  
`audioconvert`는 오디오 포맷을 변환한다.  
`audioresample`은 오디오 샘플링 레이트를 변환한다.  
`autoaudiosink`는 오디오 스트림을 오디오 카드에 렌더링한다.  
  
```c++
if (!gst_element_link (data.convert, data.sink)) {
  g_printerr ("Elements could not be linked.\n");
  gst_object_unref (data.pipeline);
  return -1;
}
```
convert부터 sink까지 연결한다.(이 시점에서 source는 source pad가 없으므로 연결하지 않는다.)  

```c++
/* Set the URI to play */
g_object_set (data.source, "uri", "https://www.freedesktop.org/software/gstreamer-sdk/data/media/sintel_trailer-480p.webm", NULL);
```
uri를 설정한다.  

### Signal
```c++
/* Connect to the pad-added signal */
g_signal_connect (data.source, "pad-added", G_CALLBACK (pad_added_handler), &data);
```
`Gsignals`는 무언가가 발생하면 콜백을 통해 알려준다. signal은 이름으로 구분되고, 각 `GObject`는 자신의 signal을 가진다.  
source에 "pad-added" signal을 부착하기 위해 `g_signal_connect()`를 사용한다. 이 때 콜백함수인 pad_added_handler와 콜백함수와 공유할 data의 포인터를 함께 넘긴다.  

### Callback
source element가 data를 생산하기 시작하기에 충분한 정보를 가졌을 때 source pad를 생성하고 "pad-added" signal이 촉발되어 콜백 함수가 호출된다.  
```c++
static void pad_added_handler (GstElement *src, GstPad *new_pad, CustomData *data) {
```
`src`는 signal을 촉발한 `GstElement`로, 예제에서는 `urldecodebin`이다.  
`new_pad`는 `src` element에 추가된 `GstPad`이다.  
`data`는 signal을 부착할 때 제공한 포인터로, 예제에서는 `CustomData`이다.  
  
```c++
GstPad *sink_pad = gst_element_get_static_pad (data->convert, "sink");
```
`gst_element_get_static_pad()`를 사용하여 convert element의 sink pad를 얻을 수 있다. 이 pad는 `new_pad`와 연결할 것이다.  
  
```c++
/* If our converter is already linked, we have nothing to do here */
if (gst_pad_is_linked (sink_pad)) {
  g_print ("We are already linked. Ignoring.\n");
  goto exit;
}
```
`uridecodebin`은 많은 pad를 생성하는데, 그 때마다 콜백이 호출된다. 위 코드는 이미 연결된 pad가 new_pad와 연결되는 것을 방지한다.  
  
```c++
/* Check the new pad's type */
new_pad_caps = gst_pad_get_current_caps (new_pad, NULL);
new_pad_struct = gst_caps_get_structure (new_pad_caps, 0);
new_pad_type = gst_structure_get_name (new_pad_struct);
if (!g_str_has_prefix (new_pad_type, "audio/x-raw")) {
  g_print ("It has type '%s' which is not raw audio. Ignoring.\n", new_pad_type);
  goto exit;
}
```
new_pad의 type을 확인하여, aduio인지 확인한다.  
`gst_pad_get_current_caps()`는 pad의 현재 capablity를 `GstCaps` 구조체의 형태로 얻는다.  
이 경우에는 pad가 audio capablity만 가지고 있기 때문에, `gst_caps_get_structure()`로 첫번째 `GstStructure`를 얻는다.  
`gst_structure_get_name()`로 이름을 가져오고, `audio/x-raw`가 아니면, decoded audio pad가 아니므로 무시한다.  
  
```c++
/* Attempt the link */
ret = gst_pad_link (new_pad, sink_pad);
if (GST_PAD_LINK_FAILED (ret)) {
  g_print ("Type is '%s' but link failed.\n", new_pad_type);
} else {
  g_print ("Link succeeded (type '%s').\n", new_pad_type);
}
```
`gst_pad_link()`는 두 pad를 연결한다.

### GStreamer States
Gstreamer에는 4가지 상태가 있다.

| State   | Description                       |
|---------|-----------------------------------|
| NULL    | element가 초기화된 상태           |
| READY   | element가 PAUSED될 준비가 된 상태 |
| PAUSED  | data를 받고 처리할 준비가 된 상태 |
| PLAYING | data가 흐르고 있는 상태           |

각 상태는 이웃한 상태로만 이동할 수 있으므로 `NULL`에서 `PLAYING`으로 바로 이동할 수 없고, `READY`와 `PAUSED`를 거쳐야 한다.  
  
```c++
case GST_MESSAGE_STATE_CHANGED:
  /* We are only interested in state-changed messages from the pipeline */
  if (GST_MESSAGE_SRC (msg) == GST_OBJECT (data.pipeline)) {
    GstState old_state, new_state, pending_state;
    gst_message_parse_state_changed (msg, &old_state, &new_state, &pending_state);
    g_print ("Pipeline state changed from %s to %s:\n",
        gst_element_state_get_name (old_state), gst_element_state_get_name (new_state));
  }
  break;
```
상태변경에 대한 bus message를 받는다.  
pipeline으로부터 오는 메시지만 받도록 필터링하였다.  

## 참고사이트
[Basic tutorial 3: Dynamic pipelines](https://gstreamer.freedesktop.org/documentation/tutorials/basic/dynamic-pipelines.html?gi-language=c)

## LICENSE

{% include_relative LICENSE %}
