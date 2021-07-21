---

title:  "Jekyll post 링크하기"
---

## post_url
- `post_url` 태그를 사용하면 post의 고유주소를 얻을 수 있다.
- post의 고유주소는 {% raw %}`{% post_url 2010-07-21-name-of-post %}`{% endraw %}이다.
- `subdir/_posts`와 같이 하위 디렉토리를 사용한다면  {% raw %}`{% post_url /subdir/2010-07-21-name-of-post %}`{% endraw %}이다.
- markdown에서 아래와 같이 연결하면 된다.

  ```md
  [Name of Link]({% raw %}{% post_url 2010-07-21-name-of-post %}{% endraw %})
  ```

## post_url을 왜 쓰는가?
Jekyll은 `_posts`에 있는 post들을 가지고 사이트를 생성한다.  
실제로 보여지는 것은 `_site`에 포함된 html들이다.  
이 html들을 링크해서 사용할 수도 있으나, 사이트에 변경이 생기면 자동으로 생성되는 html들의 위치가 변경될 수 있다.  
따라서 생성된 html이 아닌 post의 고유주소로 연결을 하는 것이다.

## 참고 사이트
- [태그필터: 게시물에 연결하기](https://jekyllrb-ko.github.io/docs/liquid/tags/#%EA%B2%8C%EC%8B%9C%EB%AC%BC%EC%97%90-%EC%97%B0%EA%B2%B0%ED%95%98%EA%B8%B0)
