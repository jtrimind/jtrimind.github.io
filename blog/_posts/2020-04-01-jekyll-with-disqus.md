---

title:  "Jekyll에 Disqus 설치하기"
---

## Disqus?
아래와 같이 댓글을 달 수 있게 해주는 서비스이다.
![disqus](https://c.disquscdn.com/next/03c8a93/marketing/assets/img/features/engage-hero.png)

## 참고자료
- 서비스 가입, 사용: [Disqus 댓글](https://dev-yakuza.github.io/ko/jekyll/disqus/)
- 지킬 블로그 연결: [지킬 환경 설정 - Disqus 연결](https://wheejinv.github.io/settings/2018/05/01/jekyll-setting_2.html)

## disqus with minima theme
서비스를 가입한 뒤, `_config.yml`에 설정을 해주었다. 변경사항은 아래 diff를 보면 된다.  
- minima에서 disqus를 쓰기 위해 `url`을 설정을 해주었다.
- disqus의 `shortname`을 설정했다. disqus의 `shortname`은 아래의 방법으로 알 수 있다.
  1. disqus 사이트에서 Admin 클릭
  2. Your Sites 클릭
  3. 블로그 선택
  4. https://jtriminds-blog.disqus.com/admin/ 로 이동됨
  5. `jtriminds-blog`가 shortname
- `comments`의 경우 post의 frontmatter에서 직접 설정해줄 수 있으나, `_config.yml`에서 일괄적으로 켜지게 하였다.

```diff
-------------------------------- _config.yml ---------------------------------
index c592be6..1b40e41 100644
@@ -26,7 +26,7 @@ description: >- # this means to ignore newlines until "baseurl:"
   line in _config.yml. It will appear in your document head meta (for
   Google search results) and in your feed.xml site description.
 baseurl: "" # the subpath of your site, e.g. /blog
-url: "" # the base hostname & protocol for your site, e.g. http://example.com
+url: "https://jtrimind.github.io/" # the base hostname & protocol for your site, e.g. http://example.com
 twitter_username: jekyllrb
 github_username:  jekyll
 
@@ -35,6 +35,10 @@ theme: minima
 plugins:
   - jekyll-feed
 
+disqus:
+  shortname: jtriminds-blog
+
+comments: true
 # Exclude from processing.
 # The following items will not be processed, by default.
 # Any item listed under the `exclude:` key here will be automatically added to
```