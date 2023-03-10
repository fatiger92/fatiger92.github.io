---
title:  "[Unity Doc] 7. [유니티 실험실] 과연 Find(string)은 성능에 큰 영향을 끼칠까?"
excerpt: "내가 볼려고 만든 유니티 잡학지식 문서"

categories:
  - UnityDocs
tags:
  - [UnityDocs]
toc: true
toc_sticky: true
date: 2023-03-10
last_modified_at: 2023-03-10
---

# 🤔 왜 갑자기 이런 생각을 했는지
<hr style="width:100%" />

개발을 하면서 항상 2갈래의 길에 맞닥뜨리게 되는데, 바로 UI 관련 코딩을 할 때이다.

인스펙터에 직접 오브젝트를 어싸인 하는 방법은 정말 편하지만,   
이렇게 구성할 경우 팀 단위 프로젝트일 경우엔 SVN을 사용 또는 다른 형상기억 프로그램을 사용할 것이다.   
하지만 팀 프로젝트이기 때문에 같은 프리팹을 작업하는 일을 피할 수는 없고 그럴 경우 프리팹 충돌을 피할 수 없다.

이 문제는 비단 팀 단위일 경우에만 적용되는 것이 아니다.  
개인 프로젝트인 경우에도 UI 구조가 정말 단순한 게임이라면 어느정도 문서화로 커버가능하겠지만,  
UI가 점점 많아 지면 많아질 수록, 그 구조가 복잡하면 복잡할 수록 내가 어느 UI오브젝트를 어싸인 했는지 어느 하위 오브젝트에 붙어 있었는지 전부 기억하지 어려워진다.

이럴 경우 나중에 업데이트 또는 수정 사항이 있을 경우, 정말 크리티컬한 문제로는 프리팹이 깨졌을 경우 일일이 스크립트에 전부 어싸인해야 되는 정말 비효율적인 일이 발생된다.

따라서 택 1을 해야하는데...

1번은 초기화시 코드에서 `GameObject.Find` or `GetComponent`로 캐싱해서 사용하는 방법.  
2번은 불편하더라도 스크립트에 직접 UI 오브젝트를 어싸인하고 전부 문서화 기록 후 기도메타를 하는 방법.

이렇게 두 가지가 존재하고, 1번은 코드 친화적인 반면에 UI 디자이너와 이름을 약속하는 전제조건이 필요하다.  
(필요하다면 테이블로 관리)

2번은 그냥 기도 메타다.

따라서 1번 방법으로 구성할 경우 `GameObject.Find`를 사용할 수 밖에 없게 된다.  
뭐 `GetComponent` 시리즈로 찾아도 되겠지만, 그게 어느 UI인지 특정 할 수 없게 된다.

![img1](/assets/images/posts/UnityDocs/2023-03-10-my-unitydoc-post_7/1.png){: width="30%" height="30%"}

위의 이미지와 같이, 물론 이렇게 버튼이 많은 UI는 없겠지만 여기서 뭐가 Close 버튼인지 Open 버튼인지 코드상에서는 알 방법이 없다.  
`child(0)` `child(1)` 이런 방법도 있겠지만 너무나도 직관적이지 못한 방법이고 순서를 바꿀 경우 타노스, 모든 인덱스에 대해서 주석을 달아놓아야 하는 불편함이 생긴다.

따라서 String 값으로 비교를 하는 방법 밖에 존재하지 않다고 판단했다.  
그리고 `Find`의 부담을 덜기 위해 최대한 현재 오브젝트의 `Transform` 하위에서 찾도록 하는 것이 최선이라고 생각했다.

실험은 3가지 방법으로 진행 하며, 수행 속도는 StopWatch 클래스를 사용한다.

검색 대상의 오브젝트들은 1000000개로 설정.

1. Find 사용 - string으로만 검색

2. GetChild 사용 - 항상 찾는 오브젝트가 맨 마지막에 있다는 가정이 필요함.

