---
title:  "[실전 게임 코드 리뷰 :: 유니티 클리커 게임] 7. Managers - GameManager"
excerpt: "Unity Lesson 1"

categories:
  - Unity Lesson 1
tags:
  - [Unity Lesson 1]
toc: true
toc_sticky: true
date: 2023-02-02
last_modified_at: 2023-02-06
---

인프런에 있는 Rookiss님의 **[실전 게임 코드 리뷰] 유니티 클리커 게임** 강의를 듣고 정리한 게시글입니다.
<br>
[🔔강의 보러가기 클릭](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84%EA%B2%8C%EC%9E%84-%EC%BD%94%EB%93%9C%EB%A6%AC%EB%B7%B0-%EC%9C%A0%EB%8B%88%ED%8B%B0-%ED%81%B4%EB%A6%AC%EC%BB%A4)
{: .notice--warning}

# 🩻 Manager를 파헤쳐 보자
<hr style="width:100%" />

오늘 부터 큰 틀인 Manager에 해당하는 모든 스크립트를 파헤쳐 볼 생각이다.

# 🧑‍⚕️ 스크립트의 첫 인상
<hr style="width:100%" />

단 한마디로 말해서 "깔끔해서 좋았다."  
역시 갓루키..

<br>

# 🧑‍💼 GameManagerEx.cs
<hr style="width:100%" />

GameManager에는 크게 GameData와 스탯, 재화, 시간, 컬렉션&프로젝트 그리고 저장과 불러오기 스크립트가 있었다.

내가 이 강의를 구매한 이유가 바로 게임데이터, 저장과 불러오기 때문이다.  
개인 프로젝트를 할 때, 항상 저장하고 불러와야 할 게임 데이터를 정하는 데 난항을 겪었고, 도대체 다른 프로젝트들은 저장을 어떻게 구현한 걸까? 라는 궁금증이 들었다.  

저장과 불러오기는 이미 구현 가능하지만, 다른 사람의 코드를 보고 싶었던게 큰 것 같다.  

![hostage](https://media.giphy.com/media/ViIh8qu8Y08swHV7dX/giphy.gif){: width="30%" height="30%"}

<strong style="color:Orange; font-size:15pt">프로젝트를 인질로 강의를 구매했으니 이제부터 잘근잘근 씹어먹어야겠다.</strong>

<strong style="color:crimson; font-size:20pt">응 너 지금 납치된거야:)</strong>
<br>

