## go home! [:house_with_garden:](https://github.com/wnsgml972/midas_log)

<br/><br/><hr/>

## 그 외 공부하면서 알아야 할 것

### gtest

#### 특징

* Google에서 개발된 C++ 테스트 프레임 워크
* Git에서 다운받아서 헤더파일 추가해서 사용
* 간단한 사용 방식은 다운 받아서, 검증해야할 부분을 검증하면 됨
* 사용하기 편하게 라이브러리를 덮어서 만들면 더 좋음, Test Fixture
* 구글에서도 실제로 사용 중 (Chrom)
* 항상 맞다고 생각하는 부분은 꼭 테스트 코드를 작성해야 함 (DB, Socket 통신 등등)
* 테스트 코드를 다 통과하더라도 버그가 없는 건 아니다.
* 사실 상 및에 참고 자료 정독하면 다 이해 됨

#### 참고

* <https://www.slideshare.net/jinhwason/ss-69528881>
* <https://www.slideshare.net/zone0000/c-7522148?next_slideshow=1>

<br/><br/>

### C++ Style Casting

#### ```static_cast<>()```

* __기본적으로 c 타입의 캐스팅과 비슷함__
* 기본 데이터 타입간의 변환
* 상속 관계의 클래스 계층 간의 변환
* 다형성 없어도 가능
* void 포인터를 다른 포인터로 변환


#### ```const_cast<>()```

* __포인터의 상수성을 제거 할 때 사용 (const를 제거함)__


#### ```dynamic_cast<>()```

* 상속 관계 안에서만 사용 가능
* 하나 이상의 가상함수를 가지고 있어야 함
* 컴파일러의 RTTI 설정이 켜져 있어야 함
* 캐스팅에 실패할 경우 포인터면 nullptr을 return, 참조자면 bad_cast 예외를 던짐
* 안전한 캐스팅
* __매우 느림!  (사실 캐스팅에 자신있다면 잘 안쓰고 거의 static_cast를 사용)__



#### ```reinterpret_cast<>()```

* __C 스타일 캐스팅과 거의 완전 같음__
* 걍 다됨
* 하지만! const_cast는 안 됨
* 다 되기 때문에 위험함, 얘도 안 쓰는게 좋음


#### 거의 static_cast를 위주로 사용하는 것이 좋다


<br/><br/>

### 익명 함수 문법(Lambda)

#### 기본 구조

__3번의 mutable 위치에는 default로 constexpr가 들어간다. (캡쳐할 때 상수로 캡쳐할 것인지 아닌지를 결정)__

![lambda](/media/lambda.png)

<pre lang="cpp">
[] (int a, int b) mutable throw() -> void {
     int sum = a + b;
     std::cout << sum << std::endl; // Output 7
}(3, 4);
</pre>

<br/>

#### lambda의 반환

##### 변수 반환

<pre lang="cpp">
int sum = [] (int a, int b) -> int {
  return a + b;
}(3, 4);

std::cout << sum << std::endl;
</pre>

##### lambda 함수 반환 (std::function)

<pre lang="cpp">
std::function func1 = [] () {
  std::cout << "My name is "  << std::endl;
};

auto func2 = []() {
  std::cout << "My name is " << std::endl;
};

func1();
func2();
</pre>

##### lambda 함수를 Parameter로 사용

<pre lang="cpp">
template<typename T>
void templateFunc(T func) {
    func();
}

int main() {
    // std::function<void (void)>
    auto foo = []() { std::cout << "Hello lambda!" << std::endl; };

    templateFunc(foo);

    return 0;
}
</pre>


##### lambda를 STL container에 저장

<pre lang="cpp">
int main() {
    std::vector<std::function<void (void)>> funcs;

    funcs.push_back([]() { std::cout << "Hello" << std::endl; });
    funcs.push_back([]() { std::cout << "lambda!" << std::endl; });

    for(auto& func : funcs) {
        func();
    }
}
</pre>


<br/>


#### 캡쳐