3. GetComponents 사용 - GetComponents로 Button 컴포넌트를 다 찾아 배열로 반환한 뒤 그 반환 값내에서 string으로 검색

## 🔖 전제 조건
 
1. 찾아야 하는 오브젝트명은 "Btn_Close"다.

2. Button 컴포넌트를 가지고 있다.

3. (Option) 항상 맨마지막 순번에 존재 한다. (GetChild 사용에서만 적용되는 조건)


## 🔖 1번 Find 사용 - string으로만 검색

```c#
    void GetObjectUseFindString()
    {
        var _stopWatch = new Stopwatch();
        _stopWatch.Start();
        Debug.Log($"GetObjectUseFindString StopWatch Turn On");
        
        findCloseButton = tr.Find("Btn_Close").GetComponent<Button>();

        if (findCloseButton is not null)
            Debug.Log($"{findCloseButton.GetType()} 타입인 {findCloseButton.gameObject.name} 을 찾았습니다.");
        
        _stopWatch.Stop();
        Debug.Log($"GetObjectUseFindString StopWatch Turn Off :: {_stopWatch.ElapsedMilliseconds} sec");
    }
```

![img2](/assets/images/posts/UnityDocs/2023-03-10-my-unitydoc-post_7/2.png){: width="50%" height="50%"}

<strong style ="color:yellow; font-size:15pt"> 총 소요된 시간 : 28 ~ 32 Sec </strong>

## 🔖 2번 Find 사용 - GetChild 사용 (전제조건 3번 적용)

```c#
  void GetObjectUseGetChild()
    {
        var _stopWatch = new Stopwatch();
        _stopWatch.Start();
        Debug.Log($"GetObjectUseGetChild StopWatch Turn On");
        
        getChildCloseButton = tr.GetChild(amount - 1).GetComponent<Button>();
        
        if (getChildCloseButton is not null)
            Debug.Log($"{getChildCloseButton.GetType()} 타입인 {getChildCloseButton.gameObject.name} 을 찾았습니다.");
        
        _stopWatch.Stop();
        Debug.Log($"GetObjectUseGetChild StopWatch Turn Off :: {_stopWatch.ElapsedMilliseconds} sec");
    }
```

![img3](/assets/images/posts/UnityDocs/2023-03-10-my-unitydoc-post_7/3.png){: width="50%" height="50%"}

<strong style ="color:yellow; font-size:15pt"> 총 소요된 시간 : 0 Sec </strong>

## 🔖 3번 Find 사용 - GetComponents 사용

```c#
    void GetObjectUseGetComponents()
    {
        var _stopWatch = new Stopwatch();
        _stopWatch.Start();
        Debug.Log($"GetObjectUseFindComponentAfterString StopWatch Turn On");
        
        var _buttons = tr.GetComponents<Button>();
        
        foreach (var btn in _buttons)
        {
            if (btn.name == "Btn_Close")
                findCloseButton = btn;
        }
        if (findCloseButton is not null)
            Debug.Log($"{findCloseButton.GetType()} 타입인 {findCloseButton.gameObject.name} 을 찾았습니다.");
        
        _stopWatch.Stop();
        Debug.Log($"GetObjectUseFindComponentAfterString StopWatch Turn Off :: {_stopWatch.ElapsedMilliseconds} sec");
    }
```

![img4](/assets/images/posts/UnityDocs/2023-03-10-my-unitydoc-post_7/4.png){: width="50%" height="50%"}

<strong style ="color:yellow; font-size:15pt"> 총 소요된 시간 : 0 Sec </strong>

1번 같은 경우 확실히 Find를 사용해 string 값으로만 오브젝트를 찾는 것은 성능에 유의미한 영향을 주진 않지만 영향을 주긴 한다.

성능에 유의미한 영향을 주지 않는다는 이유로는 "UI에서 사용할 때" 라는 가정하에 오브젝트가 100만개나 되는 UI가 존재할리가 없기 때문이다. 

