---
title:  "[실전 게임 코드 리뷰 :: 유니티 클리커 게임] 13. 유니티 에디터에 엑셀 CSV to XML 변환 에디터 툴 만들어보기"
excerpt: "Unity Lesson 1"

categories:
  - Unity Lesson 1
tags:
  - [Unity Lesson 1]
toc: true
toc_sticky: true
date: 2023-03-22
last_modified_at: 2023-03-22
---

인프런에 있는 Rookiss님의 **[실전 게임 코드 리뷰] 유니티 클리커 게임** 강의를 듣고 정리한 게시글입니다.
<br>
[🔔강의 보러가기 클릭](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84%EA%B2%8C%EC%9E%84-%EC%BD%94%EB%93%9C%EB%A6%AC%EB%B7%B0-%EC%9C%A0%EB%8B%88%ED%8B%B0-%ED%81%B4%EB%A6%AC%EC%BB%A4)
{: .notice--warning}

# 🧑‍💼 엑셀 CSV to XML
<hr style="width:100%" />

![keyboard](https://media.giphy.com/media/SwImQhtiNA7io/giphy.gif){: width="30%" height="30%"}

게임을 개발할 때 테이블에서 값을 불러와서 써야하는 경우가 많다.
일일이 코드안에 하드코딩하면 그 부분을 코딩한 사람만 알고 있게 될 것이고, 아무리 문서화를 잘 한다고 해도 물리적인 숫자가 늘어나면 유지보수가 아ㅏㅏㅏㅏ주 오래 걸리게 된다.

따라서 이 것을 보완하기 위해 테이블을 사용하는 것이고 실제로 아주 직관적이고 관리하기가 수월하다.   
(하드코딩해서 코드 내부에서 찾는 것 보다는)  

대부분의 테이블은 엑셀로 되어 있는데, 이러한 이유는 기획자들이 수정 / 추가하기가 편하기 때문이다.
결론적으로 우리가 원하는 건, "테이블 즉 엑셀에 작성되어 있는 값을 어떻게 클라이언트로 불러오냐?" 다.

이건 개발자의 특성에 따라 다른데, 쉬운방법이 CSV to XML 로 변환을 한 뒤 XML을 읽어들여 불러오는 방법이기 때문에 이 포스팅은 CSV to XML을 기준으로 작성할 것이다.

# 🧑‍💼 본격적으로 들어가기 전에
<hr style="width:100%" />

엑셀에 값을 넣는 것은 좋은데 저장은 CSV로 해야함.

![img01](/assets/images/posts/Unity_Lecture_1/2023-03-22-my-unitylec1-post_13/1.png){: width="70%" height ="70%"}

난 위와 같이 엑셀 값을 설정했다.
그리고 저장은 Assets/Resources/Data/Excel/StartData.csv 경로로 했다. 

그리고 DataParser 라는 스크립트를 하나 생성했다.
여기서 중요한 점은 Assets/Editor 경로에 추가를 해줘야 한다는 점이다.
이유는 빌드 진행시 유니티 내부에서 Editor 폴더를 인식해 빌드에는 안들어가게 해주기 때문이다.

![img02](/assets/images/posts/Unity_Lecture_1/2023-03-22-my-unitylec1-post_13/2.png){: width="30%" height ="30%"}

위와 같이 만들어 줬으면 이제 스크립트를 작성해보자.

## DataParser.cs

먼저 네임스페이스를 보면

```c#
using System.IO;
using System.Text;
using System.Xml.Serialization;
using UnityEditor;
using UnityEngine;
```

이렇게 추가한다.

그리고 `EditorWindow` 클래스를 상속 받는다.

```c#
public class DataParser : EditorWindow
```

![img03](/assets/images/posts/Unity_Lecture_1/2023-03-22-my-unitylec1-post_13/3.png){: width="50%" height ="50%"}

그 후에 나는 위의 Extend Tools 처럼 메뉴 아이템에 추가를 하고 싶기 때문에 `[MenuItem("탭 이름 / 실제 이름")]` MenuItem Attribute를 사용한다.

```c#
public class DataParser : EditorWindow
{
  public static int s_readerCount;

  [MenuItem("Extend Tools/ParseExcelCSV")]
    public static void ParseExcelCSV()
    {
        Debug.Log("Parsing start");
        //ParseStartData();
    }

    //...
}
```

결과 적으로 위와 같이 코드를 작성했다.
따라서 위의 Extend Tools 탭을 누르면 ParseExcelCSV를 선택할 수 있다.  
그리고 선택했을 때, 연결된 ParseExcelCSV() 메서드가 호출 된다.

ParseExcelCSV 메서드의 내부에 주석 처리 되어 있는 ParseStartData 메서드를 볼 수 있는데, 앞으로 이 메서드를 작성할 것이다.

그 전에 먼저 위에 엑셀에 입력한 값과 매칭되는 Data 클래스를 작성해야한다.

StartData.cs를 Scripts 경로 안에 추가한다.

그리고 최상단에 int형 static 변수 `s_readerCount` 를 볼 수 있는데, 간단하게 엑셀에서 값을 불러올 경우 Key 즉 Cursor가 될 값이 필요하다.  
자세한 건 아래에서 설명할 것이다.

## StartData.cs

```c#
using System.Xml.Serialization;

public class StartData
{
   [XmlAttribute] public int ID;
   [XmlAttribute] public int maxHp;
   [XmlAttribute] public int maxMp;
   [XmlAttribute] public int maxDamage;
   [XmlAttribute] public int maxArmor;
   [XmlAttribute] public int defaultHp;
   [XmlAttribute] public int defaultMp;
   [XmlAttribute] public int defaultDamage;
   [XmlAttribute] public int defaultArmor;
   [XmlAttribute] public int gold;
}
```

위의 코드를 보면 우리는 Xml로 Serialize 할 예정이기 때문에, 네임 스페이스 `using System.Xml.Serialization` 가 필요하다.  
해당 네임 스페이스를 선언하면 `[XmlAttribute]` 라는 Attribute를 사용할 수 있는데, 이건 .NET 에서 XML 직렬화를 수행할 때 클래스의 멤버를 XML 특성으로 Serialize 하기 위해 사용하는 Attribute다.

이렇게 작성하면 StartData는 작성 끝이다.

## ParseStartData() 메서드

실질적으로 CSV를 읽어 들여서 XML로 뱉어내는 메서드이다.

```c#
static void ParseStartData()
{
  StartData startData = null;
  s_readerCount = -1;

  var lines = Resources.Load<TextAsset>($"Data/Excel/StartData").text.Split("\n");

  // 두번째 라인까지 값이 아니므로 스킵
  var rows = lines[2].Replace("\n", "").Split(',');
}
```

먼저 엑셀 값을 가져와서 변수에 담은 후 그것을 XML로 만들어주기 위해서 `startData` 지역 변수를 하나 선언했다.  
그리고 위에서 만든 커서 값에 -1 을 할당해 준다.  
왜 0으로 초기화하지 않고 -1로 했는지는 차차 설명하겠다.  

그 아래의 lines은 string[] 타입이다.  
코드를 순서대로 읽어보면 Resourse 폴더 내부 Data/Excel/StartData 경로에 있는 파일을 TextAsset 형식으로 받아온다.  
그리고 그 TextAsset의 text 값 즉 string 값을 Split을 사용해 `\n` 개행 문자를 기준으로 잘라서 나오는 결과물 string 배열을 변수에 할당한다.

그럼 그냥 TextAsset.text 값을 어떻게 생겼을까?

![img04](/assets/images/posts/Unity_Lecture_1/2023-03-22-my-unitylec1-post_13/4.png){: width="50%" height ="50%"}

위와 같이 행을 개행 문자를 사용해 구분한다는 것을 볼 수 있다.  
따라서 결과적으로 위에서 `/n`를 이용해 라인을 구분하는 것이다.

잠깐 여기서 행은 뭐고 열은 무엇인지 헷갈릴 수도 있으니 다 같이 따라 읽어보자.  
행거는 가로로 걸고, 열은 세로 줄에 맞춘다.

![understand1](https://media.giphy.com/media/3o7abKhOpu0NwenH3O/giphy.gif){: width="30%" height ="30%"}

라인을 구분했으니 이제 행에 있는 값을 읽어 들일 차례다.

```c#
var rows = lines[2].Replace("\n", "").Split(',');
```

그게 이 부분인데 `lines[2]` 부터 시작하는 이유는 첫번째, 두번째 라인은 값이 아닌 우리가 확인하기 위해서 넣은 구분 값이기 때문이다.   
Replace를 이용해 Enter를 없애버리고 `','` 를 기준으로 잘라서 문자열 배열로 반환해 변수에 넣는다.

![img05](/assets/images/posts/Unity_Lecture_1/2023-03-22-my-unitylec1-post_13/5.png){: width="30%" height ="30%"}

내가 만든 엑셀을 기준으로 `rows`엔 위와 같은 배열이 할당되었을 것이다.  
값을 불러왔으니 이제 우리가 만든 Data Class에 잘 담아 보자.  

```c#
startData = new StartData
{
    ID = ReadInt(rows),
    maxHp = ReadInt(rows),
    maxMp = ReadInt(rows),
    maxDamage = ReadInt(rows),
    maxArmor = ReadInt(rows),
    defaultHp = ReadInt(rows),
    defaultMp = ReadInt(rows),
    defaultDamage = ReadInt(rows),
    defaultArmor = ReadInt(rows),
    gold = ReadInt(rows),

};
```

위의 코드를 보면 ReadInt 라는 메서드를 사용한 것을 볼 수 있는데, 이제부터 위에서 왜 커서 변수인 `s_readerCount` 를 왜 만들었는지 설명할 것이다.

먼저 `ReadInt` 메서드는 아래와 같다.

```c#
static int ReadInt(string[] rows)
{
    ++s_readerCount;
    return int.Parse(rows[s_readerCount]);
}
```

rows 값의 배열을 인수로 받는 메서드인데, 본문을 보면 `++s_readerCount` 를 볼 수 있다.  
여기서 처음에 `s_readerCount = -1` 로 초기화를 했고 `++s_readerCount` 를 한 시점에서 `s_readerCount` 는 0이 된다.  
따라서 반환하는 부분의 `return int.Parse(rows[s_readerCount]);` 의 `rows[s_readerCount]` 번째는 `row[0]` 과 같다는 얘기다.

풀어서 얘기하면 `row[0]` 값을 `int.Parse` 를 사용해 반환 받은 int 값을 다시 반환한다.  
그리고 Id, maxHp, maxMp 순서대로 ReadInt를 호출하는 과정에서 `s_readerCount` 커서 변수는 1씩 계속 증가하면서 `s_readerCount` 번째 값을 Parsing 해 반환 해주게 된다.

![img06](/assets/images/posts/Unity_Lecture_1/2023-03-22-my-unitylec1-post_13/6.png){: width="30%" height ="30%"}

이걸 그림으로 설명하면 위와 같다.

이렇게 값을 할당하고 나서 XML로 변환을 한다.

```c#
var xml = ToXML(startData);
File.WriteAllText($"{Application.dataPath}/Resources/Data/StartData.xml", xml);
AssetDatabase.Refresh();
```

ToXML 이라는 메서드가 있다.

```c#
static string ToXML<T>(T obj)
{
    using ExtentedStringWriter sw = new(new StringBuilder(), Encoding.UTF8);
    var xmlSerializer = new XmlSerializer(typeof(T));
    xmlSerializer.Serialize(sw, obj);
    return sw.ToString();
}
```

이건 제네릭 메서드로 되어 있다.
왜냐하면 Data Class는 그 특징에 따라 종류가 다양할테고, 내가 정의한 Data Class 대로 XML을 추출하기 위해서이다.

using을 사용한 것은 리소스를 단 한번만 사용하면 되기 때문에 사용하고 나서 리소스를 시스템에 반환하기 위해서다.  
그리고 한정한 타입으로 XmlSerializer를 초기화하고, Serialize 한다.
Serialize 한 값을 반환한다.

[🔔Using에 대해서 궁금하다면??](http://127.0.0.1:4000/c%20sharp/my-csharp-post_1/)
{: .notice--warning}

```c#
sealed class ExtentedStringWriter : StringWriter
{
    readonly Encoding m_encoding;

    public ExtentedStringWriter(StringBuilder builder, Encoding encoding) : base(builder)
    {
        m_encoding = encoding;
    }
    public override Encoding Encoding => m_encoding;
}
```

결과적으로 using을 쓸 수 있었던 이유는 StringWriter가 TextWriter를 상속 받았고 그 TextWriter가 IDisposable을 상속 받았기 때문이다.

생성자를 보면 부모 클래스의 생성자를 상속받은 것을 볼 수 있고 아래와 같이 생겼다.

```c#
[__DynamicallyInvokable]
public StringWriter(StringBuilder sb)
  : this(sb, (IFormatProvider) CultureInfo.CurrentCulture)
{
}
```

이렇게 생성자를 상속을 받으면 자식 객체를 생성할 경우 부모의 생성자가 먼저 호출되고 자식의 생성자가 호출 된다.  
그리고 `Encoding` 프로퍼티를 override해서 생성자 호출과 동시에 받아온 인수로 할당한다.

```c#
var xml = ToXML(startData);
File.WriteAllText($"{Application.dataPath}/Resources/Data/StartData.xml", xml);
AssetDatabase.Refresh();
```

다시 돌아와서 xml을 만들어서 File 클래스의 내부 메서드 WriteAllText를 사용해 내가 설정한 경로에 xml 파일을 만든다.

마지막으로 `AssetDatabase.Refresh();` 를 호출하는데, 이건 에셋 파일의 변경 사항을 찾은 후 소스 에셋 데이터베이스를 업데이트하는 메서드이다.  
만약 탭에서 ParseExcelCSV를 선택했을 경우 `AssetDatabase.Refresh();` 이게 없다면 에디터에서 생성된 xml 파일을 에디터에 갱신하는데 시간이 걸리지만, 호출할 경우 바로 갱신되서 에디터에서 파일을 확인할 수 있다.

## 보너스

```c#
static string ReadString(string[] rows)
{
    ++s_readerCount;
    return rows[s_readerCount];
}

static float ReadFloat(string[] rows)
{
    ++s_readerCount;
    return float.Parse(rows[s_readerCount]);
}

public static T ReadEnum<T>(string[] rows)
{
    ++s_readerCount;
    return (T)System.Enum.Parse(typeof(T), rows[s_readerCount], ignoreCase: true);
}
```

혹시 까먹을까봐 기록해놓는다.

# 📢 오늘의 한마디
<hr style="width:100%" />

<strong style="color:Yellow; font-size:15pt">이 정도면 다음에 봤을 때 바로 기억하겠지?</strong>

<br>

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr>

