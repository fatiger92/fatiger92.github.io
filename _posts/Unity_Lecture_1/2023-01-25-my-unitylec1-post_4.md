---
title:  "[실전 게임 코드 리뷰 :: 유니티 클리커 게임] 4. 프리팹 제작 및 실전 팁"
excerpt: "Unity Lesson 1"

categories:
  - Unity Lesson 1
tags:
  - [Unity Lesson 1]
toc: true
toc_sticky: true
date: 2023-01-25
last_modified_at: 2023-01-25
---

인프런에 있는 Rookiss님의 **[실전 게임 코드 리뷰] 유니티 클리커 게임** 강의를 듣고 정리한 게시글입니다.
<br>
[🔔강의 보러가기 클릭](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84%EA%B2%8C%EC%9E%84-%EC%BD%94%EB%93%9C%EB%A6%AC%EB%B7%B0-%EC%9C%A0%EB%8B%88%ED%8B%B0-%ED%81%B4%EB%A6%AC%EC%BB%A4)
{: .notice--warning}

# 📕 UI 프리팹 만들기
<hr style="width:100%" />

UGUI Image를 사용하면 Canvas가 꼭 필요하며, Canvas의 하위 오브젝트가 될 Image의 사이즈는 Canvas에 종속되게 된다.
따라서 Canvas의 인스펙터 내 Canvas Scaler 컴포넌트에 영향을 받게 되고,

UI Scale Mode에 따라 설정해야 하는 속성이 달라지고 캔버스에서 UI 요소가 스케일 되는 방법을 결정하는 기능을 가졌다.  

## 📑 Constant Pixel Size 
  - UI 요소가 화면 크기에 관계없이 동일한 픽셀 크기로 유지된다.

  Constant Pixel Size 설정:

  |**프로퍼티:**|**기능:**|
  |------|---|
  | Scale Factor | 캔버스의 모든 UI 요소를 이 배율로 스케일합니다. |
  | Reference Pixels Per Unit | 스프라이트에 이 ‘Pixels Per Unit’ 설정이 적용된 경우 스프라이트의 1픽셀이 UI의 유닛 하나에 해당합니다. |

## 📑 Scale With Screen Size
  - 화면이 커질수록 UI 요소도 커진다.

  Scale With Screen Size 설정:
  
  |**프로퍼티:**|**기능:**|
  |------|---|
  | Reference Resolution | UI 레이아웃에 적합한 해상도입니다. 화면 해상도가 크면 UI가 더 크게 스케일되고 작으면 UI가 더 작게 스케일된다. |
  | Screen Match Mode | 현재 해상도의 종횡비가 레퍼런스 해상도에 맞지 않는 경우 캔버스 영역을 스케일하는 데 사용되는 모드이다. |
  | &nbsp;&nbsp;&nbsp;&nbsp;Match Width or Height | 캔버스 영역의 너비 또는 높이를 레퍼런스로 사용하여 스케일하거나 그 사이로 스케일한다. |
  | &nbsp;&nbsp;&nbsp;&nbsp;Expand | 캔버스 크기가 레퍼런스보다 더 작아지지 않도록 캔버스 영역을 수평 또는 수직으로 확장한다. |
  | &nbsp;&nbsp;&nbsp;&nbsp;Shrink | 캔버스 크기가 레퍼런스보다 커지지 않도록 캔버스를 수평 또는 수직으로 자른다. |
  | Match | 스케일링 레퍼런스로 너비 또는 높이를 사용할지, 아니면 둘 사이의 배합을 사용할지 결정한다. |
  | Reference Pixels Per Unit | 스프라이트에 이 ‘Pixels Per Unit’ 설정이 적용된 경우 스프라이트의 1픽셀이 UI의 유닛 하나에 해당한다. |

## 📑 Constant Physical Size
  - 화면 크기와 해상도에 관계없이 UI 요소가 동일한 물리적인 크기로 유지된다.

  |**프로퍼티:**|**기능:**|
  |------|---|
  | Physical Unit | 포지션 및 크기를 지정하는 물리적 단위이다. |
  | Fallback Screen DPI | 화면 DPI를 알 수 없는 경우 가정되는 DPI이다. |
  | Default Sprite DPI | ‘Pixels Per Unit’ 설정이 ‘Reference Pixels Per Unit’ 설정과 일치하는 스프라이트에 사용할 인치당 픽셀(DPI)이다. |
  | Reference Pixels Per Unit | 스프라이트에 ‘Pixels Per Unit’ 설정이 있는 경우 DPI는 ‘Default Sprite DPI’ 설정과 일치한다. |

## 📑 Canvas 컴포넌트가 World Space로 설정된 경우 표시되는 World Space Canvas 설정:

  |**프로퍼티:**|**기능:**|
  |------|---|
  | Dynamic Pixels Per Unit | UI에서 동적으로 생성되는 비트맵(예: 텍스트)에 사용할 단위당 픽셀의 양이다. |
  | Reference Pixels Per Unit | 스프라이트에 'Pixels Per Unit' 설정이 있는 경우 스프라이트의 픽셀 하나가 월드의 유닛 하나에 해당한다. <br> 'Reference Pixels Per Unit'을 1로 설정하면 스프라이트의 'Pixels Per Unit' 설정이 그대로 사용된다. |

<hr style="width:100%" />

대체로 **Scale With Screen Size** 을 자주 사용하는 편이다.  
왜냐하면 Reference Resolution에 해상도를 설정해 놓고, 실제 기기의 해상도가 설정한 해상도 보다 크거나 작을 경우 비율에 맞춰 자동으로 UI 사이즈를 어느정도 맞춰 주기 때문이다.
추가적으로 UI를 구성할 때 모든 해상도를 완벽하게 대응할 수는 없다.

해당 프로젝트도 Scale With Screen Size를 사용하여 UI를 구성했다.

# 💡 Tip

  1. UI 스크립트에 어싸인도 좋지만 <strong style="color:Yellow;">코드에서 Initialize 하는 편이 관리적인 방향으로 봤을 때 좋다.</strong>
  2. 이것 저것 오브젝트들을 스크립트 인스펙터에 닥치는 대로 어싸인해서 만드는게 과연 좋은 방법인지 생각해보자.

<br>

# 📢 오늘의 한마디
<hr style="width:100%" />

  ⚡규모가 작은 프로젝트를 만들 때, <strong style="color:crimson;">프레임을 신경 안 쓰고 즉흥적으로 만드는 경우가 많은데 규모가 어떻게 되든지 프레임을 신경써서 만드는 것</strong>이 장기적으로 나의 스타일을 만들 수 있기 때문에 좋은 것 같다.  

  그리고 나 또한 미니 프로젝트 구성을 스크립트에 이것저것 오브젝트 어싸인으로 하는 것이 좋은 방법이라고 생각 했는데, 다시 한번 그게 맞는지에 대해 생각해 보는 강의였다.  
  

<br>

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr>

