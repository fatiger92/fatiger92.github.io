---
title:  "[실전 게임 코드 리뷰 :: 유니티 클리커 게임] 9. Managers - DataManager"
excerpt: "Unity Lesson 1"

categories:
  - Unity Lesson 1
tags:
  - [Unity Lesson 1]
toc: true
toc_sticky: true
date: 2023-02-09
last_modified_at: 2023-02-09
---

인프런에 있는 Rookiss님의 **[실전 게임 코드 리뷰] 유니티 클리커 게임** 강의를 듣고 정리한 게시글입니다.
<br>
[🔔강의 보러가기 클릭](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84%EA%B2%8C%EC%9E%84-%EC%BD%94%EB%93%9C%EB%A6%AC%EB%B7%B0-%EC%9C%A0%EB%8B%88%ED%8B%B0-%ED%81%B4%EB%A6%AC%EC%BB%A4)
{: .notice--warning}

# 🧑‍💼 DataManager.cs
<hr style="width:100%" />

![research](https://media.giphy.com/media/3orieUe6ejxSFxYCXe/giphy.gif){: width="40%" height="40%"}

DataManager는 과연 어떻게 구성되어 있을까?

## 🚪 [Interface] ILoader<Key, Item>

ILoader 인터페이스는 xml을 변환해서 가져오는 xml Loader에 전부 상속되어 있다.

Dictionary를 반환하는 메서드와 Bool을 반환하는 메서드로 구성되어 있다.

DataManager는 여러가지 데이터의 Dictionary와 List를 프로퍼티로 가지고 있다.

그리고 setter를 private으로 한정한다.

## 🖨️ [Method] Init()

Init 메서드에서는 Loader 타입 반환형 LoadXml 메서드로 인수로 보내는 스트링 값에 해당하는 Loader를 반환 받는다.  
그리고 Loader의 내장 메서드 MakeDic (ILoader 인터페이스를 상속받은 클래스) 을 호출해서 결론적으로 Dictionary를 반환 받는다.

컬렉션 데이터, 다이얼 로그 이벤트 데이터 등등의 게임에 필요한 데이터들을 전부 받아오는 메서드이다.

## 🖨️ [Method] LoadSingleXml()

Key, Value가 있는 데이터가 아닌 그저 Value 만 존재하는 xml 데이터를 Load 하는데 사용하는 메서드.

```c#
private Item LoadSingleXml<Item>(string name)
```
Item 을 직접 반환받는 것을 확인할 수 있다.  
(Item은 제네릭형임, T를 단지 Item으로 쓴 것 뿐임)

## 🖨️ [Method] LoadXml()

Key, Value가 있는 xml 데이터를 Load 하는데 사용하는 메서드  

```c#
private Loader LoadXml<Loader, Key, Item>(string name) where Loader : ILoader<Key, Item>, new()
```

각 클래스로 정의한 Loader, Key, Item을 타입으로 지정해주고, 인수로 xml의 파일 명을 받는다.  
where로 제네릭 형식 제약 조건을 걸어 주었고, 끝에 new() 생성자 제약 조건을 걸었다.

> new() 생성자 제약 조건을 사용하면 new 연산자를 사용하여 형식 매개변수의 인스턴스를 만들 수 있다.
> new() 제약조건을 사용하면 컴파일러에서 제공된 형식 인수에 액세스 가능하고 매개 변수가 없는 생성자가 있어야 한다는 것을 알게 된다.

따라서 new() 제약 조건이 붙은 메서드를 호출할 때 기본 생성자가 없는 클래스를 형식 매개 변수로 넘기면 컴파일 에러가 남.

>new()를 굳이 붙일 필요가 있는지;; 없어도 인스턴스화 되지 않나;

<br>

# 🌈 번외
<hr style="width:100%" />

## 🏠 [Class] StartData - Value만 있는 데이터

xml을 불러오기 위해서는 기본적으로

```c#
using System.Xml.Serialization;
```

해당 네임 스페이스가 필요하다.

```c#
public class StartData
{
  [XmlAttribute]
	public int ID;
	[XmlAttribute]
	public int maxHp;

  //...
}
```

그리고 위와 같이 `XmlAttribute` 라는 어트리뷰트를 써준다.
이제 데이터를 받을 데이터 클래스는 완성됐다.

`LoadSingleXml` 메서드를 호출하면 본문에

```c#
XmlSerializer xs = new XmlSerializer(typeof(Loader));
```

`XmlSerializer` 클래스 변수를 정의 및 초기화하고, `Resource.Load()` 메서드를 통해 TextAsset으로 xml을 불러온다.
그리고 나서

```c#
using (MemoryStream stream = new MemoryStream(System.Text.Encoding.UTF8.GetBytes(textAsset.text)))
```

사실 이 부분은 하나하나 뜯어 봐야 할 것 같다.

먼저 `textAsset.text` 를 UTF8 형식으로 인코딩해서 Byte화 시키고, 그걸 메모리 스트림에 집어 넣는다.

그런데 using을 왜 썼는지 이해가 가지 않는다.

>using문 이라는 게, 따로 존재하는 것 같은데 이건 추후에 C#에서 심도있게 다뤄보겠음.  
>일단은 블로그 글로 대체

[🔔까치의 일상노트 - c# using문 사용 법 && 사용 이유](https://magpienote.tistory.com/65)
{: .notice--warning}

위의 블로그를 보면, 대충 file, font, DB의 같은 경우에 사용할 때 일정부분의 메모리를 잡아 먹는데 이부분에서 컴퓨터의 자원이 할당 된다.

그런데 한 번만 불러오는 데이터인데 불구하고 게임이 구동되는데 계속해서 메모리에 올려놓고 있다고 생각해보자.  
얼마나 비효율적이고 답답해 미칠지경인가? 신경이 너무 쓰여서 위궤양이 생길 것 같은 기분이 든다.

하지만 using문을 사용한다면 데이터를 사용하고 바로 사라지게 할 수 있는 것이다.

설명에는 개체의 범위를 정의할 때 사용하고 그 범위를 벗어나면 자동으로 dispose(처분) 된다. 라고 쓰여져 있다.

```c#
return (Loader)xs.Deserialize(stream);
```

XmlSerializer 클래스의 내장 메서드 Deserialize에 stream을 인수로 호출한다.  

마지막으로 (Loader) 형식 매개 변수로 캐스팅해서 반환한다.

## 🏠 [Class] ShopData - Key Value가 있는 데이터

데이터 부분은 Value만 있는 데이터와 다를 바가 없다.

```c#
public class ShopData
{
  [XmlAttribute]
	public int ID;
	[XmlAttribute]
	public int name;

  //...
}
```

데이터 부분은 구조가 같다.

```c#
[Serializable, XmlRoot("ArrayOfShopData")]
public class ShopDataLoader : ILoader<int, ShopData>
{
	[XmlElement("ShopData")]
	public List<ShopData> _shopDatas = new List<ShopData>();

  public Dictionary<int, ShopData> MakeDic()
	{
    //...
  }

  public bool Validate()
	{
    //...
  }
}
```

다른 점은 Loader가 있다는 것이다.

먼저 어트리뷰트 확인해보자.   
Serializable, XmlRoot("ArrayOfShopData") 라는 것이 있는데, Serializable는 우리가 잘 아는 직렬화 어트리뷰트다.

그런데 옆에 XmlRoot("ArrayOfShopData") 이건 대체 뭘까?
먼저 XmlRoot 어트리뷰트는 

```c#
namespace System.Xml.Serialization
```

해당 네임 스페이스에 존재하는 어트리뷰트인데 아래와 같이 되어있다.

```c#
namespace System.Xml.Serialization
{
  [AttributeUsage(AttributeTargets.Class | AttributeTargets.Enum | AttributeTargets.Interface | AttributeTargets.ReturnValue | AttributeTargets.Struct)]
  public class XmlRootAttribute : Attribute
  {
    public XmlRootAttribute();

    public XmlRootAttribute(string elementName);

    public string DataType { get; set; }

    public string ElementName { get; set; }

    public bool IsNullable { get; set; }

    public string Namespace { get; set; }
  }
}
```

그리고 프로젝트에서 인수로 ArrayOfShopData로 보낸 것은 

```c#
public XmlRootAttribute(string elementName);
```

위의 생성자를 사용한 것으로 추정된다.

<strong style="color:orange; font-size:13pt">XmlRoot 를 ArrayOfShopData 를 가진 xml을 Root로 잡는 것으로 보여진다.</strong>

왜냐하면 ShopData.xml 을 확인해보면 알 수 있는데, `<ArrayOfShopData></ArrayOfShopData>`로 배열과 같은 형태로 요소가 묶여 있는 것을 확인할 수 있다.

하지만 궁금한 점이 있는데, 애초에 `ArrayOfShopData` 라는 건 어디서 나온 string 값일까?

위에 <strong style="color:orange; font-size:13pt"> "XmlRoot 를 ArrayOfShopData 를 가진 xml을 Root로 잡는 것으로 보여진다."</strong> 말 그대로 였다.

결과적으로 XmlRoot("xml의 명칭") 인 것 이었고, Unity 에디터 Tool 탭에 ParseExcel 이라는 항목이 있다.   
`DataTransformer` 클래스로 만든 커스텀 에디터 툴이며, 

ShopData를 가져오는 부분

```c#
static void ParseShopData()
```
위의 메서드의 본문을 보면 마지막 부분에

```c#
static void ParseShopData()
{
  //...
  string xmlString = ToXML(new ShopDataLoader() { _shopDatas = shopDatas });
  File.WriteAllText($"{Application.dataPath}/Resources/Data/ShopData.xml", xmlString);
  AssetDatabase.Refresh();
}
```

`ShopDataLoader` 를 new 생성자로 만드는 과정에서 XmlRoot의 인수를 읽어와서 xml을 string으로 만드는 것으로 추측 된다.

왜냐하면 xmlString을 로그로 찍어 볼 경우, `ArrayOfShopData` 속성에 ShopData 배열을 가진 xml이 출력되기 때문이다. 

<br>

# 📢 오늘의 한마디
<hr style="width:100%" />

![Squidward Q](https://media.giphy.com/media/zgSWpnMeK7dCM/giphy.gif){: width="30%" height="30%"}

<strong style="color:Yellow; font-size:15pt">u징징이?</strong>

<br>

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr>

