---
title:  "[실전 게임 코드 리뷰 :: 유니티 클리커 게임] 11. Managers - SceneManager"
excerpt: "Unity Lesson 1"

categories:
  - Unity Lesson 1
tags:
  - [Unity Lesson 1]
toc: true
toc_sticky: true
date: 2023-02-28
last_modified_at: 2023-02-28
---

인프런에 있는 Rookiss님의 **[실전 게임 코드 리뷰] 유니티 클리커 게임** 강의를 듣고 정리한 게시글입니다.
<br>
[🔔강의 보러가기 클릭](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84%EA%B2%8C%EC%9E%84-%EC%BD%94%EB%93%9C%EB%A6%AC%EB%B7%B0-%EC%9C%A0%EB%8B%88%ED%8B%B0-%ED%81%B4%EB%A6%AC%EC%BB%A4)
{: .notice--warning}

# 🧑‍💼 SceneManager.cs
<hr style="width:100%" />

![keyboard](https://media.giphy.com/media/o0vwzuFwCGAFO/giphy.gif){: width="30%" height="30%"}

해당 프로젝트는 Scene이 하나밖에 존재하지 않는다.
그런데, SceneManager가 따로 있는 것을 보아 무언가 깊은 뜻이 있겠지하고 분석을 시작한다.

먼저 스크립트명은 SceneManager 지만 본문에 들어있는 클래스는 SceneManagerEx 다.

아마도 유니티 내장 클래스에 이미 SceneManager가 있기 때문이겠지.

따라서 SceneManagerEx 클래스를 분석해야 할 것 같다.

해당 클래스에서는 아래와 같이 Scene이라는 Enum 클래스를 사용한다.

```c#
public enum Scene
{
	Unknown,
	Dev,
	Game,
}
```

아마 개발하는 Dev Scene과 Game Scene을 나누려는 의도가 있었던 것으로 보여진다.

현재 Scene 스크립트 오브젝트를 가지고 있는 변수도 보인다.


## 🏗️ [Constructor] SceneManagerEx

로그만 찍고 특별한 건 없다.

## 🖨️ [Method] ChangeScene()

```c#
public void ChangeScene(Define.Scene type)
```

현재 Scene 오브젝트 변수를 스크립트에서 초기화주고, 인수로 받아온 type을 할당해준다.
그리고 type을 기준으로 Scene을 Load 한다.

## 🖨️ [Method] GetSceneName()

인수로 받아온 Enum 값을 Scene 이름으로 치환해서 반환해줌.

```c#
string GetSceneName(Define.Scene type)
```

`System.Enum.GetName()` 이라는 메서드로 Scene 이름으로 치환한다.

```c#
public static string GetName(Type enumType, object value)
```

해당 메서드는 위와 같다.

본문으로는 가져온 Enum을 string 값으로 바꿔서 각 char 배열의 요소에 저장한 뒤, 카멜 형식으로 변경해준다.
그리고 문자열을 반환한다.

<br>

# 📢 오늘의 한마디
<hr style="width:100%" />

<strong style="color:Yellow; font-size:15pt">프레임 워크에 들어있는 스크립트로 보인다.</strong>

<br>

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr>

