---

title:  "[Jekyll] 기본 이미지(트위터 카드, 오픈그래프) 설정"
image: /assets/images/blog/2021-07-13-default-image.png
---

## 기본 이미지 설정

url 링크를 복사해서 트위터나 카카오톡에 글을 썼는데, 링크에서 이미지가 함께 표시될 때가 있다.

![기본 이미지 예시](/assets/images/blog/2021-07-13-default-image.png)

지킬에서는 `front matter`를 이용해서 설정할 수 있는데, `_config.yml`에서 `default`를 사용하여 기본 이미지를 설정할 수 있다.

## 기본 이미지 설정방법

1. 자신이 기본 이미지로 사용하고 싶은 이미지(`default-card.png`)를 에셋 폴더의 적당한 위치(`/assets/images/`)에 옮겨둔다.
2. `_config.yml`를 아래와 같이 수정한다.

```yml
# _config.yml
defaults:
  - scope:
      path: ""
    values:
      image: /assets/images/default-card.png
```

## 참고링크
- [Front Matter Defaults](https://jekyllrb.com/docs/configuration/front-matter-defaults/)
- [Setting a default image](https://github.com/jekyll/jekyll-seo-tag/blob/master/docs/advanced-usage.md#setting-a-default-image)
