# The Tour of C++ (2nd Edition) // Section 1

## 1. Programs

C++ 는 컴파일 언어이다. 프로그램이 실행되기 위해선 C++ 문법으로 작성된 소스 텍스트가 컴파일러에 의해 처리되어야 하고, 이를 통해 오브젝트 파일(\*.obj)을 생성한 후, 링커에 의해 이들을 묶어 실행 파일(\*.exe)로 생성해야 한다.

C++ 프로그램은 일반적으로 매우 많은 소스 파일들로 구성되어 있다.

실행 가능한 프로그램은 하드웨어/시스템과 긴밀히 결합되어 있다. 이 말은 즉, Portable하지 않다는 뜻이다. 그러나 많은 경우에 C++은 Portable한 언어로 정의되는데, 이때 언급되는 Portable은 컴파일을 통해 완성한 실행 파일이 시스템 독립적이라는 것이 아니라 코드 자체가 시스템 독립적이라는 뜻이다. 다시 말해 언어의 중심 문법과 표준 라이브러리들이 시스템에 종속되지 않는다는 뜻이다.

(Windows에서 생성된 실행 파일 -> Mac에서 실행 : X)

(Windows에서 작성되어 컴파일 가능한 소스 코드 -> Mac에서 컴파일 : O)

*   중심 문법(Core Language Feature)  : 타입(int, char 등), for-loop, while-loop, etc.

*   표준 라이브러리(Standard Library) : <<(put to), >>(get from), getline(), etc.

특히, C++ 표준 라이브러리는 Thread의 Context Switching(문맥 교환)같은 예외적인 몇몇 기능을 제외하면 모두 일반적인 C++코드로 구현되어 있다.

C++의 다른 특징으로는 강한 타입 언어가 있다. 이는 모든 개체(Entity - Object, Value, Name and Expression)는 컴파일러에게 자신의 타입을 선언 시점에 알려야 한다. 타입이 해당 개체가 사용 가능한 연산과 저장 가능한 값의 집합을 정의하기 때문이다.

## 2. Hello World!

C++로 작성할 수 있는 가장 간단한 프로그램은 다음과 같다.

```c++
int main() {}	// The minimal C++ program.
```

이 말은 곧, 모든 C++로 작성된 프로그램에는 `main`함수가 존재해야 한다는 뜻이다.

1.  main()

    main함수는 일반적으로 `int(integer type)` 값을 반환하도록 정의하는데, 0을 정상 종료값으로, 0이 아닌 값을 비정상 종료값으로 가정한다.

    참고로, 모든 OS와 실행 환경이 main의 리턴값을 활용하지는 않는다. Linux/Unix 계열 운영체제는 이를 항상 활용하지만 Windows는 사용하지 않는 경우가 있다.

2.  중괄호({}, Curly braces)

    C++은 모든 블록(또는 그룹)을 중괄호`{}`를 이용해 구분한다.

