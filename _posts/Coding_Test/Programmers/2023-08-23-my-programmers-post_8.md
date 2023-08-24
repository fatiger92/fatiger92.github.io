---
title:  "[알린이의 코테 입문 #029 - 032] 배열, 구현, 수학"
excerpt: "프로그래머스"

categories:
  - Programmers
tags:
  - [Programmers]
toc: true
toc_sticky: true
date: 2023-08-23
last_modified_at: 2023-08-24
---

# 📍 문자열, 조건문, 수학, 반복문

## 🔎 문제 1. 배열 자르기

![image1](/assets/images/posts/Coding_Test/Programmers/2023-08-23-my-programmers-post_8/1.png){: width="50%" height="50%"}

## 🤔 나의 풀이 1번째 시도

```csharp
using System;

public class Solution {
    public string solution(string my_string, string letter) {
        return my_string.Replace(letter, "");
    }
}
```

이건 막히는 게 없었음.

## 🔎 문제 2. 외계행성의 나이

![image2](/assets/images/posts/Coding_Test/Programmers/2023-08-23-my-programmers-post_8/2.png){: width="50%" height="50%"}

## 🤔 나의 풀이

```csharp
using System;
using System.Text;

public class Solution {
    public string solution(int age) {
         var sb = new StringBuilder();
         var arrChar = age.ToString().ToCharArray();
         var arrInteger = new int[arrChar.Length];

         for (var i = 0; i < arrInteger.Length; i++)
             arrInteger[i] = Convert.ToInt32(arrChar[i]) + 49;

         foreach (var data in arrInteger)
             sb.Append(Convert.ToChar(data));

         return sb.ToString();
    }
}
```
일단 정수형 값으로 받은 것을 char로 변환, 그리고 int[] 로 바꿔서 넣어주고  
아스키 코드를 사용해서 숫자와 소문자 알파벳의 차를 구해서 더해주는 것으로 풀어봤다.  

하고 나니 일단은 문제를 풀었는데, 뭔가 지저분하게 코드를 작성한 느낌이다.

아래는 아스키 코드표이다.

![image2](/assets/images/posts/Coding_Test/Programmers/2023-08-23-my-programmers-post_8/2-1.png){: width="50%" height="50%"}

## 😮 다른 사람의 풀이 확인

발상의 전환 풀이이다.

### ⚒️ 1번 풀이

```csharp
using System;

public class Solution {
    public string solution(int age) {
        string answer = "";
        while(age > 0)
        {
            answer = (char)(age % 10 + 97) + answer;
            age /= 10;
        }
        return answer;
    }
}
```

코드를 읽어보면 age가 0보다 크면  while 본문을 반복한다.  
age가 23이라고 가정했을 때, while 문은 반복을 하고 본문이 실행된다.  

`answer = (char)(23 % 10 + 97) + answer`  
위의 코드에서 %는 +보다 우선순위가 높고, 현시점에서 마지막의 answer의 값은 없다.  
10으로 잘라서 비교하고 97을 더해서 string에 계속 합친다.

이렇게 생각을 할 수 있다는 자체가 너무 신기했다.  
아마도 이 문제에 대해서 여러방면으로 어떻게하면 더 효율적일지 다른 방법은 없을지 많이 고민해서 나온 풀이법인 것 같다는 생각이 들었음.  

## 🔎 문제 3. 진료순서 정하기

![image3](/assets/images/posts/Coding_Test/Programmers/2023-08-23-my-programmers-post_8/3.png){: width="50%" height="50%"}

위의 문제는 정렬을 하는 문제이다.  
매개변수로 받는 정수 배열의 요소가 가장 큰 순서대로 1 ~ ...의 숫자를 매긴다.
그리고 배열에 담아 반환한다.

LINQ를 사용할 수도 있고, 그냥 배열로 해결할 수도 있다.  
나는 후자의 방법으로 풀어볼 것이다.

여기서 중요한 건 뭘까?  
먼저 크기순으로 정렬을 해서 순서를 매겨야 한다고 생각했다.  


## 🤔 나의 풀이

```csharp
using System;

public class Solution {
    public int[] solution(int[] emergency) {
        var answer = new int[emergency.Length];
        var arrTemp = new int[emergency.Length];
        Array.Copy(emergency, arrTemp, emergency.Length);

        // 1. 버블 정렬
        for (var i = 0; i < arrTemp.Length; i++)
        {
            for (var j = 0; j < arrTemp.Length - 1; j++)
            {
                if (arrTemp[j] < arrTemp[j + 1])
                    {
                        var temp = arrTemp[j];
                        arrTemp[j] = arrTemp[j + 1];
                        arrTemp[j + 1] = temp;
                    }
            }
        }

        // 2. 2중 for문으로 순서를 찾아서 거기에 값을 넣는다.
        for (var i = 0; i < arrTemp.Length; i++)
        {
            for (var j = 0; j < emergency.Length; j++)
            {
                if (arrTemp[i] == emergency[j])
                    answer[j] = i + 1;
            }
        }
        return answer;
    }
}
```

일단 한번에 정답을 맞추긴 했는데, 역시나 코드가 너무 길다.  
따라서 Array 함수로 코드를 줄여보자면,  

