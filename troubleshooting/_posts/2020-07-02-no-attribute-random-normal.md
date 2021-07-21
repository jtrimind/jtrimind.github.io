---

title:  "[해결법] AttributeError: module 'tensorflow' has no attribute 'random_normal'"
---

`AttributeError: module 'tensorflow' has no attribute 'random_normal'` 에러는 tensorflow 2에서 random_normal()을 실행 시 나타난다.  

## 해결방법
### 방법 1. random.normal 사용
tensorflow 2에서는 random_* 함수들이 random.*로 변경되었다.  
tensorflow 2를 쓴다면 random.normal을 사용하자.
- [tf.random.normal API 문서](https://www.tensorflow.org/api_docs/python/tf/random/normal)

```python
tf.random.normal(shape)
```

### 방법 2. v1 API 사용
tensorflow 1 문법을 사용하기 위해서는 compat.v1을 추가하여 사용하면 된다.  

```python
tf.compat.v1.random_normal(shape)
```
