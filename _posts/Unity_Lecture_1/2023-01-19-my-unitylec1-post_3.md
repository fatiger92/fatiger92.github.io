---
title:  "[실전 게임 코드 리뷰 :: 유니티 클리커 게임] 3. Sprite Renderer vs UGUI Image"
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

Tsubaki_t1님의 블로그 글을 제 마음대로 번역한 내용입니다.
<br>
[🔔Tsubaki_t1님의 블로그 원문보기 클릭](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84%EA%B2%8C%EC%9E%84-%EC%BD%94%EB%93%9C%EB%A6%AC%EB%B7%B0-%EC%9C%A0%EB%8B%88%ED%8B%B0-%ED%81%B4%EB%A6%AC%EC%BB%A4)
{: .notice--warning}

<br>

# 📢 uGUI의 Image와 Sprite Renderer의 구분
<hr style="width=100%" />

<Strong style="Color:crimson">Referenced Post of Tsubaki_t1 blog</Strong>

    - Unity에서 2D 오브젝트를 만드는 데는 크게 Sprite Renderer와 같은 2D 기능을 사용하는 방법과 UGUI의 Image 기능을 사용하는 방법이 있다.
    - 두가지 방법 전부 [텍스쳐를 표시한다] 라고 하는 행동이므로, 씬에 그릴 때는 전혀 구별이 되지 않는다.

  ![image_1](/assets/images/posts/Unity_Lecture_1/2023-01-19-my-unitylec1-post_3/1.png)

<p style="color:limegreen; font-size:15pt; font-weight:bold">두 가지 방법 어느쪽을 사용해도 좋지만, 장단점이 존재한다. </p>  

## 📍 Sprite Renderer의 경우  
  
  Sprite Renderer는 단독으로 자신을 그리는 기능을 가진 Renderer다.  
  Sprite 속성에 어싸인 되어 있는 Sprite 정보를 바탕으로 그리기를 한다.

  그리기 순서의 제어는 Sorting Layer와 Order In Layer로 사용하기에 단순하게 되어 있고, 좌표도 Transfrom 을 사용하여 Unity의 기본 기능을 조금 확장한 것 같은 내용으로 구성되어 있다.

  ![image_2](/assets/images/posts/Unity_Lecture_1/2023-01-19-my-unitylec1-post_3/2.png)

  또한 Sprite Renderer는 Sprite를 2D Scene View로 오직 드래그 드롭으로 아주 쉽게 사용할 수 있다.

<br>

## 📍 UGUI Image의 경우

  UGUI의 Image는 CanvasRenderer와 Canvas의  조합으로 그리는 기능이다.
  Sprite 에 지정한 Texture와 UV 정보를 바탕으로 그리기를 한다.

  그리기의 제어는 Hierarchy의 순서에 따라 다르지만, 동시에 Canvas로 설정한 Sorting Layer와 Order in Layer에 의한 그리기 순서 제어도 수행한다.

  좌표는 RectTransfrom 이라는 Transform을 확장한 자산이 사용되며, 사이즈의 변경은 Width / Height에 의해 행해져 Scale을 사용하면 레이아웃 기능이 박살나는 경우가 많다.

  ![image_3](/assets/images/posts/Unity_Lecture_1/2023-01-19-my-unitylec1-post_3/3.png)

  UGUI의 Image를 사용하려면 먼저 Canvas 오브젝트 아래에 Image가 포함된 오브젝트를 만들고 Sprite를 설정하는 절차가 있다.

<br>

## 📍 최적화와 관련된 접근법의 차이

  궁극적으로는 Sprite를 표시한다는 점에서 비슷한 기능이지만 접근법이 완전히 다르며 사용하는 경우가 다르다.

  ![image_4](/assets/images/posts/Unity_Lecture_1/2023-01-19-my-unitylec1-post_3/4.png)

<br>

