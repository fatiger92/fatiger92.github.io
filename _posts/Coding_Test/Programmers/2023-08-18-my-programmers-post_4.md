---
title:  "[알린이의 코테 입문 #013 - 016] 수학, 배열"
excerpt: "프로그래머스"

categories:
  - Programmers
tags:
  - [Programmers]
toc: true
toc_sticky: true
date: 2023-08-18
last_modified_at: 2023-08-18
---

# 📍 수학, 배열

## 🔎 문제 1. 피자 나눠 먹기 (1)

![image1](/assets/images/posts/Coding_Test/Programmers/2023-08-18-my-programmers-post_4/1.png){: width="50%" height="50%"}

문제를 읽고 풀이를 생각해봤다.  
1 ~ 7 인 경우 1을 반환.  
8 ~ 14 인 경우에는 2 반환.   
15 ~ 21 인 경우에는 3을 반환해야 하는 문제이다.  

여기서 해결해야 하는 것은 1 ~ 7인 경우를 제외한 경우의 수에서 어떠한 기준으로 카운팅을 할지 생각해봐야 한다.  
어떤 방법이 있을까?  

일단 n을 7로 나눈 뒤 나머지가 존재한다면 +1 해주고 나머지가 존재하지 않는다면 그냥 반환하면 되지 않을까 싶음.

## 🤔 나의 풀이

```csharp
using System;

public class Solution 
{
    const double quantity = 7d;

    public int solution(int n) {
        var quotient = Math.Truncate(n / quantity);
        return n % quantity == 0 ? Convert.ToInt32(quotient) : Convert.ToInt32(quotient) + 1;
    }
}
```

## 😮 다른 사람의 풀이 확인

### ⚒️ 1번 풀이

```csharp
using System;

public class Solution {
    public int solution(int n) {
        return (int)Math.Ceiling((double)n / 7);
    }
}
```

첫 번째 풀이는 `Math.Ceiling(double a)` 이라는 메서드가 있길래 가져와 봤다.  

> System.Math 클래스에 포함된 함수  
> 올림 -> Math.Ceiling(double 값)   
> 내림 -> Math.Truncate(double 값)  
> 반올림 -> Math.Round(double 값 [, 자릿수])  

나는 `Math.Truncate` 메서드를 사용했는데, 사실 `Math.Ceiling` 메서드를 사용했으면 더 간단하게 구현할 수 있었다.  
명시적 캐스팅을 사용한 것을 확인할 수 있는데, 데이터 손실에 민감하지 않다면 위와 같이 구현해도 될 것 같다.  

만약 데이터 손실에 민감하다면,
```csharp
    return Convert.ToInt32(Math.Ceiling(n / 7d));
```
위와 같이 작성하는 것이 좋다. 

### ⚒️ 2번 풀이

```csharp
using System;

public class Solution {
    public int solution(int n) {
        return (n - 1) / 7 + 1;
    }
}
```

이건 만약 n이 7이라고 가정 해보자.  
`return (7 - 1) / 7 + 1` 을 계산해보면

```csharp
// 6일 경우 -> (6 - 1) / 7 + 1 = 5 / 7 + 1 = 1.7142857142857143 => 1
// 7일 경우 -> (7 - 1) / 7 + 1 = 6 / 7 + 1 = 1.857142857142857 => 1
// 8일 경우 -> (8 - 1) / 7 + 1 = 7 / 7 + 1 = 2 => 2
// 9일 경우 -> (9 - 1) / 7 + 1 = 8 / 7 + 1 = 2.142857142857143 => 2
// 10일 경우 -> (10 - 1) / 7 + 1 = 9 / 7 + 1 = 2.285714285714286 => 2
``` 
위와 같이 나온다.  

풀이 2가지를 비교해 봤을 때, 가독성은 1번째 풀이가 좋은 것 같다.  
사실상 명시적 캐스팅과 Convert의 성능 차이는 거의 미미하다.   
따라서 상황에 맞는 코드를 작성하자.  

## 🔎 문제 2. 피자 노나 먹기 (2)

![image2](/assets/images/posts/Coding_Test/Programmers/2023-08-18-my-programmers-post_4/2.png){: width="50%" height="50%"}

첫 번째 문제와 다른 것은   
1. 여섯 조각으로 잘라준다.
2. 피자를 나눠먹을 사람의 수 n이 매개변수로 주어진다.  
3. n 명이 주문한 피자를 남기지 않고 모두 같은 수의 피자 조각을 먹어야 한다면 최소 몇 판을 시켜야 하는지?
   
예를 들어 10명이 모두 같은 양을 먹기 위해서는 최소 5판을 시켜야 피자가 30조각으로 모두 3조각씩 먹을 수 있다.  
따라서 10과 6의 최소 공배수를 구하고 그걸 6으로 나누면 피자의 수가 나온다.  
다른 예시로 4명이 모두 같은 양을 먹기 위해서는 최소공배수인 12조각으로 모두 3조각씩 먹을 수 있고 이것은 2판의 피자의 양과 같다.  

최소 공배수를 구하는 공식은 `두수의 곱 / 최대 공약수` 이다.  
따라서 잊어버린 최대 공약수를 구하는 유클리드 호제법을 다시 복기해보자.  

