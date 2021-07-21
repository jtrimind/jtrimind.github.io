---

title:  "GitHub에서 Jekyll 사용하기"
---

[Jekyll 설치하기](2020-03-16-install-jekyll.md)가 끝나면, 로컬이 아니라 웹사이트를 통해서 확인하고 싶어질 것이다.  
[GitHub Pages](https://pages.github.com/)를 사용하여 블로그를 호스팅할 수 있다.  

1. *username*.github.io 라는 레포를 만든다.
2. *username*.github.io 프로젝트를 clone한다.
3. *username*.github.io 디렉토리에 Jekyll 사이트를 생성한다.
4. 변경사항을 push한다.

```bash
git clone https://github.com/*username*/*username*.github.io.git
jekyll new *username*.github.io
cd *username*.github.io
git add .
git commit -m "Apply jekyll"
git push
```

## Github Pages에서 제공하는 Jekyll 테마와 플러그인
Github Pages에서 제공하는 Jekyll 테마와 플러그인을 확인하려면 [여기]({% post_url blog/2020-04-07-github-pages-jekyll-dependency %})를 참고한다.
