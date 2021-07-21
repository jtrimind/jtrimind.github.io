---

title:  "Jekyll Plugin: jekyll-last-modified-at 사용하기"
---

## jekyll-last-modified-at?
`jekyll-last-modified-at` 플러그인은 마지막에 수정된 날짜를 표시해준다.  
git을 사용하고 있는 경우 git commit 날짜를 이용하고, 그렇지 않은 경우 mtime을 사용한다.  
- Github Repo: [jekyll-last-modified-at](https://github.com/gjtorikian/jekyll-last-modified-at)

## jekyll-last-modified-at 설치
1. `jekyll-last-modified-at`을 설치하고 bundle을 실행한다.
   ```bash
   gem install jekyll-last-modified-at
   bundle
   ```
2. `Gemfile`에 다음과 같이 추가한다.
   ```gemfile
   group :jekyll_plugins do
     gem "jekyll-last-modified-at"
   end
   ```
3. `_config.yml`에 다음과 같이 추가한다.
   ```yml
   plugins:
     - jekyll-last-modified-at
   ```

## jekyll-last-modified-at 사용
{% raw %}
```
{% last_modified_at %}
```
```
{% last_modified_at %Y:%B:%A:%d:%S:%R %}
```
```
{{ page.last_modified_at }}
```
```
{{ page.last_modified_at | date: '%Y:%B:%A:%d:%S:%R' }}
```
{% endraw %}