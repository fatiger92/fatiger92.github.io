---
title:  "[#001] 옵시디언 깃에 연동하기"
excerpt: "Oh Yeah"

categories:
  - Obsidian
tags:
  - [Obsidian]
toc: true
toc_sticky: true
date: 2023-08-29
last_modified_at: 2023-08-29
---

# 😮 OBSIDIAN!!!!!!!!!!!!!!!
<hr style="width:100%" />

This app is actually extremely famous these days.  
The app is an application with help you memo which is based on the mark-down.

I used the Notion application for the memo, but Notion had a critical problem that was so slow especially when you wrote long code.

So I'm finding an app that is not delayed and is fast.  
And I think I found a certain app.

Sooooooooooooo excited, let's move on!

# 🥱 Let's connect Obsidian to the git repo for backup and synchronization.
<hr style="width:100%" />

<strong style="color:orange; font-size:15pt">Yayyyyyyyyyyyyy</strong>

먼저 옵시디언을 설치해야함.  
아래 링크를 이용한다. 

[🔔 Obsidian official web site ](https://obsidian.md/)
{: .notice--warning}

나는 윈도우 환경에서 진행할 것임.  
깃과 깃허브 데스크탑이 전부 설치되어 있다는 가정하에 기록할 것임.


옵시디언이 설치되었다면 아래의 이미지에 있는 설정을 누른다.

![img01](/assets\images\posts\ETC\Obsidian\2023-08-29-my-obsidian-post_0/1.jpg){: width="50%" height="50%"}

그리고 커뮤니티 플러그인 사용을 Turn On 해줘야함.

![img02](/assets\images\posts\ETC\Obsidian\2023-08-29-my-obsidian-post_0/2.jpg){: width="50%" height="50%"}

Turn On을 했다면 이제 obsidian git 플러그인을 찾으러 ㄱㄱ

![img03](/assets\images\posts\ETC\Obsidian\2023-08-29-my-obsidian-post_0/3.jpg){: width="50%" height="50%"}

이제 검색을 해서 install 한다.

![img04](/assets\images\posts\ETC\Obsidian\2023-08-29-my-obsidian-post_0/4.jpg){: width="50%" height="50%"}

obsidian git이 설치가 완료되었다면 Enable을 해줘야 플러그인이 정상 작동한다.  

![img05](/assets\images\posts\ETC\Obsidian\2023-08-29-my-obsidian-post_0/5.jpg){: width="50%" height="50%"}

그리고 깃허브에서 옵시디언용 레포를 하나 만든다.  
깃 레포를 만들면 .git 파일이 하나 딸랑 들어있는 레포가 만들어질 것이다.(선택에 따라 Readme.md 가 있을지도..)  
로컬 어딘가 양지바른 곳에 클론을 만들고 .git의 경로까지 들어가서 경로를 복사해둔다.  

나의 경우
>D:\WorkSpace\git\TigerLab\.git

그리고 옵시디언으로 돌아가서 새로운 옵시디언 vault를 만들어 줘야한다.  
아래 이미지에서 빨간 동그라미가 쳐진 수상한 아이콘을 눌러야함.  

![img06](/assets\images\posts\ETC\Obsidian\2023-08-29-my-obsidian-post_0/6.png){: width="50%" height="50%"}

그리고 Create를 누르면,

![img07](/assets\images\posts\ETC\Obsidian\2023-08-29-my-obsidian-post_0/7.png){: width="50%" height="50%"}

1번은 메모가 관리되어질 메인 파일명, 2번은 저장 경로, 3번은 만들기 버튼이다.  
하지만 여기서 1번, 2번은 지금 그렇게 중요하지 않다.   

![img08](/assets\images\posts\ETC\Obsidian\2023-08-29-my-obsidian-post_0/8.png){: width="50%" height="50%"}

나는 Wow라는 파일명으로 만들 것이고, 저장 경로를 지정하면 2번에 선택한 저장 경로가 하이라이트되어 나오는 것을 볼 수 있다.  

![img09](/assets\images\posts\ETC\Obsidian\2023-08-29-my-obsidian-post_0/9.png){: width="50%" height="50%"}

새로운 Vault를 만들면 옵시디언 에디터가 새로 켜진다.  
나도 새로 해보면서 깨달았는데, 옵시디언은 각 메타파일 같은 것이 존재하고 각각 별개로 관리되어진다.  
따라서 새로 만들었으니 그 메타데이터에 맞는 에디터가 켜진 것.  

켜진 에디터에서 파일명을 확인하고 수상한 아이콘을 클릭한다. 

![img10](/assets\images\posts\ETC\Obsidian\2023-08-29-my-obsidian-post_0/10.png){: width="50%" height="50%"}

해당 버튼을 클릭하면, 현재 vault가 있는 경로를 보여주는 탐색기가 열린다.  

![img11](/assets\images\posts\ETC\Obsidian\2023-08-29-my-obsidian-post_0/11.png){: width="50%" height="50%"}

탐색기의 모습.

![img12](/assets\images\posts\ETC\Obsidian\2023-08-29-my-obsidian-post_0/12.png){: width="50%" height="50%"}

경로가 확인됐으면 직접 그 경로로 들어가보면 .obsidian 이라는 폴더가 덩그러니 하나 있는 걸 확인할 수 있는데, 이게 메타데이터(?) 같은 것이다.  
깃으로 예를 들면 .git 같은 녀석이랄까  

![img13](/assets\images\posts\ETC\Obsidian\2023-08-29-my-obsidian-post_0/13.png){: width="50%" height="50%"}

열려있는 obsidian 에디터를 끄고 이녀석을 옮길 건데, 기존에 만들어 놓았던 레포로 옮긴다.  

![img14](/assets\images\posts\ETC\Obsidian\2023-08-29-my-obsidian-post_0/14.png){: width="50%" height="50%"}

그리고 옵시디언 에디터를 바탕화면에서 아이콘으로 열면, 런처 같은게 열린다.  

![img15](/assets\images\posts\ETC\Obsidian\2023-08-29-my-obsidian-post_0/15.png){: width="50%" height="50%"}

탐색기에서 자신의 레포지토리가 있는 경로를 찾아간다. 

![img16](/assets\images\posts\ETC\Obsidian\2023-08-29-my-obsidian-post_0/15.png){: width="50%" height="50%"}

여기까지 하면 레포에 연결된 옵시디언 vault를 만드는 게 끝인줄 알았으나?  
아까 메타데이터에 대해서 얘기한게 혹시 ㄱ이 나는지?  
메타데이터안에는 플러그인의 설치 유무를 기록하는 부분이 있기 때문에, 새로운 vault를 만들었다면 우리가 설치한 obsidian git 플러그인도 없어져 있을 것이다.  

위에 있는 설치법을 보고 다시 설치한다.  
그리고 깃 레포 로컬 폴더 위치를 설정해줘야 하는데, 아래의 이미지 순서대로 따라한다.  

![img17](/assets\images\posts\ETC\Obsidian\2023-08-29-my-obsidian-post_0/17.png){: width="50%" height="50%"}

![img18](/assets\images\posts\ETC\Obsidian\2023-08-29-my-obsidian-post_0/18.png){: width="50%" height="50%"}

아까 전에 .git의 경로까지 들어가서 복사해둔 경로를 복붙  

![img19](/assets\images\posts\ETC\Obsidian\2023-08-29-my-obsidian-post_0/19.png){: width="50%" height="50%"}

여기까지 하면 연동 끝.  

마지막으로 obsidian git 플러그인 옵션에서 업데이트 주기만 바꿔주자

![img20](/assets\images\posts\ETC\Obsidian\2023-08-29-my-obsidian-post_0/20.png){: width="50%" height="50%"}

근데 사실 생각해보면 옵시디언 vault를 먼저 만들고 그 로컬 파일을 로컬에서 git 레포로 add repo해서 만들어도 상관없었음.  
서순상 그게 더 괜찮은 방법인 것 같지만, 나도 처음이었으니 으쯜쓰그읎음

<br>
<strong style="color:yellow; font-size:100pt;">Done</strong>
<br>

# 🔎 참고한 내용
<hr style="width:100%" />

[🔔 dannyhatcher 블로그](https://dannyhatcher.com/obsidian-git-for-beginners/)
{: .notice--warning}


<hr style="width:100%" />

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr style="width:100%" />

[Top](#){: .btn .btn--primary }{: .align-right}