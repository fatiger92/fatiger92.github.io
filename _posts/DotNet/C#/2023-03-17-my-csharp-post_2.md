---
title:  "[C# Tip] 가상(virtual)? 추상(abstract)? 인터페이스(interface)?"
excerpt: "그래서 언제 써야하는 건데?"

categories:
  - C Sharp
tags:
  - [C Sharp]
toc: true
toc_sticky: true
date: 2023-03-17
last_modified_at: 2023-03-17
---

# 💁‍♂️ 가상? 추상? 인터페이스?

Framework 를 만들다가 갑자기 virtual, abstract, interface를 언제 써야하는 가에 대해 궁금증이 생겼다.

class와 interface의 차이는 완전히 이해했다.  
만약 내가 정의해야 할 무언가가 class와 interface 이렇게 두 가지를 사용할 수 있다고 가정했을 때, 이 두가지중 하나를 선택해야할 경우, 정의해야 할 요소가 "~이다" 일 경우 class를 사용하고 "~할 수있는" 일 경우 interface 를 사용한다.

하지만 내가 정의해야 할 무언가가 virtual, abstract, interface 를 사용할 수 있을 때, 세 가지중 하나를 선택해야 할 경우 무엇을 어떤 기준으로 선택해야 하는가에 대해서는 내 지식안에서 정리 되지 않았기 때문에 포스팅을 하면서 정리를 하려고 한다.

## 🔖 1. Virtual

virtual 키워드는 메서드, 속성, 인덱서 또는 이벤트 선언을 수정하고 파생 class에서 재정의 하도록 허용하는 데 사용된다.  
즉, 파생 class에서 이 함수를 재정의 할 수 있도록 허용하겠다는 의미이다.  
가상 멤버의 구현은 파생 class의 재정의 멤버(override)로 변경할 수 있으나, 필수적이진 않다.

아래의 예제를 보자.

```c#
public class Monster 
{
   protected virtual void Bark() { }
   protected virtual void Attack() { }
}
```
나는 먼저 Monster 라는 class를 만들었다.  
class 내부 멤버 메서드로는 Bark(울기), Attack(때리기) 라는 메서드가 존재한다.

정리하자면 나는 Monster라면 그게 어떤 몬스터가 되었던 Bark(울기), Attack(공격)을 할 수 있다고 정의한 것이다.  
이 두가지의 메서드에 virtual이 붙어 있는 것을 볼 수 있는데, 이것은 Monster를 상속받는 파생 class에서 재정의하기 위함이다.

추가적으로 protected로 한정자를 건 것은 메서드의 진입 부분이 이곳이 아니기 때문에 모듈화를 위해 건 것이다.

```c#
public class Ork : Monster
{
   protected override void Bark()
      => Debug.Log("우워엉");
   
   protected override void Attack()
      =>  Debug.Log("몽둥이로 때리기");
}
```
그래서 Monster를 상속 받는 파생 class Ork를 만들었다.  
눈치챘는지 모르겠지만, 각 Monster의 종류 마다 울음소리가 다르고 공격하는 방식이 다르기 때문에 위와 같이 파생 class에서 재정의 한 것이다.

재정의하기 위해서는 override를 사용하면 된다.  
나는 virtual은 위와 같이 사용하기로 결정했다.

## 🔖 2. Abstract

파생 class에서 abstract 한정자가 달린 것을 전부 필수로 재정의해야 한다.  
이 한정자를 사용하는 목적은 여러 파생 class에서 공유할 수 있는 기본 class의 공통적인 정의를 제공하는 것이다.

아래의 예제를 보자

```c#
public class Monster
{
    public void Idle(){};
    public void Walk(){};
    public void Run(){};
}

public class Player
{
    public void Idle(){};
    public void Walk(){};
    public void Run(){};
}

public class NPC
{
    public void Idle(){};
    public void Walk(){};
    public void Run(){};
}

```

게임을 개발하다보면 Monster, Player 또는 NPC가 공통적으로 같는 메서드가 있을 경우가 있다.  
이렇게 위와 같이 3가지의 class가 공통적인 메서드를 가지고 있을 경우 

