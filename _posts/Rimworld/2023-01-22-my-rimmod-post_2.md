---
title:  "[Rimworld Modding Tutorial] 2. 튜토리얼 XML 작성"
excerpt: "림월드 모딩을 해보자고"

categories:
  - Rimworld
tags:
  - [Rimworld]
toc: true
toc_sticky: true
date: 2023-01-22
last_modified_at: 2023-01-22
---

Youtuber <strong style="color:orange;"> Eck314님의 How to Make a RimWorld Mod - Step by Step</strong>  영상을 보고 정리한 게시글입니다.
<br>
[🔔영상 보러가기 클릭](https://youtu.be/UgCOhFzeX4A)
{: .notice--warning}

<br>

# 1. XML 작성
<hr style="width:100%" />

## A. About.xml 작성

![mod1](/assets/images/posts/Rimworld/2023-01-22-my-rimmod-post_2/1.png)

먼저 위의 튜토리얼을 그대로 진행할 것인데, 해당 포스트가 2017년을 마지막으로 업데이트가 안되었기 때문에 그 당시의 버전에 해당하는 xml 형식과 지금(1.4 버전)의 형식이 조금 다르다.

그건 진행하면서 설명할 예정이다.

1) Create a mod folder 
- 모드 폴더를 만들어라!  
    steamapps>common>RimWorld>Mods 의 경로에 PlagueGun 폴더를 만든다.  

2) Inside PlagueGun, make an About folder. 
- PlagueGun 폴더 내부에 About 폴더를 만든다. 

      RimWorld>Mods>PlagueGun>About

3) Inside the About folder, make a new text file and rename it About.xml.  
- About 폴더 내부에 새로운 text 파일을 만들고 이름을 About.xml로 변경한다. (주의! 확장자명이 xml로 바뀌었는지 확인)

      RimWorld>Mods>PlagueGun>About>About.xml

- About.xml 파일을 만들었다면, 파일을 열어서 아래와 같이 작성한다.  
  이 내용은 항상 림월드 xml 파일 최상단에 입력된다.

  ```xml 
  <?xml version="1.0" encoding="utf-8"?> 
  ```
  
- 그리고 MetaData tags 부분을 작성하는데, 이 부분이 조금 다르다.  
  ```xml
  <ModMetaData>
	  <name>Test Mod - Plague Gun</name>
	  <author>YourName</author>
    <packageId>AuthorName.ModName.Specific</packageId>
	  <supportedVersions>
      <li>1.4</li>
    </supportedVersions>
	  <description>V1.0
      This mod adds a plague gun, a weapon that has a chance to give your enemies the plague.
    </description>
  </ModMetaData>
  ```
  위에 작성된 내용은 1.4 버전을 타겟으로 하고 있다.
  
  ```xml
  <supportedVersions>
      <li>1.2</li>
      <li>1.3</li>
      <li>1.4</li>
  </supportedVersions>
  ```
  만약 1.2, 1.3 버전도 호환되는 모드를 만들고 싶다면 위와 같이 추가하면 된다.

