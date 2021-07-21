---

title:  "[해결법] AttributeError: module 'tensorflow' has no attribute 'enable_eager_execution'"
---

`AttributeError: module 'tensorflow' has no attribute 'enable_eager_execution'` 에러는 tensorflow 2.0이상에서 enable_eager_execution()을 실행 시 나타난다.

## 해결방법

### 아무것도 안함(추천)
tensorflow 2.0에서는 eager mode가 기본적으로 활성화되어 있다.  
tf.executing_eagerly()으로 확인할 수 있다.
```python
tf.executing_eagerly()
```
```
True
```

### tf1.compat.v1 사용(비추천)
tf.compat.v1.enable_eager_execution() 을 실행한다.

## 참고자료
[Tensorflow Eager execution](https://www.tensorflow.org/guide/eager)
