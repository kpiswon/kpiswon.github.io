---
layout: post
title:  "Google에 내 블로그가 검색되도록 설정하기"
date:   2020-08-20 01:03:00
categories: Jekyll
tags : [jekyll, github page, 지킬, github blog, google, 검색, search]
---

보고 있는 현재 블로그는 깃헙 페이지를 이용하여 만든 블로그이다.  
보통 네이버나 다음과 같은 포털사이트를 이용하여 만든 블로그는 내가 굳이 따로 설정을 하지 않더라도 검색이 가능하지만, 
깃헙 페이지로 만든 블로그는 구글이나, 네이버, 다음과 같은 사이트에서 검색이 되도록 따로 등록해주어야 한다.

---
### sitemap 생성

사이트맵은 나의 블로그 내에 있는 모든 글들과 목록들을 나열한 파일(.xml)이다. 즉, 검색엔진에 내 블로그 내에 있는 모든 내용들을 제공하여 크롤링 될 수 있도록 도와주는 역할을 한다.  

sitemap을 생성하는 방법에는 여러가지가 있지만 작성자가 가장 쉬웠던 방법만 일단 소개하고자 한다. 이 내용은 (http://jinyongjeong.github.io/ 의 블로그를 참고하였다.)

##### 1. /root 경로에 /sitemap.xml 파일을 생성하고 아래 내용을 붙여넣는다.  
![root](/assets/images/root.png)


```
{% raw %}
---
layout: null
---
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9 http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd" xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  {% for post in site.posts %}
    <url>
      <loc>{{ site.url }}{{ post.url }}</loc>
      {% if post.lastmod == null %}
        <lastmod>{{ post.date | date_to_xmlschema }}</lastmod>
      {% else %}
        <lastmod>{{ post.lastmod | date_to_xmlschema }}</lastmod>
      {% endif %}

      {% if post.sitemap.changefreq == null %}
        <changefreq>weekly</changefreq>
      {% else %}
        <changefreq>{{ post.sitemap.changefreq }}</changefreq>
      {% endif %}

      {% if post.sitemap.priority == null %}
          <priority>0.5</priority>
      {% else %}
        <priority>{{ post.sitemap.priority }}</priority>
      {% endif %}

    </url>
  {% endfor %}
</urlset>
{%endraw%}
```

##### 2. github에 push하고 블로그주소/sitemap.xml로 접속했을 때 아래와 같은 화면이 나오면 정상적으로 sitemap에 등록된 것이다.

![sitemapadd](/assets/images/sitemapadd.png)

##### 3. sitemap.xml에 default 값으로 설정되어 있는 changefreq를 daily로 바꿔주자.  
changefreq를 너무 짧게 하면 접속을 자주하게 되어 안좋은 영향을 미칠 수 있다고 하니 하루에서 일주일이 적당하다. 이 블로그는 포스팅 수가 많지 않으므로 daily로 바꿔주도록 하겠다.


```
{% raw %}
<url>
<loc>/jekyll/2020/08/20/googlingable.html</loc>
<lastmod>2020-08-20T01:03:00+00:00</lastmod>
<changefreq>weekly</changefreq> # <- 이 부분을 수정할 것
<priority>0.5</priority>
</url>
{%endraw%}
```


{% highlight ruby %}
#root folder에서 sitemap.xml을 수정한다.
root % vi sitemap.xml
{% endhighlight %}

vi 모드에 들어가서 I를 눌러 weekly로 된 부분을 daily로 수정하고 esc를 누르고 Z를 눌러 vi 모드를 나간다.
(기본적인 건데 필자는 몰라서 쩔쩔메던 기억이 있다. 더 자세한 것은 vi 단축키를 구글링하면 자세하게 나온다.)



---


---

최종수정일 2020.08.20



> ###### References
> http://jinyongjeong.github.io/2017/01/13/blog_make_searched/
> https://www.twinword.co.kr/blog/3-different-ways-to-generate-and-submit-sitemap/

{% highlight ruby %}
print("hello, neighbors!")
print("Sungwon Kim © 2020 • All rights reserved.")
{% endhighlight %}





[Github][githuburl]

[githuburl]: https://github.com/kpiswon