## B. About.xml 최종 완성본

  최종으로 작성한 About.xml은 아래와 같다.
  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <ModMetaData>
    <name>Victor's Mod - Plague Gun</name>
    <author>Victor</author>
      <packageId>Victor.PlagueGun.0001</packageId>
    <supportedVersions>
          <li>1.4</li>
      </supportedVersions>
    <description>V1.0
      This mod adds a plague gun, a weapon that has a chance to give your enemies the plague.
      And also this mod is my first custom mod!
      </description>
  </ModMetaData>
  ```

4) Add a Preview.png or Preview.jpeg to your About folder.
  - Preview.png 또는 review.jpeg 파일을 About 폴더에 추가한다.  

        RimWorld>Mods>PlagueGun>About>Preview.png
    
    이 사진은 스팀의 창작마당에서의 프리뷰가 된다.  
    480x300px 사이즈의 사진을 선호하지만, 정해진 것은 없기 때문에 사이즈는 사실 신경쓰지 않아도 된다.

    ![mod2](/assets/images/posts/Rimworld/2023-01-22-my-rimmod-post_2/2.png)  

    해당 샘플 이미지를 사용한다.

5) Make a Defs folder in your Mod's directory.
  - steamapps>common>RimWorld>Mods>PlagueGun 의 경로에 Defs 폴더를 만든다. 

        RimWorld>Mods>PlagueGun>Defs

    림월드는 모든 디렉토리에서 xml 파일을 읽는다.  
    /Defs/의 하위폴더들은 원하는 대로 디렉토리 이름을 지정할 수 있으나 Defs는 림월드에서 xml을 불러오는 정해진 경로이기 때문에 무조건 Defs 이어야 한다.
    이번 튜토리얼에서는 림월드의 표준 구조를 사용한다고 함.

## C. Def는 무엇인가?

림월드는 게임 내 객체에 대한 Blueprint와 같은 Defs 라는 것을 사용한다.  
숨겨진 C# 코드를 사용하는 대신에, 림월드는 xml Def를 검색해서 인게임에 copy 하는 방식을 사용한다.  
이러한 방식은 모더들에게 더욱 더 접근하기 쉽게 만든다.  
캐릭터, 동물, 바닥, 건물, 그리고 심지어 림월드의 질변까지 모든 것을 Defs로 사용한다.  
이 튜토리얼에서는 우리만의 Plague Gun과 Plague Bullet의 Defs를 만들 것이다.  

<br>

## D. 총과 총알의 ThingDef를 찾아 RangedWeapon_PlagueGun.xml 작성

6) Make a new ThingDefs folder in your Defs folder.
  - Defs 폴더 안에 새로운 ThingDefs 폴더를 만들자!

        RimWorld>Mods>PlagueGun>Defs>ThingDefs

7) Make a new text file in your ThingDefs folder, and change it to RangedWeapon_PlagueGun.xml.  
  - ThingDefs 폴더에 새로운 text 파일을 만들고 RangedWeapon_PlagueGun.xml 로 이름을 변경한다.

        RimWorld>Mods>PlagueGun>Defs>ThingDefs>RangedWeapon_PlagueGun.xml
    
    이 파일에 우리가 새로 만들 총과 총알의 Blueprints를 작성할 것이다.  
    다음으로 기존 리볼버의 ThingDef와 리볼버 Bullet ThingDef를 복사하여 xml 파일을 작성합니다.  
    림월드에서는 key taps를 만들 때 xml attribute ParentName="BaseBullet"을 사용하는 것이 가장 좋다.  
    이는 기존의 BaseBullet ThingDef에서 xml 코드를 복사하여 시간과 key taps를 절약할 수 있기 때문이다.

    👉여기서 Key taps가 무엇을 뜻하는지 아직 잘 모르겠음.

    리볼버와 리볼버 Bullet의 ThingDef는 아래의 경로의 xml에 있다.
    
        steamapps\common\RimWorld\Data\Core\Defs\ThingDefs_Misc\Weapons\RangedIndustrial.xml

    그리고 추가적으로 ThingDef를 찾을 때, 위에서 언급한 FindinFiles 프로그램을 사용하면 더욱더 수월하게 찾을 수 있다.

    ![mod3](/assets/images/posts/Rimworld/2023-01-22-my-rimmod-post_2/3.png)  

    먼저 FindinFiles 프로그램을 설치했다면 Defs 폴더를 오른쪽 클릭시 FindInFiles...라는 탭이 생겼을 것이다.
    이걸 선택하면 아래와 같은 프로그램이 열린다. 

    ![mod5](/assets/images/posts/Rimworld/2023-01-22-my-rimmod-post_2/4.png) 

    상단의 찾을 문자에 우리가 찾을 revolver 라는 단어를 넣고, 찾기를 클릭한다.  
    그럼 Def 폴더 내부 경로에 있는 파일 중 텍스트에 revolver 라는 단어를 가지고 있는 파일들이 전부 검색되어 나온다.  
    우리는 ThingDef를 찾고 있기 때문에 최상단에 나온 ThingDefs_Misc 경로에 있는 RangedIndustrial.xml 파일을 열어서 확인한다.  

    리볼버의 ThingDef (1.4 버전 기준)
    ```xml
    <ThingDef ParentName="BaseHumanMakeableGun">
      <defName>Gun_Revolver</defName>
      <label>revolver</label>
      <description>An ancient pattern double-action revolver. It's not very powerful, but has a decent range for a pistol and is quick on the draw.</description>
      <possessionCount>1</possessionCount>
      <graphicData>
        <texPath>Things/Item/Equipment/WeaponRanged/Revolver</texPath>
        <graphicClass>Graphic_Single</graphicClass>
      </graphicData>
      <uiIconScale>1.4</uiIconScale>
      <soundInteract>Interact_Revolver</soundInteract>
      <statBases>
        <WorkToMake>4000</WorkToMake>
        <Mass>1.4</Mass>
        <AccuracyTouch>0.80</AccuracyTouch>
        <AccuracyShort>0.75</AccuracyShort>
        <AccuracyMedium>0.45</AccuracyMedium>
        <AccuracyLong>0.35</AccuracyLong>
        <RangedWeapon_Cooldown>1.6</RangedWeapon_Cooldown>
      </statBases>
      <weaponTags>
        <li>SimpleGun</li>
        <li>Revolver</li>
      </weaponTags>
      <weaponClasses>
        <li>RangedLight</li>
      </weaponClasses>
      <costList>
        <Steel>30</Steel>
        <ComponentIndustrial>2</ComponentIndustrial>
      </costList>
      <recipeMaker>
        <skillRequirements>
          <Crafting>3</Crafting>
        </skillRequirements>
        <displayPriority>400</displayPriority>
      </recipeMaker>
      <verbs>
        <li>
          <verbClass>Verb_Shoot</verbClass>
          <hasStandardCommand>true</hasStandardCommand>
          <defaultProjectile>Bullet_Revolver</defaultProjectile>
          <warmupTime>0.3</warmupTime>
          <range>25.9</range>
          <soundCast>Shot_Revolver</soundCast>
          <soundCastTail>GunTail_Light</soundCastTail>
          <muzzleFlashScale>9</muzzleFlashScale>
        </li>
      </verbs>
      <tools>
        <li>
          <label>grip</label>
          <capacities>
            <li>Blunt</li>
          </capacities>
          <power>9</power>
          <cooldownTime>2</cooldownTime>
        </li>
        <li>
          <label>barrel</label>
          <capacities>
            <li>Blunt</li>
            <li>Poke</li>
          </capacities>
          <power>9</power>
          <cooldownTime>2</cooldownTime>
        </li>
      </tools>
    </ThingDef>
    ```

    리볼버 Bullet ThingDef (1.4 버전 기준)
    ```xml
    <ThingDef ParentName="BaseBullet">
      <defName>Bullet_Revolver</defName>
      <label>revolver bullet</label>
      <graphicData>
        <texPath>Things/Projectile/Bullet_Small</texPath>
        <graphicClass>Graphic_Single</graphicClass>
      </graphicData>
      <projectile>
        <damageDef>Bullet</damageDef>
        <damageAmountBase>12</damageAmountBase>
        <stoppingPower>1</stoppingPower>
        <speed>55</speed>
      </projectile>
    </ThingDef>
    ```

    RangedWeapon_PlagueGun.xml를 열고, 먼저 최상단에 아래의 코드를 작성한다.
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Defs></Defs>
    ```

    그리고 찾아온 리볼버 Bullet과 리볼버의 코드를 RangedWeapon_PlagueGun.xml의 Def 태그 사이에 복사 붙여넣기하고, defName과 label 등의 속성을 수정해준다.

        <Defs>(여기에 복사 붙여 넣기)</Defs>

