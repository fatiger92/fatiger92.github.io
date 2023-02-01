---
title:  "Big O 표기법이 뭔지 도저히 몰라서 팝핀 댄스를 추고 싶을 때 보려고 쓴 글"
excerpt: "와우"

categories:
  - Dev Dictionary
tags:
  - [Dev Dictionary]
toc: true
toc_sticky: true
date: 2023-02-01
last_modified_at: 2023-02-01
---

# 이걸 왜 알아야 하죠?
<hr style="width:100%" />

  물론 몰라도 상관없다.  

  하지만 이것을 몰라도 상관없다는 얘기는 <strong style="color:crimson">시간복잡도를 고려하지 않아도 될 정도의 간단한 일을 하고 싶다는 것을 방증하는 것</strong>과 같다.  

  이 개념을 모르면 다른 플머들과 협업할 때 혼자 이해를 못하는 상황이 오고, 시간 복잡도 자체를 모른다면 <strong style="color:crimson">성능의 최적화는 이번생에는 먼 얘기</strong>가 된다.  

<br>

# Big-O 표기법이 뭔가요? 
<hr style="width:100%" />

빅 OOOOOOOOOOOOOOOOO 점근 표기법의 핵심은 시간 복잡도를 빠르게 설명할 수 있다는 것이다.  

선형 검색(Linear search)을 예를 들어 설명할 것이다.   
선형 검색은 하나씩 하나씩 검색을 한다.

그래서 데이터가 10개라면 한개씩 검색을 하기 때문에 시간복잡도는 상수로 10 이고,  
데이터가 20개 라면? 20개 전부를 검색해야 하니 시간복잡도는 상수로 20 이 되는 것이다.

따라서 <strong style="color:limegreen">데이터의 개수가 N 이라면 선형 검색 알고리즘은 N 만큼의 시간이 걸리는 것</strong>이다.
이것을 빅O 표기법으로 하면 아래와 같은 O(n)으로 표기 된다.  

![img01](/assets/images/posts/ETC/Dev_Dictionary/2023-02-01-my-dev-dictionary-post_2/1.png){: width="30%" height="30%"}

>데이터의 개수가 N개면 N 만큼의 시간이 필요하다. = O(N)  

<br>

# 그래서 배우면 좋은 점이 뭐죠?
<hr style="width:100%" />

Big O를 이해하면, <strong style="color:orange">알고리즘 분석을 빠르게 할 수 있고 언제 무엇을 쓸지 빠르게 파악이 가능</strong>하다.

그리고 시간복잡도를 표기함으로써 <strong style="color:orange">내가 만든 코드의 평가를 내릴 수 있다.</strong>
왜냐하면 미래에 어떻게 작동할 지 알 수 있기 때문이다.

## O(1)

```c#
public void PrintFirstElement(int[] arr)
{
    Console.WriteLine(arr[0]);
}
```

위와 같은 메서드가 있다고 가정하자 매개변수로 int 배열을 받아와서 배열의 첫번째 요소를 출력하는 것이다.

이 메서드는 배열의 크기가 10이 돼도 100이 돼도 단 한번만 실행을 한다.
따라서 배열의 크기와 관계 없이 동일한 수의 스텝만 실행한다.

이 메서드의 <strong style="color:limegreen">시간복잡도는 constant time(상수 시간)</strong> 이라고 할 수 있다.

### 상수 시간 (constant time)

    상수 시간은 O(1)로도 표기하는데, 어떤 문제를 풀이할 때 필요한 수학적 연산 시간이 주어진 입력 자료에 관계 없이 일정할 때의 연산 시간을 의미한다.

![img02](/assets/images/posts/ETC/Dev_Dictionary/2023-02-01-my-dev-dictionary-post_2/2.png){: width="30%" height="30%"}