```csharp
public int gcd(int n, int m)
{
    return m == 0 ? n : gcd(m, n % m);
}
```
m이 0이 될때까지 멈추지 않는 유클리드 재귀 함수를 다시 복기했다.  

최소 공배수 = `a * b / gcd(a, b)`  

그리고 최소 공배수를 6으로 나누면 끝.

## 🤔 나의 풀이

```csharp
using System;

public class Solution {
    public int solution(int n) 
        => (n * 6 / gcd(n, 6)) / 6;

    public int gcd(int n, int m) 
        =>  m == 0 ? n : gcd(m, n % m);
}
```

괜찮아 보이는 풀이법이 있었으나 가독성이 너무 안좋아서 GG

## 🔎 문제 3. 피자 나눈걸 또 노나 먹기 (3) - 또피자

![image3](/assets/images/posts/Coding_Test/Programmers/2023-08-18-my-programmers-post_4/3.png){: width="50%" height="50%"}

두 번째 문제와 다른 점  

1. 피자를 두 조각에서 열 조각까지 원하는 조각 수로 잘라 준다 ( 2 <= slice <= 10 )
2. 피자 조각 수 slice가 매개변수로 주어짐
3. 피자를 먹는 사람의 수 n이 매개변수로 주어짐
4. n명의 사람이 최소 한 조각 이상 피자를 먹으려면 최소 몇 판의 피자를 시켜야 하는지?

아니 그냥 이전 문제에 7, 6이 slice로 변한거 아닌가

## 🤔 나의 풀이 1번째 시도

```csharp
using System;

public class Solution 
{
    public int solution(int slice, int n) 
        => n * slice / gcd(n, slice) / slice;
    
    public int gcd(int a, int b) // 유클리드 호제법
        => b == 0 ? a : gcd(b, a % b);
}
```

이때는 몰랐다 무언가 잘못 되었다는 것을ㅋㅋ

![image3-1](/assets/images/posts/Coding_Test/Programmers/2023-08-18-my-programmers-post_4/3-1.png){: width="50%" height="50%"}

아니라는 것 같음.  
무엇이 다른고 하니 7조각으로 잘라주고 10명이 먹어야 하는 경우 실패한다.  

코드에 대입해서 읽어보자.

```csharp
    public int solution(int slice, int n) 
        => n(=10) * slice(=7) / gcd(n(=10), slice(=7)) / slice(=7);
        => 10 * 7 / gcd(10, 7) / 7;
        => 70 / 1 / 7 = 10
```
내가 빠뜨린 부분이 있었는데, "최소 한 조각 이상 피자를 먹으려면" 이다.  
위의 연산식으로는 10명이 "모두 같은 양을 먹기 위한 최소의 피자 개수" 가 구해진다.  

따라서 이건 최소 공배수를 구하는 것이 아니다.  

1. 외부에서 count 라는 변수를 만든다. 
2. n으로 slice에 나머지 연산자를 써서 그 값이 0이 아니라면 count++ 해준다.
3. count를 return 한다.

## 🤔 나의 풀이 2번째 시도

```csharp
using System;

public class Solution 
{
    public int solution(int slice, int n) 
        => (int)Math.Ceiling((double)n / slice);
}
```

통과~  

다른 코드들은 비슷하거나 메서드를 쓰지않고 코드를 날 것으로 풀어 작성한게 다였음.  
(특별한 코드 없음)  

## 🔎 문제 4. 배열의 평균값

![image4](/assets/images/posts/Coding_Test/Programmers/2023-08-18-my-programmers-post_4/4.png){: width="50%" height="50%"}

## 🤔 나의 풀이

```csharp
using System;

public class Solution {
    public double solution(int[] numbers) {
        double answer = 0;
        
        foreach(var number in numbers)
            answer += number;
        
        return answer / numbers.Length;
    }
}
```

## 😮 다른 사람의 풀이 확인

### ⚒️ 1번 풀이

LINQ를 사용해 한줄로 깔끔하게 처리한 코드이다.

```csharp
using System;
using System.Linq;
public class Solution {
    public double solution(int[] numbers) {
        return numbers.Average();
    }
}
```

### ⚒️ 2번 풀이

제한 사항은 이미 정해진 규칙이다.  
그런데 굳이 명시할 필요가 있나 싶은 코드이다.  
그냥 쓸때없는 코드를 추가적으로 작성한 Worst case 인 것 같다.  
사실 이 코드에서 필요한 부분은 `return numbers.Average();` 한줄이 끝이다.  

```csharp
using System;
using System.Linq;

public class Solution {
    public double solution(int[] numbers) {
        if (numbers.Length == 0 || numbers.Length > 100) {
            throw new Exception("invalid param length");
        }
        if (numbers.Where(x=>x<0 || x>1000).Any() == true) {
            throw new Exception("invalid element");
        }
        return numbers.Average();
    }
}
```

## 🤣 한마디

의미없는 코드를 작성하는 건, 가독성을 헤칠 뿐만아니라 다른 팀원과 협업할 때 스트레스만 줄 뿐이다.  
코드로 암호

<br>

<hr style="width:100%" />

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr style="width:100%" />

[Top](#){: .btn .btn--primary }{: .align-right}