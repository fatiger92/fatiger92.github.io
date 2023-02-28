---
title:  "[C# 으로 이해하는 자료구조] 4. 연결리스트 극장 4부작 (2/4)"
excerpt: "C# 으로 이해하는 자료구조 - Alex Lee 선생님의 책으로 공부하는 공간입니다."

categories:
  - Data Structure 1
tags:
  - [Data Structure 1]
toc: true
toc_sticky: true
date: 2023-02-20
last_modified_at: 2023-02-28
---

알라딘에 있는 Alex Lee님의 **C#으로 이해하는 자료구조** 책을 보고 공부하며 정리 및 요약한 게시글입니다.  
종이서적을 별로 좋아하지 않기 때문에 링크는 전자책입니다.
<br>
[🔔책 구매하기 링크](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=183854923)
{: .notice--warning}

본 글은 내돈내산이기 때문에 다소 비판적인 내용이 있을 수도 있습니다.
{: .notice--warning}

# 💾 연결 리스트 (LinkedList)
<hr style="width:100%" />

## 🔖 연결 리스트의 기초 개념

  <strong style="color:Yellow; font-size:15pt">각 노드가 데이터와 포인터를 가지고 있으면서 노드들이 한 줄로 쭉 연결되어 있는 방식으로 데이터를 저장하는 구조.</strong>  

각 노드가 한방향으로 다음 노드를 가리키고 있는 리스트를 단일 연결 리스트(Singly Linked List)라고 하고, 각 노드가 양방향으로 이전 노드와 다음 노드를 모두 가리키는 경우 이중 연결 리스트(Doubly Linked List)라고 한다.

## 🔖 원형 연결 리스트 (Circular Linked List)

<strong style="color:orange; font-size:15pt">🔎일반 연결 리스트에서 마지막 노드를 처음 노드에 연결시켜 원형으로 만든 구조.</strong>

- 원형 단일 연결 리스트 (Singly Circular Linked List) : 원형 연결 리스트를 한 방향으로 연결한 것
- 원형 이중 연결 리스트 (Doubly Circular Linked List) : 원형 연결 리스트를 양 방향으로 연결한 것

## 🔖 이중 원형 연결 리스트 (Doubly Circular Linked List)

![img01](/assets/images/posts/Data_Structure/Data_Structure_1/2023-02-10-my-data-structure-1-post_4/1.png){: width="50%" height="50%"}

위의 이미지는 <strong style="color:limegreen; font-size:13pt">4개의 노드를 갖는 이중 원형 연결 리스트</strong>다.

원형 연결 리스트는 단일, 이중 연결 리스트에서 처음과 마지막 노드를 서로 연결하는 것만 추가해 주면 된다.

### 📍 Add(newNode) 메서드
<hr style="width=100%" />

이중 원형 연결 리스트는 이중 연결 리스트와 달리 마지막 노드를 찾기 위해 순차적으로 전부 검색할 필요가 없다.  
왜냐라면 Head의 이전 노드가 항상 마지막 노드이기 떄문이다.

![img02](/assets/images/posts/Data_Structure/Data_Structure_1/2023-02-10-my-data-structure-1-post_4/2.png){: width="70%" height="70%"}

```c#
public void Add(DoublyLinkedListNode<T> newNode)
{
    if (head != null) // 리스트가 비어 있지 않으면 head와 tail 사이에 삽입
    {
        // 순환 구조이기 때문에 head의 이전 노드가 Tail임. 
        var tail = head.PrevNode; 
        
        head.PrevNode = newNode;
        tail.NextNode = newNode;
        newNode.PrevNode = tail;
        newNode.NextNode = head;
    }
    else // 리스트가 비어 있으면
    {
        head = newNode;
        head.NextNode = head;
        head.PrevNode = head;
    }
}
```

### 📍 AddAfter(curNode,newNode) 메서드
<hr style="width=100%" />

![img03](/assets/images/posts/Data_Structure/Data_Structure_1/2023-02-10-my-data-structure-1-post_4/3.png){: width="100%" height="100%"}

이중 연결 리스트와 다른 점은 null 체크 부분이다.  

    이중 연결 리스트는 마지막 노드의 NextNode가 null 이기 때문에 null 체크해야 하지만,   
    이중 원형 연결 리스트는 마지막 노드의 NextNode가 Head 이기 때문에 별도의 null 체크를 하지 않는다.

