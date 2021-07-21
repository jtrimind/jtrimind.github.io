---

title:  "[해결법] AttributeError: 'NoneType' object has no attribute 'image_data_format'"
---

아래와 같이 keras application을 import했을 경우 `AttributeError: 'NoneType' object has no attribute 'image_data_format'`와 같은 에러가 발생할 수 있다.

```python
from keras_applications.resnet50 import ResNet50

model = ResNet50(weights='imagenet')
```

## 해결방법
아래와 같이 코드를 변경한다.

```python
from tensorflow.keras.applications.resnet50 import ResNet50
```
