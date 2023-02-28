---
title:  "[Unity Doc] 6. 메모리 최적화를 위한 에셋 관리 (2/2)"
excerpt: "내가 볼려고 만든 유니티 잡학지식 문서"

categories:
  - UnityDocs
tags:
  - [UnityDocs]
toc: true
toc_sticky: true
date: 2023-02-27
last_modified_at: 2023-02-28
---

Unity Korea 유투브 채널 **2월 알쓸유잡 : 메모리 최적화를 위한 에셋 관리** 영상을 보고 정리한 게시글입니다.
<br>
[🔔영상 보러가기 클릭](https://www.youtube.com/live/52ehLUfk3DQ?feature=share)
{: .notice--warning}

<br>

# 💁‍♂️ 메모리 최적화를 위한 에셋 관리
<hr style="width:100%" />

### 📍 Shader Variants

에디터에서 쉐이더를 작성하면서 보면 Standard Shader 이렇게 프리셋이 있는 걸 확인할 수 있을거야.
이게 단지 하나의 쉐이더로 보이겠지만, 내부적으로는 여러가지 조합들의 경우의 수 때문에 수천 ~ 수만가지의 바이너리 조각들이 발생할 수 있어.

- 패키지 용량 이슈가 크지만 메모리도 영향 있음.
- 하나의 쉐이더가 수 많은 바이너리로 파생됨
- 수 많은 조건들의 조합 경우의 수
  - 노멀맵, 포그, 라이트맵, 인스턴싱, 플랫폼 등등
- 에디터에서의 작동 != 플레이어 빌드에서의 작동
- 쉐이더 베이언츠를 줄이기 위한 기법들
  - Graphics API 지정 (Player Settings) : 기본적으로 auto로 켜져 있음.
  - Shader Stripping (Graphics / URP Settings) : ProjectSetting의 Graphics에도 있고, URP Settings에도 있음. 
  - URP Asset에서 미 사용 기능 비활성화
  - multi_compile vs shader_feature
  - ShaderVariantCollection (Hiccup vs Memory)
  - Log Shader Compilation

에디터에서는 미리 모든 경우의 수의 쉐이더를 만들어 놓지 않는다고 해.
왜냐하면 계속 테스트를 하면서 확인해야하고, 그 때마다 모든 경우의 수의 쉐이더를 빌드하게 되면 굉장히 비효율적이고 느리기 때문이야.
따라서 에디터에서는 실행할 때 필요한 쉐이더만 생성해서 랜더링을 하는 거지.

결론적으로 에디터에서의 작동 != 플레이어 빌드에서의 작동이 성립하는거야.

multi_compile은 다 만드는 것, shader_feature 선택한 것만 만들어내는 것.
하지만 위의 multi_compile, shader_feature는 쉐이더를 직접만들 때 사용됨.

ShaderVariantCollection을 쓰면 미리 전부 로딩해놓고 보여주는 것이라 Memory를 잡아먹고 Hiccup을 방지할 수 있음.
하지만 메모리 용량이..

### 📍 Text Mesh Pro

- 글자들을 미리 텍스쳐로 제작
- 영미권 언어는 큰 문제 없음
- 한글은 초성 중성 종성 조합 방식
- 허용 글자 범위?
  - 한글 2350자
  - 추가 129자
    - 괞웱궴긜꺆꺍꺠꾤꾺끨뉌뉍뉑댤댬댱됀됏
  - 한글의 위대함
    - 꾩떃눭늂뎭됇뒱랢럪뮕밢볝봹뺁볞뽋쑖싩

- Static vs Dynamic
  - Static
    - 사용될 모든 문자 아틀라스를 미리 생성
    - 런타임시 원본 폰트가 필요 없음
  - Dynamic
    - 실시간으로 필요한 폰트 아틀라스 갱신
    - 런타임시 원본 폰트 필요
    - 사용 문자들의 범위 예측 불가능 시 유용
      - ╰(*°▽°*)╯
- Multi Atlas Textures
  - Draw call vs 대역폭 (Atlas Resolution)

![img1](/assets/images/posts/UnityDocs/2023-02-27-my-unitydoc-post_6/1.png){: width="50%" height="50%"}

4096의 크기는 모바일에서 느려진다고 쓰지 않는 것을 권장하고, 많아도 2096 이런 식으로 사용해야한다고 해.

그래서 한 아틀라스에 다 넣을 수 없다면 Multi Atlas Textures를 사용해야 하는데,
다만 아틀라스를 여러개 쓰다보면 드로우콜의 배칭이 깨지는 조건들이 많아질 것 이라고 하네.

따라서 `Draw call vs 대역폭 (Atlas Resolution)` 이렇게 되는거지.

### 📍 Baked Lighting

- Lightmap
  - static object에 적용
  - 저장형태: Texture 2D
  - 메모리 이슈 발생 가능성 있음
  - Subtractive, Baked Indirect, Shadowmask
- Light Probe
  - 주로 dynamic object 대응 용도 (static도 가능)
  - Diffuse term만 가능 (specular term 불가)
  - 저장 형태: 개당 float x 30 + a;
- Reflection Probe
  - Specular term을 위한 데이터
  - 저장형태: 개당 Cubemap Texture
  - 메모리 이슈 발생 가능성 큼

Lightmap은 배경, 건물, 배경 Prop 같은 Static한 오브젝트에 적용되는 베이킹 Lighting 임.
텍스쳐에 라이트의 결과를 미리 구워서 저장해 놓는 것임.

좁은 씬 같은 건 상관없는데 넓으면 텍스쳐를 많이 만들어 내기 때문에 메모리에 영향을 많이 끼침.

굉장히 광활한 오픈월드면 메모리 이슈로 라이트맵을 못 쓸수도 있음.

Light Probe는 메모리를 그렇게 많이 차지하지 않음.

Reflection Probe는 Cubemap Texture임
1개가 6장이기 때문에 많이 쓰면 많이 쓸 수록 메모리에 영향을 많이 끼침

![img2](/assets/images/posts/UnityDocs/2023-02-27-my-unitydoc-post_6/2.png){: width="30%" height="30%"}

Lightmap, Reflection Probe 의 사용 빈도 수를 조절해야함.

### 📍 Texture

- 사이즈 가능한 작게
- 적절한 Texture Comperssion Format
- 2D 및 UI는 SpriteAtlas 적극 활용
- Read/Write Enabled 옵션 해제
  - CPU 메모리와 GPU 메모리 양쪽에 존재
- Mipmap
  - 2D 및 UI에서는 대부분 필요 없음
  - 3D 게임에서는 거의 필수

### 📍 Texture Compression

- PNG, JPEG 등 원본 이미지 포맷은 무관
  - ETC, PVRTC, ASTC ...
- 기기별 지원되는 Texture Format 사용
  - 기기 미 지원 포맷 사용 시 압축이 풀림
  - PVRTC - Android
  - ETC2 - OpenGL ES2
  - *ASTC - iPhone 5
- 모바일에서는 ASTC 추천
  - 타겟 디바이스에 따라 다름
- POT vs NPOT
  - Power of Two (256, 512, 1024 ...)
  - ASTC는 NPOT 지원하지만 
    - 밉맵 사용 시 POT

### 📍 *ASTC 포맷

AOS, IOS 둘다 지원

캐릭터 모델링 텍스쳐 같은 경우 4x4 를 사용하고 연기나 스파크 같은 휘발적인 성격이 강한 텍스쳐(디테일 하지 않은 텍스쳐)는 12x12를 사용

원본 == ASTC 4x4(거의 같음) >>> PVRTC == ETC >>>>>> ASTC 12x12

ASTC가 압축율도 좋고 퀄리티도 훨씬 좋음.

![img3](/assets/images/posts/UnityDocs/2023-02-27-my-unitydoc-post_6/3.png){: width="40%" height="40%"}

텍스처 압축형식을 지원하는 기기의 비율도 확인하고 국가마다 고려해서 사용하는 것이 좋음

### 📍 POT vs NPOT

- POT(Power of Two): 128, 256, 512, 1024, ...
- ASTC는 NPOT(Not POT) 사이즈 지원
- Mipmap 사용 시 POT 강제
  - 즉, 3D 에셋용 텍스쳐는 사실상 POT만
- UI 및 2D는 NPOT 가능
  - 하지만, Sprite Atlas 권장
  - Sprite Atlas는 POT


>Only POT texture can be compressed if mip-maps are enabled 

2D, UI는 아틀라스 쓰는 것을 권장함.
그 아틀라스가 유니티에서는 POT임.

ASTC를 실질적으로 POT로 사용될 것임.
MipMaps을 켜면 용량(33.3333...%)이 조금 늘어남.

MipMap 이란 가까운 애들은 고해상도로 먼 애들은 저해상도로 랜더링하는 것임.
따라서 각 해상도(원근 처리)에 따라 텍스쳐를 만들어냄
(2D에서는 원근 처리가 필요 없기 때문에 MipMap을 off 하는 것이 좋음)

## Scene loading

![img4](/assets/images/posts/UnityDocs/2023-02-27-my-unitydoc-post_6/4.png){: width="40%" height="40%"}

에셋들이 너무 많으면 씬 이동간 메모리가 터질 수도 있다고 함.

예를 들어 위의 이미지에서 왼쪽을 기준으로 Scene A가 마을 필드고 Scene B가 사냥터라고 가정 했을 때, A에서 B로 이동한다고 하자.

그러면 A의 리소스가 메모리에 있는 상태에서 B를 불러오게 되는데, 각 씬이 무거운 Scene이라고 가정하자.


![bomb](https://media.giphy.com/media/HhTXt43pk1I1W/giphy.gif){: width="30%" height="30%"}

그러면 A에서 B를 불러올 때, 무거운 리소스들이 중첩되어 메모리에 올라가기 때문에, 순간적으로 메모리가 터지게 된다.

따라서 이미지에서 오른쪽을 보면, A와 B가 중첩되어 메모리에 올라가지 않게 하기 위해 중간에 가벼운 Scene C를 끼워 넣는 것을 확인할 수 있다.

![img5](/assets/images/posts/UnityDocs/2023-02-27-my-unitydoc-post_6/5.png){: width="40%" height="40%"}

분명 Loading 창이 되겠지.

가끔 모바일 게임에서 레벨 넘어가다가 게임이 꺼지는 경우가 있는데, 이런 이유로 인한 문제일 경우가 크다.

# 🥱 끝 마치며
<hr style="width:100%" />

코딩 공부를 하다가 보면 그 외적인 것에 신경을 잘 쓰지 않는 경우가 많다, 그렇기 때문에 이런 정보나 지식을 찾아보는 것은 참 유익한 것 같다.

<br>

<strong style="color:yellow; font-size:100pt;">끝</strong>


<hr style="width:100%" />
<br>

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr>

