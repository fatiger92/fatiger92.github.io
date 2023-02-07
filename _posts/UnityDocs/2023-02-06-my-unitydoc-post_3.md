---
title:  "[Unity Doc] 3. GameObject.Find 성능 실화냐? 라는 건에 대해서"
excerpt: "내가 볼려고 만든 유니티 잡학지식 문서"

categories:
  - UnityDocs
tags:
  - [UnityDocs]
toc: true
toc_sticky: true
date: 2023-02-06
last_modified_at: 2023-02-07
---

<br>

# 💁‍♂️ GameObject.Find 
<hr style="width:100%" />

특정 GameObject를 찾는 방법 중 하나인데 <strong style="color:yellow; font-size:13pt">Object의 이름으로 대상을 찾아</strong>.   
만약 이름이 같을 경우에는 가장 처음 검색된 Object 반환하지.

하지만 <strong style="color:red; font-size:13pt">2가지 전제 조건이 존재</strong>해.

1. 찾고자 하는 오브젝트는 객체화가 되어 있어야 함.
2. 찾고자 하는 오브젝트는 활성화(Active)가 되어 있어야 함.

이름으로 찾는 거라서 그냥 string 값만 인수로 넣어주면 유니티가 알아서 오브젝트를 찾아줘서 접근할 수 있어.  

