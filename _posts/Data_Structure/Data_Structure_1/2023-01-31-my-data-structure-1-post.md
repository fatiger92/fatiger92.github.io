---
title:  "[C# 으로 이해하는 자료구조] 1. 자료구조"
excerpt: "C# 으로 이해하는 자료구조 - Alex Lee 선생님의 책으로 공부하는 공간입니다."

categories:
  - Data Structure 1
tags:
  - [Data Structure 1]
toc: true
toc_sticky: true
date: 2023-01-31
last_modified_at: 2023-01-31
---

알라딘에 있는 Alex Lee님의 **C#으로 이해하는 자료구조** 책을 보고 공부하며 정리 및 요약한 게시글입니다.  
종이서적을 별로 좋아하지 않기 때문에 링크는 전자책입니다.
<br>
[🔔책 구매하기 링크](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=183854923)
{: .notice--warning}

# 💾 자료구조 (Data Structure)
<hr style="width:100%" />

## 🔖 자료구조란?

  자료구조 (Data Structure)란 데이터를 효율적으로 액세스하고 조작할 수 있도록 데이터의 구조를 만들어 데이터를 저장하고 관리하는 것을 말한다.

## 🔖 자료구조를 사용하는 목적?

  처리해야할 데이터들이 방대해 질 경우 <strong style="color:yellow">데이터를 더 효율적으로 저장하고, 관리하기 위해 사용</strong>하며 잘 선택된 자료구조는 <strong style="color:yellow">실행시간을 단축시켜주거나 메모리의 절약을 이끌어내 성능 최적화에 큰 영향</strong>을 준다.

## 🔖 자료구조의 선택 기준
    1. 자료의 처리 시간
    2. 자료의 크기
    3. 자료의 활용 빈도
    4. 자료의 갱신 정도
    5. 프로그램의 용이성

## 🔖 자료구조와 알고리즘의 차이

  자료구조는 데이터를 상황에 맞게 저장하기 위한 구조이고, 알고리즘은 자료구조에 있는 데이터를 활용해 어떠한 문제를 해결하기 위한 
  여러 방법들의 모임이라고 할 수 있다.

## 🔖 추상적 자료형과 자료구조

  추상적 자료형은 자료구조와 동일한 것으로 사용되기도 하고, 실제 사용하는 용어도 비슷하다.  
  하지만 둘은 분명히 개념적으로 약간의 차이점이 존재한다.

  - 추상적 자료형은 "무엇(What)" 이 구현되어져야 하는지를 정의한 것으로 자료형의 논리적 형태를 정의한다.
  - 자료구조는 이것을 "어떻게(How)" 구현할 지를 파악해서 물리적 형태로 구현하는 것이다.

  예를 들어 Stack은 추상적 자료형 측면으로 본다면 마지막에 입력된 데이터가 먼저 꺼내지는 방식을 정의한 것이다.  
  그리고 자료구조 구현 측면으로 본다면 배열 형태로 구현할 수도, 연결 리스트 형태로 구현할 수 있다.

## 🔖 자료 구조의 종류

![img01](/assets/images/posts/Data_Structure/Data_Structure_1/2023-01-31-my-data-structure-1-post/1.png)

  자료구조는 크게 <strong style="color:yellow">선형 자료구조</strong>와 <strong style="color:yellow">비선형 자료구조</strong>로 나뉜다.

  <p> 📍단순 구조 (Primitive Data Structrue) : 기본적인 데이터 타입으로 정수, 실수, 문자 그리고 불값 등의 기초 타입이다.</p>
  <p> 📍선형 구조 (Linear Data Structrue) : 자료 요소가 선형적으로 연결되어 있는 구조이며, 앞과 뒤의 자료가 1:1 인 구조를 가진다.</p>  
  <p>&nbsp;&nbsp;👉 (예시로는 배열, 연결 리스트, 스택, 큐와 같은 자료구조가 이에 해당함.)</p>
  <p> 📍비선형 구조 (Non-linear Data Structrue) : 자료 간 관계가 1:다수 혹은 다수:다수 구조로서 계층구조 혹은 네트워크 망 구조를 가짐.</p>
  <p>&nbsp;&nbsp;👉 (예시로는 트리와 그래프가 이에 해당.)</p>
  <p> 📍파일 구조 (File Structure) : 레코드의 집합인 파일에 대한 자료구조로서 순차파일, 색인 파일, 직접파일 등이 이에 해당한다.</p>


# 🔎 참고한 블로그
<hr style="width:100%" />

[🔔 filoscoder님 블로그](https://velog.io/@filoscoder/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EC%A2%85%EB%A5%98%EC%99%80-%EB%B6%84%EB%A5%98)<br>
[🔔 Andrew Shin님 블로그](https://andrew0409.tistory.com/148)
{: .notice--warning}

<br>

# 📢 오늘의 한마디
<hr style="width:100%" />
  
<strong style="color:Yellow; font-size:15pt">⚡자료구조는 알고리즘보다 중요하다고 사람들이 말하는데 과연 왜일까?</strong>


<hr style="width:100%" />

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr style="width:100%" />

[Top](#){: .btn .btn--primary }{: .align-right}