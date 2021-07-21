---

title:  "[해결법] ./tensorflow/lite/schema/schema_generated.h:20:10: fatal error: flatbuffers/flatbuffers.h: No such file or directory"
---

tensorflow-lite를 빌드하기 위해 `./tensorflow/lite/tools/make/build_lib.sh`를 실행하다 제목과 같은 오류가 발생할 수 있다.  

## 해결법
`./tensorflow/lite/tools/make/download_dependencies.sh`를 실행하여 dependency를 설치한다.

## 참고자료
- [Build TensorFlow Lite for Raspberry Pi](https://www.tensorflow.org/lite/guide/build_rpi)
- [tensorflow issue](https://github.com/tensorflow/tensorflow/issues/21965)
