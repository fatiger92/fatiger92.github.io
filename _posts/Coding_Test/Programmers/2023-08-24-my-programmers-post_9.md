---
title:  "[알린이의 코테 입문 #033 - 036] 수학, 문자열, 해시, 완전탐색, 조건문"
excerpt: "프로그래머스"

categories:
  - Programmers
tags:
  - [Programmers]
toc: true
toc_sticky: true
date: 2023-08-24
last_modified_at: 2023-08-28
---

# 📍 수학, 문자열, 해시, 완전탐색, 조건문

## 🔎 문제 1. 개미 군단

![image1](/assets/images/posts/Coding_Test/Programmers/2023-08-24-my-programmers-post_9/1.png){: width="50%" height="50%"}

장군 개미 공격력 - 5  
병정 개미 공격력 - 3
일 개미 공격력 - 1

여치의 체력 - 23  

최적의 방법 - 장군개미 4 마리 = 20, 병정 개미 1 마리 = 3  

먼저 제일 큰수로 나누고 안 나눠지면 그 보다 작은 수로 0이 될 때까지 반복하면 되지 않을까?


## 🤔 나의 풀이 1번째 시도

```csharp
using System;

public class Solution {
    public int solution(int hp) {
       var answer = 0;
         while (hp > 0)
            {
                if (hp >= 5)
                {
                    hp -= 5;
                    answer++;
                    continue;
                }

                if (hp >= 3)
                {
                    hp -= 3;
                    answer++;
                    continue;
                }

                hp -= 1;
                answer++;
            }
        return answer;
    }
}
```

switch 표현식을 사용했으면 더 간결했을텐데 프로그래머스 코드에디터 C# 9.0을 지원하지 않는다.

## 😮 다른 사람의 풀이 확인

미리 하나하나 구해서 전부를 더하는 방법도 있음

### ⚒️ 1번 풀이

```csharp
using System;

public class Solution {
    public int solution(int hp) {
        var ant_5 = hp / 5;
        var ant_3 = hp % 5 / 3;
        var ant_1 = hp % 5 % 3 / 1;

        return ant_5 + ant_3 + ant_1;
    }
}
```
이렇게 하면 각각 몫만 가지게 되므로 값이 바로 나옴

## 🔎 문제 2. 모스 부호 (1)

![image2](/assets/images/posts/Coding_Test/Programmers/2023-08-24-my-programmers-post_9/2.png){: width="50%" height="50%"}

## 🤔 나의 풀이

```csharp
using System;
using System.Linq;

public class Solution {
    public string solution(string letter) {
        var answer = "";

        string[] mos = {".-", "-...", "-.-.", "-..", ".", "..-.", "--.", "....", "..", ".---", "-.-",
            ".-..", "--", "-.", "---", ".--.", "--.-", ".-.", "...", "-", "..-", "...-", ".--",
            "-..-", "-.--", "--.."};
        var alpha = Enumerable.Range(97, 122).Select(x => (char)x).ToArray();

        answer = letter.Split(' ').Aggregate(answer, (current, temp) => {
            current += alpha[Array.IndexOf(mos, temp)];
            return current;
        });

        return answer;
    }
}
```

LINQ를 사용했으니 배열을 쉽게 만들기 위해 Enumerable.Range 메서드를 사용했다.  
그리고 앞서 자주 사용했던 Aggregate 메서드를 사용했다.  

## 😮 다른 사람의 풀이 확인

내가 생각했던 부분과는 또 다른 방법으로 생각해서 풀이 하신 분이 있어서 그 풀이를 분석했음.  
그중에는 모스부호를 구분자를 사용해서 하나의 string 변수에 넣은 것도 있었다.  

확실히 배열을 여러개 만드는 것 보다 하나의 string 값으로 만드는 것도 좋은 방법이라고 생각한다.  
하지만 string 같은 경우 참조 형식이기 때문에 split 을 사용할 시 새로운 string 배열을 메모리에 만들어 낸다.   
따라서 split을 쓸거면 그냥 배열로 먼저 선언해도 문제 없다는 얘기를 하고 싶었다.  
그 후로는 취향의 차이다.  

### ⚒️ 1번 풀이

```csharp
using System;

public class Solution {
    public string solution(string letter) {
        string answer = "";
        string[] split = letter.Split(' ');
        string[] morse =  { 
    ".-","-...","-.-.","-..",".","..-.",
    "--.","....","..",".---","-.-",".-..",
    "--","-.","---",".--.","--.-",".-.",
    "...","-","..-","...-",".--","-..-",
    "-.--","--.."
};
        foreach(string s in split)
        {
            var temp = Array.FindIndex(morse, x=> x.Equals(s));
            answer += (char)(temp+97);
        }
        return answer;
    }
}
```

