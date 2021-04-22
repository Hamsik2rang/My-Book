# 테스팅을 위한 C++의 실행 시간 측정 방법

*   C++의 **chrono 라이브러리**를 이용한다.

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <random>
#include <chrono>

class Timer
{
private:
	using clock_t	= std::chrono::high_resolution_clock;
	using second_t	= std::chrono::duration<double, std::ratio<1>>;

	std::chrono::time_point<clock_t> startTime = clock_t::now();
public:
	void Elapsed()
	{
		std::chrono::time_point<clock_t> endTime = clock_t::now();

		std::cout << std::chrono::duration_cast<second_t>(endTime - startTime).count() << std::endl;
	}
};

int main()
{
	std::random_device randDevice;
	std::mt19937 mersenneEngine{ randDevice() };

	std::vector<int32_t> v(10000);
	
	for (int i = 0; i < v.size(); i++)
	{
		v[i] = i;
	}
	std::shuffle(v.begin(), v.end(), mersenneEngine);

	Timer timer;

	std::sort(v.begin(), v.end(), std::less<int>());

	timer.Elapsed();

	return 0;
}
```

예시는 무작위로 나열된 10000개의 원소를 정렬하는데 드는 시간을 측정하는 코드이다.

코드에서 사용된 random 관련 객체들은 [**다른 글**](작성 예정) 을 참고하자.

**Timer** 클래스는 생성되는 순간 startTime 멤버 변수가 현재 시간으로 초기화되며, **elapsed()** 멤버 함수를 호출하면 호출 시점의 시간을 endTime멤버 변수에 저장한 후 둘의 차이를 이용해 실행 시간을 측정한다.

```c++
#include <chrono>
//...

class Timer
{
private:
	using clock_t	= std::chrono::high_resolution_clock;
	using second_t	= std::chrono::duration<double, std::ratio<1>>;

	std::chrono::time_point<clock_t> startTime = clock_t::now();
public:
	void Elapsed()
	{
		std::chrono::time_point<clock_t> endTime = clock_t::now();

		std::cout << std::chrono::duration_cast<second_t>(endTime - startTime).count() << std::endl;
	}
};
```

*   **chrono::high_resolution_clock**은 chrono 라이브러리에 구현된 clock 객체 중 나노초 단위의  측정을 지원하는 객체이다.

*   **chrono::duration** 은 Time Interval, 즉 시간 간격을 나타내는 객체이며, 첫 번째 탬플릿 인자로 Type을, 두 번째 탬플릿 인자로 단위 비율을 지정한다.

    이 때 단위 비율로 C++ 표준 비율(분수) 객체인 **`std::ratio`** 를 이용하며, ratio객체의 두 번째 탬플릿 인자(분모, denom)은 default value로 1을 가지므로 **std::ratio<1>은 1/1초, 즉 초당 1을 카운팅한다는 의미**이다.

*   **Elapsed()** 함수의 구현부를 보면, 호출 시점의 시간을 endTime에 저장한 후 **chrono::duration_cast<>** 를 이용해 Time Interval을 캐스팅하는데, 앞서 1초당 1로 카운팅하도록 지정한 duration, 즉 second_t를 탬플릿 인자로 넣음으로써 나노초 단위를 초 단위로 변경한다.