## 📍 SetPass 감소에 대한 동작 비교

  2D에서 성능을 얻고 싶다면 첫 번째 항목은 SetPass의 감소다.  
  기본적으로 SetPass는 적을수록 좋다.  
  SetPass의 수는 Profiler의 Renderer와 GameView의 Stats에서 확인 가능하다.

  ![image_5](/assets/images/posts/Unity_Lecture_1/2023-01-19-my-unitylec1-post_3/5.png)

  Sprite Renderer의 SetPass 감소는 기본적으로 Dynamic Batching과 Atlas화에 의존한다.

  이 기능은 다른 3D 드로잉 기능의 확장으로 만들어졌고, Atlas에서 Material을 전환하는 횟수를 가능한 한 줄이고 Batching으로 메쉬를 모아서 일괄 드로잉을 수행한다.  
  반대로 Atlas가 서로 다른 스프라이트가 그리기 순서로 끼워지면 SetPass는 증가한다.

  ![image_6](/assets/images/posts/Unity_Lecture_1/2023-01-19-my-unitylec1-post_3/6.png)
  ![image_7](/assets/images/posts/Unity_Lecture_1/2023-01-19-my-unitylec1-post_3/7.png)

  Canvas Image의 경우, 사실 Atlas가 따로 있더라도 SetPass는 1이다.  
  다만 Atlas가 아무런 의미가 없다는 것은 아니고, 일단 Batching의 실행수에 영향을 준다.  
  하지만 퍼포먼스의 영향적으로는 SetPass 쪽이 중요하기 때문에 이것이 오차범위이다.

  또한 장면에 여러 개의 Canvas가 있는 경우 Canvas의 수에 따라 SetPass도 증가 한다.  
  또한 UI의 Material에 다른 Material을 설정하면 거기가 별도로 그려지거나 SetPass가 증가한다.

  ![image_8](/assets/images/posts/Unity_Lecture_1/2023-01-19-my-unitylec1-post_3/8.png)

<br>

## 📍 필레이트 오버드로우 감소에 대한 행동 비교

  특히 고해상도이고 GPU 가 낮은 사양의 모바일 환경에서는 오버드로우(많은 드로잉)는 매우 큰 문제가 된다.  
  또, Power VR 를 사용한 단말(iPhone 등)의 경우는 Cutout도 별로이기 때문에 모델이나 UI를 겹치도록 배치하는 것은, 매우 낭비라고 말할 수 있다.

  ![image_9](/assets/images/posts/Unity_Lecture_1/2023-01-19-my-unitylec1-post_3/9.png)

  Sprite Renderer의 경우 transparent information를 기반으로 다각형을 만든다.  
  텍스쳐의 해상도가 높은 경우는 하이폴리를, 텍스쳐 해상도가 낮은 경우는 로우 폴리인 메쉬를 작성한다.  
  transparent 부분을 깔끔하게 뽑아주므로 오버드로우는 감소시킬 수 있지만, 폴리곤이 조금 늘어난다.  

  옵션으로 FullRect (다각형으로 빼내지 않는다)를 선택할 수 있지만, 아래의 이미지와 같이 오버드로우의 범위가 늘어나기 쉬워진다.  
  덧붙여서 이 때 에디터로 작성하는 폴리곤은 조정할 수 없다.  
  2D Experimental에서는 가능하므로 2D Experimetal이 통합되면 추가될 것 같다. 

  ![image_10](/assets/images/posts/Unity_Lecture_1/2023-01-19-my-unitylec1-post_3/10.png)
  ![image_11](/assets/images/posts/Unity_Lecture_1/2023-01-19-my-unitylec1-post_3/11.png)

  UGUI Image의 경우 기본적으로 사각형으로 자른다.  
  또한 Slice했을 때 중앙 부분을 적용하지 않는 옵션이 있다.  

  이 다각형에 구멍을 뚫는 과정은 향후 스프라이트 렌더러와 비슷하겠지만(2D Experimental로 어떻게 작동하는지 볼 수 있다), 현재는 uGUI만 사용할 수 있는 것으로 보인다.

  ![image_12](/assets/images/posts/Unity_Lecture_1/2023-01-19-my-unitylec1-post_3/12.png)  
  
  또한 FullRect와 달리 투명 범위를 QUAD로 빼낸다.

  ![image_13](/assets/images/posts/Unity_Lecture_1/2023-01-19-my-unitylec1-post_3/13.png)

<br>