라고 하는데 뭔가 이상하다는 점을 느꼈다.  

책에 소개되는 이중 연결 리스트에서도 AddAfter 메서드에서 별도의 null 체크를 하지 않는다.

물론 이중 연결 리스트일 경우 마지막 노드의 NextNode가 null일 경우가 존재한다.  
하지만 엄연히 마지막 노드가 null인 경우는 Add 메서드를 사용하고, 중간에 노드를 추가할 경우에는 AddAfter 메서드를 사용한다.

이건 이중 원형 연결 리스트도 동일하다.  
(함수 본문이 동일하다는 말이 아님)

그런데 이 내용을 대체 왜 여기에 써놨을까?  

모르겠으니 무시한다.

```c#
public void AddAfter(DoublyLinkedListNode<T> curNode, DoublyLinkedListNode<T> newNode)
{
    if (head == null || curNode == null || newNode == null)
    {
        throw new InvalidOperationException();
    }

    newNode.NextNode = curNode.NextNode;
    curNode.NextNode.PrevNode = newNode;
    newNode.PrevNode = curNode;
    curNode.NextNode = newNode;
}
```

코드를 보면 알 수 있듯이 이중 연결리스트와 같다.

### 📍 Remove(removeNode) 메서드
<hr style="width=100%" />

삭제할 노드가 Head이고 현재 노드가 Head 하나뿐이라면 Head에 null을 할당한다.  
이 경우가 아니라면 이중 연결 리스트와 같이 처리한다.

먼저 첫번째 코드는 이중 연결 리스트의 Remove 메서드다.

```c#
   public void Remove(DoublyLinkedListNode<T> removeNode)
    {
        if (head == null || removeNode == null)
            return;
        
        //삭제 할 노드가 첫 노드면
        if (removeNode == head)
        {
            head = head.NextNode;

            if (head != null)
            {
                head.PrevNode = null;
            }
        }
        else // 첫 노드가 아니면, prevNode와 nextNode를 연결
        {
            removeNode.PrevNode.NextNode = removeNode.NextNode;

            if (removeNode.NextNode != null)
            {
                removeNode.NextNode.PrevNode = removeNode.PrevNode;
            }
        }
        removeNode = null;
    }
```

그리고 아래의 두번째 코드는 이중 원형 연결 리스트의 Remove 메서드다.

```c#
public void Remove(DoublyLinkedListNode<T> removeNode)
{
    if (head == null || removeNode == null)
        return;
    
    // 삭제할 노드가 헤드고 노드가 1개면
    if (removeNode == head && head == head.NextNode)
    {
        head = null;
    }
    else // 아니면 Prev 노드와 Next 노드 연결
    {
        removeNode.PrevNode.NextNode = removeNode.NextNode;
        removeNode.NextNode.PrevNode = removeNode.PrevNode;
    }
    removeNode = null;
}
```

둘이 다른 부분만 뽑아서 비교해보자.

서로 if - else 문이 다르다.
분석한다.

```c#
// 이중 연결 리스트 Remove 메서드 본문 내 if - else 문
if (removeNode == head) //삭제 할 노드가 첫 노드면
{
    head = head.NextNode;

    if (head != null)
    {
        head.PrevNode = null;
    }
}
else // 첫 노드가 아니면, prevNode와 nextNode를 연결
{
    removeNode.PrevNode.NextNode = removeNode.NextNode;

    if (removeNode.NextNode != null)
    {
        removeNode.NextNode.PrevNode = removeNode.PrevNode;
    }
}
```

먼저 위의 이중 연결 리스트 같은 경우에는 if문의 조건으로 삭제하고자 하는 removeNode가 첫 노드인지 아닌지 확인한다. 

<strong style="font-size:15pt">[🚀 첫 노드 일 경우]</strong>  

만약 첫 노드 즉 Head라면 `head = head.NextNode` 로 현재 첫 노드의 다음 노드를 head에 할당한다.

그 다음에는 head의 null 체크를 하는데, head가 null이 아니라면 head에는 이전 노드에 null을 할당한다.
왜냐하면 이중 연결 리스트에서 head의 이전 노드는 존재하지 않는다.

