---

title:  "Jekyll에서 html 언어 설정하기"
---

## html language attribute
웹페이지 내 텍스트의 기본 언어값에 대한 속성이다.  
한국어는 "ko"이다.
- 참고자료 : [웹 접근성 html lang 속성](https://miaow-miaow.tistory.com/26)

## Jekyll에서 html lang="ko" 설정하기
- 한줄 요약: `_config.yml`에서 `lang="ko"`를 추가한다.

minima theme의 경우, `_layouts/default.html`을 보면 아래와 같은 코드가 있다.
```html
{% raw %}<html lang="{{ page.lang | default: site.lang | default: "en" }}">{% endraw %}
```
page에 지정된 lang, site에 지정된 lang, 둘 다 없으면 "en"으로 설정한다는 의미이다.  
`site` 변수는 `_config.yml`에서 설정할 수 있다.  
즉, `_config.yml`에 `lang` 속성을 찾아 `"ko"`로 변경하거나 추가하면 된다.  
다른 theme을 쓰는 경우도 비슷한 코드가 있을 테니 검색해서 어떻게 구성되어 있는지 찾아 변경하면 된다.
