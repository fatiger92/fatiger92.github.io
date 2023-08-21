---
title:  "[알린이의 코테 입문 #017 - 020] 수학, 배열"
excerpt: "프로그래머스"

categories:
  - Programmers
tags:
  - [Programmers]
toc: true
toc_sticky: true
date: 2023-08-21
last_modified_at: 2023-08-21
---

# 📍 수학, 배열

## 🔎 문제 1. 옷가게 할인 받기

![image1](/assets/images/posts/Coding_Test/Programmers/2023-08-21-my-programmers-post_5/1.png){: width="50%" height="50%"}

## 🤔 나의 풀이 1번째 시도

```csharp
using System;

public class Solution {
    public int solution(int price) 
    {
        var answer = 0;
        if(price >= 100000 && price < 300000)
            answer = price - price / 100 * 5;
        else if(price >= 300000 && price < 500000)
            answer = price - price / 100 * 10;
        else if(price >= 500000)
            answer = price - price / 100 * 20;
        else 
            answer = price;
        return answer;
    }
}
```
이렇게 했는데, 내가 간과한 부분이 있다.  

![image1-1](/assets/images/posts/Coding_Test/Programmers/2023-08-21-my-programmers-post_5/1-1.png){: width="50%" height="50%"}

"price는 10원 단위로(1의 자리가 0) 주어집니다." 이걸 보고 문득 떠오른게 만약 5%, 10%, 20%가 소수점으로 내려갈 경우, int값으로 연산할 경우 부정확한 값이 나올 수도 있겠다는 생각이 들었다.  

```csharp
using System;

public class Solution {
    public int solution(int price) 
    {
        double answer;
        if(price >= 100000 && price < 300000)
            answer = price - (double)price / 100 * 5;
        else if(price >= 300000 && price < 500000)
            answer = price - (double)price / 100 * 10;
        else if(price >= 500000)
            answer = price - (double)price / 100 * 20;
        else 
            answer = price;
        return (int)answer;
    }   
}
```

따라서 double로 받아서 마지막에 넘길 때 int형으로 변환한다.

![image1-2](/assets/images/posts/Coding_Test/Programmers/2023-08-21-my-programmers-post_5/1-2.png){: width="50%" height="50%"}

## 😮 다른 사람의 풀이 확인

### ⚒️ 1번 풀이

```csharp
using System;

public class Solution {
    public int solution(float price) {
        float  answer = 0;
        answer = price >= 500000 ? price * 0.8f : price >= 300000 ? price * 0.9f : price >= 100000 ? price * 0.95f : price;   
        return (int)answer;
    }
}
```

이 풀이를 보고 느낀 것은 double까지 쓸 필요가 없고 float까지만 써도 됐었음.  
무한의 삼항 연산자를 썼음.  

그리고 5%, 10%, 20% 를 역으로 생각해서 0.95f, 0.9f, 0.8f을 곱한 값으로 비교를 함.  

1. 값이 500,000 이상일 경우 price * 0.8f
2. 값이 500,000 이상이 아닐 경우 price >= 300,000 
3. 값이 300,000 이상일 경우 price * 0.9f
4. 값이 300,000 이상이 아닐 경우 price >= 100,000
5. 값이 100,000 이상일 경우 price * 0.95f
6. 값이 100,000 이상이 아닐 경우 price

answer를 반환.

### ⚒️ 2번 풀이

```csharp
using System;

public class Solution {
    public int solution(int price) {
        if (500000 <= price)
        {
            return (int)(price * 0.8f);
        }
        else if (300000 <= price)
        {
            return (int)(price * 0.9f);
        }
        else if (100000 <= price)
        {
            return (int)(price * 0.95f);
        }

        return price;
    }
}
```

1번 풀이 코드를 if-else if로 풀어쓴 것.  

## 🔎 문제 2. 아이스 아메리카노

![image2](/assets/images/posts/Coding_Test/Programmers/2023-08-21-my-programmers-post_5/2.png){: width="50%" height="50%"}

## 🤔 나의 풀이

```csharp
using System;

public class Solution {
    public int[] solution(int money) {
       var cups = (int)Math.Truncate(money / 5500d);
       return new[]{cups, money - 5500 * cups};
    }
}
```

위와 같이 해도 상관 없지만 더 깔끔한 방법이 있어서 가져옴.  

## 😮 다른 사람의 풀이 확인

### ⚒️ 1번 풀이

```csharp
using System;

public class Solution {
    public int[] solution(int money) {
        int[] answer = new int[2] { money / 5500, money % 5500 };
        return answer;
    }
}
```

비슷하지만 이게 더 나은 것 같음.  

## 🔎 문제 3. 나이 출력

![image3](/assets/images/posts/Coding_Test/Programmers/2023-08-21-my-programmers-post_5/3.png){: width="50%" height="50%"}

## 🤔 나의 풀이

```csharp
using System;

public class Solution {
    public int solution(int age) {
        return 2022 - age + 1;
    }
}
```
## 🔎 문제 4. 배열 뒤집기

![image4](/assets/images/posts/Coding_Test/Programmers/2023-08-21-my-programmers-post_5/4.png){: width="50%" height="50%"}

## 🤔 나의 풀이

```csharp
using System;

public class Solution {
    public int[] solution(int[] num_list) {
       Array.Reverse(num_list);
       return num_list;
    }
}
```

## 🤣 한마디

3, 4번은 쉬웠다.

<br>

<hr style="width:100%" />

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr style="width:100%" />

[Top](#){: .btn .btn--primary }{: .align-right}