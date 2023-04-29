---
title:  "Jekyll blog 세팅 버전 불일치 문제"
excerpt: "이걸로 1시간 삽질함"

categories:
  - Blog
tags:
  - [Blog, jekyll, Github, Git]
toc: true
toc_sticky: true
 
date: 2023-04-29
last_modified_at: 2023-04-29
---

    본 게시글은 광고가 아닌 100% 개인적인 생각입니다.

# 갑자기 bundle exec jekyll serve -t 가 안먹힘
<hr style="width:100%" />

  먼저 새로 포맷을 한 뒤, 작업환경을 다시 세팅해야 해서 VS code를 설치하고 git도 받고 열심히 하던 도중에 Jekyll을 설치하고 bundle exec jekyll serve -t 를 했는데 에러가 남.

  일단 

  1. Ruby 설치
  2. cmd 열고 gem install jekyll bundler
  3. 그리고 jekyll -v 로 버전 뜨는 지 확인.
  4. cmd에서 블로그 경로로 들어감
  5. bundle install 함
  
  ```text
  Bundler 2.4.12 is running, but your lockfile was generated with 2.4.3. Installing Bundler 2.4.3 and restarting using that version.
  ```

  갑자기 위와 같은 메세지가 뜸
  그리고 install됨

  6. bundle exec jekyll serve

  ![image1](/assets/images/posts/Blog/2023-04-29-my-blog-post/1.png)

  위의 이미지와 같이 뭐라고 자꾸 함
  그래서 처음에는 -trace 즉 -t 를 안붙여서 문제가 생기는 줄 알았음

  7. bundle exec jekyll serve -t

  ![image2](/assets/images/posts/Blog/2023-04-29-my-blog-post/2.png)

  다시 뭐라고 함.

  ![image3](/assets/images/posts/Blog/2023-04-29-my-blog-post/3.png)

  그런데 위의 이미지를 보면 내가 jekyll을 설치할 때 나는 4.3.2 를 설치함
  
  ![image4](/assets/images/posts/Blog/2023-04-29-my-blog-post/4.png)

  그리고 lockfile에는 jekyll 4.3.1 버전을 사용하고 있음.

  ![image5](/assets/images/posts/Blog/2023-04-29-my-blog-post/5.png)

  혹시 몰라서 gemfile.lock 파일을 수정함

  8. bundle update

  그리고 업데이트를 해줌

  9. bundle exec jekyll serve -t 다시 시도 

  아주 잘됨

  http://127.0.0.1:4000/ 아주 잘 들어가짐.

  <strong style="color:orange; font-size:20pt">무~야~호</strong>


<hr style="width:100%" />

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr>


[Top](#){: .btn .btn--primary }{: .align-right}