<strong style="font-size:15pt">[🚀 첫 노드가 아닐 경우]</strong>  

removeNode의 이전노드의 다음노드에 remove 노드의 다음 노드를 연결한다.
(removeNode가 삭제될 것 이기 때문에)

그리고 만약 removeNode의 다음 노드가 null이 아니라면 즉 삭제하고자 하는 removeNode가 마지막 노드가 아님을 뜻한다.
따라서 removeNode가 마지막 노드가 아니라면, removeNode의 다음 노드의 이전 노드에 removeNode의 이전 노드를 연결한다.
(이전에 Next를 연결해줬으니 이번엔 Prev를 연결해 줌.)

```c#
// 이중 원형 연결 리스트 Remove 메서드 본문 내 if - else 문
if (removeNode == head && head == head.NextNode)
{
    head = null;
}
else // 아니면 Prev 노드와 Next 노드 연결
{
    removeNode.PrevNode.NextNode = removeNode.NextNode;
    removeNode.NextNode.PrevNode = removeNode.PrevNode;
}
```

이중 원형 연결 리스트를 살펴보자.  
if 문 조건으로는 삭제하고 하는 removeNode가 첫 노드인지 그리고 첫 노드와 첫 노드의 다음 노드인지 확인하고 있다.  
첫 번째 조건은 이해가 가지만, 두 번째는 잘 이해가 가지 않는다.

천천히 생각해보니 첫 노드 즉 head와 head의 다음 노드가 같은 경우는 리스트에 노드가 1개 뿐일 때이다.  
(이중 원형 연결 리스트는 순환하는 구조이기 때문에 노드가 하나 뿐일 경우, 해당 노드의 다음 노드는 자신이 된다.)

따라서 해당 조건은 "삭제할 노드가 첫 노드이고 리스트의 노드가 1개 뿐이라면" 이 될 것이다.

<strong style="font-size:15pt">[🚀 삭제할 노드가 첫 노드이고 리스트의 노드가 1개 뿐일 경우]</strong>

head에 null을 할당한다.

<strong style="font-size:15pt">[🚀 삭제할 노드가 첫 노드가 아니고 리스트의 노드가 여러개일 경우]</strong>

removeNode의 이전 노드의 다음 노드를 removeNode의 다음 노드에 연결, 그리고 Prev 연결도 진행

여기까지가 Remove 메서드의 분석이다.

>이중 연결 리스트는 마지막 노드의 NextNode가 null 이기 때문에 null 체크해야 하지만,

그럼 위에서 말한 체크는 대체 언제한다는 것일까?

### 📍 GetNode(index) 메서드
<hr style="width=100%" />

이중 연결 리스트에서 특정 위치 인덱스에 있는 노드를 리턴, 루프를 돌며 다시 순환해서 Head로 돌아온다면 찾는 Node가 존재하지 않는 것 이기 때문에 null을 리턴한다.

```c#
public DoublyLinkedListNode<T> GetNode(int index)
{
    if (head == null)
        return null;

    var cnt = 0;
    var curNode = head;

    while (cnt < index)
    {
        curNode = curNode.NextNode;
        cnt++;
        
        if (curNode == head)
            return null;
    }
    
    return curNode;
}
```

이중 원형 연결 리스트는 순환구조이기 때문에, 하나씩 돌면서 curNode가 head면 찾는 노드가 없음을 뜻하기 때문에 null 반환. 

### 📍 Count() 메서드
<hr style="width=100%" />

Head 부터 마지막 노드까지 이동하면서 카운트를 증가시킨다.   
순환되는 구조이기 때문에 마지막 노드의 다음 노드는 Head이므로 이럴 경우 루프를 중지한다.

```c#
public int Count()
{
    if (head == null)
        return 0;

    int cnt = 0;
    var curNode = head;

    do
    {
        cnt++;
        curNode = curNode.NextNode;
    } while (curNode != head);
    
    return cnt;
}
```

이게 조금 특이 한데 이중 연결 리스트에서는 그냥 while문을 쓴 방면, 이중 원형 연결 리스트는 do while문을 사용했다.  
이유는 순환구조이기 때문인데 차근차근 분석해보자.

