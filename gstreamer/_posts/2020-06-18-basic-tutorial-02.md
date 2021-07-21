---

title:  "Gstreamer 기본 튜토리얼 02: GStreamer concepts"
---

## 목표
아래 항목들을 배운다.
- Gstreamer element와 만드는 방법
- 각 element를 연결하는 방법
- element의 동작을 커스터마이징하는 방법
- bus의 에러 상태를 보고 Gstreamer 메시지를 추출하는 방법

## Manual Hello World

```c++
#include <gst/gst.h>

int main(int argc, char *argv[]) {
  GstElement *pipeline, *source, *sink;
  GstBus *bus;
  GstMessage *msg;
  GstStateChangeReturn ret;

  /* Initialize GStreamer */
  gst_init (&argc, &argv);

  /* Create the elements */
  source = gst_element_factory_make ("videotestsrc", "source");
  sink = gst_element_factory_make ("autovideosink", "sink");

  /* Create the empty pipeline */
  pipeline = gst_pipeline_new ("test-pipeline");

  if (!pipeline || !source || !sink) {
    g_printerr ("Not all elements could be created.\n");
    return -1;
  }

  /* Build the pipeline */
  gst_bin_add_many (GST_BIN (pipeline), source, sink, NULL);
  if (gst_element_link (source, sink) != TRUE) {
    g_printerr ("Elements could not be linked.\n");
    gst_object_unref (pipeline);
    return -1;
  }

  /* Modify the source's properties */
  g_object_set (source, "pattern", 0, NULL);

  /* Start playing */
  ret = gst_element_set_state (pipeline, GST_STATE_PLAYING);
  if (ret == GST_STATE_CHANGE_FAILURE) {
    g_printerr ("Unable to set the pipeline to the playing state.\n");
    gst_object_unref (pipeline);
    return -1;
  }

  /* Wait until error or EOS */
  bus = gst_element_get_bus (pipeline);
  msg = gst_bus_timed_pop_filtered (bus, GST_CLOCK_TIME_NONE, GST_MESSAGE_ERROR | GST_MESSAGE_EOS);

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
        break;
      case GST_MESSAGE_EOS:
        g_print ("End-Of-Stream reached.\n");
        break;
      default:
        /* We should not reach here because we only asked for ERRORs and EOS */
        g_printerr ("Unexpected message received.\n");
        break;
    }
    gst_message_unref (msg);
  }

  /* Free resources */
  gst_object_unref (bus);
  gst_element_set_state (pipeline, GST_STATE_NULL);
  gst_object_unref (pipeline);
  return 0;
}
```

## 분석
element는 Gstreamer의 기본 요소이다. 데이터 프로세스는 source element(데이터 생산자)로부터 filter element를 거쳐 sink element(데이터 소비자)로 흐른다.  
![Example Pipeline](https://gstreamer.freedesktop.org/documentation/tutorials/basic/images/figure-1.png)

### Element creation
```c++
/* Create the elements */
source = gst_element_factory_make ("videotestsrc", "source");
sink = gst_element_factory_make ("autovideosink", "sink");
```
`gst_element_factory_make()`로 element를 만들 수 있다.  
첫번째 파라미터는 element의 타입이고, 두번째 파라미터는 element의 이름이다. 두번째 파라미터에 NULL을 넣어도, Gstreamer는 유일한 이름을 짓는다.  
`videotestsrc`는 source element로 테스트를 위한 비디오 패턴을 생성한다.  
`autovideosink`는 sink element로 받은 이미지를 윈도우로 보여준다.  

### Pipeline creation
```c++
/* Create the empty pipeline */
pipeline = gst_pipeline_new ("test-pipeline");
```
모든 element는 pipeline 안에 포함되어야 한다. `gst_pipeline_new()`로 pipeline을 만들 수 있다.

```c++
/* Build the pipeline */
gst_bin_add_many (GST_BIN (pipeline), source, sink, NULL);
if (gst_element_link (source, sink) != TRUE) {
  g_printerr ("Elements could not be linked.\n");
  gst_object_unref (pipeline);
  return -1;
}
```
pipeline은 다른 element를 담을 때 쓰는 element인 `bin`의 특정한 타입이다.  
`gst_bin_add_many()`을 호출하여 element를 pipeline에 추가한다. 이 함수는 element들과 마지막에 NULL을 인자로 받는다.  
`gst_bin_add()`를 사용하여 하나씩 추가할 수도 있다.  
이 element들은 같은 bin안에 있지만 연결되지는 않은 상태이다.  
`gst_element_link()`를 사용하여 첫번째 파라미터부터 두번째 파라미터까지 연결한다. 같은 bin 안에 있어야만 연결할 수 있다.  

### Properties
```c++
/* Modify the source's properties */
g_object_set (source, "pattern", 0, NULL);
```
Gstreamer element는 커스터마이징이 가능한 속성들을 가지고 있다.  
`g_object_get()`로 읽고, `g_object_set()`로 쓴다.  
`g_object_set()`은 NULL을 마지막으로 받는 <속성 이름, 속성 값> 쌍의 리스트를 받을 수 있다.  
위 코드는 `videotestsrc`의 "pattern" 속성을 0으로 설정한다.  

### Error checking
```c++
/* Start playing */
ret = gst_element_set_state (pipeline, GST_STATE_PLAYING);
if (ret == GST_STATE_CHANGE_FAILURE) {
  g_printerr ("Unable to set the pipeline to the playing state.\n");
  gst_object_unref (pipeline);
  return -1;
}
```
`gst_element_set_state()`의 반환값을 확인한다.  

```c++
/* Wait until error or EOS */
bus = gst_element_get_bus (pipeline);
msg = gst_bus_timed_pop_filtered (bus, GST_CLOCK_TIME_NONE, GST_MESSAGE_ERROR | GST_MESSAGE_EOS);

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
      break;
    case GST_MESSAGE_EOS:
      g_print ("End-Of-Stream reached.\n");
      break;
    default:
      /* We should not reach here because we only asked for ERRORs and EOS */
      g_printerr ("Unexpected message received.\n");
      break;
  }
  gst_message_unref (msg);
}
```
`gst_bus_timed_pop_filtered()`은 실행이 중단될때까지 대기했다가 `GstMessage`를 받는다. 반환값을 확인하여 화면에 출력한다.  
`GST_MESSAGE_TYPE()`를 사용하여 `GstMessage`가 error를 포함하는지 확인할 수 있다.  
그 뒤, `gst_message_parse_error()`를 사용하여 Glib의 `GError` 형태로 파싱할 수 있다.  

## 참고사이트
[Basic tutorial 2: GStreamer concepts](https://gstreamer.freedesktop.org/documentation/tutorials/basic/concepts.html?gi-language=c)

## LICENSE

{% include_relative LICENSE %}
