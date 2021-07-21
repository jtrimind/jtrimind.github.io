---

title:  "Gstreamer 기본 튜토리얼 01: Hello world!"
---

## 목표
video를 재생시켜본다.

## basic-tutorial-1.c
```c++
#include <gst/gst.h>

int
main (int argc, char *argv[])
{
  GstElement *pipeline;
  GstBus *bus;
  GstMessage *msg;

  /* Initialize GStreamer */
  gst_init (&argc, &argv);

  /* Build the pipeline */
  pipeline =
      gst_parse_launch
      ("playbin uri=https://www.freedesktop.org/software/gstreamer-sdk/data/media/sintel_trailer-480p.webm",
      NULL);

  /* Start playing */
  gst_element_set_state (pipeline, GST_STATE_PLAYING);

  /* Wait until error or EOS */
  bus = gst_element_get_bus (pipeline);
  msg =
      gst_bus_timed_pop_filtered (bus, GST_CLOCK_TIME_NONE,
      GST_MESSAGE_ERROR | GST_MESSAGE_EOS);

  /* Free resources */
  if (msg != NULL)
    gst_message_unref (msg);
  gst_object_unref (bus);
  gst_element_set_state (pipeline, GST_STATE_NULL);
  gst_object_unref (pipeline);
  return 0;
}
```

## 빌드
리눅스의 경우 아래와 같이 하면 된다.  

```bash 
gcc basic-tutorial-1.c -o basic-tutorial-1 `pkg-config --cflags --libs gstreamer-1.0`
```

자세한 정보는 아래를 참고한다.  

- [Linux](https://gstreamer.freedesktop.org/documentation/installing/on-linux.html#InstallingonLinux-Build)
- [Mac OS X](https://gstreamer.freedesktop.org/documentation/installing/on-mac-osx.html#InstallingonMacOSX-Build)
- [Windows](https://gstreamer.freedesktop.org/documentation/installing/on-windows.html#InstallingonWindows-Build)

## 분석

### gst_init
```c
/* Initialize GStreamer */
gst_init (&argc, &argv);
```
처음에는 `gst_init()`을 호출하여 초기화를 해준다.

### gst_parse_lauch
```c++
/* Build the pipeline */
pipeline =
    gst_parse_launch
    ("playbin uri=https://www.freedesktop.org/software/gstreamer-sdk/data/media/sintel_trailer-480p.webm",
    NULL);
```
Gstreamer에서 `파이프라인`은 각 `엘리먼트`들을 조합해야하지만, 파이프라인이 간단하다면 `gst_parse_launch()`로 쉽게 만들 수 있다.

### playbin
playbin은 특별한 엘리먼트로 내부적으로 필요한 엘리먼트들을 연결한다.

```c++
/* Start playing */
gst_element_set_state (pipeline, GST_STATE_PLAYING);
```
Gstreamer 엘리먼트는 상태를 가지고 있으며, `gst_element_set_state`로 `GST_STATE_PLAYING` 상태로 변경한다.  

```c++
/* Wait until error or EOS */
bus = gst_element_get_bus (pipeline);
msg =
    gst_bus_timed_pop_filtered (bus, GST_CLOCK_TIME_NONE,
    GST_MESSAGE_ERROR | GST_MESSAGE_EOS);
```
`gst_element_get_bus()`는 파이프라인의 버스를 얻고, `gst_bus_timed_pop_filtered()`는 버스로부터 ERROR나 EOS를 얻기 전까지 블록된다.  

### cleanup
```c++
/* Free resources */
if (msg != NULL)
  gst_message_unref (msg);
gst_object_unref (bus);
gst_element_set_state (pipeline, GST_STATE_NULL);
gst_object_unref (pipeline);
```
`gst_bus_timed_pop_filtered()`로 얻은 메시지를 `gst_message_unref()`로 해제한다.  
`gst_element_get_bus()`로 얻은 버스를 `gst_object_unref()`로 해제한다.  
파이프라인을 `GST_STATE_NULL` 상태로 설정하고, 파이프라인과 내부의 엘리먼트들을 `gst_object_unref()`로 해제한다.  

## 참고사이트
[Basic tutorial 1: Hello world!](https://gstreamer.freedesktop.org/documentation/tutorials/basic/hello-world.html?gi-language=c)

## LICENSE

{% include_relative LICENSE %}
