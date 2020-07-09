---
layout: post
title:  "[해결법] AttributeError: module 'tensorflow' has no attribute 'random_normal'"
---

`AttributeError: module 'tensorflow' has no attribute 'random_normal'` 에러는 tensorflow 2에서 random_normal()을 실행 시 나타난다.  

## 해결방법
tensorflow 2에서는 random_* 함수들이 random.*로 변경되었다.  
tensorflow 1 문법을 사용하기 위해서는 compat.v1을 추가하여 사용하면 된다.  
- tf.random.normal 사용
- tf.compat.v1.random_normal 사용
