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

# 👀 참고한 블로그 

[출처 Yenarue's Log 블로그 바로가기🔔](https://yenarue.github.io/tip/2020/04/30/Search-SEO/)
{: .notice--warning}

# 🤔 깃 블로그 검색엔진에 등록하기

![Damn](https://media.giphy.com/media/voOhKPgzYsyPu/giphy.gif){: width="30%" height="30%"}

생각해보니까 블로그를 만들고 나서 여태 검색엔진에 등록하질 않고 있었다.  
나만 모르고 있었는듯...



# 📍 1. sitemap.xml 생성하기

---

기본적으로 sitemap.xml은 검색엔진에서 사용하는 내 깃 블로그의 모든 페이지 정보를 모아두는 파일이다.

먼저 sitemap.xml 을 루트 폴더에 만든다.
깃 블로그 기준으로 레포지토리 폴더 안을 말한다.

<strong style="font-size:20pt"> [🚀 sitemap.xml 코드 링크](https://github.com/fatiger92/fatiger92.github.io/blob/960f5dfc5d940786793aaa8337489e39baa374ca/sitemap.xml)</strong>
{: .notice--warning}

위의 링크를 타고 들어가 코드를 만든 파일에 복붙한다.

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


# 📍 2. 포스팅 작성시 sitemap 설정하기

---

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

# 📍 3. robots.txt 생성하기

---

사이트를 검색엔진에 등록하면 크롤러가 사이트에 방문해서 크롤링을 해간다고 함.
이 때, 크롤러가 참고할 수 있는 정책을 명시하는 파일임

마찬가지로 루트 폴더에 만들어 준다.

다음의 내용을 작성

```text
User-agent: *
Allow: /
Sitemap: https://깃 블로그/sitemap.xml
```

# 📍 4. RSS Feed 등록을 위한 feed.xml 생성하기

---

RSS Feed를 등록하는 feed.xml 이다.

## 💡 RSS Feed란?

RSS 피드는 블로그 또는 온라인 잡지와 같은 즐겨 찾는 웹 사이트와 최신 정보를 쉽게 사용할 수 있는 방법이다.   
사이트에서 RSS 피드를 제공하는 경우 게시물이 올라가면 알림을 받을 수 있으며 요약 또는 전체 게시물을 읽을 수 있다.  

마찬가지로 루트 폴더에 만든다.

<strong style="font-size:20pt"> [🚀 feed.xml 코드 링크](https://github.com/fatiger92/fatiger92.github.io/blob/960f5dfc5d940786793aaa8337489e39baa374ca/feed.xml)</strong>
{: .notice--warning}

그리고나서 head.html 에 아래의 태그도 추가해야한다.  
link가 모여있는 곳에 살포시 삽입해주자.

```html
<link rel="alternate" type="application/rss+xml" href="{{ site.url }}/feed.xml" title="{{ site.title }} Feed">
```

`https://깃 블로그/feed` 접속 후 잘 나오는 지 확인

# 📍 5. 검색엔진에 등록하기

---

## 🔖 5-1. 구글(Google)

[Google Search Console](https://search.google.com/search-console/welcome?hl=ko) 접속 -> URL 접두어 선택 -> https://깃헙주소 입력 -> 계속 버튼 클릭

그럼 인증 확인 후에 소유권 확인이라는 팝업창이 나온다.
그리고 2가지 단계가 보이는데,

1. 다음 파일을 다운로드 합니다.
2. 다음 위치에 파일을 업로드 합니다. 
https://깃헙주소

그대로 따라한다.

2번은 다운받은 파일을 그냥 루트 폴더에 업로드해서 커밋하면 된다.

그리고 돌아가서 확인을 누른다.

다음의 팝업이 나온다면 성공

![img2](/assets/images/posts/Blog/2024-01-17-my-blog-post_1/2.png)

확인 말고 속성으로 이동을 누른다.

그럼 다음과 같은 대시보드가 나온다.

![img3](/assets/images/posts/Blog/2024-01-17-my-blog-post_1/3.png)

여기서 왼쪽 메뉴에 Sitemaps를 클릭한다.

![img4](/assets/images/posts/Blog/2024-01-17-my-blog-post_1/4.png)

제출 빡

![img5](/assets/images/posts/Blog/2024-01-17-my-blog-post_1/5.png)

성공

그리고 왼쪽 메뉴에서 URL 검사를 클릭하면 검색 탭이 하이라이트 된다.

당황하지 말고 깃 블로그 주소를 넣고 돋보기 아이콘을 클릭하자.  
그럼 다음과 같이 URL이 Google에 등록되어 있지않다고 나온다.

![img6](/assets/images/posts/Blog/2024-01-17-my-blog-post_1/6.png)

바로 아래 `색인 생성 요청` 을 누른다.

그러면 색인 생성 요청 상태가 되는데 크롤링 대기열에 추가되기 때문에 바로 적용되지는 않는다.   
블로그의 경우 짧게는 30분, 길게는 2일 정도가 소요된다고 한다.

다음과 같이 나왔다면 끝

![img14](/assets/images/posts/Blog/2024-01-17-my-blog-post_1/14.png)

## 🔖 5-2. 네이버

네이버는 [Naver Search Advisor](https://searchadvisor.naver.com/) 에 접속한다.
당연히 로그인을 하고 웹 마스터 도구 -> 사이트 관리 -> 사이트 등록 페이지로 이동한다.

- URL 입력 박스에 내 깃 블로그 주소를 집어넣고 엔터를 친다.  
- 사이트 소유확인 페이지가 나오면 HTML 파일 업로드의 1번 부터 따라한다. (다운 받아서 루트 폴더에 넣고 커밋)  
- 소유 확인을 누른다.  
-> 깃 블로그 동기화 시간이 있으니 커밋 후 적어도 1~2분 정도 뒤에 소유 확인을 누른다.

그럼 소유 확인 완료 알림창이 뜨고 다음과 같이 사이트 목록에 내 깃 블로그 URL이 추가된다.

![img7](/assets/images/posts/Blog/2024-01-17-my-blog-post_1/7.png)

이제 사이트 맵을 등록하러 가보자.

방금 사이트 목록에 올라간 내 깃 블로그 주소를 클릭한다.
왼쪽에 메뉴가 보이는데, 요청을 누른다.
그곳에 사이트맵 제출이 있다.

다음과 같이 제출

![img8](/assets/images/posts/Blog/2024-01-17-my-blog-post_1/8.png)

아까 만든 RSS도 등록하자.

다시 왼쪽 메뉴의 요청 탭 하위를 보면 RSS 제출이 있다 클릭한다.
 
입력 박스에 `https://깃 블로그 주소/feed` 를 작성하고 확인을 누른다.

## 🔖 5-3. 최종 확인

다음과 같이 간단 체크 탭을 누른다.

사이트 간단 체크라는 페이지가 나오는데, 자신의 깃 블로그 주소를 입력하고 엔터를 누르면 간단하게 체크를 해준다.

목록을 체크하면서 변경하고 싶은 게 있다면 가이드 쪽의 링크를 클릭하자.  
아주 친절하게 뭘 고쳐야 되는지 알려준다.

![img9](/assets/images/posts/Blog/2024-01-17-my-blog-post_1/9.png)

나는 사이트 설명을 바꾸고 싶어서 [사이트 설명 설정/변경](https://searchadvisor.naver.com/guide/markup-content) 을 클릭했다.

다음과 같이 같이 뭘 고쳐야 하는지 알려준다.

![img10](/assets/images/posts/Blog/2024-01-17-my-blog-post_1/10.png)

그래서 해당 구문에 해당하는 것을 변경하고 다시 사이트 간단 체크를 했다.

다음과 같이 변경된 것을 확인 가능

![img11](/assets/images/posts/Blog/2024-01-17-my-blog-post_1/11.png)

# 📍 6. 다음

망해버린 검색엔진이지만 그냥 해보자.

[다음 검색 등록](https://register.search.daum.net/index.daum) 에 접속한다.  

하단의 이미지와 같이 내 깃 블로그 주소를 넣는다.

![img12](/assets/images/posts/Blog/2024-01-17-my-blog-post_1/12.png)

그러면 개인 정보 수집 동의 페이지가 나오는 데 동의한다.

![img13](/assets/images/posts/Blog/2024-01-17-my-blog-post_1/13.png)

본격적으로 등록한다.

확인을 누르고 나서 등록완료가 떴다면 끝.
