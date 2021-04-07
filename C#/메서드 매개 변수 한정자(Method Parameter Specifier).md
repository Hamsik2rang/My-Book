# C#의 메서드 매개 변수 한정자

## ref

C#은 **Call-By-Reference**형태를 **ref 한정자**를 이용해 지원한다.

메서드의시그니처에 reference 형태로 전달받을 매개변수의 타입 앞에 **`ref`** 키워드를 붙인 후, 해당 메서드를 호출할 때에도 **`ref`** 키워드를 붙이면 된다.

```c#
using System;

namespace CSharp
{
    class MainProgram
    {
        static void Main(string[] args)
        {
 			int a = 4, b = 3;
        
        	Swap(ref a, ref b);
        
        	Console.WriteLine($"{a}, {b}");
        }
        
        static void Swap(ref int a, ref int b)
        {
            var temp = a;
            a = b;
            b = temp;
        }
    }
}
```

```text
3, 4
```

마찬가지로, `ref`를 함수의 리턴에 사용할 수 있다. 이 때 ref 변수가 아닌 일반 변수에 리턴값을 저장하면 일반 변수로서 기능하고, ref 변수에 저장하면 레퍼런스로써 기능한다.

```c#
public ref int foo()
{
    //...
}

ref int a = foo();	// reference 형태
int b = foo();		// value 형태
```

## out

C# 함수는 일반적으로 하나의 리턴 값을 가지므로, 여러 개의 리턴을 원한다면 **`out`** 키워드를 이용한다.

out 키워드는 ref처럼 변수를 참조 형태로 전달받을 수 있게 하면서, 함수 내에서 해당 참조 변수에 값을 저장하는 것을 강요한다.

즉, **전달받은 out parameter가 수정되지 않으면 컴파일 에러가 발생**한다.

```c#
public void foo(ref int a, out int b)
{
    // Do Nothing -> a is OK, b is Error.
}
```

**`out`** 파라미터 역시 **`ref`** 파라미터와 마찬가지로 메서드 호출 시 변수 타입 앞에 키워드를 붙여야 한다.

```c#
public void foo(out int a, out int b)
{
    a = 3;
    b = 4;
}

//...

int a = 33;
int b = 44;

foo(out a, out b);

Console.WriteLine($"{a}, {b}");
```

  ```text
3, 4
  ```

또한, **Out 파라미터는 값이 저장되는 것이 확실하므로 임의의 선언 없이 호출 지점에서 변수를 선언할 수 있다.**

```c#
public void foo(out int a, out int b)
{
	a = 3;
    b = 4;
}

//...

foo(out int x, out int y);

Console.WriteLine($"{x}, {y}");
```

```c#
3, 4
```

## in

메서드에 파라미터를 넘기게 되면 **Call-by-Value 형태로 전달되므로 값 복사 비용(주소라 할지라도!)이 발생**한다.

**C++은 const Reference를 통해 이 비용을 해소**할 수 있는데, **C#에서는 in 키워드**를 사용한다.

**`in`** 키워드를 사용하면 **`ref`**와 동일하게 참조 형태로 파라미터가 전달되지만 함수 내에서 해당 파라미터의 값을 수정할 수 없게 강제한다.

```c#
public void foo(ref int a, in int b)
{
	a = 3;	// OK
    b = 4;	// Error
}
```

**`in`** 파라미터는 호출 사이트에서 한정자를 붙일 필요가 없다.

```c#
public void foo(ref int a, out int b, in int c)
{
	// Do Something
}

int x = 3;
int y = 4;
int z = 5;

foo(ref x, out y, z);
```

이를 통해 호출 사이트에서 한정자를 붙이는 것은 해당 함수가 호출되면 전달된 파라미터가 수정될 수 있음을 호출 코드만 보고도 알 수 있도록 하기 위한 의도라는 것을 알 수 있다.

## params

메서드에 인자의 개수를 지정하지 않고 인자로 넘기고 싶다면 **`params`** 한정자를 사용한다.

```c#
public void foo(params int[] a)
{
    foreach (var i in a)
    {
        Console.WriteLine(i);
    }
}

//...

foo(1,2);
foo(3,4,5);
foo(6,7,8,9,10);
```

```text
1
2
3
4
5
6
7
8
9
10
```

params 파라미터는 다른 키워드들과 함께 사용될 경우 **가장 마지막**에 위치해야 한다.

```c#
public void foo(params int[] a, int b)
{
    // Error
}

public void foo(int a, params int[] b)
{
    // OK
}
```

---

이 외에도 C#은 **Named Parameter**와 **Default Parameter**를 지원한다.

```c#
public void foo(int a, int b = 0);

foo(b:3, a:4);
foo(4);
```