기존의 이중 연결 리스트는 while문 조건이 `curNode != null` 이고 이중 원형 연결 리스트의 do while 문 조건은 `curNode != head` 이다.

둘의 차이는 하나는 null과 비교하고 있고, 하나는 head 즉 첫 노드와 비교하고 있다.

앞의 이중 연결 리스트는 curNode가 마지막 노드를 넘어 간다면 null이 될 경우가 있기 때문에 null과 비교한다.

하지만 이중 원형 연결 리스트는 순환 구조이고, 따라서 마지막 노드를 넘어 간다면 head가 나오기 때문에 현재 노드가 head냐? 아니냐? 에 따라 현재 노드가 마지막인지 아닌지를 판단하는 것이다.

그렇다면 의문이 든다.

![why](https://media.giphy.com/media/l3vR3gvEdsdJl26oU/giphy.gif){: width="50%" height="50%"}

<strong style="color:orange; font-size:15pt">왜? 이중 원형 연결 리스트는 do while로 구현 했을까?</strong>

예를 들어서 while을 썼다고 가정하자.  
그렇다면 아래의 코드처럼 된다.

```c#
public int Count()
{
    //... 생략

    while (curNode != head);
    {
        cnt++;
        curNode = curNode.NextNode;
    } 
    
    return cnt;
}
```

<strong style="color:orange; font-size:20pt">뭔가 이상함을 느낀다.</strong>  

카운팅을 하기 위해서는 첫 노드부터 시작을 해야하는데, 그러면 while문의 조건 때문에 셈을 시작하기도 전에 첫 노드에서 나와버려서 0을 반환 받게 된다.  
(첫 노드의 조건은 `curNode == head` 이다.)

따라서 조건이 어떻든 일단 본문을 한번 시작하는 do while문을 사용하는 것이다. 

이로써 궁금함은 해소되었다.

### 📍 C# 으로 표현한 DoublyCircularLinkedListApp Class
<hr style="width=100%" />

```c#
public class DoublyCircularLinkedListApp : MonoBehaviour
{
    public int Length = 4;
    public int SelectIndex = 0;

    DoublyCircularLinkedList<int> IntDoublyCircularLinkedList = new();
    
    void Start()
    {
        for (int i = 0; i < Length; i++)
        {
            var node = new DoublyLinkedListNode<int>(i);
            
            IntDoublyCircularLinkedList.Add(node);
        }
    }
    
    [ContextMenu("이중 원형 연결 리스트에서 입력한 인덱스 뒤에 int 값 50을 가지고 있는 Node 추가 AddAfter")]
    public void AddAfter()
    {
        var selectedNode = IntDoublyCircularLinkedList.GetNode(SelectIndex);
        var node = new DoublyLinkedListNode<int>(50);
        IntDoublyCircularLinkedList.AddAfter(selectedNode, node);
    }
    
    [ContextMenu("이중 원형 연결 리스트에서 입력한 인덱스의 노드 삭제 Remove")]
    public void Remove()
    {
        var selectedNode = IntDoublyCircularLinkedList.GetNode(SelectIndex);
        
        if(selectedNode != null)
            IntDoublyCircularLinkedList.Remove(selectedNode);
    }
    
    [ContextMenu("이중 원형 연결 리스트의 요소 전부 출력")]
    public void PrintAll()
    {
        for (int i = 0; i < IntDoublyCircularLinkedList.Count(); i++)
        {
            var data = IntDoublyCircularLinkedList.GetNode(i).Data;
            Debug.Log($"DoublyCircularLinkedListData :: {i} 번째 Data = {data}");
        }
    }
}
```

위는 내가 임의로 만든 이중 원형 연결 리스트 테스트 코드다.

<br>

# 📢 오늘의 한마디
<hr style="width:100%" />
  
<strong style="color:Yellow; font-size:15pt">⚡포스트의 길이를 적당하게 해서 올리는 게 좋겠다. </strong>

내용을 다 담을려다 보면 포스트를 올리는 데도 소요되는 시간이 당연히 내용에 비례해서 늘어나는 데, 늘어나는 과정에서 굉장히 루즈해진다.

<hr style="width:100%" />

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr style="width:100%" />

[Top](#){: .btn .btn--primary }{: .align-right}