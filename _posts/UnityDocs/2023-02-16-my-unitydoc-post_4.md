---
title:  "[Unity Doc] 4. 메모리 최적화를 위한 에셋 관리 (1/2)"
excerpt: "내가 볼려고 만든 유니티 잡학지식 문서"

categories:
  - UnityDocs
tags:
  - [UnityDocs]
toc: true
toc_sticky: true
date: 2023-02-16
last_modified_at: 2023-02-28
---

Unity Korea 유투브 채널 **2월 알쓸유잡 : 메모리 최적화를 위한 에셋 관리** 영상을 보고 정리한 게시글입니다.
<br>
[🔔영상 보러가기 클릭](https://www.youtube.com/live/52ehLUfk3DQ?feature=share)
{: .notice--warning}

<br>

# 💁‍♂️ 메모리 최적화를 위한 에셋 관리
<hr style="width:100%" />

![img1](/assets/images/posts/UnityDocs/2023-02-16-my-unitydoc-post_4/1.png){: width="50%" height="50%"}

## 🔖 PC 장치의 가상 메모리 시스템
- 실제 물리 메모리 < 게임에서 사용할 수 있는 메모리
- 저장장치(HDD/SSD)가 물리 메모리를 보조 (스왑 페이지 / 페이지 파일)
- 시스템 메모리가 그래픽 메모리를 보조 (Grephics Library)


## 🔖 모바일 장치의 가상 메모리 시스템

![img2](/assets/images/posts/UnityDocs/2023-02-16-my-unitydoc-post_4/2.png){: width="70%" height="70%"}

- PC: 실제 물리 메모리 < 게임에서 사용할 수 있는 메모리
- 모바일: 게임에서 사용할 수 있는 메모리 <<<< 실제 물리 메모리
- UMA(Unified Memory Architecture)
  - CPU GPU 물리적인 메모리 구분이 없음
- (전통적인) 스왑페이지 없음
  - NAND flash 수명
  - 대역폭
  - 발열
- Compressed memory
- LMK(Low Memory Killer)

특히 모바일 게임을 만들 때, 메모리가 더욱 중요한데 왜냐하면 PC는 가상 메모리 기능을 쓰면 실제 물리 메모리보다 더 많은 메모리를 사용할 수 있기 때문이라고해.

모바일에서도 가상 메모리 기능이 있긴 하나, PC 처럼 실제 메모리를 확장시켜 주지 않아.

모바일에서는 실제 메모리에서 운영체제와 다양한 어플리케이션이 구동되어야 하고 여러가지 작업을 처리해야하기 때문에 굉장히 메모리가 협소하지.

그리고 만약 특정 모바일 게임이나 앱이 메모리를 지나치게 많이 사용할 경우 운영체제에서 강제로 종료시켜버려(크래시).

그래서 모바일 게임일 수록 메모리관리에 신경을 써야한다고 해.

## 🔖 Unity에서 제공하는 Profiler Memory Module

모바일 기기와 연결해서 현재 사용하고 있는 메모리를 실시간으로 확인할 수 있어.
(Assets, Scene Memory, Scripts 등)

![img3](/assets/images/posts/UnityDocs/2023-02-16-my-unitydoc-post_4/3.png){: width="100%" height="100%"}

    Window > Analysis > Profiler

위의 이미지와 같은 경로에 Profiler가 있는데 클릭해봐

![img4](/assets/images/posts/UnityDocs/2023-02-16-my-unitydoc-post_4/4.png){: width="100%" height="100%"}

그럼 위의 이미지와 같은 유니티가 제공하는 Profiler를 사용할 수 있어.

메모리를 더 세부적으로 보고 싶다면 Memory Profiler를 사용하는 것을 권장하고 있어.
이건 Profiler 처럼 기본적으로 내장되어 있는 게 아니기 때문에, Package Manager에서 Install 해줘야 해.

하지만 설치하는 법이 조금 다르기 때문에 아래에서 설명해줄게.

## 🔖 Unity에서 제공하는 Memory Profiler 

조금 더 디테일하게 알고 싶다면? Unity Korea의 Memory Profiler 1.0 활용법 소개 영상을 확인해보자.<br>
[🔔영상 바로가기](https://youtu.be/rdspAfOFRJI)
{: .notice--warning}

    Window > Analysis > Memory Profiler

- 에디터와 플레이어의 런타임 메모리에 스냅샷 생성
- 메모리 할당 상태 확인 : 과하거나 불필요한 메모리 사용 원인을 빠르게 식별 / 메모리 누수 및 힙 조각화를 확인
- Memory Profiler의 상단 메뉴 표시줄을 사용
  - 플레이어 선택 대상을 변경하고 스냅샷을 캡쳐, 가져오기 가능

<strong style="color:orange; font-size:15pt">주의 사항 : Unity Editor에서 프로파일링시 Editor에 의해 추가된 오버헤드로 인해 부정확한 수치가 발생가능해</strong>

결론적으로 실제 기기랑 연결해서 Snapshot을 떠야 그게 가장 정확한 수치라고 말하고 있어.
에디터에서도 Snapshot을 떠서 확인할 수 있지만 이건 대략적인 부분(전체적인 흐름)만 알 수 있을 뿐이고, 신뢰할 수 없는 데이터로 본다고 해.

마지막으로 반드시 타겟 디바이스를 올려서 확인하는 걸 권장하고 있어.

### 🤔 장점

Snapshot을 떠서 두개를 비교할 수 있는 점이 장점인데 로비 씬, 인게임 씬을 각각 떠서 서로 비교하는게 되는 거야.

또 이렇게도 사용가능하다고 해.

AOS(안드로이드) Snapshot을 하나 뜨고 IOS Snapshot을 떠서 서로 비교를 할 수 있고, 또 서로 다른 기종의 비교도 가능하다고 해.

### 🔎 설치하는 법

이건 유니티 버전 2021.3 또는 이후의 버전부터 제공하는 기능이야.
따라서 버전체크 잘하고 만약 내가 2021.3 또는 이후의 버전을 사용한다고 하면 

    Window > Package Manager 

위의 경로에서 Package Manager를 클릭해.
그러면 아래와 같이 Package Manager가 켜질 꺼야.

![img5](/assets/images/posts/UnityDocs/2023-02-16-my-unitydoc-post_4/5.png){: width="100%" height="100%"}

위의 빨간색 동그라미 부분의 + 를 클릭해서 <strong> Add By Name </strong> 을 선택해줘.

![img6](/assets/images/posts/UnityDocs/2023-02-16-my-unitydoc-post_4/6.png){: width="100%" height="100%"}

위에서 Add prackage by name... 이야

![img7](/assets/images/posts/UnityDocs/2023-02-16-my-unitydoc-post_4/7.png){: width="100%" height="100%"}

    com.unity.memoryprofiler

라고 입력한 뒤에 Add 버튼을 누르면 로딩 프로세스가 시작되고 설치가 진행 될거야

![img8](/assets/images/posts/UnityDocs/2023-02-16-my-unitydoc-post_4/8.png){: width="100%" height="100%"}

제대로 설치가 되었다면 위와 같이 Package Manager에 Memory Profiler가 추가 돼.

그리고 어디서 실행하냐면

![img9](/assets/images/posts/UnityDocs/2023-02-16-my-unitydoc-post_4/9.png){: width="100%" height="100%"}

    Window > Analysis

위의 경로에 Memory Profiler가 추가 되는 데, 실행해보자

![img10](/assets/images/posts/UnityDocs/2023-02-16-my-unitydoc-post_4/10.png){: width="100%" height="100%"}

위의 이미지와 같이 난생 처음 보는 녀석이 켜져.
이미지에는 왼쪽 하단에 무언가 있지만, 너희들은 처음 켰으니까 아무 것도 없을 거야.
놀라지 말고 설명을 이어서 들어봐.

가운데 Capture New Snapshot 이라고 카메라 아이콘과 함께 버튼이 하나 있는 게 보일 거야.
에디터에서 Play 버튼을 누르고,
저 버튼을 클릭하면

![img11](/assets/images/posts/UnityDocs/2023-02-16-my-unitydoc-post_4/11.png){: width="100%" height="100%"}

이미지처럼 왼쪽 하단에 뭔가 하나 더 추가된게 보일 거야.

![img12](/assets/images/posts/UnityDocs/2023-02-16-my-unitydoc-post_4/12.png){: width="100%" height="100%"}

그걸 클릭하면 이렇게 어떤 에셋이 메모리를 얼만큼 먹고 있고, 쉐이더가 얼만큼 먹고 있고, 특정 텍스트가 얼마나 먹고 있는지 디테일하게 확인할 수 있어.

기기에 연결해서도 확인할 수 있는데, 하지만 실시간은 아니야 snapshot 이니 이름 그대로의 기능이고 그렇다면 반실시간이라고 해야겠지. 

결론적으로 메모리 최적화에 관심이 있다면 이걸 사용하는 것은 필수적이라는 거야.

# 💁‍♂️ 메모리에 영향을 미치는 Asset 관리
<hr style="width:100%" />

## 🔖 중복 리소스
- 유니티는 중복 파일 체크 X
- 실수로 동일한 파일을 다른 폴더에
- Font, Audio, Texture, Mesh ...

### 📍 Asset Dependency

![img13](/assets/images/posts/UnityDocs/2023-02-16-my-unitydoc-post_4/13.png){: width="100%" height="100%"}

예를 들어 위와 같이 탱크 프리팹이 있다고 가정할 때, 탱크는 Material에 종속되어 있고 Material은 Texture에 종속 되어 있어.

이렇게 종속관계를 가진 Asset이 많이 존재하지.
(Mesh, Material, Texture etc..)

따라서 이런 걸 잘 고려해서 Asset Bundle을 사용해야 해.
왜냐하면 모든 종속관계가 1:1이 아닌 1:다 일 경우가 대부분이기 때문이야.

실제로 중복 리소스는 정말 많이 일어나는 일이야.

![img14](/assets/images/posts/UnityDocs/2023-02-16-my-unitydoc-post_4/14.png){: width="100%" height="100%"}

예를 들어 위와 같은 바위 A, B가 존재하고 같은 Material을 쓰고 있다고 생각해보자

그리고 이런 상황에서 두가지를 Asset Bundle로 묶는다고 했을 때,

![img15](/assets/images/posts/UnityDocs/2023-02-16-my-unitydoc-post_4/15.png){: width="100%" height="100%"}

A, B의 Material을 각각 Bundle로 묶고, C도 따로 묶어줘야해.
그렇지 않으면 A와 B의 Material을 Asset Bundle로 묶는 과정에서 C라는 텍스쳐가 A, B 각각 따로 복제되어 묶이게 되는데 이렇게 되면 메모리에 같은 C의 텍스쳐가 2개가 올라가는 거야.

사실상 이런일이 비일비재한데, 이런 상황을 예방하기 위해서는 Addressables을 사용하라고 권장하고 있어.

Addressables이 제공하는 ToolKit을 이용해서 Dependency를 조금 더 수월하게 효율적으로 관리할 수 있다고 해.
그리고 Addressables는 전혀 다른 기능으로 느껴지는 데, 유니티 내부적으로는 Asset Bundle 로 인식되고 다른 점은 어셋 번들을 레이어화해서 한번 더 랩핑하는 거야.

그렇기 때문에 사용할 수 있다면 Addressables을 사용하는 것을 권장한다고 한번 더 이야기 해주고 있어. 

### 📍 Audio

![img16](/assets/images/posts/UnityDocs/2023-02-16-my-unitydoc-post_4/16.png){: width="100%" height="100%"}

- Force To Mono : 스테레오 정보를 없애버리고 그냥 Mono로만 듣는 것 (용량이 줄어듦)
- Load type
  - Decompress on load (< 256kb) 
  - Compressed into memory (< 1mb)
  - Streaming (> 1mb)
- Compression Format
  - 매우 짧은 클립은 ADPCM (압축비 3.5)
  - 대부분의 경우에는 Vorbis
  - IOS에서 MP3 사용 이점 적음
  - 유니티는 H/W 디코딩 X
- Mute 되어도 메모리에는 존재.

모바일에서는 오디오가 스테레오 일리가 없기 때문에 Force To Mono를 권장해.

그리고 Load type도 굉장히 중요한데,

1. Decompress on load : 사운드 압축을 풀어서 메모리에 올려버림, 따라서 1mb -> 20~30mb가 될 수도 있기 때문에 굉장히 위험한 옵션
   - 장점은 재생속도가 빠르다.
   - 사이즈가 작은 게 아니면 이 옵션은 쓰지 않는 것이 좋다.
2. Compressed into memory
   - 주로 권장을 하는 건 1mb 이하는 이 옵션을 권장
3. Streaming
   - 긴 배경음악이라 사이즈가 크면 Streaming을 권장

IOS는 MP3를 H/W 레벨로 디코딩 가능하지만, Unity 같은 경우 H/W가 아닌 S/W를 사용해서 처리하기 때문에 IOS에서 MP3 이점이 딱히 있진 않아.

### 📍 Mesh

![img17](/assets/images/posts/UnityDocs/2023-02-16-my-unitydoc-post_4/17.png){: width="100%" height="100%"}

- Mesh Compression은 저장 용량
  - 메모리와는 무관
- Read/Write Enabled 옵션 해제
  - CPU 메모리와 GPU메모리 양쪽에 존재
  - 2019.3부터는 기본 꺼짐
- 불 필요 시 끌 옵션들 확인
  - Rig
  - BlendShapes
  - Normal
  - Tangent
  - Lightmap UVs
  - Generate Colliders

Read/Write Enabled 옵션을 키게 되면 메모리에 복제본이 생성되고, 따라서 메모리가 2배 뻥튀기가 되는 현상이 벌어지게 되는데 왜냐하면 API에서 런타임 중에 접근해서 Mesh를 변형하거나 그런 걸 작동하려고 키는 옵션이야.

그러면 GPU 영역에서 Mesh Data가 들어가고 API에서 접근하는 것은 CPU 메모리(시스템 메모리)에 들어가야 하기 때문에 CPU, GPU 영역 둘 다 넣는다.

그래서 메모리가 중복되기 때문에 꺼주는 것이 좋아.


# 🥱 끝 마치며
<hr style="width:100%" />

내용이 너무 많아서 2편에 걸쳐서 올려야 겠다.

<br>

<strong style="color:yellow; font-size:100pt;">끝</strong>


<hr style="width:100%" />
<br>

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr>

