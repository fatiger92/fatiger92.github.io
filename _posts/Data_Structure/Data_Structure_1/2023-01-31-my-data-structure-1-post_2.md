---
title:  "[C# 으로 이해하는 자료구조] 2. 배열"
excerpt: "C# 으로 이해하는 자료구조 - Alex Lee 선생님의 책으로 공부하는 공간입니다."

categories:
  - Data Structure 1
tags:
  - [Data Structure 1]
toc: true
toc_sticky: true
date: 2023-01-31
last_modified_at: 2023-02-01
---

알라딘에 있는 Alex Lee님의 **C#으로 이해하는 자료구조** 책을 보고 공부하며 정리 및 요약한 게시글입니다.  
종이서적을 별로 좋아하지 않기 때문에 링크는 전자책입니다.
<br>
[🔔책 구매하기 링크](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=183854923)
{: .notice--warning}

# 💾 배열 (Array)
<hr style="width:100%" />

## 🔖 배열의 기초 개념

  <strong style="color:Yellow; font-size:15pt">연속적인 메모리 상에 동일한 데이터 타입의 요소들을 순차적으로 일렬로 저장하는 자료구조.</strong>  
  
  대부분의 프로그래밍 언어에서 사용할 수 있는 가장 기초적인 자료구조이다.  
  대체로 방(room) 개념으로 설명을 자주 한다.

  특징은 고정된 크기를 가진다는 것이고, 인덱스로 요소에 접근이 가능하다.
  또한 <strong style="color:limegreen">배열은 차원이라는 개념이 있는데</strong>, 한 배열 요소를 선택하기 위해 사용하는 인덱스의 수이다.

  요소가 9개인 A 배열은 아래와 같이 표현할 수 있다.

  ![img01](/assets/images/posts/Data_Structure/Data_Structure_1/2023-01-31-my-data-structure-1-post_2/1.png){: width="50%" height="50%"}

  위의 그림과 같이 한 줄로된 배열을 1차원 배열이라고 하며, 배열 요소를 접근할 때 1개의 인덱스만 필요하다.  
  반면, <strong style="color:limegreen">2차원 배열은 행(row)과 열(column)을 갖는 배열을 의미</strong>한다.

  ![img02](/assets/images/posts/Data_Structure/Data_Structure_1/2023-01-31-my-data-structure-1-post_2/2.png){: width="50%" height="50%"}

  예를 들어, 아래는 4개의 행과 3개의 열을 갖는 2차원 배열이고 배열 요소에 접근할 때 2개의 인덱스가 필요하다.

  <strong style="color:orange; font-size:15pt">C#은 1차원 ~ 32차원 배열까지를 지원한다.</strong>

## 🔖 가변 배열 (Jagged Array)

  가변 배열(Jagged Array)은 <strong style="color:limegreen;">그 배열의 배열 요소가 배열 타입인 경우</strong>를 말함.  

  따라서 배열의 <strong style="color:limegreen;">배열 요소가 서로 다른 차원과 크기를 갖는 배열</strong>이 될 수 있음.    

  ```c#
  // 가변 배열
  float[][] A = new float[4][]

  // 서로 다른 크기의 배열 할당
  A[0] = new float[2];
  A[0] = new float[4]{1,2,3,4};
  A[0] = new float[6]{1,2,3,4,5,6}; 
  A[0] = new float[3]; 
  ```

  아래는 그림으로 표현한 것이다.

  ![img03](/assets/images/posts/Data_Structure/Data_Structure_1/2023-01-31-my-data-structure-1-post_2/3.png){: width="50%" height="50%"}

  <strong style="color:orange; font-size:15pt">C# 에서는 다차원 배열을 [,] 와 같이 표현하는 반면 가변 배열은 [][] 와 같이 표현한다.</strong>

## 🔖 동적 배열 (Dynamic Array)

  초기화시 미리 크기가 지정되는 정적 배열(Static Array)과는 달리 이를 보완하고자 <strong style="color:limegreen;">확장과 축소하는 기능을 가지고 있는 배열</strong>을 동적 배열(Dynamic Array) 이라고 한다.

  >정적 배열의 예시 :: int[], string[,] 

  동적 배열을 만드는 가장 간단한 방법은 <strong style="color:limegreen;">배열 요소가 추가될 때마다 크기를 하나씩 늘려주는 것</strong>이다.

### 📍 배열의 요소가 추가될 때마다 크기를 하나씩 확장하는 코드

  ```c#
  public static class DynamicArray
    {
        static object[] arr = new object[0];

        public static void Add(object element)
        {
            var temp = new object[arr.Length+1];
            for (int i = 0; i < arr.Length; i++)
            {
                temp[i] = arr[i];
            }

            arr = temp;
            arr[arr.Length-1] = element;
        }
    }
  ```

  위와 같은 방식은 꼭 필요한 공간만 사용한다는 장점이 있지만, <strong style="color:crimson;">매번 공간을 생성하고 배열의 요소를 전부 복사해야 한다는 단점</strong>이 있음.  
  이 방식은 하나의 요소를 추가할 때마다 전체 기존 배열을 복사해야 하고, 배열의 크기가 n 이라고 가정할 때 O(n)의 시간이 소요되는 것이다.  

