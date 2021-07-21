---

title:  "Jekyll minima theme"
---

## minima
minima는 어디에서나 적용할 수 있는(one-size-fit-all) Jekyll 테마이다.  
또한 기본적으로 적용되는 테마이기도 하다.

## minima 파일구조 보기
minima theme가 어떻게 구성되어있는지 보는 방법은 2가지가 있다.

1. github에서 보기: [minima github](https://github.com/jekyll/minima)
2. gem 설치 디렉토리에서 확인
   1. bash에서 `gem env`
   2. INSTALLATION DIRECTORY 확인
   3. (INSTALLATION DIRECTORY)/minima-(버전명)

## minima 적용
`_config.yml`에 `theme: minima`로 변경/추가

## minima 커스터마이징
minima 파일을 자신의 저장소에 가져와서 수정한다.  
예를 들어 post layout을 변경하고 싶다면, `minima-2.5.1/_layouts/post.html`을 `(블로그저장소)/_layouts/post.html`에 복붙한 뒤, `post.html`을 수정한다.
