---

title:  "[해결법] AttributeError: module 'tensorflow._api.v2.lite' has no attribute 'constants'"
---

tensorflow 2에서 `tf.lite.constants.QUANTIZED_UINT8`와 같은 deprecated된 값을 사용시 발생한다.

## 해결법
아래와 같이 변경한다.  
tensorflow 1: tensorflow 2
- `tf.lite.constants.FLOAT`: `tf.float32`
- `tf.lite.constants.INT8`: `tf.int8`
- `tf.lite.constants.INT32`: `tf.int32`
- `tf.lite.constants.INT64`: `tf.int64`
- `tf.lite.constants.STRING`: `tf.string`
- `tf.lite.constants.QUANTIZED_UINT8`: `tf.uint8`

## 참고 사이트
[1x_compatibility](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/lite/g3doc/convert/1x_compatibility.md#liteconstants-api)