![drooling](https://media.giphy.com/media/3o6ZsTBERbqBTPkRKo/giphy.gif){: width="30%" height="30%"}

<strong style="color:Yellow; font-size:15pt">벌써 부터 코드를 먹을 생각에 군침이 싹 돈다.</strong>

## 🏠 [Class] PlayerState (Serializable)
플레이어의 현재 상태에 따라 이벤트를 처리하려고 만든 클래스로 추측 된다.

## 🏠 [Class] GameData (Serializable)

대망의 GameData는 Class로 만들어졌고, 멤버 변수로는 저장 후 불러올 데이터에 해당하는 속성들이 정의되어 있었다.   
그 속성으로는 게임 내 사용되는 플레이어의 스탯, 재화, 시간, 컬렉션 그리고 게임 내의 컨텐츠인 프로젝트 완료 횟수 등이 있었다.

내부에는 초기 기획에 있었으나 없어진 기능으로 추정되는 속성이 보였다.  
(나중에 이 프레임 워크를 참고해서 맨땅부터 내 프레임 워크를 만들 때, 없애야 겠다.)

## 🏠 [Class] GameManagerEx

메인 클래스다.

```c#
GameData _gameData = new GameData();
public GameData SaveData { get { return _gameData; } set { _gameData = value; } }
```

일단 GameData 클래스 변수를 하나 선언해서 초기화 하였고, 이것은 저장용 데이터로 취급해서 사용하는 것으로 보인다.

그리고 선언한 변수 _gameData의 속성에 접근할 수 있도록 프로퍼티를 만들어줬다.

### 🖨️ [Method] Init

테이블로 설정해 놓은 초기 설정 값 데이터를 가져와서 지역 변수에 담아 놓는다.  
그리고 저장된 데이터가 있는지 없는지 확인 후 있다면 컬렉션 데이터만 가져와서 현재 저장용 데이터에 넣는다.

데이터가 없으면 새로 만들어주고, 캐릭터를 생성한다. 

```c#
public enum JobTitleType
{
  Intern = 0, // 인게임에 배치만 하고 아무것도 안함
  Sinib, // 주인공 시작
  Daeri,
  Gwajang,
  Bujang,
  Esa,
  Sajang,
  Cat,
}
```

총 8명이니 아래와 같이 작성되어 있다.

```c#
_gameData.Players = new PlayerState[JOB_TITLE_TYPE_COUNT + 1];
```

여기서 의문이 드는게 하나 있는데, 
Define 스크립트를 보면

```c#
public const int JOB_TITLE_TYPE_COUNT = (int)JobTitleType.Sajang + 1; // 고양이 제외
```

고양이 제외라는 주석이 달려있다.  
[Q] 이건 그냥 분류를 위해서 위와 같이 작성한 건지 의문이다.  
사실 위의 값이 `(int)JobTitleType.Cat` 이 값과 같기 때문이다.  
이건 코드를 더 보면서 확인해봐야겠다.  

그리고 나머지 스탯, 재화, 시간등을 설정해둔 초기값으로 초기화 한다.

그리고 `ReApplyCollectionStats` 라는 메서드를 호출한다.  

### 🖨️ [Method] ReApplyCollectionStats

이건 무슨 역할을 하는 메서드 인고?
  
DataManager에서 컬렉션 데이터를 가져와서, 컬렉션이 추가적으로 주는 스탯을 현재 스탯에 적용해주는 메서드다.

그리고 마지막에 `OnNewCollection` 델리게이트를 구독한 함수가 있을 경우 호출 한다.

[Q] 여기서 의문이 든다.
왜냐하면 `OnNewCollection` 델리게이트를 구독하는 곳은 단 한 군데 밖에 없는데 UI_PlayPopup 스크립트에 Init 메서드 본문 하단

```c#
Managers.Game.OnNewCollection = OnNewCollection;
```

이 곳에서 밖에 없고, 호출하는 곳은 GameManagerEx 클래스의 `ReApplyCollectionStats` 메서드 뿐이다.  
GameManagerEx 클래스의 `Init` 메서드를 두번 이상 호출하는 것이 아니라면, `ReApplyCollectionStats` 메서드는 단 한번만 호출되는 것이고, 그렇다면 델리게이트를 이어줄 필요도 없는 것이라 생각했기 때문에, `OnNewCollection` 델리게이트의 존재의 이유가 의문이 들었다.

분명 저 코드가 있는건 2번이상 부르기 때문이다.  
내가 뭘 빠뜨린 부분이 있는 걸까? 다시 한번 GameManagerEx 클래스와 진입점을 차근차근 다시 정리해보자.

![img01](/assets/images/posts/Unity_Lecture_1/2023-02-02-my-unitylec1-post_7/1.jpg)

정리해보니 GameManagerEx 클래스의 `Init` 을 호출하는 부분이 3가지 부분이 있는 것을 확인 할 수 있었다.

![img02](/assets/images/posts/Unity_Lecture_1/2023-02-02-my-unitylec1-post_7/2.png){: width="30%" height="30%"}

UI_TitlePopup의 `OnClickStartButton`, `OnClickContinueButton`, `OnClickCollectionButton` 메서드에서 호출된다.  
이 3개의 메서드는 위의 이미지의 각 버튼에 해당하는 메서드들이다.

따라서 타이틀 화면으로 나왔다가 다시 게임을 시작하는 경우 새로운 컬렉션이 있다면 GameManagerEX 클래스의 `Init` 메서드의 마지막 부분 `ReApplyCollectionStats` 메서드의 호출 후 `ReApplyCollectionStats` 메서드 본문 하단의 `OnNewCollection?.Invoke(data);`가 실행되고 구독되어 있는 메서드(`OnNewCollection`)를 호출하게 된다.

이로써 의문은 풀리게 됐다.

### 🖨️ [Method] SaveGame

Newtonsoft.Json을 사용하지 않고 유니티에서 기본적으로 제공하는 JsonUtility를 사용했다.  
GameData 클래스로 묶어서 파일시스템을 이용해 SaveData.json 파일로 저장을 한다.

딱히 특별한 점은 없다.

### 🖨️ [Method] LoadGame - bool 반환형

저장 경로에 파일이 있는지 없는지 판단하는 방어적인 코드로 작성되어 있다.
만약 파일이 있다면 JsonUtility를 이용해 Deserialization을 통해 GameData로 변환시켜 `Managers.Game.SaveData = data` 해당 코드를 실행한다.

정상적으로 실행되었다면 true를 반환한다.

<br>

# 📢 오늘의 한마디
<hr style="width:100%" />

<strong style="color:orange; font-size:15pt"> JsonUtility vs Newtonsoft.Json = ? </strong>

![herolanding](https://media.giphy.com/media/xT0GqHJMxnHhLTEbYY/giphy.gif){: width="30%" height="30%"}

>이 두가지의 차이 관련 글은 아래 링크에서 확인

[🔔JsonUtility vs Newtonsoft.Json](https://fatiger92.github.io/unitydocs/my-unitydoc-post_2/)
{: .notice--warning}

<br>

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr>

