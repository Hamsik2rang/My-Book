# A Tour of C++ (2nd Edition) // Section 2.

## 1. Introduce of User-Defined Types

C++에서 *built-in type*은 다음을 지칭한다.

*   fundamental types(`int`, `char`, `double`, etc.)
*   `const` 한정자
*   declarator operators(*, &, [], etc.)

이러한 built-in 타입들은 매우 다양하지만 저수준의 동작을 하도록 설계되어 있다. 즉, 하드웨어의 상태나 그 구조를 반영하는 데 초점이 맞춰져 있는 것이다.

그 결과 이러한 타입들과 연산자들은 프로그래머에게 고수준의 어플리케이션 작성을 위한 기능들을 지원하는 데 한계를 가지는데, C++은 이러한 단점을 해소하고자 프로그래머들로 하여금 원하는 고급 기능들을 직접 구현할 수 있는 복잡한 *추상 메커니즘(Abstraction Mechanism)*을 제공한다.

C++의 추상 메커니즘은  주로 프로그래머들이 적절한 표현과 동작을 이용해 그들에게 필요한 타입을 직접 설계하고 사용할 수 있도록 디자인되어 있다. 이러한 추상 메커니즘을 통해 설계한 타입들을 *사용자 정의 타입(user-defined type)* 이라 하며, 클래스(class), 또는 열거체(enumerations)가 이에 속한다.

따라서 많은 책들이 이러한 타입들을 설계하고, 사용하는 방법에 집중하는데, 이러한 사용자 정의 타입들은 다음과 같은 이유 덕분에 *built-in type*보다 선호되기도 한다.

*   사용이 간편하다.
*   에러 발생 가능성이 상대적으로 낮다.
*   효율적이며, 때로는 더 좋은 성능을 보인다.

Section 2에서 소개하는 사용자 정의 타입들은 다음과 같다.

*   구조체(Structure)
*   클래스(Class)
*   공용체(Union)
*   열거체(Enumeration)

## 2. Structure

사용자 정의 타입을 만드는 첫 번째 단계는 해당 타입이 필요로 하는 여러 원소(element)들을 한데 모으는 것이며, 이를 **구조체**로 구현할 수 있다.

구조체 안에 존재하는 여러 원소들을 멤버*(member)* 라 하는데, C++의 구조체에는 C와 달리 멤버로서 함수도 포함될 수 있다.

```c++
struct Vector
{
	int size;
	double* elem;

	void init(int s)
	{
		size = s;
		elem = new double[size];
	}
	
	void setData(int count...)
	{
		va_list list;
		va_start(list, count);

		for (int i = 0; i < count; i++)
		{
			elem[i] = va_arg(list, double);
		}
		va_end(list);
	}

	void print() const
	{
		for (int i = 0; i < size; i++)
		{
			std::cout << elem[i] << ' ';
		}
		std::cout << std::endl;
	}
};
```

## 3. Class





## 4. Union





## 5. Enumeration





## 6. Advice

1.  내장 타입이 너무 저수준의 역할을 하는 것 같다면 내장 타입보다는 잘 정의된 사용자 정의 타입을 사용하라.

    *Prefer well-defined user-defined types over built-in types when the built-in types are too low-level.*

2.  연관된 데이터들은 **구조체** 또는 **클래스** 형식으로 조직화하라.

    *Organize related data into structures(**struct**s or **class**es).*

3.  **클래스**를 이용해 인터페이스와 구현 간의 차이를 표현하라.

    *Represent the distinction between an interface and an implementation using a **class**.*

    (간단히 말하면 **정보은닉*(information hiding)***을 구현하라는 말 같습니다.)

4.  **구조체**는 단순히 모든 멤버가 `public`인 클래스이다.

    *A **struct** is simply a **class** with its members **public** by default.*

5.  **클래스**의 초기화를 보장하기 위해 생성자를 정의하라.

    *Define constructors to guarantee and simplify initialization of **class**es.*

6.  구조가 드러나 있는 **공용체**의 사용을 피하라. 대신 공용체를 타입 필드와 함께 클래스로 묶어라.

    *Avoid "naked" **union**s. wrap them in a class together with a type field.*

    (구조가 드러나 있는 공용체란, 공용체 내 데이터의 타입을 구분할 타입 필드와 공용체가 독립적으로 접근 가능한 상태에 놓인 공용체를 말합니다.)

7.  명명된 상수들의 집합을 표현할 때 **열거체**를 사용하라.

    *Use **enumerations** to represent sets of named constants.*

8.  의도하지 않은 실수를 최소화하기 위해 단순 열거체 대신 열거체 클래스를 사용하라.

    *Prefer **class enum**s over "plain" **enum**s to minimize surprises.*

9.  안전하고 간단한 사용을 위해 열거체의 연산을 정의하라.

    *Define Operations on enumerations for safe and simple use.*

