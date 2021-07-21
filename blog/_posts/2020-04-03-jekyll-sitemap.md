---

title:  "Jekyll에서 sitemap 생성하기"
---

## Sitemap이란?
sitemap은 사이트에 대한 정보를 제공하는 파일이다.  
검색엔진은 sitemap을 읽어 사이트를 크롤링한다.  
더 자세한 설명은: [사이트맵이란 무엇인가요?](https://support.google.com/webmasters/answer/156184)

## Jekyll에서 sitemap plugin 사용하기
1. `jekyll-sitemap`을 설치하고 `bundle`을 실행한다.
   ```bash
   gem install jekyll-sitemap
   bundle
   ```
2. `Gemfile`에 `gem 'jekyll-sitemap'`을 추가한다.
3. `_config.xml`에
   1. `plugins`에 `jekyll-sitemap`을 추가한다.
   2. `url`에 자신의 주소가 적혀있는지 확인한다.
4. `url`/sitemap.xml에 sitemap이 추가된 것을 확인한다.

```gemfile
# Gemfile
gem 'jekyll-sitemap'
```

```yml
# _config.yml
url: "https://example.com" # the base hostname & protocol for your site
plugins:
  - jekyll-sitemap
```

Jekyll sitemap plugin에 대한 자세한 정보를 원한다면: [Jekyll Sitemap Generator Plugin](https://github.com/jekyll/jekyll-sitemap)