LINQ를 쓰지 않은 풀이법이다.  
먼저 매개변수로 받아온 string 값을 split 메서드를 사용해서 새로운 문자열 배열을 만들었다.  
그리고 foreach 문을 사용해서 새로 만든 배열을 순회한다.  
모스 배열을 기준으로 요소와 새로 만든 배열 요소와 비교해서 같다면 모스 배열 기준의 인덱스를 반환받는다.  
그리고 string 타입 answer 변수에 아스키코드로 매칭되는 소문자 알파벳을 더한다.  

여기까지가 분석한 내용이 끝이고, 코드를 분석하다가 깨달은 부분이 있다.  
주관적인 생각으로 사실 이것도 사실 고쳐야할 부분이 보인다.  
지금 같이 매개변수로 받는 문자열 값이 짧으면 상관없지만, 적당히 긴 문자열 값을 매개변수로 메서드가 호출된다면 string 변수를 += 하는 과정에서 new string 이 계속 발생하기 때문에 그리 좋지 않은 코드가 된다.  

따라서 이 때는 StringBuilder를 쓰는 것이 좋아 보인다.  

```csharp
using System;
using System.Text;

public class Solution {
    public string solution(string letter) {
        var sb = new StringBuilder();
        string[] morse =  { ".-","-...","-.-.","-..",".","..-.", "--.","....","..",".---","-.-",".-..", "--","-.","---",".--.","--.-",".-.", "...","-","..-","...-",".--","-..-", "-.--","--.." };
            
        foreach(string str in letter.Split(' '))
            sb.Append(Convert.ToChar(Array.IndexOf(morse, str) + 97));
            
        return sb.ToString();
    }
}
```

StringBuilder를 사용했고, 주관적으로 제일 간결하고 효과적인 코드라고 생각한다.  

## 🔎 문제 3. 가위 바위 보

![image3](/assets/images/posts/Coding_Test/Programmers/2023-08-24-my-programmers-post_9/3.png){: width="50%" height="50%"}

가위는 2  
바위는 0  
보는 5

매개변수로 주어지는 문자열 값에 매칭되는 가위바위보를 이길 수 있는 문자열 값을 반환해야함.  
어떻게 풀어야 할까?  

먼저 받아온 문자열 매개변수를 하나하나 분리해서 배열을 만들어 줘야함.

## 🤔 나의 풀이

```csharp
using System;

public class Solution {
    public string solution(string rsp) {
        var answer = "";
        var arrChars = rsp.ToCharArray();
            
        for (var i = 0; i < arrChars.Length; i++)
        {
            switch (arrChars[i])
            {
                case '0': answer += '5'; break;
                case '2': answer += '0'; break;
                case '5': answer += '2'; break;
            }
        }
        return answer;
    }
}
```

그냥 말그대로 문제를 풀기 위한 코드를 작성했다. 
매개변수로 받아온 문자열 값을 Char[] 타입으로 변환하고, 그 배열을 순회하면서 switch 분기를 돈다.  
이것 말고 다른 방법이 있을까?  

0, 2, 5 사이의 연관성이 따로 있는 건지 곰곰히 생각해봤음.  
0 = 5  
2 = 0  
5 = 2  

없다.  
뭔가 공통점을 찾아서 공식으로 만들고 싶지만, 현재 나의 관점에서는 보이지 않는다.  

## 😮 다른 사람의 풀이 확인

LINQ로 한줄로 끝낸 풀이가 있길래 가져와봤다.  

### ⚒️ 1번 풀이

```csharp
using System;
using System.Linq;

public class Solution {
    public string solution(string rsp) {
        var answer = string.Concat(rsp.Select(x => x == '2' ? '0' : x == '0' ? '5' : '2'));
        return answer;
    }
}
```
위와 같이 LINQ를 사용하면 간단하게 끝낼 수 있음.  

## 🔎 문제 4. 구슬을 나누는 경우의 수

![image4](/assets/images/posts/Coding_Test/Programmers/2023-08-24-my-programmers-post_9/4.png){: width="50%" height="50%"}

![image4-1](/assets/images/posts/Coding_Test/Programmers/2023-08-24-my-programmers-post_9/4-1.png){: width="50%" height="50%"}

- 매개변수  
머쓱이가 갖고 있는 구슬의 개수 = balls  
친구들에게 나누어 줄 구슬 개수 = share

- 결과값  
balls 개의 구슬 중 share 개의 구슬을 고르는 가능한 모든 경우의 수 

balls, share의 최소값은 1, 그리고 balls는 항상 share와 같거나 크다.
구슬을 고르는 순서는 고려하지 않는다.

팩토리얼이 들어가는 문제인 것 같다.  


## 🤔 나의 풀이 첫트

