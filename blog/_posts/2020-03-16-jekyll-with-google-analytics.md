---

title:  "Jekyll에 Google Analytics 적용하기"
---

## Google Analytics 적용하기
[Google Analytics](https://analytics.google.com/)를 이용하여 블로그를 방문하는 사람들을 분석할 수 있다.  
Jekyll을 사용하고 있다면 간단히 적용 가능하다.  

- Google Analytics 계정생성
- `관리` -> (`관리자` -> `속성` ->) `속성 설정` (-> `기본 설정`)에 `추적 ID` 값을 복사
- _config.yml에 `google_analytics` 혹은 `g-analytics` 항목에 추적 ID 기입(없으면 추가)

```yml
# _config.yml
google_analytics: 'UA-*mynumber*'
```

## 참고 사이트
[[깃블로그] 깃블로그(jekyll)에 구글 애널리틱스(Google Analytics) 적용법!](https://mingnol2.tistory.com/70)