### 📍 배열의 요소가 추가될 때마다 크기를 성장인자만큼 확장하는 코드

  ```c#
  public static class DynamicArray
    {
        static object[] arr;
        const int GROWTH_FACTOR = 2;
        public static int Count { get; private set; }
        public static int Capacity => arr.Length;
        
        public static void Initialize()
        {
            int capacity = 15;
            arr = new object[capacity];
            Count = 0;
        }

        public static void Add(object element)
        {
            int newSize = Capacity * GROWTH_FACTOR;
            var temp = new object[newSize];
         
            for (int i = 0; i < arr.Length; i++)
            {
                temp[i] = arr[i];
            }
            arr = temp;
            arr[Count] = element;
            Count++;
        }
    }
  ```

  이런 비효율적인 <strong style="color:Yellow;">단점을 해결하기 위해서는 위와 같이 배열을 2배씩 확장해 주는 것</strong>이다.

  동적 배열에서 <strong style="color:limegreen;">성장인자(GROWTH_FACTOR)를 흔히 다루는데, 이는 배열이 꽉 찼을 경우 배열을 얼마만큼 늘려야 하는지 정하는 인자</strong>이다.
  
  >이건 각 Framework 또는 Library 마다 다르지만 통상 2배 혹은 1.5배를 많이 사용함.

  위의 샘플 코드와 같이 작성하면 처음 배열의 크기가 15이고, 다음 확장시 30, 60 순으로 성장인자 값(2배)만큼 증가한다.  
  따라서 배열의 0번 인덱스부터 14까지는 새로운 배열요소를 추가할 때 즉시 추가되고, 16번째 요소 추가시 30 배열 크기를 가진 새 배열로 확장한 후, 15개의 기존 요소들을 복사한다.

  ![img04](/assets/images/posts/Data_Structure/Data_Structure_1/2023-01-31-my-data-structure-1-post_2/4.png){: width="50%" height="50%"}

  배열 요소 추가에 따른 수행시간을 그래프로 그리면 위와 같다.
  차트를 보면 16번째 배열 요소 추가시, 다음 확장은 30 배열 크기를 가지는 새 배열로 확장될 것이고, 15개의 기존 요소들을 복사하기 때문에 15의 수행 시간을 가지는 것을 볼 수 있다.  
   
  하지만 15의 복사 수행 시간을 기존 1번째 요소부터 15번째 요소까지의 수행시간에 각각 분할해서 분산시킨다면 
  >결국 O(1)*2이 되어 결론적으로 수행시간은 O(1)이 됨을 알 수 있다.  

  이는 31번째 배열 요소 추가시에도 동일하게 적용된다.

  이렇게 일부 수행에서 일어나는 <strong style="color:limegreen;">비싼 수행 비용을 분산시켜 여러 다른 일반 수행들로 분할 상환하여 비용을 계산하는 방식을 분할 상환 분석 (Amortized Analysis)</strong>이라 부른다.

  분할 상환 분석을 통해 하나씩 배열을 증가시키는 동적 배열 방식(수행시간 O(n)) 보다 배열을 1.5배 혹은 2배로 증가하는 방식 (수행시간 O(1))이 훨씬 효율적임을 알 수 있다.

  >위에서 O(n), O(1) 이렇게 작성한 것은 Big-O 표기법 이라고 하는 점근 표기법(알고리즘의 수행시간을 대략적으로 나타내는 방법)중 하나이다.  
  >이건 나도 헷갈리고 완전히 이해한게 아니라서 나중에 심도 있게 다룰 예정

## 🔖 원형 배열 (Circular Array)

    처음에 이 배열을 보고 든 생각은 "세상에는 정말 신기한 배열이 많구나" 라는 생각이었다.  

  원형 배열(Circular Array)은 고정된 크기의 배열을 마치 양 끝이 연결된 것 처럼 사용할 수 있게 한 자료구조이다.
  
  >이것은 다른 이름으로 원형 버퍼(Circular Buffer), 링 버퍼(Ring Buffer) 라고도 불리운다.  

  <strong style="color:Yellow; font-size:15pt">배열의 마지막 요소에 도착하면 다시 첫번째 요소로 순환하는 구조. </strong>

  따라서 <strong style="color:salmon;">원형 배열은 FIFO(First in first out)구조의 데이터 버퍼에 적합</strong>하고, <strong style="color:dodgerblue;">비원형의 일반 배열은 LIFO(Last in first out) 구조의 버퍼에 적합</strong>하다.  

  >원형 배열은 FIFO 구조를 가진 Queue 또는 데이터 스트림 버퍼 등을 구현할 때 흔히 사용된다.

  ![img05](/assets/images/posts/Data_Structure/Data_Structure_1/2023-01-31-my-data-structure-1-post_2/5.png){: width="50%" height="50%"}

  원형 배열은 배열을 순환하는 구조로 만들어야 하므로, 배열 인덱스를 증가시킬 때 나머지(mod) 연산자를 사용하여 마지막 배열의 다음 인덱스가 첫 배열 인덱스로 돌아오게 한다.
  
  <strong style="color:orange; font-size:15pt">C#에서 나머지 연산자는 %로 표시된다.</strong>  

