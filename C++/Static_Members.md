# Static Members(정적 멤버 변수& 정적 멤버 함수)

## 정적 멤버 변수

일반적으로 각 객체는 자신만의 고유한 데이터(인스턴스 멤버 변수)를 가진다. 이와 달리 모든 객체가(즉, 클래스 전체가) 공유하는 변수를 선언할 수 있는데 이를 **정적 멤버 변수(Static Member Variable)** 라 한다.

정적 멤버 변수는 **`static`** 접두 키워드를 이용해 선언할 수 있다.

```c++
/* Test.h */

class Test
{
    public:
    int m_int = 3;
    static int s_int;
    static constexpr int sc_int = 5;
    
    void print() const;
};
```

위 코드에서 각 변수는 다음과 같다.

*   m_int : 인스턴스 멤버 변수
*   s_int : 정적 멤버 변수
*   sc_int : 정적 멤버 상수

정적 멤버들은 클래스의 모든 인스턴스가 공유하므로, 다음과 같은 구조이다.

![](./Images/Class_Instance_Structure.png)

각 인스턴스들은 자신만의 고유한 m_int를 가지지만, s_int와 sc_int는 모두가 함께 사용한다.

```c++
//...

int Test::s_int = 3;    // 정적 변수 초기화

int main()
{
    std::cout << &Test::s_int << " " << Test::s_int << std::endl;
    
    Test t1;
    Test t2;
    
    std::cout << &t1.s_int << " " << t1.s_int++ << std::endl;
    std::cout << &t2.s_int << " " << t2.s_int++ << std::endl;
    
    return 0;
}
```

위 코드의 결과는 다음과 같다.

```text
0B842A64 3
0B842A64 3
0B842A64 4
```

살펴볼 점은 다음과 같다.

*   **정적 변수 초기화는 클래스 내부에서 하지 않는다.**

    정적 변수의 경우, 클래스 내부에서 초기화가 불가능하다. 이유는 다음과 같다.

    *   클래스 내부에서 초기화 코드를 작성하면 인스턴스가 생성될 때마다 정적 변수가 초기화되어야 하는데, 이미 존재하는 정적 변수를 다시 초기화할 수 없기 때문이다.
    *   정적 변수는 인스턴스와 상관 없이 클래스 단위의 데이터인데, 인스턴스가 생성되어야만 해당 변수가 초기화된다면 클래스만으로 사용할 수 없게 된다.

    따라서 **정적 멤버 변수의 초기화는 클래스의 정의 부분(*.cpp)에서 해 주게 된다.**

    단, **정적 멤버 상수의 경우 선언과 동시에 초기화가 되어야 하기 때문에 클래스 내부에서 초기화가 가능**하다.

*   **클래스와 범위 지정 연산자**만을 이용해 **인스턴스 없이 접근할 수 있다.**

*   **두 인스턴스에서 각각 호출한 s_int의 주소가 동일**하며, **동일한 값을 공유**한다(t1의 s_int를 증가시켰더니 t2의 s_int도 증가되어 있다).



---



## 정적 멤버 함수

정적 멤버 변수를 public으로 선언한 경우에는 아무 제약 없이 접근 가능하다.

그러나 정적 멤버 변수가 private으로 선언되었다면 직접 접근이 불가능해진다.

어쨌든 각 인스턴스는 정적 멤버 변수에 접근할 수 있으므로 정적 멤버 변수에 간접적으로 접근하도록 하는 인스턴스 멤버 변수를 생성해도 되지만, 이 경우 클래스만으로 정적 멤버 변수에 접근하는 것은 불가능하다.

**정적 멤버 함수(Static Member Function)** 는 별도의 인스턴스 없이 호출할 수 있는 멤버 함수이다.

```c++
class Test
{
    private:
    int _value = 3;
    static int s_value;
    
    public:
    int GetValue() const
    {
        return _value;
    }
    
    static int GetStaticValue()
    {
        return s_value;
    }
}

int Test::s_value = 31;

int main()
{    
    std::cout << Test::GetStaticValue() << std::endl; 
    
    Test t1;
    Test t2;
    
    std::cout << t1.GetStaticValue() << std::endl;
    std::cout << t2.GetStaticValue() << std::endl;
    
    return 0;
}
```

출력 결과는 다음과 같다.

```text
31
31
31
```

살펴볼 점은 다음과 같다.

*   **정적 멤버 함수** 역시 정적 멤버 변수와 마찬가지로 클래스와 범위 지정 연산자만으로 호출할 수 있으며, 각 인스턴스를 통해서도 호출할 수 있다.

*   **정적 멤버 함수**는 const 한정자를 사용할 수 없다.

    const 한정자는 함수를 공유하는 인스턴스 중 특정 인스턴스가 함수를 사용하려고 this 포인터를 넘겨줄 때 해당 인스턴스가 가진 멤버 변수를 변경하지 않도록 this 포인터에 const를 지정해 주는 것인데, 정적 멤버 함수는 별도의 this 포인터 전달이 없으므로 const를 붙일 수가 없다.

    -이후 작성예정-



```c++
#include <iostream>
#include <functional>

class Test
{
private:
	int _value = 3;
	static int s_value;

public:
	// static member function은 this 포인터로 지정할 수 있는 멤버를 사용할 수 없다. 즉, 정적 멤버 변수만 사용할 수 있다.
	static int GetValue()
	{
		return s_value;
	}

	int temp()
	{
		return _value;
	}
};

int Test::s_value = 31;

int main()
{
	Test t1;
	Test t2;

	// 객체의 멤버 함수는 각 인스턴스마다 가지는 것이 아니라 모두가 공유하며, 사용할 때 사용하는 인스턴스의 this 포인터를 넘김으로써 해당 객체의 정보를 기반으로 사용한다.
	// 따라서 멤버 함수를 포인터로 가리킬 때는 아래처럼 할 수 없고,
	// int(Test::*fptr)() = &t1.temp;
	// 아래처럼 해야 한다.
	int (Test:: * fptr)() = &Test::temp;
	// C++11의 functional을 이용할 땐 아래처럼 한다.
	std::function<int(Test*)> myFptr = &Test::temp;
	// 만약 특정한 객체를 바인딩할 때에는 std::bind와 함께 쓸 수도 있다.
	std::function<int()> myBoundFptr = std::bind(&Test::temp, t1);
	
	// 멤버 함수 포인터는 다음과 같이 사용한다.
	std::cout << (t1.*fptr)() << std::endl;
	std::cout << (t2.*fptr)() << std::endl;

	// 반면, 정적 멤버 함수는 특정 객체의 정보를 이용하지 않는 함수이므로 일반 함수와 동일하게 사용해야 한다.
	int(*sfptr)() = &Test::GetValue;
	// C++11의 functional을 이용할 때에도 마찬가지로 일반 함수처럼 사용한다.
	std::function<int()> mySFptr = &Test::GetValue;


	return 0;
}
```

