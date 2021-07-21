---

title:  "[해결법] ModuleNotFoundError: No module named 'keras'"
---

[Keras Documentation](https://keras.io/ko/applications/)을 보면 ResNet50와 같은 모델을 사용한 예시 코드가 있다.

```python
from keras.applications.resnet50 import ResNet50
```

하지만 이 코드를 실행하면 `ModuleNotFoundError: No module named 'keras'`가 발생한다.

## 해결방법
아래와 같이 코드를 변경한다.

```python
from tensorflow.keras.applications.resnet50 import ResNet50
```
