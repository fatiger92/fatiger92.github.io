---
title:  "Git 블로그 검색엔진에 등록하기 (Google / 네이버 / 다음)"
excerpt: "깃 블로그를 선택한 순간부터 모든게 수동"

categories:
  - Blog
tags:
  - [Blog, jekyll, Github, Git]
toc: true
toc_sticky: true
 
date: 2024-01-17
last_modified_at: 2024-01-17
---

# 참고한 블로그 

[출처 Yenarue's Log 블로그 바로가기🔔](https://yenarue.github.io/tip/2020/04/30/Search-SEO/)
{: .notice--warning}

# 깃 블로그 검색엔진에 등록하기

생각해보니까 블로그를 만들고 나서 여태 검색엔진에 등록하질 않고 있었다.  
나만 모르고 있었는듯...

# 1. sitemap.xml 생성하기

기본적으로 sitemap.xml은 검색엔진에서 사용하는 내 깃 블로그의 모든 페이지 정보를 모아두는 파일이다.

먼저 sitemap.xml 을 루트 폴더에 만든다.
깃 블로그 기준으로 레포지토리 폴더 안을 말한다.

그리고

```xml
---
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
        <changefreq>daily</changefreq>
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
```

위의 내용을 추가한다.

그리고나서 `https://블로그주소/sitemap.xml`에 접속해서 내 블로그의 모든 페이지 주소를 정상적으로 출력하는지 확인

짧게 설명하겠음.

![img1](/assets/images/posts/Blog/2024-01-17-my-blog-post_1/1.png)

위와 같은 주소들이 나온다면 성공이다.
사실 포스팅을 업로드 할때 마다 해당 컬럼에 대한 값을 넣어줘야 되는데 그게 귀찮기 때문에 블로그 작성자가 만들어 둔 자동화 코드를 삽입하는 것이다.

다음은 작성자가 Default로 넣어 놓은 값이다.
> 마음에 안들면 입맛에 맞게 바꾸자.

```xml
lastmode : date와 같은 값
sitemap.changefreq : weekly
sitemap.priority : 0.5
```

확인했으면 다음 단계로 넘어 간다.

# 2. 포스팅 작성시 sitemap 설정하기

포스팅 작성할 때 수동으로 넣어 줄 수도 있음.

```xml
... title, date 등 생략
lastmode: 2020-04-28 13:00:00
sitemap:
  changefreq: daily
  priority : 1.0
```

각 컬럼 설명
- lastmode : 마지막 수정일
- sitemap.changefreq : 스크랩 주기 (짧으면 트래픽 저하)
- sitemap.priority : 스크랩 우선순위

위에 자동화 코드 삽입했으니 패스 

# 3. robots.txt 생성하기

사이트를 검색엔진에 등록하면 크롤러가 사이트에 방문해서 크롤링을 해간다고 함.
이 때, 크롤러가 참고할 수 있는 정책을 명시하는 파일임

마찬가지로 루트 폴더에 만들어 준다.

다음의 내용을 작성

```text
User-agent: *
Allow: /
Sitemap: https://yenarue.github.io/sitemap.xml
```

# 4. feed.xml 생성하기

RSS Feed를 등록하는 feed.xml 이다.

### RSS Feed란?

RSS 피드는 블로그 또는 온라인 잡지와 같은 즐겨 찾는 웹 사이트와 최신 정보를 쉽게 사용할 수 있는 방법이다.   
사이트에서 RSS 피드를 제공하는 경우 게시물이 올라가면 알림을 받을 수 있으며 요약 또는 전체 게시물을 읽을 수 있다.  

마찬가지로 루트 폴더에 만든다.

```xml
---
layout: none
---
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
  <channel>
    <title>{{ site.name | xml_escape }}</title>
    <description>{{ site.description | xml_escape }}</description>
    <link>{{ site.url }}</link>
    <atom:link href="{{ site.url }}/feed.xml" rel="self" type="application/rss+xml" />
	<lastBuildDate>{% for post in site.posts limit:1 %}{{ post.date | date_to_rfc822 }}{% endfor %}</lastBuildDate>
	{% for post in site.posts limit:10 %}
	<item>
		<title>{{ post.title | xml_escape }}</title>
        {% if post.author.name %}
            <dc:creator>{{ post.author.name | xml_escape }}</dc:creator>
        {% endif %}
        {% if post.excerpt %}
            <description>{{ post.excerpt | xml_escape }}</description>
        {% else %}
            <description>{{ post.content | xml_escape }}</description>
        {% endif %}
        <pubDate>{{ post.date | date_to_rfc822 }}</pubDate>
        <link>{{ site.url }}{{ post.url }}</link>
        <guid isPermaLink="true">{{ site.url }}{{ post.url }}</guid>
      </item>
    {% endfor %}
  </channel>
</rss>
```

# 5. 검색엔진에 등록하기

## 5-1. 구글(Googld)

[Google Search Console](https://search.google.com/search-console/welcome?hl=ko) 접속 -> URL 접두어 선택 -> https://깃헙주소/sitemap.xml 입력 -> 계속 버튼 클릭

그럼 인증 확인 후에 소유권 확인이라는 팝업창이 나온다.
그리고 2가지 단계가 보이는데,

1. 다음 파일을 다운로드 합니다.
2. 다음 위치에 파일을 업로드 합니다. 
https://깃헙주소/sitemap.xml/

그대로 따라한다.

2번은 다운받은 파일을 그냥 루트 폴더에 업로드해서 커밋하면 된다.

## 5-2. 네이버

## 5-3. 그외