```csharp
using System;

public class Solution {
    public int[] solution(int[] emergency) {
        var answer = new int[emergency.Length];
        var arrTemp = new int[emergency.Length];
        Array.Copy(emergency, arrTemp, emergency.Length);
        Array.Sort(arrTemp);
        Array.Reverse(arrTemp);
        
        for (var i = 0; i < arrTemp.Length; i++)
        {
            for (var j = 0; j < emergency.Length; j++)
            {
                if (arrTemp[i] == emergency[j])
                    answer[j] = i + 1;
            }
        }
        return answer;
    }
}
```

위와 같이 줄일 수 있다.   
그리고 LINQ를 사용하는 풀이가 있었는데,  

## 😮 다른 사람의 풀이 확인

LINQ를 이용한 풀이

### ⚒️ 1번 풀이

```csharp
using System;
using System.Linq;

public class Solution {
    public int[] solution(int[] emergency) {
        int[] order = emergency.OrderByDescending(x => x).ToArray();
        int[] answer = emergency.Select(x => Array.IndexOf(order, x) + 1).ToArray();
        return answer;
    }
}
```

정수형 배열을 order를 선언하고 거기에 배개변수로 받아온 정수형 배열을 정렬한 배열을 할당한다.
그리고 answer 배열에 emergency 배열을 순회하면서 order의 요소와 비교하며 일치하는 인덱스를 가져온다.  
인덱스는 0부터 시작하니 가져온 값에 + 1해준다.  

## 🔎 문제 4. 순서쌍의 개수

![image4](/assets/images/posts/Coding_Test/Programmers/2023-08-23-my-programmers-post_8/4.png){: width="50%" height="50%"}

## 🤔 나의 풀이

```csharp
using System;
using System.Linq;

public class Solution {
    public int solution(int n) {
        var answer = 0;
        var arrTemp = Enumerable.Range(1, n).Where(x => n % x == 0).ToArray();
        for (var i = 0; i < arrTemp.Length; i++)
            answer = arrTemp.Aggregate(answer, 
               (current, temp) => arrTemp[i] * temp == n ? current + 1 : current);
        return answer;
    }
}
```

위의 코드로 통과했고, 코드를 작성하면서 고려한 점은 정렬과 배열을 만드는 과정에서 코드가 길어질 것을 우려하여 LINQ를 사용했다.  
그리고, LINQ를 사용하기 때문에 코드는 최대한 간결하고 효과적이게 작성하려고 노력했다.  

크게 코드를 설명하자면,  
첫 번째로 매개변수로 받은 정수 값을 기준으로 1 ~ n 까지의 수를 가지는 배열을 생성했다.  
그리고 Where 절을 사용해서 n의 약수들의 배열을 반환받는다.  

위에서 만든 배열을 순회하면서 "두 숫자의 곱이 n인 자연수 순서쌍의 개수" 를 구했다.  
Aggregate() 메서드를 사용해서 정수형 지역변수 answer 를 증가하면서 순서쌍의 개수를 구한다.  
해당 메서드는 Func 대리자를 매개변수로 받는다.  

그 구간이 `(current, temp) => arrTemp[i] * temp == n ? current + 1 : current` 이다.  
current는 현재 누적된 수이고, temp는 arrTemp 배열의 요소이다.  
따라서 위의 코드는 결국에는 2중 for문이 돌아가는 것이다.  
왜냐하면 Aggregate() 메서드도 해당 배열을 순회하면서 진행되기 때문이다.   

더 자세히 풀어내자면, `arrTemp[i] * temp = arrTemp[i] * (arrTemp[0] ~ [n])` 왼쪽의 식과 오른쪽의 식이 같기 때문에 순회를 한다는 것이다.  
해서 `arrTemp[i] * n` 가 n과 같다면 즉 두 수의 곱이 n과 같다면 카운팅을 해야하기 때문에 current + 1을 해주고, 그 반대는 current를 반환하게 작성했다.  

## 😮 다른 사람의 풀이 확인

한줄이라고?

### ⚒️ 1번 풀이

```csharp
using System;
using System.Linq;

public class Solution {
    public int solution(int n) {
        int answer = Enumerable.Range(1, n).Count(x => n % x == 0);
        return answer;
    }
}
```

이게 정답일 수도 있겠구나 싶은데, 이런 변칙적인 풀이가 나오는 건 문제자체가 타당성이 부족한게 아닐까싶다.  
사실 약수만 구해도 되는 문제이긴하다.  
그래서 위의 풀이는 뭔가 반만 푼 느낌이 드는 건 왜일까?  

순서쌍의 수를 구하라 했는데, 순서쌍 = 약수가 될 수도 있다는 사실을 가져가기만 하면 될 것 같다.  

### ⚒️ 2번 풀이

```csharp
using System;

public class Solution
{
    public int solution(int n)
    {
        int answer = 0;

        for(int i=1; i<=n; i++)
        {
            if (n % i == 0)
                answer++;
        }

        return answer;
    }
}
```

이건 LINQ를 그냥 풀어낸 식이다.  
마찬가지로 약수만 구한다.  

## 🤣 한마디

문제를 읽고 원하는 값이 무엇인지에 대해서 제대로 인지하기.

<br>

<hr style="width:100%" />

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr style="width:100%" />

[Top](#){: .btn .btn--primary }{: .align-right}