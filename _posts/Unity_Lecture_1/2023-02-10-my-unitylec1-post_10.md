---
title:  "[실전 게임 코드 리뷰 :: 유니티 클리커 게임] 9. Managers - ResourceManager"
excerpt: "Unity Lesson 1"

categories:
  - Unity Lesson 1
tags:
  - [Unity Lesson 1]
toc: true
toc_sticky: true
date: 2023-02-10
last_modified_at: 2023-02-10
---

인프런에 있는 Rookiss님의 **[실전 게임 코드 리뷰] 유니티 클리커 게임** 강의를 듣고 정리한 게시글입니다.
<br>
[🔔강의 보러가기 클릭](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84%EA%B2%8C%EC%9E%84-%EC%BD%94%EB%93%9C%EB%A6%AC%EB%B7%B0-%EC%9C%A0%EB%8B%88%ED%8B%B0-%ED%81%B4%EB%A6%AC%EC%BB%A4)
{: .notice--warning}

# 🧑‍💼 ResourceManager.cs
<hr style="width:100%" />

![keyboard](https://media.giphy.com/media/JIX9t2j0ZTN9S/giphy.gif){: width="30%" height="30%"}

ResourceManager의 차례다.

해당 Manager는 Sprite 들과, spine이 제공하는 SkeletonDataAsset 데이터 클래스를 관리하는 것으로 추측된다.

## 🏗️ [Constructor] ResourceManager

로그만 찍고 특별한 건 없다.

## 🖨️ [Method] Load() - T 형식 반환형 메서드

```c#
public T Load<T>(string path) where T : Object
```

제네릭으로 구성되어 있고, Object를 상속받는 개체에 제한을 두었다.

본문은 if - else if 문으로 Type이 Sprite일 경우, SkeletonDataAsset일 경우로 분기를 나눠 데이터 로드를 하는 코드가 작성되어 있다.

분기가 끝나고 해당 리소스를 리턴한다.

## 🖨️ [Method] Instantiate()

GameObject 동적생성하는 메서드 

```c#
public GameObject Instantiate(string path, Transform parent = null)
```

string 타입 path 값을 기준으로 동적생성


## 🖨️ [Method] Instantiate() - Overloading

GameObject 동적생성하는 메서드 오버로딩함.

```c#
public GameObject Instantiate(GameObject prefab, Transform parent = null)
```

GameObject 타입 prefab을 기준으로 동적생성 

## 🖨️ [Method] Destroy()

```c#
public void Destroy(GameObject go)
```

GC한테 인수로 받아온 오브젝트를 패스하는 메서드

<br>

# 🌈 번외
<hr style="width:100%" />

## 🤔 Object.Destroy(Object obj)에 대해서 

`Object.Destroy` 메서드는 Unity에서 GameObject를 제거하는 데 사용되고, GameObject를 즉시 제거하는 것이 아닌 제거할 오브젝트의 컴포넌트 및 자식 오브젝트도 같이 제거한다.

```c#
Object.Destroy(gameObject);
```

만약 GameObject를 즉시 제거하고 싶으면 `Object.DestroyImmediate` 메서드를 사용하면 된다.

## 🤔 Object.Destroy의 성능적인 문제

`Object.Destroy` 메서드는 사실상 GameObject를 제거하는 것이 아닌, GameObject에 대한 참조를 제거하는 것이다.  
GameObject가 실제로 제거되는 시점은 GC(Garbage Collection)이 수행되는 시점에 따라 달라진다.

따라서 `Object.Destroy` 메서드를 많이 호출하게 되면, 메모리 누수의 위험이 높아질 수 있고, GC가 발생하는 시점에서 그동안 `Object.Destroy` 메서드를 호출하며 인수로 보낸 GameObject가 한꺼번에 삭제되므로 치명적인 성능저하가 발생할 수도 있다.

성능저하를 예방하기 위해서는 GameObject의 제거 주기를 지정하여 성능 저하를 최소화할 수 있다.    
예를 들어, GameObject를 제거할 때 제거할 오브젝트와 관련된 코드를 정리하고, 몇 초 후에 GameObject를 제거하는 등의 접근법을 사용할 수 있다.  
(참조를 제거하는 것이기 때문에, GameObject에 붙어있는 컴포넌트 속성 값들을 null로 초기화한 뒤 삭제하는 것을 뜻하는 것 같음.)

## 🤔 Object.Destory를 대체할 방법?

가장 많이 사용되는 방법 중 하나는 <strong style="color:limegreen; font-size:13pt">오브젝트 풀링 시스템</strong>을 사용하는 것이다.  
이 방법은 <strong style="color:yellow;">오브젝트를 제거하지 않고 나중에 다시 사용하도록 비활성화하는 것</strong>이다.  
이를 사용하면 생성하고 제거해야 하는 오브젝트의 수를 크게 줄일 수 있기 때문에 성능을 향상시킬 수 있다.

>오브젝트 풀링은 심도 있게 연구해서 포스트를 올리겠음.

또 다른 방법 중 하나는 <strong style="color:limegreen; font-size:13pt">참조 개수 시스템</strong>을 사용하는 것이다.  
이 방법에서는 <strong style="color:yellow;">오브젝트에 대한 참조 개수를 추적하고 참조 개수가 0이 되면 오브젝트를 제거</strong>한다.  
이 방법은 여러씬에서 공유되는 자산과 같은 특정 유형의 오브젝트에 유용할 수 있다.

>참조 개수 시스템은 심도 있게 연구해서 포스트를 올리겠음.

일반적으로 최상의 접근 방식은 프로젝트의 특정 요구 사항에 따라 다르므로 다양한 옵션을 확인해보고 사용 사례에 가장 적합한 옵션을 선택하는 것 좋다.

<br>

# 📢 오늘의 한마디
<hr style="width:100%" />

![Destroy](https://media.giphy.com/media/ckJF143W1gBS8Hk833/giphy.gif){: width="30%" height="30%"}

<strong style="color:Yellow; font-size:15pt">인 간 따 위 파 괴 한 다</strong>

<br>

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr>

