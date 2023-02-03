---
title:  "[Unity Doc] 2. JsonUtility vs Newtonsoft.Json"
excerpt: "내가 볼려고 만든 유니티 잡학지식 문서"

categories:
  - UnityDocs
tags:
  - [UnityDocs]
toc: true
toc_sticky: true
date: 2023-02-03 
last_modified_at: 2023-02-03
---
<br>

# 👶 이 두개가 뭔지도 모르겠어요. 응애
<hr style="width:100%" />

![waa](https://media.giphy.com/media/TL2Yr3ioe78tO/giphy.gif){: width="30%" height="30%"}

핑프 같은 녀석 JsonUtility, Newtonsoft.Json은 <strong style="color:salmon; font-size:13pt">Json은 둘 다 유니티 환경에서 Json 데이터를 사용할 수 있게 해주는 Json 라이브러리</strong>란다.

# 💁‍♂️ Json이 뭔데요?
<hr style="width:100%" />

Json은 말이죠?  

웹이나 네트워크에서 서버와 클라이언트 사이에서 데이터를 주고 받을 때 사용하는 개방형 표준 포맷인데, 텍스트를 사용하기 때문에 사람이 이해하기 쉽다는 장점이 있어.

![fast](https://media.giphy.com/media/3ornjIhZGFWpbcGMAU/giphy.gif){: width="30%" height="30%"}

그리고 <strong style="color:Yellow; font-size:13pt">Json은</strong> 텍스트(문자열)를 전송 받은 후에 해당 텍스트(문자열)를 바로 파싱하기 떄문에 XML에 비교해서 <strong style="color:Yellow; font-size:13pt">데이터 읽는 속도가 무진장 빠르지</strong>.

이 엄청난 <strong style="color:limegreen; font-size:13pt">Json</strong>으로 게임을 개발할 때 <strong style="color:limegreen; font-size:13pt">게임에 필요한 데이터를 받아오거나, 데이터를 저장하거나, 설정 등을 저장하는 방식으로 사용가능</strong>해.

## 🔖 Json의 기본 구조

아래는 Json의 기본적인 구조야

```
{
    "name":"Victor",
    "lv":"999",
    "hp":"999",
    "mp":"999",
    "power":"999",
    "Inventory":
    [
        "Great Sword",
        "Full Plate Armor",
        "Necklace"
        "Ring",
    ]
}
```

- Json 데이터는 C#의 Dictionary 처럼 Key, Value 타입으로 되어있어.  
값으로 들어가는 데이터는 배열 데이터, 객체, 객체안에 객체 그리고 너가 전부 기억할 수 있다면 객체안에 객체안에 객체 무한반복도 가능해.

- Json 데이터는 정수 타입, 실수 타입, 문자열 타입, 불 타입, null 타입을 지원해.  
마지막으로 {}는 객체를 의미하고, []는 순서가 있는 배열을 뜻하지.

- XML과는 다르게 Json은 주석을 지원하지 않기 때문에, 오로지 키의 이름만으로 그 값을 추측해야하고 따라서 작명에 신경을 많이 써야해.

## 🔖 Json의 단점

<strong style="color:limegreen; font-size:13pt">Json은</strong> 아주 예민해서 작은 문법 오류에도 <strong style="color:limegreen; font-size:13pt">Very 민감</strong>한게 단점이야.  

중간에, 대괄호, 콜론, 쉼표가 하나라도 빠지면 당연히 Json 파일이 깨져버리고 읽을 수 없게돼.  
따라서 Json 데이터가 유효한지 검사해주는 웹페이지들이 많아.

그런 이유로 <strong style="color:Yellow; font-size:13pt">Json 데이터 파일을 작성하고 한번씩 꼭 검사를 해보는 게 좋다</strong>고 생각해.  

물론 툴을 만들어서 쓰는게 가장 Best 겠지?

[🔔Json 검사기](https://kr.piliapp.com/json/validator/)
{: .notice--warning}

<br>

# 😩 에이 기획자들을 위해서는 XML이 최고인거 아니에요?
<hr style="width:100%" />

대다수의 사람들은 <strong style="color:Yellow; font-size:13pt">Json이 XML보다 가독성이 좋다</strong>고 말해.

내 개인적인 생각으로는 XML과 Json 가독성을 따진다고 할 때, 사실 이 내용은 사람마다 느끼는 점이 달라서 뭐가 좋다 뭐가 나쁘다 단정지어 말하긴 어렵다 생각하고 있어.

자 그럼 XML의 구조를 한번 볼까?

```xml
<?xml version="1.0" encoding="UTF-8"?>
<shop city="서울" type="무기상점">
    <food>
        <name>롱소드</name>
        <sort>무기</sort>
        <cost>1000</cost>
    </food>

    <food>
        <name>체인 메일</name>
        <sort>갑옷</sort>
        <cost>3000</cost>
    </food>
</shop>
```

Json의 기본구조는 위에 있으니까 비교를 해보자고

넌 어떻게 생각해?  

난 XML이 내용이 많아지면 많아질수록 길이는 길어지지만 더 가독성이 좋다고 생각해.  
내가 하는 게임이 림월드라서 그런지 모르겠지만(모드폴더에 XML이 산더미 만큼 있어서) 사실상 조금 더 직관적이라고 생각하거든.  

Json은 뭐랄까 흐물흐물 거리는 젤리같다면, XML은 단단한 두부같은 느낌이야.  

<strong style="color:orange; font-size:15pt">하지만 이건 확실히 말할 수 있어 XML은 데이터를 저장하거나 가져올 때 Parsing 하는 과정이 까다로워.</strong>  

![relaxing](https://media.giphy.com/media/A6aHBCFqlE0Rq/giphy.gif){: width="30%" height="30%"}

그에 반해 <strong style="color:Yellow; font-size:13pt">Json은 직렬화(Serialize)와 역직렬화(Deserialize) 메서드를 통해 Data -> Json, Json -> Data로 편하게 변환할 수 있다는 장점</strong>을 가지고 있지.

<br>

# 🎯 JsonUtility
<hr style="width:100%" />

JsonUtility 는 앞서 말한대로 유니티에서 기본 제공하는 Json 라이브러리야.  
그런 만큼 정말 필요한 기능만 제공하기 때문에, 더 범용성있게 사용하고 싶다면 NewtonSoft Json 라이브러리를 쓰는 것을 추천할게.  

>JsonUtility의 치명적인 단점은 기본 데이터 타입, 배열, 리스트에 대한 직렬화(Serialize)만 지원해.  

백문이 불여일견, 우리 플머잖아? 정말 그런지 확인해보자고,  
먼저 나는 유니티 빈 프로젝트를 아무거나 만든 후 JsonUtilitySample 이라는 스크립트를 만들었어.

대충 게임오브젝트하나 만들어서 컴포넌트에 추가해버려.

그리고 나서는 Json 데이터로 만들 클래스를 하나 작성할 거야.

## 🔖 JsonUtility 샘플 코드

```c#
public class JsonData
{
    public string name;
    public int lv;
    public float hp;
    public float mp;
    public float power;
    public List<string> Inventory = new();
    public Dictionary<int, string> Equipment = new();
    public Quest quest;
    
    public JsonData()
    {
        name = "Victor";
        lv = 999;
        hp = 999f;
        mp = 999f;
        power = 999f;
        
        Inventory.Add("그레이트 소드");
        Inventory.Add("풀 플레이트 아머");
        Inventory.Add("체인 메일");
        Inventory.Add("실버 링");
        
        Equipment.Add(0, "투구");
        Equipment.Add(1, "무기");
        Equipment.Add(2, "갑옷");
        Equipment.Add(3, "신발");
        Equipment.Add(4, "링");

        quest = new Quest(0, "고블린을 소탕하세요");
    }

    public void Print()
    {
        Debug.Log($"name : {name}");
        Debug.Log($"lv : {lv}");
        Debug.Log($"hp : {hp}");
        Debug.Log($"mp : {mp}");
        Debug.Log($"power : {power}");
        
        foreach (var item in Inventory)
        {
            Debug.Log($"Inventory List :: {item}");
        }
        
        foreach (var group in Equipment)
        {
            Debug.Log($"Equipment Dictionary :: Key [{group.Key}] , Value [{group.Value}]");
        }
        
        Debug.Log($"Quest :: index [{quest.Index}], reward [{quest.Reward}]");
    }

    public class Quest
    {
        int _index;
        string _reward;

        public int Index { get => _index; set => _index = value; }
        public string Reward { get => _reward; set => _reward = value; }

        public Quest(int index, string reward)
        {
            _index = index;
            _reward = reward;
        }
    }
}
```

이렇게 하나 만든 다음에  

메인 클래스를 작성했어.

```c#
public class JsonUtilitySample : MonoBehaviour
{
    void Start()
    {
        var data1 = new JsonData();
        string json = JsonUtility.ToJson(data1);
        Debug.Log(json);

        var data2 = JsonUtility.FromJson<JsonData>(json);
        data2.Print();
    }
}
```

이렇게 모든 코드를 작성했고,

```c#
string json = JsonUtility.ToJson(data1);
```

위의 코드가 data -> Json으로 직렬화 해주는 코드.

```c#
var data2 = JsonUtility.FromJson<JsonData>(json);
```

이 코드가 Json -> data로 역직렬화를 해주는 코드야. 

결과적으로 이 코드를 실행하면

![image1](/assets/images/posts/UnityDocs/2023-02-02-my-unitydoc-post_2/1.png){: width="100%" height="100%"}

우리가 만든 Dictionary와 Quest Class를 쏙 빼먹은 걸 확인할 수 있어.

클래스는 아래와 같이 위에 어트리뷰트로 `[System.Serializable]` 이걸 붙여주면 해결이 돼.  
하지만 Dictionary는 무슨 짓을 해도 지원하지 않아.

```c#
public class JsonData
{
  //...
  [System.Serializable]
  public class Quest
  {
    //...
  }
}
```

<strong style="color:salmon; font-size:13pt">만약 Dictionary도 Json 데이터로 사용하고 싶다면 Newtonsoft Json 라이브러리를 사용해야 해.</strong>

## 🔖 JsonUtility 만이 가지고 있는 매력

Newtonsoft Json 라이브러리는 유니티 내장 클래스를 직렬화(Serialize) 할 때(특히 Vector3) normalized 프로퍼티가 문제를 일으켜 에러가 나지.

이를 해결한다고 해도 Vector3의 좌표 값만이 아닌 <strong style="color:Crimson; font-size:13pt">Vector3가 가지고 있는 모든 값(normalized, magnitude)을 직렬화(Serialize) 해서 쓸모없는 정보를 전부 가지고 오는 결과</strong>를 초래해.

하지만 JsonUtility를 사용하면 필요한 좌표 값만 가져올 수 있게 되는데 이것도 장점이야.

또한 JsonUtility 사용시 Mono를 상속받는 클래스 오브젝트를 직렬화(Serialize) 할 수 있고,

아래는 샘플 코드야.

```c#
public class TestClass : MonoBehaviour
{
    public string name = "Lego";
    public int index = 0;
    public Vector3 tr = new(2,4,6);
}

public class JsonUtilitySample : MonoBehaviour
{
    void Start()
    {
        var go = new GameObject();
        go.AddComponent<TestClass>();
        var json = JsonUtility.ToJson(go.GetComponent<TestClass>());
        
        Debug.Log(json);
    }
}
```

이렇게 정상적으로 작동해서 JsonUtilitySample 클래스에서 Start문 본문 안에 `Debug.Log(json);` 이 실행되면

![image2](/assets/images/posts/UnityDocs/2023-02-02-my-unitydoc-post_2/2.png){: width="100%" height="100%"}

Mono를 상속 받는 클래스 오브젝트가 제대로 나온 것을 확인 할 수 있어.

주의할 점은 GameObject를 ToJson 메서드 인수로 넣는게 아닌, GetComponent로 직접 가져온 클래스 오브젝트로 직렬화를 진행해야해.

![image3](/assets/images/posts/UnityDocs/2023-02-02-my-unitydoc-post_2/3.png){: width="100%" height="100%"}

그리고 위와 같이 이어서 역직렬화(Deserialize)를 하려고 하면 아래와 같은 에러가 나게 돼.

![image4](/assets/images/posts/UnityDocs/2023-02-02-my-unitydoc-post_2/4.png){: width="100%" height="100%"}

에러의 내용은 Json을 'TestClass' 유형의 새 인스턴스로 역직렬화 할 수 없다는 내용이야.

이 에러가 난 이유는 `JsonUtility.FromJson<TestClass>(json);` 이 부분에서 FromJson은 기본적으로 Json 데이터를 역직렬화하는 과정에서 새로운 객체를 생성해.

하지만 우리가 변환한 클래스는 <strong style="color:limegreen; font-size:13pt">Mono를 상속받았고, 그 뜻은 해당 클래스를 컴포넌트로 만든다는 것을 의미하게 되기 때문에 결론적으로 new 키워드를 사용해 객체를 생성할 수 없다</strong>는 거야.  
(컴포넌트는 GameObejct에 붙어 동작하기 때문)

따라서 오브젝트에서 Json 데이터로는 만들어지지만, Json 데이터에서 다시 오브젝트로 만들려고 할 때, 클래스가 Mono를 상속받았기 때문에 객체가 만들어 지지 않아 나오는 에러로 보면 될거야.

    해외 포럼 같은 곳을 보면 Mono를 붙이지 않는 data 클래스들에 JsonUtility를 사용하는 것을 권장해

>The JsonUtility can only be used for data classes.
 
하지만 "권장" 한다는 거지, Mono를 상속받는 클래스에 쓰지 말라는 내용은 없어.  

만약 Mono를 상속받는 클래스 객체 생성 문제를 해결하고 싶다면,

```c#
public class JsonUtilitySample : MonoBehaviour
{
  void Start()
    {
        var go = new GameObject();
        var script = go.AddComponent<TestClass>();
        script.index = 100;
        script.tr = new Vector3(5, 10, 20);
        
        var json = JsonUtility.ToJson(go.GetComponent<TestClass>());
        
        Debug.Log(json);

        var go1 = new GameObject();
        JsonUtility.FromJsonOverwrite(json,go1.AddComponent<TestClass>());
        
        Debug.Log(go1.GetComponent<TestClass>().index);
        Debug.Log($"{go1.GetComponent<TestClass>().tr.ToString("N1")}");
    }
}
```

`JsonUtility.FromJsonOverwrite(json,go1.AddComponent<TestClass>());` 코드의 `FromJsonOverwrite` 라는 메서드를 사용하면 돼.

이건 새로 객체를 생성하는 게 아닌, <strong style="color:limegreen; font-size:13pt">원래 있는 객체에 Json 데이터만 덮어쓰기를 해주는 메서드</strong>야.

사실 아까 Mono를 상속받았기 때문에, new 키워드를 사용못해서 객체를 못만든 걸 코드로 수동으로 객체를 만들어 준거야. 
그리고 그 위에 json 데이터를 덮어쓰기한 꼴이기 때문에 이건 권장하는게 아닌 걸 딱 봐도 알 수 있어.


![alert](https://media.giphy.com/media/elHDhmQ4GDHIUAWPNs/giphy.gif){: width="20%" height="20%"}

뭐 어쩔수 없는 상황이라면 모르겠지만, <strong style="color:Yellow; font-size:15pt">되도록이면 Mono를 상속받지 않는 Data Classes에 쓰는게 좋을 거라고</strong> 생각해.

<br>

# 🎯 Newtonsoft Json
<hr style="width:100%" />

외부 라이브러리기 때문에 먼저 다운로드를 해야해.

[🔔Newtonsoft.Json 받으러 가기](https://github.com/JamesNK/Newtonsoft.Json/releases)
{: .notice--warning}

링크를 들어가면 이것 저것 보이는데 하단에 Json130r2.zip 파일을 다운받자.  
(만약 버전이 바뀐다고 해도 JsonXXXrX.zip 를 받으면 됨)

![image5](/assets/images/posts/UnityDocs/2023-02-02-my-unitydoc-post_2/5.png){: width="70%" height="70%"}

다운이 완료 됐다면 압축을 풀자.

그리고 폴더 내부로 들어가서 Bin 폴더 내부로 들어가면,

![image6](/assets/images/posts/UnityDocs/2023-02-02-my-unitydoc-post_2/6.png){: width="50%" height="50%"}

위와 같이 여러가지 폴더가 보이는데 net45 폴더를 더블 클릭해서 들어가자.

![image7](/assets/images/posts/UnityDocs/2023-02-02-my-unitydoc-post_2/7.png){: width="50%" height="50%"}

    그럼 3가지 파일이 보이는데 우리가 사용할 파일은 아래의 Newtonsoft.Json.dll 이야.

![image8](/assets/images/posts/UnityDocs/2023-02-02-my-unitydoc-post_2/8.png){: width="30%" height="30%"}

그리고 유니티로 돌아가서 아래와 같이 Asset 경로에 Plugins 라는 폴더를 추가하자

![image9](/assets/images/posts/UnityDocs/2023-02-02-my-unitydoc-post_2/9.png){: width="30%" height="30%"}

거기에 아까 Newtonsoft.Json.dll 파일을 드래그 앤 드롭해서 넣어주자.

그리고 유의할 점이 있어.

Newtonsoft Json은 .NET 4.x 부터 사용할 수 있는데, 유니티의 기본 설정은 .NET 2.0이야.
그래서 유니티로 돌아가서 

    File 탭> Build Settings> Player Settings> Player> Other Settings> Configuration 

아마 .NET Standard 2.1으로 설정되어 있을 거야.
그걸 .NET Framework으로 변경해줘

4.0 이라고 안쓰여있어도 괜찮아 버전에 따라 다르게 나오는 것 같거든

[🔔Unity에서 .NET 4.X 사용 MS 문서](https://learn.microsoft.com/ko-kr/visualstudio/gamedev/unity/unity-scripting-upgrade)
{: .notice--warning}

설정을 다 했다면 NewtonsoftSample 이라는 스크립트를 하나 만들자.  
그리고 Newtonsoft Json 라이브러리를 사용하려면 네임스페이스를 선언해줘야해

![image10](/assets/images/posts/UnityDocs/2023-02-02-my-unitydoc-post_2/10.png){: width="50%" height="50%"}

그럼 이제 정말 Newtonsoft Json 라이브러리를 사용할 준비가 끝난거야.

이제 Json 데이터를 테스트할 클래스를 하나 만들자.
```c#
public class JsonDataClass
{
    public int i;
    public float f;
    public bool b;
    public string str;
    public int[] iArray;
    public List<int> iList = new();
    public Dictionary<int, string> fDictionary = new();
    public IntVector2 iVector;

    public JsonDataClass()
    {
        i = 10;
        f = 99.9f;
        b = true;
        str = "JSON Test String";
        iArray = new int[] { 1, 2, 3, 4, 5, 6, 7 };

        for (var j = 0; j < 5; j++)
            iList.Add(j * 2);
        
        fDictionary.Add(0, "빨강");
        fDictionary.Add(1, "노랑");
        fDictionary.Add(2, "파랑");

        iVector = new IntVector2(3, 2);
    }

    public void Print()
    {
        Debug.Log($"i = {i}");
        Debug.Log($"f = {f}");
        Debug.Log($"b = {b}");
        Debug.Log($"str = {str}");
        
        foreach (var data in iArray)
            Debug.Log($"iArray :: {data}");
        
        foreach (var data in iList)
            Debug.Log($"iList :: {data}");
        
        foreach (var data in fDictionary)
            Debug.Log($"fDictionary :: Key = {data.Key}, Value = {data.Value}");
        
        Debug.Log($"iVector :: Key = {iVector._x}, Value = {iVector._y}");
    }

    public class IntVector2
    {
        public int _x;
        public int _y;

        public IntVector2(int x, int y) { _x = x; _y = y; }
    }
}
```

그리고 메인 클래스를 작성

```c#
public class NewtonsoftSample : MonoBehaviour
{
    void Start()
    {
        var jTest1 = new JsonDataClass();
        var json = JsonConvert.SerializeObject(jTest1);
        Debug.Log(json);
    }
}
```

제대로 작성했다면 실행하면 아래와 같이 출력될거야.

![image11](/assets/images/posts/UnityDocs/2023-02-02-my-unitydoc-post_2/11.png){: width="50%" height="50%"}

아까 JsonUtility 와는 달리 모든 타입들이 Json 데이터로 변환 후 잘 나오는 것을 확인할 수 있어.
그리고 이제 Json -> Data를 해보자

```c#
public class NewtonsoftSample : MonoBehaviour
{
    void Start()
    {
        var jTest1 = new JsonDataClass();
        var json = JsonConvert.SerializeObject(jTest1);
        Debug.Log(json);

        var jTest2 = JsonConvert.DeserializeObject<JsonDataClass>(json);
        jTest2.Print();
    }
}
```

`var jTest2 = JsonConvert.DeserializeObject<JsonDataClass>(json);` 코드로 역직렬화를 하고 제대로 됐는지 확인하기 위해서 모든 요소를 출력하는 Print 메서드를 호출했어.

![image12](/assets/images/posts/UnityDocs/2023-02-02-my-unitydoc-post_2/12.png){: width="50%" height="50%"}

출력이 잘되는 것으로 봐서 변환이 잘 됐다는 것을 방증하지.

## 🔖 Newtonsoft Json 사용시 주의해야 할 점

Newtonsoft Json 라이브러리 사용 시 주의해야할 점이 있는데, Mono를 상속하는 클래스를 Newtonsoft Json 라이브러리를 이용해 직렬화(Serialize)를 시도할 경우, 자기참조 에러가 발생하게 돼.

![image13](/assets/images/posts/UnityDocs/2023-02-02-my-unitydoc-post_2/13.png){: width="50%" height="50%"}

이 것은 위의 이미지 처럼 gameobject 에서 gameobejct 를 계속해서 호출할 수 있어서 생기는 문제야.  
따라서 에러를 해결할 수는 있지만, 그것으로 인해 또 다른 사이드 이펙트가 연쇄적으로 발생하기 때문에, 꼭 Mono를 상속받는 클래스를 직렬화 하고 싶다면 이전에 다룬 JsonUtility를 사용하는 걸 추천할게.  

이어서 Newtonsoft Json 라이브러리는 유니티 내장 클래스인 Vector3의 직렬화를 지원하지 않고,  
어떻게 방법을 쓰면 가능하긴 한데 이것도 사이드 이펙트가 존재하는 방법이라 추천하지 않아.

아니면 아에 다른 방법으로 Vector3 각각 좌표의 값을 담을 Data 클래스를 정의한 뒤, 작성한 클래스를 직렬화하는 방법도 있어.

# 🥱 끝 마치며
<hr style="width:100%" />

![balance](https://media.giphy.com/media/xT8qBit7YomT80d0M8/giphy.gif){: width="30%" height="30%"}

JsonUtility, Newtonsoft Json 라이브러리는 뭐가 더 우월하다라고 말할 수 없고, 각각의 장단점이 명확해.

따라서 내가 만약 Dictionary 타입을 주로 많이 쓴다고 한다면 무조건 Newtonsoft Json을 선택하는게 좋고, 딱히 Dictionary가 필요없고 배열과 리스트로도 충분하다라고 한다면 컴팩트한 JsonUtility를 사용하는 게 좋을 것 같다고 생각해.

<strong style="color:orange; font-size:20pt">목적에 따라 알아서 골라 쓰면 좋을 것 같아.</strong>

쓰다보니 엄청 길어졌네;

<br>

# 🔎 참고한 내용
<hr style="width:100%" />

[🔔 베르님의 블로그](https://wergia.tistory.com/164)<br>
{: .notice--warning}

<br>
<strong style="color:yellow; font-size:100pt;">끝</strong>


<hr style="width:100%" />
<br>

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr>