[🔔 상수 시간 위키 설명](https://ko.wikipedia.org/wiki/%EC%83%81%EC%88%98_%EC%8B%9C%EA%B0%84)
{: .notice--warning}

상수 시간이라는 개념을 알았으니 이제 아래의 코드를 보자

```c#
public void PrintFirstElement(int[] arr)
{
    Console.WriteLine(arr[0]);
    Console.WriteLine(arr[0]);
}
```

방금 전의 코드에서 변경된 건 단지 출력을 두번하는 것 뿐이다.  
<strong style="color:limegreen">(설명하기 쉽게 방금 전의 코드를 A code, 현재의 코드를 A' code 라고 지칭하겠다.)</strong>

그렇다면 방금 전의 코드 A code 에서 시간 복잡도가 O(1) 이었으니
A' code 의 시간 복잡도는 O(2)인걸까?

<strong style="color:Yellow; font-size:15pt">답은 아니다.</strong>

여전히 동일하게 O(1) 라고 표현하는데, 그 이유는 <strong style="color:orange">Big O 표기법은 메서드의 디테일에는 관심이 없기 때문</strong>이다.

<strong style="color:orange">Big O 는 러프하게 이 메서드가 데이터의 크기에 따라 어떻게 동작하는지에 대한 큰 원리에만 관심이 있다.</strong>

따라서 Big O를 이해하려면 이 표기법은 상수 (constant)에 전혀 신경을 쓰지 않는 다는 것을 꼭 기억해야 한다.

결론적으로 위의 메서드에서 배열의 크기가 10, 100, 1000, 1000000000이 된다고 해도 시간 복잡도는 여전히 O(1)로 표기되는 것이다.

## O(n)

또 다른 시간 복잡도를 확인해보자

```c#
public void PrintAllElement(int[] arr)
{
    foreach (var n in arr)
        Console.WriteLine(n);
}
```

위의 코드는 배열의 모든 요소를 출력하는 코드이다.
만약 배열의 크기가 10개면 10번, 100개면 100번 출력하게 될 것이다.

이 작동 방식은 선형 검색 알고리즘과 동일하다.
배열의 크기가 N일 경우 N번의 스텝을 걸쳐야 하기 때문이다.

이건 Big O 표기법으로 O(n)으로 표기한다.

그렇다면 이것은 어떨까?

```c#
public void PrintAllElement(int[] arr)
{
    foreach (var n in arr)
        Console.WriteLine(n);
    
    foreach (var n in arr)
        Console.WriteLine(n);
}
```
이 코드의 시간 복잡도는 각 N마다 2번 작업을 하니 O(2N) 으로 표기하는 걸까?

답은 아니다.
2는 상수이기 때문에 날려버리고 O(N)으로 표현하게 된다.

## O(n²)

또 다른 코드를 확인해보자.

```c#
public void PrintAllElement(int[] arr)
{
    foreach (var n in arr)
        foreach (var x in arr)
            Console.WriteLine($"{x}{n}");
}
```
이 코드는 배열의 각 아이템에 대하여 루프를 반복해서 실행한다.
따라서 시간복잡도는 배열의 크기의 n² 인 것이고, 이 말은 배열의 크기가 10이라면 작업이 완료 되는데 10의 제곱인 100번의 스텝이 필요한 것이다.

이것은 Big O 표기법으로 O(n²) 이다.

![img03](/assets/images/posts/ETC/Dev_Dictionary/2023-02-01-my-dev-dictionary-post_2/3.png){: width="30%" height="30%"}

따라서 위의 코드 보다는 선형 검색 알고리즘 시간 복잡도가 더 효율적인 것을 알 수 있다.

## O(log n)

또 다른 시간 복잡도로는 로그 시간 (Logarithmic time) 이 있다.
이것은 이진 검색 알고리즘을 설명할 때 쓰는 것이다.

    10 items -> 20 items
    3 steps -> 4 steps

이진 검색은 배열의 크기가 2배가 되어도, 스텝은 단 1 밖에 증가하지 않는다.
왜냐하면 이진 검색에서는 각 프로세스의 스텝을 절반으로 나눠서 진행하기 때문이다.

이것을 Big O로 표현하면, O(log N)이 된다.

>이진 검색 알고리즘은 다음번에 심도있게 다룰 것임

로그(logarithm)는 지수(exponent)의 정 반대다.

![img04](/assets/images/posts/ETC/Dev_Dictionary/2023-02-01-my-dev-dictionary-post_2/4.png){: width="50%" height="50%"}

이어서 이진 검색에 대해서 계속 말하자면, 배열의 크기를 일단 반으로 나누고 시작한다.
그리고 매 스텝마다 계속 반으로 나눈다.
따라서 배열의 크기가 2배로 커져도, 검색을 하기 위한 스텝은 1 만 증가하게 된다.

왜냐면 한번만 더 나누면 되는 것이기 때문이다.

결론적으로 이진 검색의 Big O 식 시간 복잡도는 O(log n) 인데, 그 이유는 Big O의 특성상 Base는 사라지기 때문이다.

![img05](/assets/images/posts/ETC/Dev_Dictionary/2023-02-01-my-dev-dictionary-post_2/5.png){: width="40%" height="40%"}

O(log n)의 그래프의 모습은 아래와 같다.

![img06](/assets/images/posts/ETC/Dev_Dictionary/2023-02-01-my-dev-dictionary-post_2/6.png){: width="30%" height="30%"}

그리고 이것은 선형 검색 알고리즘보다는 빠르다.
하지만 이진 검색에는 조건이 존재하는데, 그 조건은 정렬되지 않은 배열엔 사용할 수 없다는 것이다.

<br>

# 총정리 그래프
<hr style="width:100%" />

마지막으로 이 그래프를 확인해보자.

![img07](/assets/images/posts/ETC/Dev_Dictionary/2023-02-01-my-dev-dictionary-post_2/7.png){: width="50%" height="50%"}

<strong style="color:orange; font-size:20pt">✨ O(1) < O(log n) < O(n) < O(n²) ✨</strong>


<br>
<strong style="color:yellow; font-size:100pt;">끝</strong>

<hr style="width:100%" />

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr style="width:100%" />

[Top](#){: .btn .btn--primary }{: .align-right}