##### 특징

  * 캡쳐는 람다 밖의 변수를 사용하기 위한 문법이다.
  * ```&```는 참조로 캡쳐, ```=```은 복사로 캡쳐한다.
  * ```=```로 캡쳐할 경우 람다 body 안에서 변수는 상수로 취급된다. 그 이유는 mutable 자리에 default로 constexpr로 되어있기 때문이다. 그 자리에 mutable로 변경할 경우 복사된 값을 변경할 수 있다.
  * ```default-capture```로서 ```[&][=]``` 같은 것들은 전역 변수를 포함해서 외부의 모든 변수를 캡쳐한다.
  * 직접 변수를 명시할 경우 람다 바로 부모의 함수 변수들만 캡쳐한다.

##### 문법

  1. ```[a,&b]``` a를 복사로 캡처, b를 참조로 캡처.
  2. ```[this]``` 현재 객체를 참조로 캡처.
  3. ```[&]``` 바디에서 쓰이는 모든 변수나 상수를 참조로 캡처하고 현재 객체를 참조로 캡처.
  4. ```[=]``` 바디에서 쓰이는 모든 변수나 상수를 복사로 캡처하고 현재 객체를 참조로 캡처.
  5. ```[]``` 아무것도 캡처하지 않음.


#### 예제

1. ```default-capture```가 아닌 경우

<pre lang="cpp">
int main() {
    int a, b, c;

    // OK
    [&, a, b]() {}();
    // OK
    [=, &c]() {}();

    // 기본 캡처 모드가 참조 방식인 경우 '&a'을(를) 사용할 수 없습니다.
    // a를 2번 캡쳐하기 때문!
    [&, &a]() {}();
    // 기본 캡처 모드가 값 방식인 경우 '&b'이(가) 필요합니다.
    // 마찬가지로 b를 2번 캡쳐하기 때문!
    [=, b]() {}();
}
</pre>

2. 클래스 멤버 함수 속 lambda

__this__ 를 통한 캡쳐로, 현재 객체를 참조로 캡쳐할 수 있다.

<pre lang="cpp">
class Person {
public:
    Person(std::string name) : name(name) {}
    void introduce() {
        [this]() { std::cout << "My name is " << name << std::endl; }();
    }
private:
    std::string name;
};

int main() {
    Person* devkoriel = new Person("Jinsoo Heo");
    devkoriel->introduce(); // My name is Jinsoo Heo

    return 0;
}
</pre>

3. lambda 재귀

재귀 방식을 이용할 경우 ```auto```를 이용해 람다의 함수를 받을 수 없다.

<pre lang="cpp">
int main() {

    std::function<int (int)> factorial = [&factorial](int x) -> int {
        return x <= 1 ? 1 : x * factorial(x - 1);
    };

    std::cout << "factorial(5): " << factorial(5) << std::endl;
}
</pre>

<br/>

#### 정리

* lambda는 익명 함수(anonymous function)이고 함수 객체를 생성한다.
* lambda는 함수 포인터와 함수 객체의 장점을 모두 가지고 있다.
* 기본 캡처 모드```(capture-default)```, 복사, 참조를 통해 변수나 상수를 캡처할 수 있다.
* 함수에서 반환할 수도 있고 함수의 파라미터로 전달할 수도 있다.
* 클래스 멤버 함수안에서 정의되는 lambda는 ```[this]```로 현재 객체를 참조로 캡처할 수 있다.
* 재귀 호출이 가능하다.


<br/><br/>

### tuple

원래는 boost 문법인데 C++11이후로부터 표준으로 채택되었다.  
2개 이상의 값을 반환하거나 전달할 때 사용하면 유용하다.  
__참고로 C++ 14 문법은 요소 하나 조회가 복잡하다. C++ 17이니까 쉽다.__  
cpp17 설정은 `cpp17.md` 참고

~~~
#include <iostream>
#include <tuple>
#include <string>


int main()
{
	// C++ 17 make tuple variable.
	std::tuple<int, std::string, bool> myNumber = std::tuple(10, "Even", true);

	// get tuple size.
	std::cout << "size : " << std::tuple_size<decltype(myNumber)>::value << std::endl;

	// C++ 17 조회가 쉬워졌다.
	auto[i, s, b] = myNumber;
	std::cout << i << ", " << s << ", " << b << std::endl;

	return 0;
}
~~~


<br/><br/>

### smart pointer