```csharp
using System;

public class Solution {
    public int solution(int balls, int share) {
        return Factorial(balls) / (Factorial(balls - share) * Factorial(share));
    }
    
    public int Factorial(int num)
    {
        var result = 1;   
            
        for (var i = 0; i < num; i++)
            result *= num - i;
            
        return result;
    }
}
```

결과는 아래와 같이 나왔다.

![image4-2](/assets/images/posts/Coding_Test/Programmers/2023-08-24-my-programmers-post_9/4-2.png){: width="50%" height="50%"}

42.9.. 예외가 존재한다는 건데..  
그 예외가 나올 수 있는 곳은 Factorial 메서드 본문일 수 밖에 없음.  

문제점을 알았는데, Factorial 메서드 본문 내 연산하는 과정에서 값이 int를 넘어서는 값이 나오기 때문에 값이 오염되는 현상이 발생.  
따라서 int의 범위안에서 해결하는 방법을 찾아야 한다.  

질문 답변을 어슬렁거리다 어떤 풀이를 봤고, 해당 내용이 전혀 이해가 가지 않았다.  

```csharp
public static int solution1(int balls, int share) {
    var answer = 1;
    var cnt = 1;

    // 카운트가 share보다 커질 때까지 반복
    while(cnt <= share) // 1, 2
    {
        // ball을 하나씩 줄이면서 answer에 곱해줌 == 팩토리얼
        answer *= balls--; 
        // 1, 3
        // 3, 2
        
        // cnt를 하나씩 늘리면서 answer에 나눠줌.
        answer /= cnt++;
        /*
        * 이 부분 연산을 왜 하는지 이해해야함.
        * 예를 들어 balls = 3, share = 2 라고 가정하자.
        * answer = 3, cnt = 1 = 3
        * answer = 6, cnt = 2 = 3
        */
    }
    return answer;
}
```

나름 분석을 해보려고 했는데, 진전이 딱히 없었다.  
공식을 보고 어떻게든 이해하려고 똥꼬쇼를 하다가 어떤 블로그 포스팅을 보는 순간 완전히 이해가 가버렸다.  

그 포스팅의 내용은 순열과 조합에 관련한 내용이었다.  

