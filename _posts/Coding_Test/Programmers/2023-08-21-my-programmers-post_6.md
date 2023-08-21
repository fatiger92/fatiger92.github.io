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

## 🔎 문제 1. 문자열 뒤집기

![image1](/assets/images/posts/Coding_Test/Programmers/2023-08-21-my-programmers-post_6/1.png){: width="50%" height="50%"}

## 🤔 나의 풀이 1번째 시도

```csharp
using System;

public class Solution {
    public string solution(string my_string) {
       var arrChar = my_string.ToCharArray();
       Array.Reverse(arrChar);
       return new string(arrChar);
    }
}
```

이전에 사용했던 `Array.Reverse()` 메서드를 사용해서 순서를 뒤바꾼다.


## 😮 다른 사람의 풀이 확인

LINQ를 사용해서 한 줄에 끝내는 법도 있었다.  

### ⚒️ 1번 풀이

```csharp
using System;
using System.Linq;

public class Solution
{
    public string solution(string my_string)
    {
        return new string(my_string.Reverse().ToArray());
    }
}
```

for문을 거꾸로 돌려서 수동으로 하는 방법도 있었는데, 굳이? 라는 생각이 들었다.  
그 코드를 보고 느낀 건 옆에 망치가 있는데 고집부리면서 돌로 못을 박고 있는 느낌이었다.  

## 🔎 문제 2. 직각삼각형 출력하기

![image2](/assets/images/posts/Coding_Test/Programmers/2023-08-21-my-programmers-post_6/2.png){: width="50%" height="50%"}

## 🤔 나의 풀이

```csharp
using System;

public class Example
{
    public static void Main()
    {
        var num = Console.ReadLine();
            
        if (num == null)
            return;

        for (int i = 0; i < int.Parse(num); i++)
        {
            for (int j = 0; j < i + 1; j++)
            {
                Console.Write("*");
            }
            Console.WriteLine();
        }
    }
}
```

코드가 정말 다 비슷비슷했음.

## 🔎 문제 3. 짝수 홀수 개수

![image3](/assets/images/posts/Coding_Test/Programmers/2023-08-21-my-programmers-post_6/3.png){: width="50%" height="50%"}

## 🤔 나의 풀이

```csharp
using System;

public class Solution {
    public int[] solution(int[] num_list) {
       var even = 0;
       var odd = 0;

       foreach (var num in num_list)
       {
         if (num % 2 == 0)
            even++;
         else
            odd++;
       }
    
       return new [] {even, odd};
    }
}
```

나는 이렇게 풀어보았음.  

## 😮 다른 사람의 풀이 확인

LINQ를 사용해서 한 줄에 끝내는 법도 있었다.  

### ⚒️ 1번 풀이

```csharp
using System;
using System.Linq;

public class Solution {
    public int[] solution(int[] num_list) {
        int[] answer = new int[2] { num_list.Count(x => x % 2 == 0), num_list.Count(x => x % 2 == 1) };

        return answer;
    }
}
```
미리 배열의 수를 정해두고 Count() 메서드를 사용하는 방법도 있음.  

## 🔎 문제 4. 문자 반복 출력하기

![image4](/assets/images/posts/Coding_Test/Programmers/2023-08-21-my-programmers-post_6/4.png){: width="50%" height="50%"}

## 🤔 나의 풀이

```csharp
using System;
using System.Text;

public class Solution {
    public string solution(string my_string, int n) {
        var arrChar = my_string.ToCharArray();
        var sb = new StringBuilder();
        
        for(var i = 0; i < arrChar.Length; i++)
        {
            for(var j = 0; j < n; j++)
                sb.Append(arrChar[i]);
        }
        
        return sb.ToString();
    }
}
```

StringBuilder를 이용해서 문제를 해결했음.  

## 😮 다른 사람의 풀이 확인

내가 알아채지 못한 방법으로 해결한 풀이법이 있어서 봄.

### ⚒️ 1번 풀이

```csharp
using System;

public class Solution {
    public string solution(string my_string, int n) {
        string answer = "";

        foreach (var c in my_string)
        {
            answer += new string(c, n);
        }

        return answer;
    }
}
```

이게 어째서..?

![image4-1](/assets/images/posts/Coding_Test/Programmers/2023-08-21-my-programmers-post_6/4-1.png){: width="50%" height="50%"}

그냥 string 내장 메서드에 count 만큼 반복하는게 이미 구현되어 있었다.   
이런 내장메서드가 있다고 한다면 LINQ를 사용할 경우 사실 한 줄로 끝난다.

```csharp
using System;
using System.Linq;

public class Solution {
    public string solution(string my_string, int n) {
        return my_string.Aggregate("", (current, c) => current + new string(c, n));
    }
}
```

위와 같이 말이다.

### Enumerable.Aggregate에 대한 정리 

Enumerable.Aggregate 는 영어를 모국어로 사용하지 않는 개발자에겐 직관적으로 와닿지 않는 메서드임.  
이 메서드가 제공하는 기능은 합계, 총액이 아닌 작은 단위가 누적되어 합쳐져 하나를 이루는 것임.  
따라서 Enumerable.Aggregate 메서드는 Enumerable.Sum 과는 확실한 차이를 가짐.  

예시는 아래와 같다.

```csharp
string[] fruits = { "apple", "mango", "orange", "passionfruit", "grape" };

string line = fruits.Aggregate((line, fruit) => line + ", " + fruit);

// line = apple, mango, orange, passionfruit, grape
```

Enumerable.Aggregate 메서드는 위 예제와 같이 foreach 문보다 짧고 간결하게 string을 연결하여 문장을 만들 수 있다.  

아래는 조건에 따라 누적시키는 예시이다.  

```csharp
double startBalance = 100.0;

int[] attemptedWithdrawals = { 20, 10, 40, 50, 10, 70, 30 };

double endBalance =
    attemptedWithdrawals.Aggregate(startBalance,
        (balance, nextWithdrawal) =>
            ((nextWithdrawal <= balance) ? (balance - nextWithdrawal) : balance));
```

위와 같이 nextWithdrawal <= balance 결과에 따라 누적시킬지 말지를 결정할 수도 있다.  

## 🤣 한마디


<br>

<hr style="width:100%" />

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr style="width:100%" />

[Top](#){: .btn .btn--primary }{: .align-right}