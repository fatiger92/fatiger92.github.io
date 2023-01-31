---
title:  "[실전 게임 코드 리뷰 :: 유니티 클리커 게임] 6. 코드 리뷰 01 - 진입점"
excerpt: "Unity Lesson 1"

categories:
  - Unity Lesson 1
tags:
  - [Unity Lesson 1]
toc: true
toc_sticky: true
date: 2023-01-31
last_modified_at: 2023-01-31
---

인프런에 있는 Rookiss님의 **[실전 게임 코드 리뷰] 유니티 클리커 게임** 강의를 듣고 정리한 게시글입니다.
<br>
[🔔강의 보러가기 클릭](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84%EA%B2%8C%EC%9E%84-%EC%BD%94%EB%93%9C%EB%A6%AC%EB%B7%B0-%EC%9C%A0%EB%8B%88%ED%8B%B0-%ED%81%B4%EB%A6%AC%EC%BB%A4)
{: .notice--warning}

# 📕 Scene 관리
<hr style="width:100%" />
  
해당 프로젝트에서 진입점은 GameScene.cs 다.  
하지만 GameScene은 BaseScene을 상속받고 있으니 최초 시작은 BaseScene에서부터 시작된다고 생각한다.

## 🔖 생성 순서 

생성 순서가 궁금해서 무식하게 전부 로그를 찍어 보았다.  
아래의 이미지에서 알 수 있듯이 BaseScene -> Manager들 생성 -> GameScene 순서로 진행된다고 볼 수 있다.

![img01](/assets/images/posts/Unity_Lecture_1/2023-01-31-my-unitylec1-post_6/1.png)

## 🔖 BaseScene.cs
```c#
public class BaseScene : MonoBehaviour
{
  private void Start()
  {
    //...
  }

  protected virtual bool Init()
  {
    //...
  }

  public virtual void Clear() { }
}
```

뭔가 특별한 점이라면 Init, Clear를 virtual로 구성했다는 점과

```c#
protected bool _init = false;
```

_init 이라는 bool 변수로 Initialize 되었는지 유무를 판단하여 방어적인 코드를 작성했다는 점이다.

```c#
GameObject go = GameObject.Find("EventSystem");

if (go == null)
  Managers.Resource.Instantiate("UI/EventSystem").name = "@EventSystem";
```

또한 의문점이 드는 것이 그 아래 있는데(Init 메서드), EventSystem을 프리팹화해서 null일 경우 생성하게끔 만든 부분이다.  

왜 이렇게까지 방어적으로 코딩한 건지 잘 모르겠다.

Scene간의 이동도 없기 때문에 딱히 EventSystem이 날아 가는 경우도 없을 텐데, 왜 이렇게까지 한 걸까?  

그에 대해 질문을 했고 답변을 기다리는 중이다.

## 🔖 BaceScene.cs 관련 질문에 대한 답변

![img02](/assets/images/posts/Unity_Lecture_1/2023-01-31-my-unitylec1-post_6/2.png)

그렇다고 하셨다.

## 🔖 GameScene.cs

```c#
public class GameScene : BaseScene
{
  protected override bool Init()
  {
    //...
  }
}
```

특별한 점이라면 

```c#
Managers.UI.ShowPopupUI<UI_TitlePopup>();
```

이 부분이 ShowPopupUI 메서드를 통해 Manager 클래스의 멤버 메서드를 호출함으로써 

Manager 클래스가 동적으로 생성된다.

## 🔖 Manager.cs

### 📄 Manager.cs - 변수 부분
```c#
public class Managers : MonoBehaviour
{
    public static Managers s_instance = null;
    // ...
    private static AdsManager s_adsManager = new AdsManager();
    // ...
    public static AdsManager Ads { get { Init(); return s_adsManager; } }
    // ...
}
```

변수는 딱히 설명할게 없음 각각의 Manager를 new 키워드로 생성함.  
각 Manager는 Mono를 상속받지 않았고 아무것도 상속받지 않는 단일 클래스임.  
그래서 new를 사용할 수 있는 것 (Mono를 상속받은 클래스는 new 키워드로 생성 불가, AddComponent를 사용해야함.)  

그리고 Manager마다 프로퍼티를 사용해서 접근하도록 했는데, 그 어떤 Manager를 get 하더라도 Init() 메서드를 호출하고 해당 Manager를 반환하게 해놓았음.  

### 📄 Manager.cs - 메서드 부분

```c#
public static string GetText(int id)
{
   // ...
}

```

GetText는 xml로 받아온 TextData를 저장하고 있는 Dictionary에서 파라미터로 받아온 id에 해당하는 string 값을 반환하는 함수이다.  
만약 Dictionary에 해당 id 의 string 값이 없다면 null 값을 반환한다.  

그리고 string 값 중에는 "{userName}" 이라는 문자열을 가지고 있는 값이 있는데, 그 문자열을 Manager.Game.Name(사용자가 정한 닉네임)으로 치환해서 반환한다.  

```c#
private void Start()
{
    Init();
}
```

Start()는 사실상 필요가 없다고 생각한다.  
이미 Start문이 실행되는 시점에는 s_instance가 null 이 아니다.  
      
    GameScene.cs 에서 Managers.UI.ShowPopupUI<UI_TitlePopup>(); 를 호출하는 시점에 이미 s_instance는 null이 아님.

```c#
private static void Init()
{
    if (s_instance == null)
    {
        GameObject go = GameObject.Find("@Managers");
        if (go == null)
            go = new GameObject { name = "@Managers" };
        // ...

        s_adsManager.Init();
        // ...
    }
}
```

따라서 Start에서 Init()은 불필요한 호출이라는 것이라고 판단된다.
어차피 Init() 본문 if문에 걸리지 않을 것 이기 때문에, 에러를 잡을려고 했다면 else로 잡거나 역으로 

```c#
private static void Init()
{
  if (s_instance != null) 
  {
    //여기에 로그 찍기
    return;
  }
  // ...

  GameObject go = GameObject.Find("@Managers");
  if (go == null)
      go = new GameObject { name = "@Managers" };

  // ...
  s_adsManager.Init();
}
```

이렇게 메서드 본문 위에서 막아버리고 "if 본문을 실행 했으면 더 좋지 않았을까" 라고 생각되는 코드였다.  
혹시 내가 빠트린게 있는지 질문을 하고 기다리는 중이다. 

## 🔖 Manager.cs 에 관련 질문에 대한 답변

![img03](/assets/images/posts/Unity_Lecture_1/2023-01-31-my-unitylec1-post_6/3.png)

라고 하셨다.

이걸로 의문은 풀렸고, 어떻게 구성하고 설계하느냐에 따라 달라지겠다.

<br>

# 💡 Tip
<hr style="width:100%" />

1. 각각의 Manager들을 <strong style="color:yellow;">하나의 스크립트에서 통합해서 관리</strong>한다.
2. UIManager를 시발점으로 Manager 오브젝트를 생성과 동시에 타이틀 UI를 실행한다.

<br>

# 📢 오늘의 한마디
<hr style="width:100%" />
  
>**각 Manager들의 프로퍼티를 호출할 때 마다 Init() 메서드를 호출하는 것이 뭔가 계속 걸린다.**  
>**일단 모든 코드를 분석하고 판단해야겠다고 생각했다.**


    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr style="width:100%" />

