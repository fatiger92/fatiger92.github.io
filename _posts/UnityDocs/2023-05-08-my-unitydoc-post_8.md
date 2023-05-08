---
title:  "[Unity Doc] 8. CanvasGroup 파헤치기"
excerpt: "내가 볼려고 만든 유니티 잡학지식 문서"

categories:
  - UnityDocs
tags:
  - [UnityDocs]
toc: true
toc_sticky: true
date: 2023-05-08
last_modified_at: 2023-05-08
---

# 🤔 왜 갑자기 이런 생각을 했는지
<hr style="width:100%" />

초심으로 돌아가자 라는 느낌?

무튼 코딩을 하다가 Dotween으로 FadeOut하고 싶은데 Copilot이 CanvasGroup의 속성 Alpha를 줄이는 코드를 추천해주길래 Canvas와 CanvasGroup의 차이점에 대해서 한번 정리해두고 싶어서 글을 쓰고 있음.

## 🔖 Canvas vs CanvasGroup

[🚀유니티 공식문서 CanvasGroup 바로가기](https://docs.unity3d.com/kr/2021.3/Manual/class-CanvasGroup.html)  
{: .notice--warning}


### 일반적으로 CanvasGroup이 사용되는 경우

- 캔버스 그룹을 창의 게임 오브젝트에 추가하고 알파 프로퍼티를 조절하여 전체 창이 페이드 인/아웃 되도록 한다.  

- 캔버스 그룹을 부모 게임 오브젝트에 추가하고 인터랙터블 프로퍼티를 거짓으로 설정하여 컨트롤 설정 전체가 상호작용 불가(“그레이 아웃”)하도록 한다. 

- 캔버스 그룹 컴포넌트를 해당 요소 또는 그 부모 중 하나에 배치하고 블록 레이캐스트 프로퍼티를 거짓으로 설정하여 한 개 이상의 UI 요소가 마우스 이벤트를 차단하지 못하도록 한다.

![img1](/assets/images/posts/UnityDocs/2023-05-08-my-unitydoc-post_8/1.png){: width="50%" height="50%"}

위의 이미지와 같이 Panel에 Canvas Group 컴포넌트를 추가해서 사용한다.
그러면 Panel의 하위 오브젝트를 포함한 Panel의 Alpha 값을 한번에 관리할 수 있다.

```c#
public void Start()
{
    DotweenFadeOutAnimation();
}
public void DotweenFadeOutAnimation()
{
    var canvasGroup = transform.Find("Panel").GetComponent<CanvasGroup>();
    canvasGroup.DOFade(0, 2f).OnComplete(() => { canvasGroup.alpha = 0; });
}
```
위의 코드와 같이 CanvasGroup에 접근하여 alpha 값을 조정하는 방식으로 Fade Out 애니메이션을 구현한다.  

[🚀베르의 프로그래밍 노트 - Canvas Group 사용법](https://wergia.tistory.com/177)  
{: .notice--warning}

위의 링크글에서 디아블로의 맵을 예시로 들어서 설명한다.  
그 예시는 맵을 띄워도 마우스로 인한 캐릭터 이동을 막지 않는 기능인데, 레이캐스트 블록 옵션 On/Off로 해당 기능을 구현할 수 있다. 

<br>

# 🥱 끝 마치며
<hr style="width:100%" />

Canvas를 한꺼번에 관리하기 위한 사실상 Alpha 속성을 통해, Fade 애니메이션을 사용하기 위함이 주된 목적이 되는 Component다.

<br>
<strong style="color:yellow; font-size:100pt;">끝</strong>


<hr style="width:100%" />
<br>

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr>