## 📍 대량으로 그릴 때와 움직였을 때의 행동의 비교

  UGUI의 Image와 Sprite Renderer의 가장 큰 차이점은 대량으로 그릴 때와 움직였을 때다.  
  우선 Sprite Renderer를 이용하여 드로잉하는 경우, 드로잉 하나하나에 드로잉 비용이 발생하기 때문에 양이 늘어날수록 드로잉 부하가 올라간다.  

  예를 들어 256x256 정도의 Sprite를 5000개 정도 묘화하려고 하면, 자신의 PC에서는 대략 1.6~2.0mx의 묘화 비용이 발생한다.  

  하지만 Sprite를 이동, 변형, 회전시켰을 때의 비용은 거의 없다.

  ![image_14](/assets/images/posts/Unity_Lecture_1/2023-01-19-my-unitylec1-post_3/14.png)

  반대로 UGUI의 Image를 그리는 경우, 움직이지 않으면 Canvas에 수집한 Canvas Renderer의 그리기 결과를 사용하기 때문에, Sprite의 개수가 증가해도 렌더링 비용은 거의 늘어나지 않는다.  

  Sprite Renderer와 같은 Sprite를 1만 2000개 정도 묘화한 경우라도, 묘화 비용은 0.2ms 전후 이다.

  ![image_15](/assets/images/posts/Unity_Lecture_1/2023-01-19-my-unitylec1-post_3/15.png)

  단, Canvas 이하의 Image에 대해 하나라도 이동, 변형, 회전을 실시한 경우, 방대한 처리 비용이 발생한다.  

  아래 이미지의 왼쪽 파란 부분이 그것으로, 5000개 정도의 Image 중 하나의 Image를 애니메이션 시키면 대략 71ms 라는 치명적인 레벨의 시간이 소요되고 처리가 중지되었다.  

  5개, 10개라면 아직 무시해도 괜찮지만, 보다 양이 많으면 Image는 매우 강력한 병목을 만들어 낼 수 있음을 예측할 수 있다.  
  
  이 부분은 Unity 5.2에서 오히려 빨라졌지만, 양이 늘어나면 역시 문제가 된다.  
  더 비용을 높이고 싶다면 Pixel Perfect에 체크를 하면 된다.

  ![image_16](/assets/images/posts/Unity_Lecture_1/2023-01-19-my-unitylec1-post_3/16.png)

  다행히, 움직이는 Image에 Canvas를 붙여 두는 것으로, 움직일 때의 비용은 격감한다.(위의 이미지의 우측)  
  단, Canvas의 Enable/Disable을 전환하면 부모 Canvas가 재구축을 시작하기 때문에, 그 부분도 주의 해야한다. (Canvas가 아닌 Image를 disable하는 것으로 비용을 최소로 줄이자.)  
  또, World Space / Screen Space Camera의 UI 상태에서 Canvas의 좌표, 스케일, 회전을 조작했을 경우도 재구축을 시작하므로 이 부분도 주의해야한다.

<br>

## 📍 즉 언제 무엇을 어떻게 써야하는 가?

  결국

    - UGUI는 실제로 이동할 것으로 예상하지 않는다.
      이동하지 않으면 부하가 적고 전력 소비량이 적다
    - UGUI는 Canvas 기반으로 실행된다.
    - Sprite는 효과가 있어야 한다.
    - 다수의 도면에 최적화 되어있다.

  같은 내용으로 요약할 수 있고,

  또한 실제로  

    - 이동하지 않지만 a0 범위가 큰 경우 Sprite Renderer.  
      (단, 직사각형이 깨끗하게 나오는 경우는 그렇지 않음)
    - 지속적으로 많은 Sprite를 실행하고 있다면 Sprite Renderer를 사용하자.
    - 너무 많이 그린다 싶으면 "Sprite Renderer".
    - Image를 이동하지 않으면 UGUI의 이미지를 사용.
    - 작은 Sprite를 이동하려면 UGUI Image를 사용.
      (움직일 Image는 별도의 Canvas 필수)

<br>

# 📢 프로젝트 
<hr style="width=100%" />

  1. 현 프로젝트에서는 UGUI Image로 전부 처리 한다.(Sprite가 적게 들어 가기 때문에.)

  2. 유니티 툴을 이용해서 컴포넌트에 함수 연동하는 것에 대해서 깊게 생각해 볼 필요가 있음, 각자 같은 프리팹을 만지고 있을 때 연동이 풀리거나 또는 UI 창의 하이어라키 상 오브젝트가 정말 많다고 가정 했을 때 일일이 연동을 해줘야 하는 불편함이 있다.
  (잘못해서 한번 풀리기라도 하면 대 공사가 일어남)

  3. 항상 게임에 오픈되어 있어야 하는 UI 창은 Scene UI라는 스크립트를 만들어 관리해주는 것이 좋음.

<br>

# 📢 오늘의 한마디
<hr style="width:100%" />

  <span style="color:crimson; font-size:15pt; font-weight:bold">⚡일본 쪽 개발 블로그에 노다지가 아주 많다.</span>

<br>

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr>

