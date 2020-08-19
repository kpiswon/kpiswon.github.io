---
layout: post
title:  "Google에 내 블로그가 검색되도록 설정하기"
date:   2020-08-20 01:03:00
categories: Jekyll
tags : [jekyll, github page, 지킬, github blog, google, 검색, search]
---

이 블로그는 깃헙 페이지를 이용하여 만든 블로그이다.  
보통 네이버나 다음과 같은 포털사이트를 이용하여 만든 블로그는 내가 굳이 따로 설정을 하지 않더라도 검색이 가능하지만, 
<u>깃헙 페이지로 만든 블로그는 구글이나, 네이버, 다음과 같은 사이트에서 검색이 되도록 따로 등록해주어야 한다.</u>

---
### sitemap 생성

사이트맵은 나의 블로그 내에 있는 모든 글들과 목록들을 나열한 파일(.xml)이다. 즉, 검색엔진에 내 블로그 내에 있는 모든 내용들을 제공하여 크롤링 될 수 있도록 도와주는 역할을 한다.  

sitemap을 생성하는 방법에는 여러가지가 있지만 작성자가 가장 쉬웠던 방법만 일단 소개하고자 한다.

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

Github에 Push하고 잘 반영이 되었는 지 확인해보자.

![cgfreq](/assets/images/chgfreq.png)

Daily로 바뀐 것을 확인할 수 있다.

---
### RSS feed 생성

RSS feed를 이용하여 네이버와 다음에 등록을 할 것이다. root 디렉토리에 feed.xml을 생성하고 다음 코드를 붙여넣는다.

```
{% raw %}
---
layout: null
---
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
  <channel>
    <title>{{ site.title | xml_escape }}</title>
    <description>{{ site.description | xml_escape }}</description>
    <link>{{ site.url }}{{ site.baseurl }}/</link>
    <atom:link href="{{ "/feed.xml" | prepend: site.baseurl | prepend: site.url }}" rel="self" type="application/rss+xml"/>
    <pubDate>{{ site.time | date_to_rfc822 }}</pubDate>
    <lastBuildDate>{{ site.time | date_to_rfc822 }}</lastBuildDate>
    <generator>Jekyll v{{ jekyll.version }}</generator>
    {% for post in site.posts limit:30 %}
      <item>
        <title>{{ post.title | xml_escape }}</title>
        <description>{{ post.content | xml_escape }}</description>
        <pubDate>{{ post.date | date_to_rfc822 }}</pubDate>
        <link>{{ post.url | prepend: site.baseurl | prepend: site.url }}</link>
        <guid isPermaLink="true">{{ post.url | prepend: site.baseurl | prepend: site.url }}</guid>
        {% for tag in post.tags %}
        <category>{{ tag | xml_escape }}</category>
        {% endfor %}
        {% for cat in post.categories %}
        <category>{{ cat | xml_escape }}</category>
        {% endfor %}
      </item>
    {% endfor %}
  </channel>
</rss>
{%endraw%}
```

---
### robots.txt 생성

robots.txt 파일에 sitemap.xml 파일의 위치를 등록하여 검색엔진의 크롤러들이 크롤링하는 데 도움을 주도록 하자.
root 디렉토리에 robot.txt 파일을 만들고 다음 내용을 입력한다.

```
User-agent: * #허용할 검색엔진 명을 넣으면 된다. *는 모든 검색엔진을 허용한다.
Allow: /

Sitemap: http://kpiswon.github.io/sitemap.xml #본인의 sitemap.xml의 url을 입력하면 된다.
```

---
### 사이트 등록

#### GOOGLE에 등록하기  
[Google Search Console](https://www.google.com/webmasters/#?modal_active=none)에 접속하여 본인의 sitemap.xml 파일을 등록하여야 한다.
> 1. 구글서치콘솔에서 'SEARCH CONSOLE' 버튼 클릭
> 2. 도메인과 URL접두어 중에 URL 접두어에 자신의 홈페이지 주소를 입력한다.
> 3. 소유권 확인이 완료되었다면, '속성으로 이동' 버튼을 클릭한다.
> 4. 색인메뉴에 Sitemaps 메뉴에 들어가 자신의 sitemap.xml을 제출한다.

![3](/assets/images/google_search_console.png)

#### 네이버에 등록하기  
[네이버 웹마스터 도구](https://searchadvisor.naver.com/)에 접속하여 등록하여야 한다. 
사이트 등록을 시작하면 사이트 소유확인을 거치게 되는 데 자신이 편한 방법으로 시키는대로 하면 된다. 소유권 확인이 완료되면 RSS를 등록하는 과정이 필요하다.

> 1. 왼쪽 메뉴의 요청 > RSS 제출을 들어간다.  
> 2. 블로그URL/feed.xml 을 입력한다.  
> 3. 다시 요청 > 사이트맵제출로 들어간다.  
> 4. 블로그URL/sitemap.xml을 입력한다.  


~~필자는 RSS제출 시 "등록한 사이트의 주소와 RSS 본문의 link 주소가 다릅니다."라는 오류가 발생하였다. 아직 해결하지 못하였다~~

#### 다음(Daum)에 등록하기  
[DAUM 검색등록](https://register.search.daum.net/index.daum)에 로그인 후 자신의 URL을 등록만 하면 된다.


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

