---
title:  "[알린이의 코테 입문 #009 - 012] 사칙연산, 배열, 수학"
excerpt: "프로그래머스"

categories:
  - Programmers
tags:
  - [Programmers]
toc: true
toc_sticky: true
date: 2023-08-09
last_modified_at: 2023-08-10
---

# 📍 사칙 연산, 배열, 수학

## 🔎 문제 1. 나머지 구하기

![image1](/assets/images/posts/Coding_Test/Programmers/2023-08-09-my-programmers-post_3/1.png){: width="50%" height="50%"}


## 🤔 나의 풀이

```csharp
using System;

public class Solution {
    public int solution(int num1, int num2) {
        return num1 % num2;
    }
}
```

## 🔎 문제 2. 중앙값 구하기

![image2](/assets/images/posts/Coding_Test/Programmers/2023-08-09-my-programmers-post_3/2.png){: width="50%" height="50%"}

먼저 조건에 값을 정렬하는 것이 있다.
버블 정렬을 이용한다.

버블 정렬 알고리즘은 따로 빼서 정리하겠음.

```csharp
// 버블 소팅
public static int[] solution(int[] array)
{
    //var result = 0;
    
    for (var i = 0; i < array.Length - 1; i++)
    {
        for (int j = 0; j < array.Length - 1; j++)
        {
            if (array[j] > array[j + 1])
            {
                var temp = array[j];
                array[j] = array[j + 1];
                array[j + 1] = temp;
            }
        }
    }
    return array;
}
```
위와 같은 방법으로 정렬을 할 수 있는데, 여기서 if 본문을 한번 더 줄일 수 있다.

```csharp
// 줄이기 전
if (array[j] > array[j + 1])
{
    var temp = array[j];
    array[j] = array[j + 1];
    array[j + 1] = temp;
}

// 줄인 후
if (array[j] > array[j + 1])
{
    (array[j], array[j + 1]) = (array[j + 1], array[j]);
}
```

둘은 같다.

이제 중앙 값을 도출하는 부분만 남았는데, 
첫 번째 조건에 array의 길이는 홀수가 있었다는 사실을 잊으면 안됨
따라서 짝수는 고려할 필요 없음.

3, 5, 7, 9, 11 ... 에 해당하는 중앙 값을 찾으면 된다.

```csharp
var middleIndex  = array.Length / 2; // 가운데 인덱스 계산
var middleValue = array[middleIndex] // 가운데 값 찾기
```


## 🤔 나의 풀이

```csharp
using System;

public class Solution {
    public int solution(int[] array) {
        for(int i = 0; i < array.Length - 1; i++)
        {
            for(int j = 0; j < array.Length - 1; j++)
            {
                if(array[j] > array[j + 1])
                {
                    var temp = array[j];
                    array[j] = array[j + 1];
                    array[j + 1] = temp;
                   
                    // C# 7.0 부터 위의 본문을 아래와 같이 튜플로 쉽게 Swap할 수 있는데 프로그래머스 에디터가 인식을 못함.
                    //(array[j], array[j + 1]) = (array[j + 1], array[j]);
                }
            }
        }
        var middleIndex = array.Length / 2;
        return array[middleIndex];
    }
}
```

버블 정렬을 이용해서 구현하면 확실히 코드의 줄이 길어진다.  
그렇다면 C#의 꽃이라고 불리는 LINQ를 사용해보자.   
아래는 LINQ를 써서 간단하게 끝난 코드이다.

```csharp
using System;
using System.Linq;

public class Solution {
    public int solution(int[] array) {
        int answer = array.OrderBy(x => x).ToArray()[array.Length / 2];
        return answer;
    }
}
```

물론 LINQ를 사용하면 네임스페이스를 제외한 줄 수가 단 한 줄이 나온다.  
하지만, 이 연산만을 위해서 LINQ를 사용하는 것은 주관적인 생각으로 별로라고 생각한다.  
왜냐하면 LINQ는 무겁다.  
편리한 만큼 정말 많은 기능들을 가지고 있기 때문에, 사용하는 데 있어서 많이 고려해보고 사용해야 한다고 생각한다.  

성능 향상은 단지 코드의 줄 수를 한 줄이라도 줄여서 보여 주기 위함이 아닌, 실질적으로 코드 내부에서 진행되는 불필요한 기능들을 없애는 게 더 중요하다고 생각한다.

## 🔎 문제 3. 최빈값 구하기

![image3](/assets/images/posts/Coding_Test/Programmers/2023-08-09-my-programmers-post_3/3.png){: width="50%" height="50%"}


## 🤔 나의 풀이

```csharp
using System;

public class Solution {
    public int solution(int num1, int num2) {
        return num1 * num2;
    }
}
```

## 🔎 문제 4. 짝수는 싫어요

![image4](/assets/images/posts/Coding_Test/Programmers/2023-08-09-my-programmers-post_3/4.png){: width="50%" height="50%"}


## 🤔 나의 풀이

```csharp
using System;

public class Solution {
    public int solution(int num1, int num2) {
        return num1 / num2;
    }
}
```

## 🤣 한마디

사실 이건 너무 쉬워서 설명도 필요 없다.

<br>

<hr style="width:100%" />

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr style="width:100%" />

[Top](#){: .btn .btn--primary }{: .align-right}