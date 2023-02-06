---
title:  "[실전 게임 코드 리뷰 :: 유니티 클리커 게임] 8. Managers - UIManager"
excerpt: "Unity Lesson 1"

categories:
  - Unity Lesson 1
tags:
  - [Unity Lesson 1]
toc: true
toc_sticky: true
date: 2023-02-06
last_modified_at: 2023-02-06
---

인프런에 있는 Rookiss님의 **[실전 게임 코드 리뷰] 유니티 클리커 게임** 강의를 듣고 정리한 게시글입니다.
<br>
[🔔강의 보러가기 클릭](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84%EA%B2%8C%EC%9E%84-%EC%BD%94%EB%93%9C%EB%A6%AC%EB%B7%B0-%EC%9C%A0%EB%8B%88%ED%8B%B0-%ED%81%B4%EB%A6%AC%EC%BB%A4)
{: .notice--warning}

# 🧑‍💼 UIManager.cs
<hr style="width:100%" />

오늘은 UIManager가 어떻게 구성되어 있는 지 분석한다.

![keyboard](https://media.giphy.com/media/JIX9t2j0ZTN9S/giphy.gif){: width="30%" height="30%"}

UI 이미지로 구성했기 때문에, Sort Order가 굉장히 중요하다.  
따라서 변수로 빼서 관리한 것을 확인 할 수 있었다.

![img01](/assets/images/posts/Unity_Lecture_1/2023-02-06-my-unitylec1-post_8/1.png)

또한 Stack를 이용해서 UI_Popup을 후입선출(LIFO)로 관리하게끔 만들었다.  
(UI 를 Open하고 Close할 시 가장 마지막에 켜진 Popup부터 순차적으로 Close 되야 하기 때문에)

## 🖨️ UI_Scene

그리고 UI_Scene 클래스 프로퍼티가 나오는데, 이 것은 현재 프로젝트에서 사용하는 것 같아 보이지 않는다.   
(아마 이전에 루키님 말대로 프레임워크 자체에 포함된 것이라고 추측)  

```c#
public class UI_Scene : UI_Base
{
	public override bool Init()
	{
		if (base.Init() == false)
			return false;

		Managers.UI.SetCanvas(gameObject, false);
		return true;
	}
}
```
뭐 사실 별건 없다.  

그리고 @UI_Root 라는 오브젝트 하위에 UI들을 붙이기 위한 프로퍼티가 있다.  
이 부분에서 `GameObject.Find` 유니티 내장 메서드를 사용하였는데, 해당 메서드도 성능에 대한 갑론을박이 있다.  
> GameObejct.Find? 라는 내용으로 나중에 심도 있게 다루자.

## 🖨️ [Method] SetCanvas

`SetCanvas(GameObject go, bool sort = true)` 라는 메서드가 보인다.

여기서는 먼저 canvas 오브젝트가 없다면 생성해주고, 있으면 오브젝트를 가져온다.  
이어서 `renderMode`와 `overrideSorting` 을 설정을 해준다.

그리고 인수 sort에 따라 `sortingOrder`를 변경 해준다. 

```c#
public void SetCanvas(GameObject go, bool sort = true)
{
  // ...
  if (sort)
    {
      canvas.sortingOrder = _order;
      _order++;
    }
    else
    {
      canvas.sortingOrder = 0;
    }
}
```

## 🖨️ [Method] MakeSubItem 제네릭 반환형

```c#
public T MakeSubItem<T>(Transform parent = null, string name = null) where T : UI_Base
```

T 반환형 메서드고, Where 절을 이용해서 제네릭 형식에 제약 조건을 주었다.  
UI_Base를 상속받아야만 사용할 수 있다.

본문을 보면, 전제 조건이 있는데 스크립트와 프리팹의 이름이 같아야 한다.

매개변수 name에 인수 string 값이 null 일 경우, 스크립트의 이름을 name으로 넣어준다.  
그리고 설정해 둔 프리팹 경로의 name 값을 기준으로 프리팹을 가져오고, 그 프리팹으로 GameObject를 동적 생성 한다.

매개변수 parent에 인수 Transfrom 값이 null이 아니면 `SetParent` 메서드로 부모를 설정해 준다.

그리고 만든 GameObject의 localScale, localPosition을 설정해주고 제네릭 형식으로 받아온 컴포넌트를 반환해주는 `Utils.GetOrAddComponent<T>(GameObejct go)` 메서드를 호출한다.

호출한 메서드의 반환 값을 반환한다.

<br>

## 🖨️ [Method] ShowSceneUI 제네릭 반환형

해당 메서드는 UI_Scene을 사용하지 않기 때문에 사용하지 않는다.

만약 씬이 존재하고, 씬 마다 UI를 보여주는 것이 다르다면 해당 메서드를 사용할 것 이다.

## 🖨️ [Method] ShowPopupUI 제네릭 반환형

```c#
public T ShowPopupUI<T>(string name = null, Transform parent = null) where T : UI_Popup
```

T 반환형 메서드며, MakeSubItem와 크게 다르지 않다.

다른 점은 `Utils.GetOrAddComponent<T>(GameObejct go)` 메서드로 반환 받은 값을 popup이라는 변수에 집어 넣고
위에서 말한 Stack 변수 _popupStack에 Push 메서드로 밀어 넣는 것.

그리고 아래의 부모 오브젝트를 정해 주는 if - else if 문이 실행 된다.

```c#
if (parent != null)
    go.transform.SetParent(parent);
else if (SceneUI != null)
    go.transform.SetParent(SceneUI.transform);
else
    go.transform.SetParent(Root.transform);
```

그리고 `localScale` 과 `localPosition` 을 설정한다.  
마지막으로 생성한 popup을 반환한다.

## 🖨️ [Method] FindPopup 제네릭 반환형

```c#
public T FindPopup<T>() where T : UI_Popup
```

_popupStack 에서 Where 내장 메서드를 사용해서 Stack 안에 들어 있는 팝업 중, 내가 제네릭으로 받아온 타입에 해당하는 팝업을 반환 해주는 메서드.

## 🖨️ [Method] PeekPopupUI 제네릭 반환형

```c#
public T PeekPopupUI<T>() where T : UI_Popup
```

_popupStack의 Count가 0이 아닐 경우 제네릭으로 받아온 타입에 해당하는 팝업을 반환함.
이 과정에서 Peek 이라는 Stack 내장 메서드가 사용됨

Peek은 Pop과 기능이 비슷한데, Pop은 top에 위치하는 요소가 리턴되고 삭제된다.
하지만 Peek은 top에 위치하는 요소가 리턴되고 삭제는 하지 않는다.

```c#
public class StackSample : MonoBehaviour
{
    public int Length = 100;
    public Stack<int> _stack;

    void Start()
    {
        _stack = new Stack<int>();

        for (int i = 0; i < Length; i++)
            _stack.Push(i);
        
        var pop =  _stack.Pop();
        Debug.Log($"pop :: {pop}");
        Debug.Log($"After pop stack count :: {_stack.Count}");
        
        var peek = _stack.Peek();
        Debug.Log($"peek :: {peek}");
        Debug.Log($"After pop stack count :: {_stack.Count}");

    }
}
```
위의 코드의 결과 값은 아래와 같이 나온다.

![img01](/assets/images/posts/Unity_Lecture_1/2023-02-06-my-unitylec1-post_8/2.png)

코드를 읽어보면 

```c#
for (int i = 0; i < Length; i++)
    _stack.Push(i);
```

for문을 이용해서 0~Length(100) 까지 스택에 넣었다.

![img02](/assets/images/posts/Unity_Lecture_1/2023-02-06-my-unitylec1-post_8/3.png){: width="40%" height="40%"}

따라서 위와 같이 되고 Stack은 마지막에 넣은 요소부터 꺼낼 수 있으니 Pop 메서드 호출시 99가 나오게 되고, 요소가 나오면서 Stack 내에서는 삭제가 완료되니 Stack의 Count는 100 개에서 99 개로 변한다.

그리고 바로 아래 Peek 메서드를 호출 할 경우 99는 이미 Pop으로 나오고 삭제 되었기 때문에 98이 밀려 나오게 된다.  
Peek은 삭제는 되지 않으니 Count는 99개에서 그대로 99개로 유지 된다.

## 🖨️ [Method] ClosePopupUI

```c#
public void ClosePopupUI(UI_Popup popup)
```

Stack의 Count가 0 이면 반환하는 방어적 코드가 작성되어있다.  
만약 Peek해온 popup이 인수로 받아온 popup 과 다를 경우 에러 처리를 함. 

## 🖨️ [Method] ClosePopupUI - overloading

```c#
public void ClosePopupUI()
```

해당 메서드는 위 메서드를 Overloading 한 메서드다.  

<strong style="color:orange; font-size:15pt">Overloading은 같은 이름을 가지는 다른 동작을 하는 메서드를 의미한다.</strong>

>Overloading의 디테일한 부분은 Overriding 과 같이 다루겠음.

Stack에서 팝업을 꺼내와서 `Managers.Resource.Destroy(GameObject go)` 메서드를 호출한다. 
그리고 `_order` count를 1 내린다.

## 🖨️ [Method] CloseAllPopupUI

Popup Stack을 전부 비우는 메서드

## 🖨️ [Method] Clear

Popup Stack, SceneUI 전부 비우기.

<br>

# 📢 오늘의 한마디
<hr style="width:100%" />

<strong style="color:Yellow; font-size:15pt">Stack을 이용해서 UI 로드 순서를 관리한다.</strong>

<br>

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr>