[🚀홍약사의 블로그 바로가기](https://m.blog.naver.com/baboedition/220929686431)
   <p style="font-size:12pt"> 👉순열과 조합에 대해서 </p>

오히려 공식을 보고 있던게 더욱 도움이 되지 않았다.  
왜냐하면 위에 작성된 코드는 공식을 기반으로 한게 아닌, 조합의 식에 더 가깝기 때문이다.  

이 개념을 알자 정말 아주 심플하게 이해해 버렸다.  

![image4-3](/assets/images/posts/Coding_Test/Programmers/2023-08-24-my-programmers-post_9/4-3.png){: width="50%" height="50%"}

예를 들어 balls가 7이고, share는 그보다 작거나 같은 수를 할당해야하니 대충 4라고 하겠음.  
그리고 저기 위에 보이는 공식에 집어 넣으면?  

![image4-4](/assets/images/posts/Coding_Test/Programmers/2023-08-24-my-programmers-post_9/4-4.png){: width="50%" height="50%"}

위와 같이 풀어 쓸 수 있고, 이건 사실  

![image4-5](/assets/images/posts/Coding_Test/Programmers/2023-08-24-my-programmers-post_9/4-5.png){: width="50%" height="50%"}

위의 이미지로 대체해서 쓸 수 있다.  
조합이라는 개념을 알고 있다면, 저 위의 쓸떼없이 긴 공식이 필요없는 것이다.  
그리고 위의 조합을 코드로 풀어쓴 것이 내가 아무리 분석해도 이해가 가지 않았었던 아래의 코드와 일치한다.  

```csharp
public static int solution1(int balls, int share) {
    var answer = 1;
    var cnt = 1;

    while(cnt <= share) 
    {
        answer *= balls--; 
        answer /= cnt++;
    }
    return answer;
}
```
조금 다른 점은 일관 연산을 하는 것이 아닌, 순차적으로 분모로 나누게 된다.  

## 🤔 나의 풀이 두번째 

```csharp
using System;

public class Solution {
    public int solution(int balls, int share) {
        return Combination(balls, share);
    }
    
    public int Combination(int balls, int share) {
        var answer = 1;
        var cnt = 1;

        while(cnt <= share) 
        {
            answer *= balls--; 
            answer /= cnt++;
        }
        return answer;
    }
}
```

사실 다 된줄 알았음~~~  

![image4-6](/assets/images/posts/Coding_Test/Programmers/2023-08-24-my-programmers-post_9/4-6.png){: width="50%" height="50%"}

![laughing](https://media.giphy.com/media/12msOFU8oL1eww/giphy.gif){: width="30%" height="30%"}

91.6 ?zzzzz  

혹시나 해서 balls = 30, share = 29를 넣고 돌려보니 아니나 다를까 -25가 나와버렸음.

## 🤔 나의 풀이 찐막

```csharp
using System;

public class Solution {
    public long solution(int balls, int share) {
        return Combination(balls, share);
    }
    
    public long Combination(int balls, int share) {
        var answer = 1L;
        var cnt = 1;

        while(cnt <= share) 
        {
            answer *= balls--; 
            answer /= cnt++;
        }
        return answer;
    }
}
```

네 성공입니다.  
long으로 바꿔도 되는 거 였으면, 진작 처음부터 그렇게 말을하라고 ㅡㅡ  
아무데도 안써놓으면 마치 int로 제한을 준 것처럼 보이잖아?  

## 😮 다른 사람의 풀이 확인

1번 풀이는 팩토리얼에서 봤던 재귀를 사용하는 방법이다.  

### ⚒️ 1번 풀이

```csharp
using System;
using System.Numerics;

public class Solution 
{
    public int solution(int balls, int share)
    {
        return Combination(balls, share);
    }

    public int Combination(int n, int m)
    {
        if (m == 0 || n == m) 
            return 1;

        return Combination(n - 1, m - 1) + Combination(n - 1, m);
    }
}
```

2번 풀이는 왜 재귀를 쓴 걸까?

### ⚒️ 2번 풀이

```csharp
using System;

public class Solution {
    public long solution(int balls, int share) { 
        share = Math.Min(balls - share, share);

        if (share == 0)
            return 1;

        long answer = solution(balls - 1, share - 1);
        answer *= balls;
        answer /= share;

        return answer;
    }
}
```

Math.min() 메서드를 사용했는데, balls = 7, share = 4로 가정해보자.  
`share = Math.Min(7 - 4, 4)` 이 부분에서 `share = 3` 이된다.  
`share != 0` 이므로 if문을 지나치고, 현재 메서드를 다시 불러낸다.  
매개변수는 `balls = 7 - 1 = 6`, share = `4 - 1 = 3`   
answer 는 갑자기 재귀, 트리거는 share가 0이 됐을 때, 실질적인 연산은 answer 줄 부터 시작.  

왜 때문에? 재귀?

```csharp
using System;

public class Solution {
    public long solution(int balls, int share) { 
        var answer = 1L;
        var cnt = 1;
            
        while(cnt <= share)
        {
            answer *= balls--; 
            answer /= cnt++;
        }
        return answer;
    }
}
```
그냥 이렇게 좀 깔끔하게 쓰면 안됨? 재귀를 꼭 써야만하는 이유가 있을까? 취향존중 하지만 나는 모르겠다.  

## 🤣 한마디

재귀함수에 대한 내 생각은 좀 부정적이다.  
구시대의 유산 같은 느낌이고, 사실상 반복문보다 느리고 시간복잡도도 나쁘다.  
또한 실질적으로 코드를 작성할 때 사용하지 않는다고는 단정지을 수 없으나, 내가 봐왔던 프로젝트에서 재귀를 본적이 정말 손에 꼽을정도로 잘 사용하지 않는다.  

엄청난 베네핏이 있다면 반복문 대신 사용하겠지만, 장단점을 보면 "왜 사용하는가?"에 대해 더욱이 이해가 안간다.

- 재귀함수의 장점  
1. 변수를 여럿 만들 필요가 없다.
2. while문이나 for문같은 반복문을 사용하지 않아도 되기에 코드가 간결해진다.

- 재귀함수의 단점  
1. 지속적으로 함수를 호출하게 되면서 지역변수, 매개변수, 반환값을 모두 stack에 저장해야하며, 이런 과정은 선언한 변수의 값만 사용하는 반복문에 비해서 메모리를 더 많이 사용하게 되고, 이는 속도저하로 이어진다.
2. 함수 호출 -> 복귀를 위한 컨텍스트 스위칭에 비용이 발생하게 된다.

장점을 씹어먹을 만큼의 치명적인 단점을 가지고 있다.  
이건 그저 개발자가 자신의 코딩 관련 지식을 뽐내기 위해서 사용하는 용도 그이상 그이하도 아닌 기능이라 생각한다.  
게다가 현업이 아닌 알고리즘 문제 또는 코딩 테스트에서만 사용하기 위해 배운다는 것도 어불성설이다.  

물론 재귀함수의 단점을 보완하기 위한 꼬리 재귀(tail call resursion) 이라는 방법도 있다.    
이렇게까지 재귀를 쓸거면 그냥 반복문으로 끝나도록 설계를 하는 것이 타당하다고 생각한다.  

가독성이 좋다고 하는데, 사실 그냥 반복문보다 가독성이 좋은지 1도 모르겠음.  

<br>

<hr style="width:100%" />

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr style="width:100%" />

[Top](#){: .btn .btn--primary }{: .align-right}