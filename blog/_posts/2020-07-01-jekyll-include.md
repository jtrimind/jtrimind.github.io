---

title:  "[Jekyll/Liquid] include 태그로 파일 내용 불러오는 법"
---

## include
`include` 태그를 사용하면 `_includes` 폴더에 저장된 내용을 가져올 수 있다.  
```
{% raw %}
{% include footer.html %}
{% endraw %}
```

## include_relative
상대경로에 있는 파일을 불러오려면 `include_relative` 태그를 사용하면 된다.  
`../`로 상위경로를 가져오는 것은 안된다.  
```
{% raw %}
{% include_relative somedir/footer.html %}
{% endraw %}
```

## 변수를 이용하여 include
```
---
title: My page
my_variable: footer_company_a.html
---

{% raw %}
{% if page.my_variable %}
  {% include {{ page.my_variable }} %}
{% endif %}
{% endraw %}
```
위와 같이 my_variable이라는 변수를 사용하여 _includes/footer_company_a.html을 가져올 수 있다.  

## include 할 때 파라미터 넘기기
```html
<!-- image.html -->
<figure>
   <a href="{{ include.url }}">
   <img src="{{ include.file }}" style="max-width: {{ include.max-width }};"
      alt="{{ include.alt }}"/>
   </a>
   <figcaption>{{ include.caption }}</figcaption>
</figure>
```

```
{% raw %}
{% include image.html url="http://jekyllrb.com"
max-width="200px" file="logo.png" alt="Jekyll logo"
caption="This is the Jekyll logo." %}
{% endraw %}
```
위와 같이 image.html을 include할 때, url, file, max-width, alt, caption을 파라미터로 넘길 수 있다.  
파라미터는 치환되어 아래와 같이 될 것이다.  

```html
<figure>
   <a href="http://jekyllrb.com">
   <img src="logo.png" style="max-width: 200px;"
      alt="Jekyll logo" />
   </a>
   <figcaption>This is the Jekyll logo</figcaption>
</figure>
```

## 참고 사이트
[Jekyll Includes](https://jekyllrb.com/docs/includes/)