3.  주석(//, /**/, Comment)

    주석은 오직 인간을 위한 문법이다. 컴파일러는 프로그램 내에 작성된 모든 주석을 무시한다.

    `//`의 경우 작성된 지점부터 해당 코드 라인의 끝까지 작성된 모든 텍스트를 주석으로 처리한다.

    `/**/`의 경우 `/*`가 작성된 지점부터 `*/`가 작성된 지점까지의 모든 텍스트를 주석으로 처리한다.

## 3. Functions

C++에서 처리가 완료된 개체(결과)를 얻는 주요한 방법은 해당 처리를 수행하는 함수를 호출하는 것이다.

함수를 정의하는 것은 특정한 작업이 어떻게 처리될지를 구체화하는 것이다.

함수의 선언을 위해선 함수의 이름과 반환 타입(반환을 한다면), 그리고 인자의 개수와 각각의 타입이 필요하다.

그리고 이들은 각각 아래 순서로 작성된다.

```text
반환_타입 함수의_이름 (인자_타입1, 인자_타입2, 인자_타입3, ...);
```

인자를 전달하는 것은 의미론적으로 인자를 **초기화(initialize)**하는 것이다. 즉, 일반적인 변수의 초기화 과정에서 일어나는 동작들(타입의 체크와 암시적 형변환, 값의 복사 등)이 그대로 수행된다.

```c++
double s2 = sqrt(2);	// Integer type '2' is converted to double type '2.0'.
double s3 = sqrt("three");	// Error. const char* can't be converted to double.
```

그러므로, 함수에 인자를 전달하는 것을 과소평가(underestimate)해선 안된다.

또, 함수의 선언에는 인자의 이름도 작성할 수 있다. 다만 해당 선언이 함수의 정의로 이어지지 않는다면 컴파일러는 그 이름을 무시한다.

```C++
void foo1(double);		// In declaration, param's name is unnecessary.
void foo2(double d1);	// d1 is ignored. it is absoultly same as 'void foo2(double);'

void foo3(double d2)	// d2 isn't ignored and it used.
{
    //...
}
```

또한 함수는 클래스의 멤버가 될 수 있으며, 이러한 함수들을 **멤버 함수(member function)**라 한다. 또한 멤버함수들은 자신이 속한 클래스 역시 시그니처의 일부로 사용된다. 예를 들어

```c++
char& String::operator[](int index);
```

함수의 시그니처는

```c++
char& String::(int)
```

이다. 그러므로

```c++
char& String::operator[](int index);
char& Name::operator[](int index);
```

는 서로 다른 함수로 간주된다.

코드 내에서 의미를 갖는 덩어리들을 함수 단위로 묶으면 코드를 간결하고 이해하기 쉽게 작성할 수 있으며, 이는 프로그램의 유지보수를 위한 최적의 방법이 될 수 있다. 모든 프로그래머들은 자신들의 코드가 유지보수에 용이하길 원하기 때문이다.(프로그램 내 에러의 수는 코드의 양과 복잡성과 강하게 상호 연관되어 있는데, 이 두 문제는 모두 함수를 자주, 그리고 간결하게 사용하는 것으로 해결할 수 있다.) 

만약 둘 또는 그 이상의 함수들이 동일한 이름으로 선언되어 있다 하더라도 각각의 타입이 다르다면(참고로, 함수의 반환 타입은 함수의 시그니처로 사용되지 않는다.) 두 함수는 공존할 수 있으며 컴파일러는 해당 이름의 함수가 호출될 때 호출 조건(인수)에 따라 가장 적합한 함수를 호출한다. 예를 들면

```c++
void print(int);
void print(double);
void print(const char*);

int main()
{
    print(42);			// void print(int) called.
    print(3.141592);	// void print(double) called.
    print("Hello");		// void print(const char*) called.
    
    return 0;
}
```

와 같다.

이처럼 이름이 같지만 타입이 서로 다른 함수를 정의하는 것을 **함수 오버로딩(function overloading)**이라 한다.

함수 오버로딩은 일반화 프로그래밍 패러다임을 위한 중요한 문법 중 하나이다.

다만, 함수 오버로딩을 할 때에는 나름의 규칙이 있는데, 마지막의 **9. Advice** 문단에도 나와 있지만 **의미론적으로 동일한 기능을 수행**할 때 이용해야 한다.

위의 예시에서도, 오버로딩된 각각의 print는 인자의 타입은 모두 다르지만 공통적으로 전달받은 인자를 출력해주는 기능을 수행할 것이다.

## 4. Types, Variables, and Arithmetic

### 4.1. Types

모든 개체와 표현식은 하나의 고정된 타입을 가져야 한다.

타입을 지정한다는 것은 다음을 의미한다.

*   가질 수 있는 값의 집합을 정의한다.
*   가능한 연산의 집합을 정의한다.
*   사용할 메모리의 범위를 정의한다.
*   비트 집합(Binary Data)를 어떤 형태로 사용할 지 정의한다.

예를 들어

```c++
int height = 182;
```

는 height라는 **변수**가 int 타입을 가진다고 **선언**하는 것이고, 이를 통해

*   가질 수 있는 값의 집합 =  {-2^31 ~ 2^31 - 1}
*   가능한 연산 = {+, -. *, /, %, <<, >>, ...}
*   4Byte 크기의 메모리를 사용
*   저장된 비트를 정수 형태로 사용

를 정의하게 된다.

### 4.2. Variables

**변수**는 타입에 따라 사용될 메모리에 이름을 붙인 것이다. 위의 예시에서는 임의의 주소로부터 4Byte크기만큼의 메모리에  **height**라는 이름을 지정한 후 182라는 값을 저장해 사용하는 것을 볼 수 있다.

C++은 다양한 기본 타입(Primitive Type)을 제공하며, 각 타입마다 표현할 수 있는 범위와 형식이 다르다.

`char`의 경우 -128 ~ 127까지의 값을 저장할 수 있으며, 일반적으로 문자 형태, 또는 정수 형태로 데이터를 표현한다.

`long long int`의 경우 -2^63 ~ 2^63-1 까지의 값을 저장할 수 있으며, 정수 형태로 데이터를 표현한다.

만약 `long long int` 처럼 매우 범위가 큰 변수에 리터럴 데이터를 사용하는 경우, 가독성을 위해 **자릿수 구분 연산자**를 사용할 수 있다.

```c++
double pi = 3.14'1592'6535'8979'3238'4626'433;
// Instead double pi = 3.1415926535897932384626433;
int maxIntSize = 21'4748'3647;	
// Instead int maxSize = 2147483647;
```

### 4.3. Arithmetic

연산자에 대해선 그 양이 너무 많아서 중요한 몇 가지만 소개하겠다.

*   연산 과정 또는 그 결과에서도 암시적 형변환이 일어난다.
*   대입 연산자`=`를 제외한 모든 연산자는 왼쪽에서 오른쪽으로 평가가 진행된다.
*   함수 파라미터의 평가 순서(함수를 호출할 때 각 인자를 파라미터에 복사하는 순서)는 정해져 있지 않다.

### 4.4. Initialization

초기화는 변수를 선언하면서 동시에 값을 저장하는 것을 말한다.

C++의 초기화 방법은 2가지가 있으며, 다음과 같다.

*   `=`
*   `{}`

첫 번째 방법(할당 연산자 이용)은 고전적인 방법이며, 다음처럼 사용한다.

```c++
int a = 3;
double d = 9.8;
bool isInit = true;
```

두 번째 방법(중괄호 이용)은 uniform(univeral form) Initialization이라고 부르며, 다음처럼 사용한다.

```c++
int a{3};
double d{9.8};
bool isInit{true};
```

Uniform Initialization은 데이터의 손실이 일어날 수 있는 암시적 형변환을 허용하지 않는다.

```c++
int a = 3.4;	// a becomes 3.
int b{3.4};		// Error: can't conversion.
```

변수를 초기화할 땐 그 값 자체가 타입을 나타내는 지표로 사용될 수 있기 때문에 `auto`키워드를 이용할 수 있다.

auto는 추론 타입(Inference Type)으로써, 컴파일러가 컴파일 시점에 초기화되는 값에 따라 자동을 타입을 추론해 지정해 준다.

```c++
auto a = 3;
auto d = 9.8;
bool isInit{true};
```

auto 키워드는 값에 따른 추론을 진행하므로 형변환이 일어날 위험이 없기 때문에, 굳이 `{}`를 이용하지 않아도 상관 없다.

일반적으로 auto는 반드시 해당 구문에 타입을 명시해야 하는 특별한 이유가 없다면 마음껏 사용할 수 있는데, 그 특별한 이유는 다음과 같다.

*   변수에 저장된 값으로 초기화를 진행하면서, 해당 변수가 너무 넓은 범위에 선언되어 있어 원 타입을 찾기 힘든 경우.
*   변수가 가지는 값의 범위나 정밀도를 강조하고 싶은 경우

## 5. Scope and Lifetime

**선언**은 프로그램에 그 이름을 소개하는 것과 더불어 속한 유효 범위에 그 이름을 소개하는 역할도 한다.

유효 범위(Scope)는 다음과 같이 구분한다.

*   지역 범위(Local Scope) : 함수, 람다식의 안(또는 그의 부분집합)을 지칭한다. 지역 범위에서 선언된 개체는 선언된 시점부터 자신이 속한 범위가 끝나는 시점(닫는 중괄호`}`를 만나는 시점)까지를 자신의 유효 범위로 갖는다.
*   클래스 범위(Class Scope) : 함수,람다식의 밖이면서 클래스 안인 범위를 지칭한다. 클래스 범위에는 클래스 멤버들이 위치하며, 클래스 멤버들은 클래스 전체가 시작된 시점(여는 중괄호`{`가 선언된 시점)부터 클래스 범위가 끝나는 시점(닫는 중괄호`}`를 만나는 시점)까지를 자신의 유효 범위로 갖는다.
*   네임스페이스 범위(Namespace Scope): 클래스의 밖이면서 네임스페이스 안인 범위를 지칭한다. 네임스페이스에서 선언된 개체는 선언된 시점부터 자신이 속한 범위가 끝나는 시점까지를 자신의 유효 범위로 갖는다.
*   전역 범위(Global Scope) : 네임스페이스 밖의 범위를 지칭한다. 전역 범위에서 선언된 개체는 선언된 시점부터 프로그램이 종료될 때 까지를 자신의 유효 범위로 갖는다.

이 때 유효 범위란 선언된 개체가 메모리에 존재하는(유효한 생명-Lifetime) 범위를 말한다.

참고로, `new`를 통해 Heap에 할당된 개체는 `delete`를 통해 소멸시키기 전까지 유효하다.

## 6. Constants



## 9. Advice

본서의 저자이자 C++의 창시자인 *Bjarne stroustrup*은 *C++ Core Guidelines*라는 책을 통해 **'좋은' C++ 코드를 작성하는 요령**을 제공하는데, 그 중 일부가 본서에도 수록되어 있으며 다음과 같다.

(영어 실력이 좋지 않아 의역이 있을 수 있어 원문도 함께 작성합니다.)

1.  당황하지 마라! 모든 것은 제 시간 안에 끝날 것이다. 

    *Don't Panic!. All will become clear in times.*

2.  C++에서 기본적으로 제공하는 기능은 단독적으로 사용하거나 그 자체를 사용하는 것보다는 일반적으로 ISO C++ 표준 라이브러리와 같은 라이브러리들을 이용하는 것이 좋다.

    *Don't use the built-in features exclusively or on their own. On the contrary, the fundamental(built-in) features are usually best used indirectly through libraries, such as the ISO C++ Standard Library.*

    (정확히 어떤 의미인지 잘 모르겠습니다. 예컨대 pow함수를 직접 구현하는 것 보다 cmath.h의 std::pow를 사용하라는 뜻일까요?)

3.  좋은 프로그램을 만들기 위해 C++의 모든 세부적인 지식을 알아야 하는 것은 아니다.

    *You don't have to know every detail of C++ to write good programs.*

4.  언어적 지식 대신 프로그래밍 테크닉에 집중하라.

    *Focus on programming techniques, not on language features.*

    (여기서 말하는 프로그래밍 테크닉은 디자인패턴이나 OOP, Generic Programming같은 코드 작성에 관한 패러다임들을 지칭하는 것 같습니다.)

5.  언어의 정의에 관한 문제를 해결하는 최후의 방법은 ISO C++ 표준을 살피는 것이다.

    *For the final word on language definition issues, see the ISO C++ standard.*

6.  의미있는 작업들은 신중하게 명명된 함수의 형태로 "감싸라".

    *"Package" meaningful operations as carefully named functions.*

7.  하나의 함수는 오직 하나의 역할을 해야 한다.

    *A function should perform a single logical operation.*

8.  함수는 최대한 짧고 간결하게 작성하라.

    *Keep functions short.*

9.  각기의 다른 타입들을 대상으로 동일한 작업을 수행하는 함수는 오버로딩하라.

    *Use overloading whe functions perform conceptually the same task on different types.*

10.  임의의 함수가 컴파일 타임에 결과를 평가하길 원한다면, `constexpr` 함수로 선언하라.

     *If a function may have to be evaluated at compile time, declare it **constexpr**.*

11.  프로그래밍 언어의 기본 기능들이 하드웨어 입장에서 어떻게 동작되는지 이해하라.

     *Understand how language primitives map to hardware.*

12.  매우 긴 길이의 리터럴 데이터는 읽기 쉽도록 자릿수 분리 연산자를 이용하라.

     *Use digit separators to make large literals readable.*

13.  복잡한 표현식을 작성하는 것을 피하라.

     *Avoid complicated expressions.*

14.  축소 변환을 피하라.

     *Avoid Narrowing conversions.*

15.  변수의 유효 할당 범위를 최소화하라.

     *Minimize the scope of a variable.*

16.  매직 넘버의 사용을 피하고, 상수화 변수를 이용하라.

     *Avoid "Magic constants", use symbolic constants.*

17.  가능하면 불변적인 데이터들을 사용하는 것이 좋다.

     *Prefer immutable data.*

18.  매 선언마다 각기 다른 이름을 선언하라.

     *Declare one name (only) per declaration.*

19.  지역 범위의 흔하게 사용되는 개체의 이름은 짧게 사용하고, 그렇지 않은 것들의 이름은 길게 사용하라.

     *Keep common and local names short, and keep uncommon and nonlocal names longer.*

20.  비슷하게 보이는 이름들을 피하라.

     *Avoid similar-looking names.*

21.  대문자만을 사용한 이름을 피하라.

     *Avoid **ALL_CAPS** names.*

22.  명명된 타입의 개체를 초기화할 땐 uniform initializer`{}`를 사용하는 것이 좋다.

     *Prefer the {}-initializer syntax for declarations with a named type*.

     (명명된 타입(named type)은 구조체, 클래스, 열거형 등을 의미합니다.)

23.  키워드의 반복 입력을 피하기 위해 `auto`를 이용하라.

     *Use **auto** to avoid repeationg type names.*

24.  초기화되지 않은 변수를 피하라.

     *Avoid uninitialized variables.*

25.  유효 할당 범위는 가능한 작게 하라.

     *Keep scopes small.*

26.  if문 내에서 변수를 선언할 땐 해당 변수에 들어간 값이 0이 아닌지 암시적으로 테스트하는 형태로 선언하라.

     *When declaring a variable in the condition of an if-statement, prefer the version with the implicit test against **0**.*

27.  `unsigned` 키워드는 오직 비트를 조작할 때에만 사용하라.

     *Use **unsigned** for bit manipulation only.*

28.  포인터는 최대한 간단하고 직관적으로 사용하라.

     *Keep use of pointers simple and straightforward.*

29.  `0` 이나 `NULL` 보다 `nullptr`을 사용하라.

     *Use **nullptr** rather than **0** or  **NULL***

30.  변수는 초기화(사용)하기 전에 미리 선언하지 말라.

     *Don't declare a variable until you have a value to initialize it with.*

31.  코드에서(또는 그 자체로) 명확히 설명될 수 있는 내용은 주석으로 달지 말라.

     *Don't say in comments what can be clearly stated in code.*

32.  주석으로 의도를 설명하라.

     *State intent in comments.*

33.  일관된 코딩 스타일을 유지하라.

     *Maintain a consistent indentation style.*

     (원문은 들여쓰기 스타일-indentation style을 유지하라고 되어 있지만, 비단 들여쓰기 뿐만 아니라 변수 명명 규칙 등의 다른 코딩 스타일에도 적용되는 내용입니다.)

