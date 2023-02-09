---
title:  "[C# 상식] 난 using이 namespace인 줄만 알았어"
excerpt: "using은 네임스페이스에서만 사용되는 게 아니란다."

categories:
  - C Sharp
tags:
  - [C Sharp]
toc: true
toc_sticky: true
date: 2023-02-09
last_modified_at: 2023-02-09
---

# 너가 왜 거기 있니?

공부를 하다가 갑자기 메서드 내부에 using이 들어가는 경우를 본 적이 있다.

```c#
private Item LoadSingleXml<Item>(string name)
{
  using (MemoryStream stream = new MemoryStream(System.Text.Encoding.UTF8.GetBytes(textAsset.text)))
    return (Item)xs.Deserialize(stream);
}
```

나는 얘가 대체 왜 여기 있는지 전혀 이해가 가지 않았다.

namespace만 따라다니던 너가 대체 왜 여기에?

그래서 정말 똑똑한 ChatGPT에게 위대한 한글로 물어 봤다.

![img1](/assets/images/posts/DotNet/CSharp/2023-02-09-my-csharp-post_1/1.png){: width="70%" height="70%"}

![img2](/assets/images/posts/DotNet/CSharp/2023-02-09-my-csharp-post_1/2.png){: width="50%" height="50%"}

그러자 두 가지의 경우로 나눠서 설명을 해줬다.

C#에서 using 문은 다음과 같은 경우에 사용된다.

## 1. namespace 임포트

- `using` 문을 사용하여 특정 네임스페이스의 타입을 자주 사용할 때 그 네임스페이스의 전체 경로를 작성할 필요 없이 타입만 작성하여 사용할 수 있다.

예시는 아래와 같다.

```c#
using System.IO;

namespace MyNamespace
{
    class Program
    {
        static void Main(string[] args)
        {
            File.WriteAllText("example.txt", "Hello, World!");
        }
    }
}
```

## 2. IDisposable 인터페이스 관리

- `IDisposable` 인터페이스 관리: `using` 문은 `IDisposable` 인터페이스를 구현한 객체를 사용할 때, 객체의 `Dispose` 메서드를 자동으로 호출하여 객체의 리소스를 반환할 수 있다.

예시는 아래와 같다.

```c#
using (StreamReader reader = new StreamReader("example.txt"))
{
    string line = reader.ReadLine();
    Console.WriteLine(line);
}
```

## Dispose가 뭐야?

C#의 `Dispose` 메서드는 객체가 사용한 리소스를 반환하는 데 사용되고, `IDisposable` 인터페이스를 구현하는 객체에서 사용할 수 있다.

`Dispose` 메서드를 호출하면 기본적으로 객체가 사용한 리소스(예를 들어, 파일 디스크, 네트워크 연결, 메모리등)를 다시 시스템에 반환할 수 있다.

또한 `Dispose` 메서드는 개발자가 수동으로 호출할 수 있지만, `using` 문을 사용하면 <strong style="color:orange; font-size:13pt">자동으로 호출되어 플머의 부담을 줄여준다.<strong> 

예를 들어, 아래의 코드에서 `StreamReader` 객체는 `using` 문을 벗어나면서 자동으로 `Dispose` 메서드가 호출되어 객체가 사용한 리소스를 반환한다.

```c#
using (StreamReader reader = new StreamReader("file.txt"))
{
    // Read the contents of the file here.
}
```

## 객체의 리소스를 반환? 대체 한글 왜이리 어려워 도와줘요 스피드 웨건!

![img3](/assets/images/posts/DotNet/CSharp/2023-02-09-my-csharp-post_1/3.jpg){: width="40%" height="40%"}

C# 객체의 리소스 반환은 <strong style="color:orange; font-size:13pt">객체가 사용한 시스템 리소스(예를 들어, 메모리, 파일 디스크, 네트워크 연결 등)를 다시 시스템에 반환</strong>하는 것을 뜻한다.

객체는 시스템 리소스를 사용하면서 작업을 수행할 수 있지만, 객체의 <strong style="color:orange; font-size:13pt">생명주기가 끝나면 이를 다시 시스템에 반환</strong>해야 한다.

그렇지 않으면 <strong style="color:deeppink; font-size:13pt">시스템의 리소스가 부족하여 다른 프로그램에서 사용할 수 없는 현상이 발생</strong>한다.

따라서, 객체가 사용한 리소스를 다시 시스템에 반환하는 것은 <strong style="color:orange; font-size:13pt">효율적인 리소스 관리와 프로그램의 안정적인 실행을 유지하는 데 매우 중요</strong>하다.

<br>
<hr style="width:100%" />

    이 게시물에는 지극히 주관적인 생각이 포함되어 있습니다. 
    오류나 틀린 부분, 또는 수정해야 할 부분이 있다면 언제든지 댓글 혹은 메일로 지적 부탁드립니다.
    
<hr style="width:100%" />

[Top](#){: .btn .btn--primary }{: .align-right}