---
title:  "[C# Tip] Switch Expression, 속성 패턴, var 패턴"
excerpt: "나 이거 진짜 처음봐"

categories:
  - C Sharp
tags:
  - [C Sharp]
toc: true
toc_sticky: true
date: 2023-04-04
last_modified_at: 2023-04-04
---

# 💁‍♂️ Switch 문은 알겠는데 switch expression 은 뭐죠?
<hr style="width:100%" />

## 🔖 Switch Expression (Switch 패턴 일치식)

혼자 코드를 리팩토링 하다가 갑자기 Switch expression를 보게 되었다.  

<strong style="color:yellow; font-size:14pt">무려 C# 8.0부터 추가 된 기능이다. </strong>  

<strong style="color:yellow; font-size:14pt"> Switch 패턴 일치 식이라고 하나보다. </strong> 

![wow1](https://media.giphy.com/media/QXPmPdudTz4So2P4OQ/giphy.gif){: width="20%", height="20%"}

[🚀 관련 MS Doc](https://learn.microsoft.com/ko-kr/dotnet/csharp/language-reference/operators/switch-expression)
{: .notice--warning}


나는 대체로 분기가 3개 이상이 되면 Switch문을 사용 하는데, 각 case의 본문이 같을 경우 뭔가 X를 보고 어디를 안닦은 기분이 들었다.

예를 들어 아래와 같은 코드에서 말이다.

```c#
enum Fruit
{
   Apple,
   Banana,
   Orange,
   Grape,
}

public void ComeOnIsThisYourBest()
{
   var _fruitName = Fruit.Apple;
   var num = 0;

   switch (_fruitName)
   {
      case Fruit.Apple:
            num = 1;
            break;
      case Fruit.Banana:
            num = 2;
            break;
      case Fruit.Orange:
            num = 3;
            break;
      case Fruit.Grape:
            num = 4;
            break;
      default:
            throw new ArgumentOutOfRangeException();
   }
}
```

놀랍게도 Switch 문 내의 각 본문이 동일한 패턴의 코드로 되어 있다면 아래로 쓸떼없이 길었던 Switch 문을 한번에 줄여버릴 수 있다.

```c#
  enum Fruit
    {
        Apple,
        Banana,
        Orange,
        Grape,
    }

    public void ThisIsMyBest()
    {
        var _fruitName = Fruit.Apple;

        var num = _fruitName switch
        {
            Fruit.Apple => 1,
            Fruit.Banana => 2,
            Fruit.Orange => 3,
            Fruit.Grape => 4,
            _ => throw new ArgumentOutOfRangeException()
        };
    }
```

바로 위와 같이 바꾸어 줄 수 있는데 이 것이 바로바로바로바로바로 Switch 패턴 일치식이다.  
가독성이 더 있는지는 모르겠지만, 이 정도도 못 알아챈다면 그냥 개발자를 접는 게 어떨까?

가슴에 손을 얹고 생각해보자.

나의 뇌의 용적량은 개미와도 같아서 이렇게 적어 놓고도 분명 잊어버릴 게 분명하기 때문에 간단하게 설명을 적어두어야 겠다.

```c#
public void ThisIsMyBest()
{
   var _fruitName = Fruit.Apple;

   // num 에 값을 넣을 건데, _fruitName을 기준으로 switch 문을 쓸거야
   var num = _fruitName switch
   {
      Fruit.Apple => 1,   // 사과면 1이고
      Fruit.Banana => 2,  // 바나나면 2고
      Fruit.Orange => 3,  // 오렌지면 3이고
      Fruit.Grape => 4,   // 포도면 4야
      _ => throw new ArgumentOutOfRangeException() // 그것도 아니다? 그러면 전부 Exception 날려 버려
   }; // 문장의 끝은 항상 세미콜론이야 ㅇㅋ?
}
```  

아주 자세하게 설명을 적어 보았다.

Switch 패턴 일치식의 전제조건은 이름에서 부터 알 수 있다시피 각 case의 본문이 정확하게 패턴화 되어 있어야 한다.

그러고 문서를 내리다가 또 재미있는 걸 찾았는데 Case Guard 라는 녀석이다.

이것을 사용하려면 우리는 속성 패턴과 var 패턴을 알아야만 한다.

<strong style="color:yellow; font-size:16pt"> 최대한 짧고 굵게 알잘딱 설명해보겠다. </strong> 

## 🔖 Property Pattern Matching

이건 별거 없다.
만약 내가 Struct 즉 구조체를 정의했는데, 그 구조체를 기준으로 속성에 따라 분기를 나눠서 무언가 하고 싶다.  
이럴 때 쓰는게 속성 패턴이다.

백문이불여일견 한번 코드로 설명해본다.

```c#
public enum Animals
{
   Tiger,
   Mouse,
   Dog,
   Horse,
}

public struct Animal
{
   public Animals Type { get; set; }
   public string Name { get; set; }
   public int Level { get; set; }
   public int Damage { get; set; }
   public int Armor { get; set; }
   public int Health { get; set; }
   public int Speed { get; set; }

   public Animal(Animals type, string name, int level, int damage, int armor, int health, int speed)
      => (Type, Name, Level, Damage, Armor, Health, Speed) = (type, name, level, damage, armor, health, speed);
}
```

위와 같은 구조체를 만들어 보았다.  
동물은 타입, 이름, 레벨, 데미지, 방어력, 체력, 스피드 속성들을 갖는다.  
그리고 나는 지금 Level이 10 이상일 경우, 데미지가 10 이상일 경우, 스피드가 10 이상일 경우 등과 같은 각 속성 마다의 분기를 나누고 싶다.

이럴 때 속성 패턴을 샤샤샥

```c#
public Animal GetAnimal(Animal animal)
{
   switch (animal)
   {
      case { Type: Animals.Tiger, Level: 5 }:
            return new Animal(Animals.Tiger, "노말 호랑이", 5, 5, 5, 5, 5);
      default:
            return default;
   }
}
```

위와 같이 쓸 수 있다.  
그리고 코드를 말로 풀어서 설명하면,

인수로 받아온 `animal`의 `Type`이 `Animals.Tiger` 이고 `Level` 이 5일 경우 case 본문 실행이다.

## 🔖 Var Pattern

이어서 var 패턴을 설명하겠다.

```c#
   public Animal GetAnimal(Animal animal)
    {
        switch (animal)
        {
            case { Type: Animals.Tiger, Level: 5 }:
                return new Animal(Animals.Tiger, "노말 호랑이", 5, 5, 5, 5, 5);

            case { Type: Animals.Dog, Level: 5, Damage: var damage } when damage < 10: 
                return new Animal(Animals.Tiger, "나약한 강아지", 5, 5, 5, 5, 5);

            default:
                return default;
        }
    }
```

여기서 나약한 강아지를 보면 `Damage: var damage` 라고 쓴 부분을 볼 수 있다.  
속성 값을 var 변수에 따로 담아야 하는 일이 존재할 경우 사용한다.

위에서는 단지 `damage` 단일 값을 비교 했지만, 2가지 이상의 속성을 비교해야 할 경우 사용하곤 한다.

마지막으로 이것을 Switch 패턴 일치식으로 변환하면 아래와 같다.

```c#
public Animal GetAnimal(Animal animal)
{
   return animal switch
   {
      { Type: Animals.Tiger, Level: 5 } => new Animal(Animals.Tiger, "노말 호랑이", 5, 5, 5, 5, 5),
      { Type: Animals.Dog, Level: 5, Damage: var damage } when damage < 10 => new Animal(Animals.Tiger, "나약한 강아지",
            5, 5, 5, 5, 5),
      _ => default
   };
}
```

## 🔖 Case Guard

빙 돌아서 왔는데 그래서 케이스 가드가 무엇이냐?

먼저 케이스 가드는 부울식 즉 Bool 값이어야 한다.

```c#
public Animal GetAnimal(Animal animal)
{
   return animal switch
   {
      { Type: Animals.Tiger, Level: 5 } => new Animal(Animals.Tiger, "노말 호랑이", 5, 5, 5, 5, 5),
      { Type: Animals.Dog, Level: 5, Damage: var damage, Armor: var armor } when damage < armor => new Animal(Animals.Tiger, "단단한 강아지", 5, 5, 10, 5, 5),
      _ => default
   };
}
```

위의 코드의 `단단한 강아지` 를 보자.  
`animal` 로 받아온 인수의 `Type` 값이 `Animals.Dog` 이고 `Level` 이 5일 경우, `Damage` 속성을 `var 변수의 damage` 에 넣고, `Armor` 속성을 `var 변수의 armor` 에 넣는다.  
그리고 `when` 케이스 가드로 `damage < armor` 가 true 면 case 본문을 실행한다.

결론적으로 케이스 가드는 일치하는 패턴과 함께 충족해야 하는 또 다른 조건이다.


## 🤔 해당 기술에 대해서 느낀점?

만약 case가 패턴화되어 있다면 무조건 쓸 것 같다.  
이것도 못 알아 본다면 찾아보고 신 문물을 받아 들여야지 언제까지 구시대 코딩을 할 순 없잖아?

스크립터면 그냥 몰라도 되고, 자신이 개발자라고 말하고 싶다면 찾아서 배워야 겠지

C#님 오늘도 일용할 지식을 주셔서 감사합니다.

<br>
<hr style="width:100%" />

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr style="width:100%" />

[Top](#){: .btn .btn--primary }{: .align-right}