![cozy](https://media.giphy.com/media/ehgd3ANQJbRmpA7GNJ/giphy.gif){: width="30%" height="30%"}

<strong style="color:yellow; font-size:13pt">너무 나도 편리한 내장 메서드지.</strong>


![no](https://media.giphy.com/media/vyTnNTrs3wqQ0UIvwE/giphy.gif){: width="30%" height="30%"}

하지만 해외 국내를 막론하고 <strong style="color:red; font-size:13pt">GameObject.Find 함수를 사용하는 것에 대해서 부정적</strong>으로 생각하고 있어.  
왜 그런지 나도 이 기회에 기록으로 남겨 놓을려고 해.

## 🔖 GameObject.Find를 사용하는 가장 좋은 방법?

<strong style="color:orange; font-size:20pt">🥱 그런 건 없어</strong>

또한 작성한 코드에 GameObject의 사용을 유무를 보고 초급자와 경력자를 구분하는 신뢰할 수 있는 지표로 보고 있다고 해.  
그리고 초보자 코드에는 많고 경력자 코드에는 존재하지 않는 이유 3가지를 말해 줄게.

1. 사용 방법을 익히는데 시간이 적게 걸리고, 빠르게 사용할 수 있다.
2. 아주 다양한 상황에서 사용할 수 있다. 
3. 항상 절대적으로 예외가 없고, 동일한 작업을 수행하는 데 사용할 수 있는 다른 방법이 복잡하다.

GameObject.Find 라는 메서드만 알고 있으면 <strong style="color:lawngreen; font-size:13pt">여러가지 상황에서 많은 일을 엄청 빠르게 완료</strong>할 수 있기 때문에, <strong style="color:lawngreen; font-size:13pt">초보자한테 매력적인 이야기</strong>가 되는 거지.

하지만 GameObject.Find에 의존하면 <strong style="color:orange; font-size:13pt">장기적으로 성능 문제, 안정성 문제, Fake true, Fake false 같은 많은 문제가 발생</strong>하며, 게임이 커지면 커질수록 GameObject.Find의 문제가 정말 중요하다고 해.

마지막으로 <strong style="color:red; font-size:13pt">멋진 플머들이 GameObject.Find 같은 걸 쓴다고 놀릴 거</strong>라고 해ㅋㅋ

## 🔖 What is it about GameObject.Find that's so bad?

그럼 대체 뭐가 나쁜 걸까? 

### 📍 속도
GameObject.Find를 호출하면 Unity는 Scene의 모든 단일 GameObject를 크롤링해서 해당 이름을 검색 중인 문자열과 비교를 해. 

이 과정 자체가 컴퓨터 과학 표준에 따르면 매우 느리다는 거지.

예시로 너가 <strong style="color:orange; font-size:13pt">도서관에 가서 알파벳 순이 아니거나 듀이 십진 분류법으로 정렬되지 않은 책들을 찾는 것</strong>과 다를 바 없어.

[🔔듀이 십진 분류법이란?](https://namu.wiki/w/%EB%93%80%EC%9D%B4%EC%8B%AD%EC%A7%84%EB%B6%84%EB%A5%98%EB%B2%95)
{: .notice--warning}

### 📍 신뢰성
GameObject.Find에 대해서 잘 알지 못한 채로 사용할 때 잘못될 수 있는 일이 너무 많다고 하는데?  

1. 개체 이름이나 함수 호출에서 한 글자를 대문자로 쓰는 것을 잊었을 경우 -> ERROR
2. 이전 엔진에서는 동적 생성시 (Clone) 이라는 스트링이 이름에 추가 됐는데, 따라서 이전 엔진과 호환 문제 -> ERROR
3. 치명적인 문제는 중복 문제, 이름이 같을 경우 어떤 오브젝트가 내가 찾는 것인지에 대한 모호성 -> ERROR

설상가상으로 <strong style="color:orange; font-size:13pt">이러한 문제는 컴파일 오류가 아닌 런타임 오류기 때문에, 게임 플레이 테스트 상황에서만 발견</strong>돼.

### 📍 완성되지 않은 코드

또한 스크립트 편집기는 코드를 작성할 때 GameObjects의 이름을 알 수 없으므로 클래스 및 함수 이름을 지정하는 방법으로 자동 완성할 수 없어.

이 말은 인스펙터에서 오브젝트의 이름을 바꾸는 행위를 스크립트 편집기는 알지 못한다는 것이지.

<strong style="color:yellow; font-size:15pt">"하지만 Start()문 에서만 사용한다면 괜찮다는 말도 있다고 하는데?"</strong>

이건 반은 맞고 반은 틀렸어.

GameObject.Find 사용을 Start()로 제한하는 것이 프레임마다 호출하거나, 프레임당 여러번 호출하는 것보다 훨씬 낫긴 하지.  

그러나 실제로 GameObject.Find의 성능 고려사항이 GameObject.Find를 피해야 하는 유일한 이유가 아니고 사용은 Start()로 제한한다고 해도 위에서 말한 안정성 문제(중복 문제 등)를 해결할 수 있는 건 아니라고해.

## 🔖 그럼 대신 사용할 수 있는 다른 메서드가 있을까? 

<strong style="color:orange; font-size:15pt">아쉽게도 1:1로 교체할 수 있는 메서드는 없어.</strong>  
하지만 좋은 소식은 실제로 5 ~ 6가지 기술만 배우면 되고 그렇게 복잡하지 않아.

그리고 이 기술로 여태까지 GameObject.Find를 사용했거나, 사용하고 싶었던 곳 모든 상황에 적용해야 한다고해.

## 🔖 GameObject.FindWithTag

```c#
GameObject myPlayer = GameObject.FindWithTag("Player");
```

해당 메서드는 .Find를 약간 개선한 메서드야.  
GameObject의 이름을 사용하는 대신 tag를 사용하는 거지.  
또한 FindGameObjectsWithTag라는 자매 메서드가 있어서 주어진 tag가 있는 모든 객체를 찾을 수 있어.

### 📍 속도

FindWithTag는 .Find보다 빨라.  
왜냐하면 Unity는 tag가 지정된 모든 GameObject에 대한 Dictionary를 생성하기 때문이야.  
그리고 Dictionary를 조회하는 건 GameObject.Find에서 사용하는 "각 개체를 하나씩 살펴보는" 방법보다 훨씬 빠르지.  
가장 빠른 기술은 아니지만, 명확해.

이것을 사용하더라고 GetComponent를 호출해야 하는데, 이상적인 방법은 아니야.  
하지만 이 문제를 해결할 다른방법이 존재해.

### 📍 신뢰성

FindWithTag는 .Find보다 약간 더 안정적이지만 이 영역은 우리가 논의할 다른 방법에 비해 가장 큰 약점이야.  
오타로 인해 문제가 발생할 가능성이 줄어들긴 하지만 그게 전부지.

### 📍 다목적성

Unity의 tag 시스템은 주어진 게임 오브젝트에 하나의 tag만 적용할 수 있다는 점에서 상당히 제한적이야.  
빨간색 팀에 "Red" tag를 지정하고 파란색 팀에 "Blue" tag를 지정하도록 할 수 있지만, 그 순간 두 팀의 모든 플레이어는 tag 지정이 완료돼.

예를 들어 tag를 "Red", "Blue" 이렇게 두가지를 이미 설정했기 때문에 tag 시스템을 사용해서 골키퍼를 찾을 순 없어.  
따라서 두 개 이상의 schema가 충돌할 수 있는 문제를 방지하려고 했던게 궁극적으로 FindWithTag의 사용을 심각하게 제한하게 되는거야.

전반적으로 FindWithTag는 아마도 GameObject.Find를 대체하는 최악의 항목일거야.  
그러나 속도와 상대적 단순성으로 인해 드물게 사용하더라도 대체 방법 리스트에 체크해 두는 것이 좋아.

## 🔖 Inspector를 통한 공개 구성원 할당 (Public member assignment via Inspector)

```c#
public class CreepyFollower : MonoBehaviour {
    public Transform target;

    void Update() {
        transform.LookAt(target.position);
        transform.Translate(Vector3.forward * Time.deltaTime);
    }
}
```

### 📍 속도
초고속, 사실 본질적으로 즉각 호출되며, 필요한 경우 정확한 유형의 구성 요소를 사용할 수도 있어.

```c#
target.GetComponent<Character>()
```

위와 같이 호출하는 경우, 대상을 Character로 만들고 GetComponent 호출을 모두 피할 수 있다고 해.  
UnityEngine.Object에서 파생된 모든 것, 특히 모든 구성 요소(Camera, Renderer 등 포함) 및 모든 Mono(확장 시 사용자의 모든 스크립트)를 이 방법으로 드래그 드롭할 수 있지.

>참고 사항: someComponent.transform은 someTransform.GetComponent<SomeComponent>()보다 빠르다(약간 더 안정적)

따라서 두 가지를 다 적용할 수 있다면, Tranform(or GameObject) 대신 SomeComponent에 직접 연결하는 것이 좋아.

### 📍 신뢰성

매우 신뢰할 수 있어.  

개체에 참조를 끌어다 놓으면 구체적으로 해당 개체가 될 거라는 것을 알 수 있어.  
그것에 대해 예외는 존재하지 않아.

주의할 점은 프리팹 내부에서 외부 또는 두 개의 다른 Scene간의 참조가 손상될 수도 있어.

### 📍 다목적성

위에서 설명한 것처럼 이 기술은 서로 논리적인 관계를 가진 개체 뿐만 아니라 계층적 관계도에 있는 개체에도 아주 잘 사용돼.

## 🔖 기회 할당 (Opportunistic Assignment)

```c#
private GameObject thingThatHitMe;

void OnCollisionEnter(Collision c) {
    thingThatHitMe = c.gameObject;
}

void Update() {
    if (thingThatHitMe != null) {
        thingThatHitMe.transform.position = Vector3.zero;
    }
}
```

궁극적으로 이 기술은 이전 기술과 매우 유사하고, 여기에 모두 나열하기에는 유용하게 사용할 수 있는 방법이 너무 많아.  
기본적으로 이게 의미하는 바는 필요한 것에 액세스할 수 있을 때 그것을 붙잡고 저장한다는 것.

가장 일반적인 사용 사례에는 충돌 및 레이캐스팅과 관련이 있다고는 하는데 거의 모든 곳에 적용할 수 있다고 해.

위의 방법들과 함께 사용하면 매우 강력해질 수 있어.

예를 들어 `ScriptMaster`가 `GetComponentsInChildren<ScriptSlave>`를 사용하여 자식을 반복하는 경우 `ScriptSlave`의 `master` 속성을 자체로 설정할 수 있고, 이 과정으로 모든 Slave는 Master가 누구인지 알게 돼.

위의 말이 잘 이해가 안가는데, 내가 이해한대로 샘플 코드를 작성해 봤어.

```c#
public class ScriptSlave : MonoBehaviour
{
   ScriptMaster _master;
   public ScriptMaster Master
   {
       set { _master = value; } get => _master; 
   }
}
```

먼저 ScriptSlave야

```c#
public class ScriptMaster : MonoBehaviour
{
    public ScriptSlave[] ArrScriptSlaves;
    
    public void SetSlave()
    {
        ArrScriptSlaves = GetComponentsInChildren<ScriptSlave>();

        if (ArrScriptSlaves is null)
            return;

        foreach (var slave in ArrScriptSlaves)
            slave.Master = this;
    }

    void Start()
    {
        SetSlave();
    }
}
```

위의 코드는 ScriptMaster야

![image1](/assets/images/posts/UnityDocs/2023-02-06-my-unitydoc-post_3/1.png){: width="50%" height="50%"}

하이어라키에는 이런식으로 구성해서 각 오브젝트에 해당하는 스크립트를 컴포넌트로 붙여줬어(인스펙터에서).

그리고 ScriptMaster 코드를 보면 `SetSlave` 메서드 에서 `GetComponentsInChildren<T>()` 메서드로 자식 오브젝트로 있는 ScriptSlave 들을 가져와서 ArrScriptSlaves 배열에 넣어.

그 후에 ArrScriptSlaves 배열의 각 요소에 접근해서 ScriptSlave 프로퍼티인 Master 에 foreach 문을 사용해서 ScriptMaster를 할당 해.
이렇게하면 Slave가 Master가 누구인지 알 수가 있지.

이런 느낌을 얘기한게 아닌가 추측해서 만든 코드야.

설명을 이어가자면, 코드 측면에서 기회 할당과 인스펙터를 통한 할당의 주요 차이점은 이 개체가 null인 경우 오류가 발생하지 않는 방식으로 코드를 작성해야 한다는 것이라고 해.  
이는 일반적으로 위에서 본 것처럼 단순히 null 체크를 수행하는 것을 의미해.

그래서 개체에 유효한 참조가 있을 때까지(null일 경우) 정상적으로 실패하지.

## 🔖 인스턴스화 시 저장 (Storage Upon Instantiation)

```c#
public class ArraySpawner : MonoBehaviour 
{
    public GameObject arrayPrefab;
    public int count = 5;
    public Vector3 offset = new Vector3(1,0,0);

    private GameObject[] spawnedObjects;

    void Start() 
    {
        spawnedObjects = new GameObject[count];
        for (int c=0;c<count;c++) {
            spawnedObjects[c] = Instantiate<GameObject>(arrayPrefab);
            spawnedObjects[c].transform.position = offset * c;
        }
    }
}
```

<strong style="color:orange; font-size:13pt">글쓴이는 Instantiate가 생성된 개체를 반환한 다는 것을 모르는 신입 Unity 플머들이 대다수</strong>라고 하고 있어.  
Instantiate로 개체를 생성할 경우 즉시 할당할 수 있으며, 바보 같이 Instantiate로 생성하고 GameObject.Find로 그 오브젝트를 찾는 경우가 대다수 있다고 해.

그리고 <strong style="color:orange; font-size:13pt">이 경우는 퍼즐 게임과 같이 첫번째 그리드 시스템을 코딩하려는 중급 플머에게서 많이 나오는 것을 발견</strong> 했다고해.  
생성한 그리드 셀을 배열에 저장하면 (필요한 경우 다차원일 수 있음) 항상 빠르고 안정적으로 액세스할 수 있다고 설명하고 있어.

### 📍 속도
즉시, 특성 구성 요소에 액세스하려면 GetComponent를 사용해야 한다는 주의 사항이 있다.

### 📍 신뢰성
오리지널 개체에 대한 참조가 잘되어 있다면, 신뢰성도 좋다.

### 📍 다목적성
사물을 인스턴스화 할 때만 유용하다.

## 🔖 GetComponentsInChildren

```c#
Renderer[] childRenderers;

void Start() 
{
    childRenderers = GetComponentsInChildren<Renderer>();
}

void Update() 
{
    foreach (Renderer r in childRenderers) {
        r.enabled = Input.GetKey(KeyCode.Space);
    }
}
```

이 기능은 계층 구조에 있는 개체에 매우 유용한데, 그 이유는 특정 개체의 계층 구조내에서 특정 클래스를 모두 찾아오기 때문이야.

또한 이유가 무엇이든지 상관없이 개체들을 필터링해야 하는 경우 필터링을 할 때 고유한 논리를 적용할 수 있지.

### 📍 속도
초고속은 아니지만 사용되는 상황에서 가장 빠른 옵션이야.

### 📍 신뢰성
매우 좋아.

### 📍 다목적성
계층 구조 내의 항목에 매우 유용해.

## 🔖 FindObject(s)OfType

```c#
void Start() 
{
    Camera[] allCamerasInScene = FindObjectsOfType<Camera>();
}
```
FindObjectsOfType은 장면에서 주어진 모든 클래스를 찾는 데 유용한 방법이야. 그 이상은 없어.

### 📍 속도
매우 느려, 심지어 GameObject.Find 보다도 느리지.  
따라서 이 방법은 내장 Unity 클래스와 같이 고유한 OnEnable/Disable 메서드를 추가할 수 없는 클래스에서 가장 일반적으로 사용돼.

### 📍 신뢰성
FindObjectsOfType가 GameObject.Find 보다 나은 점은, 내가 FindObjectsOfType를 통해 무엇을 얻을 수 있는지 정확히 알 수 있다는 것이야.


## 🔖 transform.Find

이것도 GameObject.Find와 같이 웬만해서는 사용하지 말라고 하는 것 중에 하나야.

```c#
void Start() 
{
    Transform cameraLocation = transform.Find("Misc/Camera Placeholder");
}
```

<strong style="color:orange; font-size:13pt">너가 반드시 이름으로 무언가를 찾아야 한다면 차라리 transform.Find를 사용해</strong>.  
하지만 사용하기 전에 다시 한번 자신한테 되묻는 거야.  

>"정말 이름으로 찾아야만 하는지, 구성 요소를 자식에 연결할 수 없고, GetComponentInChildren으로 찾을 수 없는지, public reference를 만들고 할당할 수 없다고 확신해?"

확신하지 못한다면 다른 방법 중 하나를 사용하라고 권장해.

>글쓴이의 경험상 이게 정말 필요한 유일한 경우는 가져온 3D 개체의 계층에서 개체를 찾아야할 때 라고 해.  
예를 들어 블렌더에서 설정한 Scene에 대한 카메라의 위치가 있다고 하네?  

transform.Find는 기본적으로 GameObject.Find가 전체 Scene이 아닌 하나의 계층 구조로 제한되는 경우의 모습이야.   

하나의 계층 구조로 제한되기 때문에 더 안정적이고(오탐에 대한 우려가 적음) 더 다재다능하며(장면에 사물의 복사본이 두 개 이상 있는 경우에도 계속 사용할 수 있음) 더 빨라(확인할 개체 수가 적음).  

특정 변환을 얻기 위해 원하는 변환에 대한 전체 계층 경로를 포함할 수도 있어.

transform.Find에 대해서는 Unity 문서에 더 좋은 정보와 예제가 있어.

[🔔 transform에 대한 Unity 문서 보러가기](https://docs.unity3d.com/ScriptReference/Transform.Find.html)
{: .notice--warning}

# 🥱 끝 마치며
<hr style="width:100%" />

GameObject.Find만 다루려고 했는데, 대체 기술들을 거의 전부 다루게 되었어.

<strong style="color:limegreen; font-size:15pt">결론은 웬만하면 GameObject.Find를 사용하는 것을 지양하고, 정말 어쩔 수 없이 이름으로 개체를 찾아와야 한다면 transform.Find 하지만 이걸 사용할 때도 정말 많이 심사숙고를 한 끝에 하는게 좋을 것 같아.</strong>

그리고 웬만하면 이름으로 검색에서 개체를 찾는 상황을 안 만들면 조금 더 좋겠지?

길고 긴 글이 드디어 끝났다.

<br>

# 🔎 참고한 내용
<hr style="width:100%" />

[🔔 fqprjs.log 블로그 바로가기](https://velog.io/@fgprjs/Microsoft%EC%97%90%EC%84%9C-%EC%A0%9C%EA%B3%B5%ED%95%98%EB%8A%94-Unity-%EC%84%B1%EB%8A%A5-%EC%B6%94%EC%B2%9C-%EC%82%AC%ED%95%AD)<br>
[🔔 UR Dev Log 블로그 바로가기](https://m.blog.naver.com/os2dr/221556006710)<br>
[🔔 UnityTipsRedux 바로가기](https://starmanta.gitbooks.io/unitytipsredux/content/first-question.html)
{: .notice--warning}

<br>
<strong style="color:yellow; font-size:100pt;">끝</strong>


<hr style="width:100%" />
<br>

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr>