### 📍 나머지 연산자 샘플 코드

  ```c#
  public static class Test
  {
      public static int Length = 8;
      public static void Start()
      {
          for (int i = 0; i < Length + 1; i++)
          {
              Console.WriteLine($"{Length} % {i} = {i%Length}");
          }
      }
  }
  ```

### 📍 나머지 연산자 샘플 코드 결과값

  ![img06](/assets/images/posts/Data_Structure/Data_Structure_1/2023-01-31-my-data-structure-1-post_2/6.png)

  위와 같이 첫번째 인덱스 0으로 돌아오는 것을 확인할 수 있다.

### 📍 나머지 연산자를 이용한 문제

  책에서는 간단한 사례로 원형 배열에 익숙해질 수 있도록 도와준다.  
  예를 들어 원형 탁자에 6명의 사람이 앉아 있다고 가정한다.

  그리고 그사람으로 부터 시계방향으로 모든 사람들의 명칭을 순서대로 출력하는 프로그램을 만들어 보는 것이다.

  ![img07](/assets/images/posts/Data_Structure/Data_Structure_1/2023-01-31-my-data-structure-1-post_2/7.png){: width="50%" height="50%"}

  이 문제를 비원형 배열로 해결할 경우 간단한 방법은 A배열 뒤에 A' 배열을 한번 더 추가해서 뒤에 붙이는 방식이다.  

  만약 그렇게 한다면 배열의 크기는 12가 되고 아래와 같이 될 것이다.  
  
    abcdefabcdef

  그리고 c로부터 시작해서 모든 사람들의 명칭을 순서대로 출력하려면 c부터 6의 크기만큼 읽으면 된다.  
  하지만 이러한 방식의 <strong style="color:Yellow;">단점은 배열의 크기만큼 중복된 공간이 더 필요하고 현재의 배열을 중복 복사해야 해서 비효율적인 공간이 생긴다는 것</strong>이다.  

  이러한 문제를 해결하기 위해 순환하는 원형 배열을 사용할 수 있다.  
  해당 원형 배열을 그림으로 표현하자면,

  ![img08](/assets/images/posts/Data_Structure/Data_Structure_1/2023-01-31-my-data-structure-1-post_2/8.png){: width="50%" height="50%"}

  위의 이미지처럼 된다.

### 📍 나머지 연산자를 이용한 문제 코드
  ```c#
  public static class Test
  {
      static char[] A = "abcdef".ToCharArray();
      public static void GetNames(int selectIndex) // selectIndex = 2
      {
          for (int i = 0; i < A.Length; i++)
          {
              var findIndex = (selectIndex + i) % A.Length;
              Console.WriteLine(A[findIndex]);
          }
      }
  }
  ```
  위의 코드는 내가 예제로 만든 코드이다.  
  외부에서 Test 클래스의 GetNames 메서드를 호출하면(나는 2를 인수로 보냄) for문 본문에서 순환이 일어나고, 아래와 같이 원하는 값이 출력 된다.

  ### 📍 나머지 연산자를 이용한 문제 결과값

  ![img09](/assets/images/posts/Data_Structure/Data_Structure_1/2023-01-31-my-data-structure-1-post_2/9.png)


## 🔖 .NET의 배열 클래스

    .NET Framework은 고정, 동적 배열을 지원한다.  

### 📍 고정 배열
  
  1. 1 ~ 32차원까지 지원한다. 
  2. 기본적으로 배열은 최대 2GB 까지의 크기를 가질 수 있다.  

  >단, 64bit 환경에서는 app.config 에서 gcAllowVeryLargeObjects 값을 enable로 하면 2GB보다 클 배열을 가질 수 있음.

  3. .NET의 고정 배열은 모두 System.Array 추상 클래스로부터 파생된다.
  4. 이 Array 클래스는 컴파일러나 시스템에서만 파생 클래스를 만들 수 있기 때문에, 개발자가 커스텀 할수 없다. 


### 📍 동적 배열

    ArrayList 와 List<T> 클래스를 지원한다.

  1. ArrayList는 object 타입의 동적 배열을 가진다.
  2. List<T>는 개발자가 임의의 타입(T)을 지정할 수 있는 Generic 타입의 동적 배열이다.
  3. List<T>는 초기 배열 크기가 4이고, 2배씩 증가한다.

<br>

# 📢 오늘의 한마디
<hr style="width:100%" />
  
<strong style="color:Yellow; font-size:15pt">⚡배열은 이쁘다.</strong>


<hr style="width:100%" />

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr style="width:100%" />

[Top](#){: .btn .btn--primary }{: .align-right}