## E. RangedWeapon_PlagueGun.xml 최종 완성본

  아래는 복사 붙여넣기해서 수정까지 완료한 RangedWeapon_PlagueGun.xml의 최종본이다.
  ```xml

  <?xml version="1.0" encoding="utf-8"?>
  <Def>
  <!-- 총알 -->
    <ThingDef ParentName="BaseBullet">
    <defName>TST_Bullet_PlagueGun</defName>
    <label>plague bullet</label>
    <graphicData>
      <texPath>Things/Projectile/Bullet_Small</texPath>
      <graphicClass>Graphic_Single</graphicClass>
    </graphicData>
    <projectile>
      <flyOverhead>false</flyOverhead>
      <damageDef>Bullet</damageDef>
      <damageAmountBase>12</damageAmountBase>
      <stoppingPower>1</stoppingPower>
      <speed>55</speed>
    </projectile>
    </ThingDef>

  <!-- 총 -->
    <ThingDef ParentName="BaseHumanMakeableGun">
    <defName>TST_Gun_PlagueGun</defName>
    <label>plague gun</label>
    <description>A curious weapon notable for its horrible health effects</description>
    <possessionCount>1</possessionCount>
    <graphicData>
      <texPath>Things/Item/Equipment/WeaponRanged/Revolver</texPath>
      <graphicClass>Graphic_Single</graphicClass>
    </graphicData>
    <uiIconScale>1.4</uiIconScale>
    <soundInteract>Interact_Revolver</soundInteract>
    <statBases>
      <WorkToMake>4000</WorkToMake>
      <Mass>1.4</Mass>
      <AccuracyTouch>0.80</AccuracyTouch>
      <AccuracyShort>0.75</AccuracyShort>
      <AccuracyMedium>0.45</AccuracyMedium>
      <AccuracyLong>0.35</AccuracyLong>
      <RangedWeapon_Cooldown>1.6</RangedWeapon_Cooldown>
    </statBases>
    <weaponTags>
      <li>SimpleGun</li>
      <li>Revolver</li>
    </weaponTags>
    <weaponClasses>
      <li>RangedLight</li>
    </weaponClasses>
    <costList>
      <Steel>30</Steel>
      <ComponentIndustrial>2</ComponentIndustrial>
    </costList>
    <recipeMaker>
      <skillRequirements>
        <Crafting>3</Crafting>
      </skillRequirements>
      <displayPriority>400</displayPriority>
    </recipeMaker>
    <verbs>
      <li>
        <verbClass>Verb_Shoot</verbClass>
        <hasStandardCommand>true</hasStandardCommand>
        <defaultProjectile>TST_Bullet_PlagueGun</defaultProjectile>
        <warmupTime>0.3</warmupTime>
        <range>25.9</range>
        <soundCast>Shot_Revolver</soundCast>
        <soundCastTail>GunTail_Light</soundCastTail>
        <muzzleFlashScale>9</muzzleFlashScale>
      </li>
    </verbs>
    <tools>
      <li>
        <label>grip</label>
        <capacities>
          <li>Blunt</li>
        </capacities>
        <power>9</power>
        <cooldownTime>2</cooldownTime>
      </li>
      <li>
        <label>barrel</label>
        <capacities>
          <li>Blunt</li>
          <li>Poke</li>
        </capacities>
        <power>9</power>
        <cooldownTime>2</cooldownTime>
      </li>
    </tools>
    </ThingDef>
  </Defs>

  ```
  
  defName, label, Bullet_Revolver 그리고 Revolver 속성을 변경해준다.
  위에서는 
  1. Bullet_Revolver - defName, label 변경, flyOverhead 추가, 
  2. Gun_Revolver - defName, label, description, defaultProjectile 변경,

<br>

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr>