```c#
public abstract class BaseObject
{
   public abstract void Idle();
   public abstract void Walk();
   public abstract void Run();
}
```

Idle, Walk, Run은 Monster, Player, NPC가 공통적으로 사용할 수 있는 메서드이기 때문에 위와 같이 BaseObject라는 abstract class로 묶어줄 수 있다.

```c#
public class Monster : BaseObject
{
   public override void Idle() { }
   public override void Walk() { }
   public override void Run() { }

   protected virtual void Bark() { }
   protected virtual void Attack() { }
}
```

그리고 위와 같이 표현할 수 있다.

## 🔖 3. Interface

interface 는 abstract와 비슷하지만 멤버필드(변수)를 사용할 수 없으며, 대신에 프로퍼티는 사용이 가능하다.  
interface 는 보통 여러 class에 공통적인 기능을 추가하기 위해서 사용한다.

이것은 어떻게 사용하는 것일까?  
아주 쉽다.

위에서 만들었던 Ork class를 나는 진화시켜서 날아다니는 오크로 만들어 주고 싶다.  
그럼 Ork의 기본 속성은 그대로 가져가지만 거기서 "날다" 라는 기능만 추가되는 것이다.

```c#
public class FlyingOrk : Ork
{
   public void Fly() { }
}
```

이럴 경우 위와 같이 FlyingOrk 라는 class를 만들어서 Fly() 라는 메서드만 추가해줘도 상관없다.  
하지만 게임에는 Ork 뿐만 아니라 고블린, 슬라임등 여러 몬스터가 존재한다.

이 몬스터들의 진화체에 전부 "날다" 라는 기능을 넣어주고 싶을 때 interface를 사용한다.

왜냐하면 최상단에 말했듯이 "날다" 라는 것은 기능이고 날수있는 오크, 날수있는 고블린, 날수있는 슬라임 등 "~할 수있는" 에 속하기 때문이다.

```c#
public interface IFly
{
   void SlowFly();
   void FastFly();
}

public class FlyingOrk : Ork, IFly
{
   public void SlowFly() { }
   public void FastFly() { }
}
```

위와 같이 IFly 라는 interface를 만들어서 상속을 걸어줄 수 있다.  
interface를 쓰는 이유는 다중 상속이 가능하기 때문이다.

따라서 설계할 때 이것을 고려해서 class와 interface를 잘 구분해서 사용해야 한다.   
(class의 다중 상속은 C#에서 안될 뿐만 아니라 C++에서 되더라도 치명적인 결함이 있는 것으로 판명남)

마지막으로 제일 중요한 것은 interface 를 상속받는다면 interface 내의 멤버 메서드들을 반드시 필수적으로 정의 해줘야 한다.  
그리고 interface class 내에서는 public 같은 접근 제한자를 사용하지 않는다.  
하지만 상속받는 파생 class 에서는 접근 제한자를 public으로 사용해줘야 한다.

## 🔖 4. 각 차이점

결과적으로 각자의 특징에 알맞게 사용하는 것이 맞다.  
왜냐하면 사실상 기능은 비슷하지만, 특징은 뚜렷하기 때문이다.

예를 들어 abstract는 파생 class가 반드시 구현해야 하는 것을 명시하거나 하나의 그룹으로 묶을 때 사용하고, interface는 class에 한정되지 않고, 범용적으로 사용할 때 또는 디자인 정의가 필요할 때, virtual은 일부 함수에 대해 파생 class에서 선택적으로 재정의가 필요할 때 사용하면 되겠다. 

<br>

# 🔎 참고한 내용
<hr style="width:100%" />

[🚀 건앤로즈님의 블로그](https://hongjinhyeon.tistory.com/93)<br>
[🚀 검은거북님의 블로그](https://zprooo915.tistory.com/11)<br>
{: .notice--warning}

<br>
<hr style="width:100%" />

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr style="width:100%" />

[Top](#){: .btn .btn--primary }{: .align-right}