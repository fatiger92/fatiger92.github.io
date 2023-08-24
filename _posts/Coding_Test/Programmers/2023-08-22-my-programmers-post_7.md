---
title:  "[알린이의 코테 입문 #025 - 028] 문자열, 조건문, 수학, 반복문"
excerpt: "프로그래머스"

categories:
  - Programmers
tags:
  - [Programmers]
toc: true
toc_sticky: true
date: 2023-08-22
last_modified_at: 2023-08-23
---

# 📍 문자열, 조건문, 수학, 반복문

## 🔎 문제 1. 특정 문자 제거하기

![image1](/assets/images/posts/Coding_Test/Programmers/2023-08-22-my-programmers-post_7/1.png){: width="50%" height="50%"}

## 🤔 나의 풀이 1번째 시도

```csharp
using System;

public class Solution {
    public int[] solution(int[] numbers, int num1, int num2) {
        var answer = new int[num2];
        var count = 0;

        for (var i = num1; i < num2 + 1; i++)
            answer[count++] = numbers[i];

        return answer;
    }
}
```

이렇게 시도를 했는데, 실패가 나왔다.  
그래서 가만히 생각해보니,  

[1, 2, 3, 4, 5] 일 경우 1, 3을 입력받으면 [2, 3, 4]를 반환됨.  
따라서 num1, num2 을 사용해 총 반복 수를 구하는 식을 생각해야함.  

1 , 3 = 3  
1 , 2 = 2  
2 , 4 = 3  
2 , 5 = 4  
2 , 6 = 5  

num2가 배열의 길이가 되는 것은 잘못되었음.  
숫자와 숫자 사이의 값을 구해야함.  
num2 - num1 + 1   

## 🤔 나의 풀이 2번째 시도

```csharp
using System;

public class Solution {
    public int[] solution(int[] numbers, int num1, int num2) {
        var answer = new int[num2 - num1 + 1];
        var count = 0;

        for (var i = num1; i < num2 + 1; i++)
            answer[count++] = numbers[i];

        return answer;
    }
}
```

성공

## 🔎 문제 2. 각도기

![image2](/assets/images/posts/Coding_Test/Programmers/2023-08-22-my-programmers-post_7/2.png){: width="50%" height="50%"}

## 🤔 나의 풀이

```csharp
using System;

public class Solution {
    public int solution(int angle) {
       var quotient = (int)Math.Truncate((double)angle / 90);

        switch (quotient)
        {
            case 0:
                return 1;
            case 1:
                return angle % 90 != 0 ? 3 : 2;
            case 2:
                return 4;
        }

        return 0;
    }
}
```

if - else if를 써도 되지만 그냥 새로운 방법을 써보고 싶었다.
 
## 😮 다른 사람의 풀이 확인

삼항의 중첩을 사용한 풀이다.  

### ⚒️ 1번 풀이

```csharp
using System;

public class Solution {
    public int solution(int angle) {
        int answer = angle < 90 ? 1 : angle == 90 ? 2 : angle < 180 ? 3 : 4;
        return answer;
    }
}
```

나도 삼항의 중첩으로 할 수 있겠다 생각은 했으나 조건을 찾지 못했음.  
문제를 보고 조건을 바로 찾아내는 것의 중요함을 깨달음.  

### ⚒️ 2번 풀이

```csharp
using System;

public class Solution {
    public int solution(int angle) {
        int answer = angle / 90 + 1 + (angle > 90 ? 1 : 0);
        return answer;
    }
}
```

위의 코드보다 조금 더 깔끔하다.  
하지만 가독성은 그리 좋지 않다.  

코드를 읽어보자.
angle 값은 91로 가정한다.  

91 / 90 은 1.011111111111111이지만 int 간의 연산이므로 1로 인식된다.  
2 + (91 > 90 ? 1 : 0) => 2 + 1 = 3;  
return 값은 3이 된다.

그렇다면 angle 값이 180일 경우로 가정해보자.  
180 / 90 은 2 다.  
3 + (180 > 90 ? 1 : 0) => 3 + 1 = 4;
return 값은 4가 된다.

검증 완료

## 🔎 문제 3. 양꼬치

![image3](/assets/images/posts/Coding_Test/Programmers/2023-08-22-my-programmers-post_7/3.png){: width="50%" height="50%"}

## 🤔 나의 풀이

```csharp
using System;

public class Solution {
    
    readonly int nSheepPrice = 12000;
    readonly int nDrinkPrice = 2000;
    
    public int solution(int n, int k) {
   
        for(int i = 1; i <= n; i++)
        {
            if(i % 10 == 0)
                k--;
        }        
        return n * nSheepPrice + k * nDrinkPrice;
    }
}
```

나머지 연산자를 잘 이용해봐야함~  

## 🔎 문제 4. 짝수의 합

![image4](/assets/images/posts/Coding_Test/Programmers/2023-08-22-my-programmers-post_7/4.png){: width="50%" height="50%"}

## 🤔 나의 풀이

```csharp
using System;

public class Solution {
    public int solution(int n) {
        var answer = 0;
        for(int i = 0; i <= n; i++)
        {
            if(i % 2 == 0)
                answer += i; 
        }        
        return answer;
    }
}
```

## 🤣 한마디

조건을 캐치하는 법을 조금 더 담금질 ㄱㄱ  
<br>

<hr style="width:100%" />

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr style="width:100%" />

[Top](#){: .btn .btn--primary }{: .align-right}