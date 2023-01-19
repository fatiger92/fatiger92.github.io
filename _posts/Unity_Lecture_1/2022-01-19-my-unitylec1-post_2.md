---
title:  "[실전 게임 코드 리뷰 :: 유니티 클리커 게임] 2. 프로젝트 세팅"
excerpt: "Unity Lesson 1"

categories:
  - Unity Lesson 1
tags:
  - [Unity Lesson 1]
toc: true
toc_sticky: true
date: 2022-01-19 
last_modified_at: 2022-01-19
---

인프런에 있는 Rookiss님의 **[실전 게임 코드 리뷰] 유니티 클리커 게임** 강의를 듣고 정리한 게시글입니다.
<br>
[🔔강의 보러가기 클릭](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84%EA%B2%8C%EC%9E%84-%EC%BD%94%EB%93%9C%EB%A6%AC%EB%B7%B0-%EC%9C%A0%EB%8B%88%ED%8B%B0-%ED%81%B4%EB%A6%AC%EC%BB%A4)
{: .notice--warning}

# Spine Animation
<hr style="width=100%" />
<span style="">

    - 스프라이트로 애니메이션을 만들면 한장한장 만들어야함, 하지만 스파인은 2D에 뼈대를 심어서 쓰기 때문에 한장한장 찍을 필요가 없다. 
    - 버전에 민감함, 상위호환이 안됨 따라서 어셋 작업할 때 스파인의 버전을 반드시 동일하게 해야 한다.  

</span>
  스파인으로 만들어진 SkeletonData를 하이어라키에 드래그 앤 드롭하면 아래와 같이 세가지 선택지가 나온다. 

  <p style="font-size:15pt; color:yellow"> 
  1. SkeletonAnimation<br>
  2. SkeletonGraphic(UI)<br>
  3. SkeletonMecanim
  </p>

  2D 게임에서는 주로 <strong style="color:greenyellow">2번인 SkeletonGraphic(UI)</strong> 을 사용한다고 함.  
  그리고 <strong style="color:greenyellow">SkeletonGraphic(UI)</strong> 를 선택하면 스파인 오브젝트가 생성되는데, 아래와 같이 <strong style="color:greenyellow">SkeletonAnmation 컴포넌트도 자동으로 추가</strong> 된다.   
  이 컴포넌트가 스파인 오브젝트를 조작하기 위한 핵심 컴포넌트다.
  
  ![lecture_1](/assets/images/posts/Unity_Lecture_1/2022-01-19-my-unitylec1-post_2/1.png)

  <span style="color:violet; font-size:15pt; font-weight:bold"> 주목해야 할 속성은 노란색으로 칠한 부분이다.</span>

  <p style="font-size:15pt"> 총 4가지로 일단은 SkeletonData Asset, Initial Skin, Animation Name, Loop 만 신경 쓰면 된다.</p> 

## 1. SkeletonData
  - 어셋 자체 라고 생각하면 됨  

## 2. Initial Skin & Animation Name
  - Skin과 그에 맞는 Animation Name을 설정하고 Play 하면 선택한 내용에 해당하는 애니메이션이 재생된다.
  - 무기나 갑옷을 들었을 때, 외향이 바뀌는 경우가 있는데 뼈대가 같기 때문에 그럴 경우 Skin을 갈아 끼우는 방식으로 사용된다.
  - 위의 내용은 코드에서 변경 할수도 있음.  

<br>

# Text Mesh Pro
<hr style="width=100%" />

    - 이전에는 텍스트를 넣을 때 Text 컴포넌트를 사용해서 넣었음, 하지만 이제는 Legacy로 가버렸기 때문에 TMP를 쓰는 것이 좋음.  
    - 기존 텍스트에 비해서 커스터마이징도 가능하고, 메모리 소모도 적어 졌지만, 사용 과정이 너무 복잡한 단점이 있다.  
    - 한글은 다시 가공을 해줘야 함.  
  
   한글을 사용하는 법 아래의 <strong style="color:orange">감귤오렌지</strong> 님의 게시글을 참고하자  
  
   [🔔감귤오렌지 님의 Text Mesh Pro 한글 사용법](https://blog.naver.com/cdw0424/221641217203) 

   복잡하다고는 했지만 사실상 처음 설정할 때만 그렇지 손에 익으면 그리 복잡하진 않은 것 같다.
   
   Generate한 폰트를 Default 폰트로 하고 싶으면

      Assets>TextMesh Pro>Resources>TMP Settings 
    
   위의 경로의 TMP Settings를 클릭 후 나오는 Default Font Asset 속성에 Generate한 Font Asset 파일을 드래그 앤 드롭하면 된다.

<br>

# 오늘의 한마디
  
  <span style="color:crimson; font-size:15pt; font-weight:bold">계속해서 무언가를 도전하고 만드는 것을 멈추지 말자.</span>

<br>

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr>

