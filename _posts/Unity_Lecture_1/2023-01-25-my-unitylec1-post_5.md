---
title:  "[실전 게임 코드 리뷰 :: 유니티 클리커 게임] 5. 게임 컨텐츠 제작 및 설계"
excerpt: "Unity Lesson 1"

categories:
  - Unity Lesson 1
tags:
  - [Unity Lesson 1]
toc: true
toc_sticky: true
date: 2023-01-25
last_modified_at: 2023-01-25
---

인프런에 있는 Rookiss님의 **[실전 게임 코드 리뷰] 유니티 클리커 게임** 강의를 듣고 정리한 게시글입니다.
<br>
[🔔강의 보러가기 클릭](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84%EA%B2%8C%EC%9E%84-%EC%BD%94%EB%93%9C%EB%A6%AC%EB%B7%B0-%EC%9C%A0%EB%8B%88%ED%8B%B0-%ED%81%B4%EB%A6%AC%EC%BB%A4)
{: .notice--warning}

# 📕 컨텐츠 관리
<hr style="width:100%" />
  
  해당 강의에서는 Manager 라는 메인 싱글톤 스크립트에서 모든 Manager를 관리하는 방식을 사용한다.  
  나 또한 미니 프로젝트를 할 때, 싱글톤을 선호하는 편이다.  

  하지만 싱글톤 패턴을 얘기하면 상당히 경기를 일으키는 개발자들이 많다고 한다.  
  그렇다면 싱글톤 패턴은 무엇이길래 이렇게 경기를 일으킬 만큼 거부를 하는 것 일까?

## 📑 싱글톤 디자인 패턴(Singleton) 그것이 알고싶다.

### 그래서 싱글톤이 뭔데?
  해당 클래스의 인스턴스가 하나만 생성되게 하고, 어디서든 그 인스턴스에 접근할 수 있도록 하는 패턴.  
  주고 시스템 자원이나 정보를 관리하는 용도로 사용한다.

### 장점:  
  모든 데이터를 전역으로 관리하기 때문에 쉽게 접근할 수 있고, 초기 객체를 생성하게 되면 정적 메모리에 올라가기 때문에 <strong style="color:Yellow">호출이 빠르다. 그리고 중복 생성과 메모리 낭비를 방지</strong>할 수 있다.

### 단점:  
  정적 메모리(static)에 할당된 객체이기 때문에 메모리 크기가 제한적이라 <strong style="color:Yellow">너무 큰 메모리가 쌓이게 될 경우 프로그램의 성능이 낮아진다.</strong>  
  하나의 정적 메모리를 사용하기 때문에 병렬처리나 동기화와 같이 여러 방법으로 메모리에 접근하는데 문제가 생긴다.  
  너무 많은 데이터를 공유할 경우, 싱글톤 인스턴스와 다른 클래스 인스턴스들 간의 결합도가 높아져 개방폐쇄원칙(확장에는 열려 있어야 하고, 변경에는 닫혀있어야 함)에 위배될 수 있다.
  
### 주의할 점:  
  <strong style="color:Yellow">상태를 가진 객체를 싱글톤으로 만들어선 안 된다.</strong>   
  앱 내에 한개의 싱글톤이 존재하고, 이를 전역에서 접근할 수 있다면 각자 다른 스레드에서 객체의 상태를 마구 변경시킬 여지가 있기 때문이다.

### 싱글톤 패턴 예시 코드(C#)

```csharp
public class Manager
{
  public static Manager _instance = null;
  void Awake()
  {
    if (_instance == null)
    {
        _instance = this;
        DontDestroyOnLoad(gameObject);
    }
    else
    {
        Destroy(gameObject);
        return;
    }
  }
}
```

<br>

# 💡 Tip

  1. UI 스크립트에 어싸인도 좋지만 <strong style="color:Yellow;">코드에서 Initialize 하는 편이 관리적인 방향으로 봤을 때 좋다.</strong>  
  2. 이것 저것 오브젝트들을 스크립트 인스펙터에 닥치는 대로 어싸인해서 만드는게 과연 좋은 방법인지 생각해보자.   
  3. UI 스크립트에서는 UI 관련 요소들만 넣고 <strong style="color:Yellow;">실제 데이터는 다른 곳에서 관리</strong>.  
  4. MVVC, MVC 같은 패턴을 고려하면서 코딩하는 것도 좋은 방법이다.  
<br>

# 📢 오늘의 한마디
<hr style="width:100%" />

  ⚡규모가 작은 프로젝트를 만들 때, <strong style="color:crimson;">프레임을 신경 안 쓰고 즉흥적으로 만드는 경우가 많은데 규모가 어떻게 되든지 프레임을 신경써서 만드는 것</strong>이 장기적으로 나의 스타일을 만들 수 있기 때문에 좋은 것 같다.  

  그리고 나 또한 미니 프로젝트 구성을 스크립트에 이것저것 오브젝트 어싸인으로 하는 것이 좋은 방법이라고 생각 했는데, 다시 한번 그게 맞는지에 대해 생각해 보는 강의였다.  
  

<br>

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr>

