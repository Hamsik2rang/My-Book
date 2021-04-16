# Ellipsis(생략 부호)

C#에 가변 길이 인자를 지원하기 위한 **`params`** 문법이 있는 것 처럼, C++에는 Ellipsis 문법이 존재한다.

Ellipsis는 단어 뜻대로 Parameter를 **생략**할 수 있도록 해 주는 문법이다.

아래는 전체 예시 코드이다.

```c++
#include <iostream>
#include <cstdarg>	//for Ellipsis

double FindAverage(int dummy,int count, ...)
{
	double sum = 0;
	// 가변인수들을 저장할 가변 리스트 생성
	va_list list;

	// va_list 초기화. 메모리 상에서 고정 인수의 바로 다음 주소로 이동시키기 때문에 마지막 고정 인수를 넣어줌.
	va_start(list, count);
	// 마지막 이전 고정 인수를 넣어주면...그 다음 고정인수부터 list가 시작되니까 결과가 요상해짐 ㅎㅎㅋㅋ;
	// 예를 들면 이렇게 ↓ 
	// va_start(list, dummy);


	for (int i = 0; i < count; i++)
	{
		// 인자를 순서대로 하나씩 사용. Iterator와 동일하게 작동. 인자를 하나 반환할 때마다 인자 크기만큼 주소 증가 연산.
		sum += va_arg(list, int);
	}
	// list 사용 종료 - va_list를 null로 초기화.
	va_end(list);

	return sum / count;
}

int main()
{
	std::cout << FindAverage(1, 1, 3, 5, 7, "Hello", "count") << std::endl;
	std::cout << FindAverage(2, 1, 3, 3, 7, "Hello", "count") << std::endl;
	std::cout << FindAverage(3, 1, 3, 5, 7, "Hello", "count") << std::endl;
	std::cout << FindAverage(4, 1, 3, 5, 7, "Hello", "count") << std::endl;

	//개수보다 인자 수가 적으면 예상하지 못한 값이 나오게 됨.
	std::cout << FindAverage(3, 1) << std::endl;


	return 0;
}
```

---

### 사용 방법

1.  Ellipsis 문법 사용을 위해선 **cstdarg(stdarg.h)** 헤더를 include 해야 한다.

2.  그리고 함수의 **가장 마지막 인자**에 **`...`** 를 작성한다.

3.  그리고 함수 내에서 가변인수들을 저장할 가변 리스트인 **`va_list`** 를 생성한다.

    ```c++
    va_list list;
    ```

4.  이후 va_start를 이용해 va_list가 가리킬 메모리 위치를 초기화 해 준다. 스택 프레임 구조상 가변 인자들은 마지막 고정 인자의 다음부터 연속적으로 위치하므로 마지막 고정 인자를 매개변수로 전달해 줘야 한다.

    예를 들어,

    ```c++
    void foo(int count, ...)
    {
    	//...
    }
    ```

    와 같은 함수에는

    ```c++
    va_start(list, count);
    ```

    로,

    ```c++
    void bar(int count, int dummy, ...);
    ```

    와 같은 함수에는

    ```c++
    va_start(list, dummy);
    ```

    처럼 전달한다.

5.  이후 va_arg를 통해 인자를 전달한 순서대로(가장 왼쪽 가변 인자부터 순서대로) 사용할 수 있다. 사용할 때에는 **`va_arg`** 를 사용하는데, **`va_arg`** 는 리스트 안의 iterator를 인자를 반환할 때마다 한 칸씩 이동시킨다. 그리고 이동하는 크기는 인자로 전달한 타입의 크기만큼 이동하게 된다.

    정의된 매크로를 살펴보면 다음과 같이 정의되어 있다.

    ```c++
    #define __crt_va_arg(ap, t)	(*(t*)((ap += _INTSIZEOF(t)) - _INTSIZEOF(t)))
    ```

    즉, list의 포인터를 타입 크기만큼 옮기고, 그 이전에 list 포인터가 가리키던 위치의 값을 반환한다는 뜻이다.

6.  모든 사용이 끝나면 va_list를 안전하게 null로 초기화 시켜야 한다. **그렇지 않으면 dangling pointer가 될 테니까.**

    이 때 **`va_end`** 를 사용한다.

    ```c++
    va_end(list);
    ```

---

### 주의사항

포인터를 이용하는 구조이다 보니, 의도하지 않은 참조에 의한 오류를 항상 조심해야 한다. 예를 들면 다음과 같은 것들이다.

*   (앞서 설명했던 것 처럼) **마지막 고정 인자가 아닌 다른 인자의 다음 주소로 list 포인터를 초기화하는 경우**
*   **전달한 가변 인자의 개수보다 더 많이 Iteration하는 경우** -> 쓰레기값이 참조될 수 있다.