그리고 영향을 주긴 한다고 말한 것은 100만개임에도 불구하고 2번, 3번은 0 Sec가 나왔기 때문이다.

하지만 2번의 경우 부모 오브젝트 기준 하위 오브젝트중 내가 찾는 오브젝트의 순서를 정확히 알아야하기 때문에 사실 비현실적인 방법이다.

따라서 제일 Best한 방법은 3번 즉 GetComponents를 사용하는 것이라는 결론을 도출할 수 있다.

소요시간도 적고, string 값으로 검색을 할 수도 있는 두 마리의 토끼를 다 잡을 수 있는 방법이다. 


## 🔖 전체 코드

```c#
using System.Diagnostics;
using UnityEngine;
using UnityEngine.UI;

public class FindTestBox : MonoBehaviour
{
    public int amount;
    public Button findCloseButton;
    public Button getChildCloseButton;
    
    Transform tr;
    void Start()
    {
        AttachObjects();
        CreateGameObjects();
        
        GetObjectUseFindString();
        GetObjectUseGetChild();
        GetObjectUseGetComponents();
    }

    void AttachObjects() => tr = transform; 
    void CreateGameObjects()
    {
        for (var i = 0; i < amount; i++)
        {
            if (i == amount - 1)
            {
                var buttonGo = new GameObject();
                buttonGo.transform.SetParent(tr);
                var btnClose = buttonGo.AddComponent<Button>();
                btnClose.name = "Btn_Close";
                
                return;
            }

            var go = new GameObject();
            go.transform.SetParent(tr);
            go.name = $"GameObject[{i}]";
        }
    }
    
    void GetObjectUseFindString()
    {
        var _stopWatch = new Stopwatch();
        _stopWatch.Start();
        Debug.Log($"GetObjectUseFindString StopWatch Turn On");
        
        findCloseButton = tr.Find("Btn_Close").GetComponent<Button>();

        if (findCloseButton is not null)
            Debug.Log($"{findCloseButton.GetType()} 타입인 {findCloseButton.gameObject.name} 을 찾았습니다.");
        
        _stopWatch.Stop();
        Debug.Log($"GetObjectUseFindString StopWatch Turn Off :: {_stopWatch.ElapsedMilliseconds} sec");
    }
    
    void GetObjectUseGetChild()
    {
        var _stopWatch = new Stopwatch();
        _stopWatch.Start();
        Debug.Log($"GetObjectUseGetChild StopWatch Turn On");
        
        getChildCloseButton = tr.GetChild(amount - 1).GetComponent<Button>();
        
        if (getChildCloseButton is not null)
            Debug.Log($"{getChildCloseButton.GetType()} 타입인 {getChildCloseButton.gameObject.name} 을 찾았습니다.");
        
        _stopWatch.Stop();
        Debug.Log($"GetObjectUseGetChild StopWatch Turn Off :: {_stopWatch.ElapsedMilliseconds} sec");
    }
    
    void GetObjectUseGetComponents()
    {
        var _stopWatch = new Stopwatch();
        _stopWatch.Start();
        Debug.Log($"GetObjectUseFindComponentAfterString StopWatch Turn On");
        
        var _buttons = tr.GetComponents<Button>();
        
        foreach (var btn in _buttons)
        {
            if (btn.name == "Btn_Close")
                findCloseButton = btn;
        }
        if (findCloseButton is not null)
            Debug.Log($"{findCloseButton.GetType()} 타입인 {findCloseButton.gameObject.name} 을 찾았습니다.");
        
        _stopWatch.Stop();
        Debug.Log($"GetObjectUseFindComponentAfterString StopWatch Turn Off :: {_stopWatch.ElapsedMilliseconds} sec");
    }
}

```
<br>

# 🥱 끝 마치며
<hr style="width:100%" />

설마 컴포넌트 넣는 걸 잊어버리겠어?


<br>
<strong style="color:yellow; font-size:100pt;">끝</strong>


<hr style="width:100%" />
<br>

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr>

