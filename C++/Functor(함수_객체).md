# Functor(함수 객체)

함수 객체는 특정 클래스에 **`()`** 연산자의 오버로딩을 이용해 bound function의 형태로 사용하는 방법이다.

아래처럼 사용할 수 있다.

```c++
#include <iostream>

class Accumulator
{
private:
	int _counter = 0;
public:
    int operator()(int i)
    {
        return _counter+=i;
    }
}

int main()
{
    Accumulator acc;
    std::cout << acc(10) << std::endl;
    std::cout << acc(20) << std::endl;
    std::cout << acc(30) << std::endl;
    
    return 0;
}
```

```text
10
30
60
```

객체를 생성한 뒤 해당 객체를 함수를 호출하듯이 호출하면 객체가 함수처럼 동작하게 된다.

이처럼 함수 형태로 동작하는 객체를 **함수 객체(Functor)** 라 한다.

그런데, 왜 함수 객체와 같은 형태의 문법을 사용하는 것일까?

이유는 다음과 같다(고 한다).

### 1. 성능 이슈

일급 함수의 형태로 함수를 사용하기 위해 **콜백 함수(Callback Fuction)** 를 넘겨줄 때, C++는 원칙적으로 함수가 일급 객체가 아니므로 함수 자체를 넘길 수 없고 해당 함수의 포인터를 넘기게 되는데, 이를 **함수 포인터(Function Pointer)** 라 한다.

그런데 함수 포인터를 이용해 함수를 넘기게 되면 해당 함수의 길이나 inline 선언 여부에 상관없이 일반 함수로서 사용하게 된다(*in **Effective STL***).

이 때 **() operator를 inline으로 오버로딩한 Functor**를 사용하게 되면 콜백 형태로 넘긴다 하더라도 inline 그대로 사용할 수 있어 성능상의 이점이 있다.

그러나 이러한 속도 이슈는 **lambda function을 이용해 일급 함수를 구현할 수 있게 된 Modern C++** 에서는 더 이상 중요한 이슈가 아니게 되었다..

### 2. 형태의 강제성

콜백 형태로 사용할 함수는 일반적으로 그 형식이 강제되지 않는다.

다시 말해 함수들간의 형식을 규격화할 어떠한 규칙도 지정되지 않는다는 뜻이다. 그러나 Functor를 이용한다면, Functor Interface를 만든 후 해당 interface를 상속하게 함으로써 콜백 함수들간의 다형성과 규격을 정의할 수 있게 된다.

Sorting function에 집어넣을 Comparator들을 정의한다고 생각해 보면, 일반 bool return타입의 함수들을 각각 정의하는 것과 Functor Interface를 이용해 형식을 강제하는 것 중 어느 것이 더 안전한 객체지향 설계를 유도할 지 생각해 보라.