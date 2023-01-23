---
title:  "[Rimworld Modding Tutorial] 1. 시작하기 앞서"
excerpt: "림월드 모딩을 해보자고"

categories:
  - Rimworld
tags:
  - [Rimworld]
toc: true
toc_sticky: true
date: 2022-01-21
last_modified_at: 2022-01-21
---

Youtuber <strong style="color:orange;"> Eck314님의 How to Make a RimWorld Mod - Step by Step</strong>  영상을 보고 정리한 게시글입니다.
<br>
[🔔영상 보러가기 클릭](https://youtu.be/UgCOhFzeX4A)
{: .notice--warning}


# 💁‍♂️시작하기 앞서 왜 림월드 모딩을 결심하셨습니까?
<hr style="width:100%" />

>
림월드라는 게임은 먼저 내가 <strong style="color:crimson;">정말 좋아하는 게임 중 하나다.</strong>    
그리고 C# 으로 만들어져 있으며, 현재도 지속적인 업데이트와 DLC가 나오고 있다.  
그래서 내가 좋아하는 게임에 내가 모드를 만들어 적용해보고 배포한다면 거기서 나오는 성취감은 배가 되지 않을까? 라는 생각을 하게 되었다.
2023 신년이 왔고, 내 버킷리스트에는 림월드 모딩이라는 글이 쓰여지게 되었고, 열심히 해볼 생각이다.    
추가적으로 C#와 Xml을 많이 만지게 될 것 같아서 사고확장에 뭔가 도움이 많이 될 것 같다.    
>

<br>

# 1. Jecrell의 림월드 모드 만들기 포스트 참고
<hr style="width:100%" />

<a href="https://ludeon.com/forums/index.php?topic=33219.0" style="color:limegreen; font-size:15pt"> [🔔How to Make a RimWorld Mod 공식 문서 보러가기]</a>

<br>

# 2. 필요한 툴
<hr style="width:100%" />

  1. Notepad++ - <strong style="color:gold">xml을 편집하기 위함</strong>
  2. Visual Studio Community Edition 또는 .NET을 편집할 수 있는 IDE - <strong style="color:gold">스크립트를 작성하기 위함</strong>  
  3. ILSpy[Microsoft Store(windows store)] - <strong style="color:gold">림월드 게임 내부의 코드를 디버깅하기 위함</strong>     
  4. [FindInFiles](https://github.com/toolscode/findinfiles) - 내 개인적인 생각으로 추가한 툴인데, <strong style="color:gold">이 프로그램을 쓰면 폴더내의 파일들의 텍스트 검색이 가능하다.</strong>  따라서 어디에 뭐가 있는지, 에러가 난다면 그 에러코드에 해당하는 내용이 어디에 있는지 찾기 정말 편리해진다. 개발자의 깃허브 링크를 걸어 놓았는데 링크로 들어가면 오른쪽 중간쯤에 Releases 탭을 클릭하고, findinfiles_win_x64_현재버전.exe 을 다운받아 설치하면 된다.





<br>

# 3. 림월드의 데이터 기반 개발
<hr style="width:100%" />

  <p style="color:gold; font-size:15pt; font-weight:bold">Steamapps>common>RimWorld>Data>Core</p>
  위의 경로에 림월드의 모든 데이터 파일이 존재한다.

## About

 ![mod1](/assets/images/posts/Rimworld/2023-01-21-my-rimmod-post_1/1.png)

림월드의 메인 About 파일과 Preview 사진이 존재한다.  
<strong style="color:crimson; font-size:15pt">이 파일은 건드릴 일이 없다.</strong>

## **Defs**
림월드 내부의 아이템의 xml 파일(아이템 등의 설정 값 ex.아이템 명, 데미지 ..etc)들이 존재하는 폴더이다.

## Languages
번역 통 파일이 있는 폴더이다.

<br>


<br>

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr>

