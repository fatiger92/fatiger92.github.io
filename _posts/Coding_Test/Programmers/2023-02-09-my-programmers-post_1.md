---
title:  "[프로그래머스 - 레벨0] 분수의 덧셈, 유클리드 호제법, 최소공배수 공식"
excerpt: "이런게 있었구나"

categories:
  - Programmers
tags:
  - [Programmers]
toc: true
toc_sticky: true
date: 2023-02-09
last_modified_at: 2023-02-10
---

# 🤔 문제
<hr style="width:100%" />

프로그래머스를 밑 바닥부터 풀면서 알고리즘을 다시 공부하고 있는데, 분수의 덧셈이라는 문제에서 새로운 개념과 부딪혔다.

먼저 문제의 내용은 이렇다.

![img1](/assets/images/posts/Coding_Test/Programmers/2023-02-09-my-programmers-post_1/1.png){: width="70%" height="70%"}

## 😧 첫 번째 고비

먼저 어떻게 풀어야 할지는 어느정도 감이 왔지만, 두 정수의 최대 공약수를 구하는 법은 몰랐다.

그래서 검색을 해봤는데, 유클리드 호제법이라는 알고리즘이 나왔다.

## 💡 유클리드 호제법

C# 에서 "유클리드 호제법"은 최대 공약수(Greatest Common Divisor, GCD)를 구하는 알고리즘이다.

이 알고리즘은 두 정수 a, b에 대해 GCD(a,b)를 구할 때 a와 b가 주어지면 a와 b의 차를 계산하여 다시 a,b와 a%b로 바꿔서 반복하여 계산한다.
이 과정을 a%b가 0이 될 때까지 반복하여 구한다.

C# 샘플 코드

```c#
int GCD(int a, int b) 
{
    while (b != 0) {
        int remainder = a % b;
        a = b;
        b = remainder;
    }
    return a;
}
```

a와 b가 각 10과 8로 주어졌다고 가정하면 b가 0이 아닐 경우, 즉 8이 0이 될 때까지 while문을 계속 해서 반복하게 된다.  
본문을 들여다보면 remainder에 10 % 8 의 연산 값을 할당 한다.

따라서 `int remainder = 2;` 가 되고 a에 b를 할당한다.  
`a = 8;` 그리고 b에 remainder 즉 2를 할당한다.

현재 `a = 8`, `b = 2` 이므로 while의 조건식 `2 != 0`에 `true`가 되므로 본문을 반복한다.  
`int remainder = 8 % 2` 나머지가 없으므로 `0` 이나오고 즉 `a = 2` `b = 0` 이 되고, while의 조건식 `0 != 0` 이 false가 되므로 while문에서 빠져나와 `a`값 즉 `2`를 리턴 한다.

결론적으로 최대공약수는 2가 된다.

## 😑 두 번째 고비

While문을 써서 돌리는 건, 이제 이해가 됐는데 뭔가 너무 길다.

```c#
public int gcd(int n, int m)
{
  if (m == 0) return n;
  else return gcd(m, n % m);
}
```

좀 더 찾아봤는데, While문을 재귀함수로 작성한 사람도 있었다.  

<strong style="color:limegreen; font-size:15pt">이거 좀 괜찮은데?</strong>

바로 가져와서 분석해봤다.

우리가 얻어야 할 값은 `if(m == 0) return n` 이 부분이다.  
그렇기 때문에 사실상 `gcd(m, n % m)` 이 부분을 재귀로 호출해도 언겐가 `m`은 `0`이 될테고 `return n` 으로 빠져나오게 된다.

if - else 문이니까 삼항 연산자로 바꿔주면 아주 깔끔하겠지?

```c#
public int gcd(int n, int m)
{
  return m != 0 ? gcd(m, n % m) : n;
}
```

<strong style="color:orange; font-size:13pt">혹시나해서 적어 두는데 n와 m의 순서는 어떤 수를 기준으로 하냐의 차이이지, 꼭 순서를 똑같이 할 필요는 없다.</strong>

```c#
public static int reversegcd(int n, int m)
{
    return n != 0 ? reversegcd(m % n, n) : m;
}
```

![organize](https://media.giphy.com/media/PjTSEQy85NKOlZ7b19/giphy.gif){: width="30%" height="30%"}

<strong style="color:Yellow; font-size:20pt">까알끔하다.</strong>

추가적으로 이 알고리즘은 매우 효율적이고, 일반적으로 빠른 결과를 제공한다고 한다.

## 🚀 제출한 코드

```c#
using System;

public class Solution {
public int[] solution(int numer1, int denom1, int numer2, int denom2) {
    var _numer = numer1 * denom2 + numer2 * denom1;
    var _denom = denom1 * denom2;
    
    var _gcd = gcd(_numer, _denom);
    return new[] { _numer / _gcd, _denom / _gcd };
}

public int gcd(int n, int m)
{
    return m == 0 ? n : gcd(m, n % m);
}
}
```

## 🌈 번외 - 최소공배수

최소 공배수의 공식은 두수의 곱 / 최대 공약수이므로

```c#
a * b / GCD(a ,b);
```

이렇게 구할 수 있다.

<br>

# 📢 오늘의 한마디
<hr style="width:100%" />

![noidea](https://media.giphy.com/media/bPTXcJiIzzWz6/giphy.gif){: width="30%" height="30%"}

<strong style="color:Yellow; font-size:15pt">솔직히 모르는 부분이었다. 인정할 건 인정 해야지</strong>

<br>

<hr style="width:100%" />

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr style="width:100%" />

[Top](#){: .btn .btn--primary }{: .align-right}