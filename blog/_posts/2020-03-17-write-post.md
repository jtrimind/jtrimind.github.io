---

title:  "Jekyll 게시글(post) 작성하기"
---

## 요약
- `_posts` 디렉토리에
- `YEAR-MONTH-DAY-title.MARKUP` 파일형식으로
- Front matter를 포함한 post를 저장하면 된다.

## 참고 사이트
- [Jekyll 머리말](https://jekyllrb-ko.github.io/docs/step-by-step/03-front-matter/)
- welcome-to-jekyll.markdown

## post 저장 위치
- `_posts`

Jekyll의 post들은 `_posts` 디렉토리에 존재한다.  
[Jekyll 설치하기](2020-03-16-install-jekyll.md)를 따라했다면, `_post` 디렉토리에 `YEAR-MONTH-DAY-welcome-to-jekyll.markdown` 파일을 볼 수 있을 것이다. 

## post 파일이름 포맷
- `YEAR-MONTH-DAY-title.MARKUP`

Jekyll post는 `YEAR-MONTH-DAY-title.MARKUP`와 같은 형식의 파일이름을 가져야 한다.  
`YEAR`은 4자리, `MONTH`, `DAY`는 2자리 숫자이다.  
`title`는 post 제목을 쓰면 되고, `MARKUP`은 `markdown`이나 `md`같은 확장자를 쓰면 된다.  

## Front matter
Front matter는 `---`로 감싼 YAML 코드 조각이다.  

```yaml
# Front matter 예시
---

title: "Jekyll 게시글(post) 작성하기"
---
```

Front matter는 post의 제일 처음에 위치한다.  
Jekyll이 Liquid 태그를 처리하기 위해서 front matter가 있어야 한다.  
Jekyll 테마마다 필요한 값이 다른데, `minima`(기본적으로 설치된 테마)를 사용 시 post에는 layout과 title이 있어야 한다.  
