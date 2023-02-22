---
title:  "[Unity Doc] 5. Debug.log의 진실 알고 있니?"
excerpt: "내가 볼려고 만든 유니티 잡학지식 문서"

categories:
  - UnityDocs
tags:
  - [UnityDocs]
toc: true
toc_sticky: true
date: 2023-02-21
last_modified_at: 2023-02-22
---

NK Studio의 유니티 스페셜 테크닉 유투브 채널 **유니티 Debug.Log.. 최적화가 필요해보이는데? 수술방 욺겨!! (Unity Debug Class Optimization Tutorial)** 영상을 보고 정리한 게시글입니다.
<br>
[🔔영상 보러가기 클릭](https://youtu.be/pmlU7l2-kYI)
{: .notice--warning}

<br>

# 💁‍♂️ Debug.Log는 다 알고 있겠지?
<hr style="width:100%" />

![Iknow](https://media.giphy.com/media/ggDwfEfxHkH6ofW6gX/giphy.gif){: width="30%" height="30%"}

유니티 콘솔에 로그를 찍기 위해서 사용하는 유니티 내장 메서드이고, 나도 정말 많이 쓰는 메서드야.

```c#
void Start()
{
    Debug.Log("Hello Log");
    Debug.LogFormat("Hello LogFormat");
}
```

대표적으로 위의 두 가지라고 생각해.

위의 코드를 실행하면, 

![img1](/assets/images/posts/UnityDocs/2023-02-21-my-unitydoc-post_5/1.png){: width="50%" height="50%"}

위와 같은 로그가 출력되겠지?
하지만 게임 뷰에서는 아무런 변화도 일어나지 않고 있어

## 🔖 이제부터 밝혀지는 나만 몰랐던 Debug.Log의 진실

클래스 명이 Debug 이다 보니 사람들이 많이 착각하는 게 있어.

그건 게임이 실제로 빌드 됐을 때, Debug 클래스는 알아서 분리되어 빌드 된 게임에 실제 영향을 주지 않을 것이다라는 생각이야.

이제부터 과연 그런지, Debug가 과연 게임에 영향을 미칠 수 있는지에 대해 알아볼거야. 

## 🔖 실험 시작

먼저 아래와 같이 코드를 준비한다.

```c#
public class TestLog : MonoBehaviour
{
    public TextMeshProUGUI _experTMP;
        
    void Start()
    {
        var num = 0; // 정수형 변수 0 초기화
        
        Debug.Log(++num); // 후증가
        _experTMP.text = num.ToString(); //빌드 된 게임 뷰의 텍스트에 1이 나오면 Debug.Log안의 연산도 실행된다는 뜻
    }
}
```

## 🔖 실험 결과

![img1](/assets/images/posts/UnityDocs/2023-02-21-my-unitydoc-post_5/2.png){: width="70%" height="70%"}

오른쪽이 빌드된 결과야.  
1로 변하는 걸 보니 Debug.Log 내에서 증감연산자를 썼을 때, 빌드된 게임에서도 적용이 된 다는 것을 확인할 수 있어.

여기서 도출되는 사실은, 무분별한 Debug.Log는 빈도수에 비례해서 직접적으로 게임에 영향을 줄 수 있다는 점이야. 

따라서 테스트를 위해 로그를 쓰고 싶다면 에디터에서는 보여주고, 빌드된 게임에서는 로그를 안나오게 하는게 좋겠지?  
(테스트 버전을 따로 뽑는 것도 좋은 방법이 될 수 있다고 생각함.)

## 🔖 최적화 방법

먼저 전처리기에 대한 개념을 알아야해.

[🔔 유니티 공식 문서 - 전처리기](https://docs.unity3d.com/kr/530/Manual/PlatformDependentCompilation.html)
{: .notice--warning}

그리고 `[System.Diagnostics.Conditional("조건부 컴파일 기호")]` 이라는 attribute를 사용할거야.

[🔔 MS놈들의 공식 문서 - ConditionalAttribute Class](https://learn.microsoft.com/ko-kr/dotnet/api/system.diagnostics.conditionalattribute?view=net-7.0)
{: .notice--warning}

위의 attribute는 "지정된 조건부 컴파일 기호가 정의되어 있지 않으면 메서드 호출 또는 특성이 무시되어야 함을 컴파일러에 알립니다." 와 같은 일을 하는 녀석이야.

따라서 내가 커스텀한 조건부 컴파일 기호를 넣을 수도 있고, 이미 정의된 지시어를 사용할 수도 있어.

잘 이해가 안가더라도 쭉 설명을 들으면 이해가 갈거야.

우리는 이제 Debug 라는 C# 클래스 파일을 하나 만들 거야.   
기존의 Debug를 랩핑해서 쓰기 위해서지.

```c#
public static class Debug
{
    [System.Diagnostics.Conditional("UNITY_EDITOR")]
    public static void Log(object msg) 
        => UnityEngine.Debug.Log(msg);
}
```

그리고 코드는 위와 같이 간단해.  
코드를 짧게 리뷰해보자면, "현재 Condition이 UNITY_EDITOR 라면 메서드를 실행해라." 라는 코드야.

실제로 이 클래스를 추가하고 아까 우리가 만들었던 테스트 코드를 실행해보면

![img3](/assets/images/posts/UnityDocs/2023-02-21-my-unitydoc-post_5/3.png){: width="70%" height="70%"}

위와 같이 에디터에서는 Debug.Log가 실행됐기 때문에 1이 출력되고, 빌드된 게임은 Debug.Log가 실행되지 않았기 때문에 0이 출력되는 것을 확인할 수 있어.

이 방법을 이용해서 여러 디버그를 위한 코드들을 빌드된 게임에 추가되지 않게 할 수 있어.

# 🥱 끝 마치며
<hr style="width:100%" />

![really](https://media.giphy.com/media/oKdjMdWXl9ys8/giphy.gif){: width="30%" height="30%"}

<strong style="color:limegreen; font-size:15pt">이건 정말 나만 몰랐던 사실이었을까?</strong>

<br>

<strong style="color:yellow; font-size:100pt;">끝</strong>


<hr style="width:100%" />
<br>

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr>

