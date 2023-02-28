---
title:  "[실전 게임 코드 리뷰 :: 유니티 클리커 게임] 12. Managers - SoundManager"
excerpt: "Unity Lesson 1"

categories:
  - Unity Lesson 1
tags:
  - [Unity Lesson 1]
toc: true
toc_sticky: true
date: 2023-02-28
last_modified_at: 2023-02-28
---

인프런에 있는 Rookiss님의 **[실전 게임 코드 리뷰] 유니티 클리커 게임** 강의를 듣고 정리한 게시글입니다.
<br>
[🔔강의 보러가기 클릭](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84%EA%B2%8C%EC%9E%84-%EC%BD%94%EB%93%9C%EB%A6%AC%EB%B7%B0-%EC%9C%A0%EB%8B%88%ED%8B%B0-%ED%81%B4%EB%A6%AC%EC%BB%A4)
{: .notice--warning}

# 🧑‍💼 SoundManager.cs
<hr style="width:100%" />

![keyboard](https://media.giphy.com/media/XIqCQx02E1U9W/giphy.gif){: width="30%" height="30%"}

해당 클래스에서는 아래와 같은 Enum 클래스로 AudioSource를 관리한다.

```c#
public enum Sound
{
	Bgm,
	Effect,
	Speech,
	Max,
}
```

그리고 `Dictionary<string, AudioClip>` 형식의 Dictionary로 AudioClip들을 관리한다.

## 🏗️ [Constructor] SceneManagerEx

로그만 찍고 특별한 건 없다.

## 🖨️ [Method] Init()

먼저 `@SoundRoot` 라는 오브젝트가 하이어라키에 존재할 경우 GameObject 변수 _soundRoot에 할당한다.

### 만약 _soundRoot가 null 일 경우 

`@SoundRoot` 라는 오브젝트를 새로 만들고 `Object.DontDestroyOnLoad(Object target)` 메서드로 오브젝트 파괴 방지를 해준다.

그리고 열거형 클래스의 내장 메서드 GetNames 을 호출해서 string[] 형식 값을 반환 받는다.
마지막 `Max` 는 열거형 요소의 Count를 뜻한다. 
따라서 직접 사용하지 않을 것이기 때문에, `soundTypeNames.Length - 1` 까지만 for loop를 돌려서 GameObject를 생성 후 AddComponent 메서드로 AudioSource 컴포넌트를 추가 후 _audioSources 배열의 요소에 넣어준다.


### _soundRoot가 null 이 아닐 경우

`Sound.Bgm` AudioSource를 loop 한다.

## 🖨️ [Method] Clear()

모든 AudioSource를 Stop() 하고 _audioClips Dictionary를 Clear 함.

## 🖨️ [Method] SetPitch()

```c#
public void SetPitch(Define.Sound type, float pitch = 1.0f)
```

인수로 받아온 pitch 할당, Default = 1.0f

## 🖨️ [Method] Play()

```c#
public bool Play(Define.Sound type, string path, float volume = 1.0f, float pitch = 1.0f)
```

Bgm, Effect, Speech 분기를 나눠서 audioSource Play

## 🖨️ [Method] Stop()

```c#
public void Stop(Define.Sound type)
```

인수로 받아온 type으로 해당하는 _audioSources 요소에 접근해 stop

## 🖨️ [Method] GetAudioClipLength()

```c#
public float GetAudioClipLength(string path)
```

path를 기준으로 가져온 AudioClip의 길이를 반환하는 메서드

## 🖨️ [Method] GetAudioClip()

```c#
private AudioClip GetAudioClip(string path)
```

해당 오디오 클립이 존재한다면 반환, 없으면 Resource.Load로 찾아와서 _audioClips Dictionary에 Add로 추가 후 반환

<br>

# 📢 오늘의 한마디
<hr style="width:100%" />

<strong style="color:Yellow; font-size:15pt">조금 꼬여있는 것 같은데...</strong>

<br>

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr>

