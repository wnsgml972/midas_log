## go home! [:house_with_garden:](https://github.com/wnsgml972/midas_log)

<br/><hr/>

## C++ Basic

## 목차

* gtest
* modern c+ framework 분석
* default, delete 키워드
* C++ Casting
* enum class
* `emplace_back` vs `push_back`
* C++11 다양한 초기화 방식
* C++ Getter, Setter
* C++11/14 변경 사항
* 익명 함수 문법(Lambda)
* tuple (pair의 발전형)
* smart pointer
* 확장할 수 있는 난수 생성기의 추가 : random 헤더 파일
* Thread, async future
* boost:: asio, multi_index

<br/><br/>
<hr/>

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

### modern c+ framework 분석

* [framework](/contents/BasicEducation/framework.md)




<br/><br/>
<hr/>

### C++ Casting

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


<br/>


### Enum Class

* Enum의 범위를 제한하기 위해 Enum Class를 사용한다.
* FeatureSolid 참조할 것

~~~cpp
class ABC
{
public:
    enum class FileState { WRITE, READ };
};

int main()
{
    ABC::FileState file1 = ABC::FileState::WRITE;
}
~~~



<br/>

###  emplace_back vs push_back

#### emplace_back
* 함수를 생성하지 않고 가변 인자를 받을 수 있다.
* 받은 인자를 가지고 vector 내에서 객체를 생성한다.

~~~
vector.emplace_back(1, 2);
~~~

#### push_back
* 함수를 생성한 뒤 객체 자체를 인자로 넘겨야 한다.
* 내부 적으로 생성된 뒤 생성자와 소멸자가 2번 씩 불리기 때문에 비효율적이다.

~~~
vector.push_back(CPoint(1, 2));
~~~

#### However
- push_back으로 하여도 최신 컴파일러 내부적으로 최적화 하기 때문에 emplace_back으로 하는 것과 별차이가 없을 수 있다. **고로 개인 프로젝트가 아니라면 호환성이 더 좋은 push_back 사용이 더 나을 수도 있다.**
- push_back함수로 할 수 있는 모든 것을 emplace_back으로 할 수 있다.




<br/>

### C++의 다양한 초기화 방법

#### 복사 초기화

```cpp
int iValue = 5; //copy initialization 
```


#### 직접 초기화

* 이니셜라이져처럼 동작, 복사 초기화보다 성능이 뛰어남
* 이니셜라이징과 대입을 구분할 수 있는 좋은 방법

```cpp
int iValue(5); //copy initialization 
```

> 그러나 위의 두 방식은 모든 변수에 대해서 초기화하지는 않음 <br/>
모든 타입이나 변수를 초기화 할 수 있는 매커니즘이 필요.

<br/>


#### C++11 다양한 초기화 방식

* 형 변환을 허용하지 않아 안전함.
* 숫자 변수는 0(또는 0.0 또는 0.0000000000 등)으로 초기화됩니다.
* Char 변수는 ‘\0’으로 초기화됩니다.
* 포인터는 nullptr로 초기화됩니다.
* 배열, POD 클래스, 구조체 및 공용 구조체는 멤버 값을 0으로 초기화합니다.

```cpp
int iValue{}; // default initialization to 0
int iValue{5};
int iValue{4.5}; //error : an integer variable can not hold a non-integer value
                 // -> 형 변환을 허용하지 않으며 컴파일러에서 경고 또는 오류가 발생한다.

BaseClass bc{};     // class is initialized  
double b{};  // value of b is 0.00000000000000000  
int* ptr{};     // initialized to nullptr 
```

#### static  (tip)

정적 변수는 암시적으로 0으로 초기화 됨

~~~cpp
static int int1;       // 0  
static char char1;     // '\0'  
static bool bool1;   // false  
static MyClass mc1;     // {0, '\0'}  
~~~

#### Code In 초기화

![uniform](/media/uniformInit.png)

* 심플한 초기화는 복사 초기화를 이용한다.
* 생성자의 경우나, 특별한 로직이나 가독성을 봤을 때 심플하지 않을 때, 직접 초기화를 이용한다.
* 일반 NULL 초기화의 경우 유니폼 초기화를 이용한다.
* 유니폼 초기화의 경우 auto와 같이 쓰지 않을 것을 유의한다.

<br/><br/>

### C++ Getter, Setter

왜인지는 잘 모르겠는데 자바 만큼 모든 것을 캡슐화하지는 않음

> 예제

~~~cpp
#include <iostream>

class Foo
{
private:
	int x_ = 3;

public:
	// 전행 반환 방식
 	int&       x() { return x_; }
 	const int& x() const { return x_; }

	// 후행 반환 방식
 	auto x()       -> int& { return x_; }	    //Getter
 	auto x() const -> const int { return x_; }  //Setter

	// C++14 이후 auto 반환  decltype(auto)는 같은 이름 오버로딩 안 됨!
	decltype(auto) setX() { return (x_); } //decltype(x) 는 int 이므로 f1은 int 를 반환
	decltype(auto) getX() { return x_; } //decltype((x)) 는 int& 이므로 f2는 int& 를 반환
};

int main(void)
{
	Foo f; //C++14 이후 auto 반환 이용

	int a = f.getX();  // Getter
	std::cout << f.getX() << std::endl;

	f.setX() = 5;		// Setter
	std::cout << f.getX() << std::endl;
}
~~~

<br/><br/>
<hr/>

###  C++11/14 변경 사항

#### C++11/14 문법적 변경 사항

1. 초기화 리스트 및 초기화 방법의 통합
2. 새로운 타입의 추가 : long long형 정수
3. 새로운 스마트 포인터 추가 : 
4. 널 포인터 상수 추가 : nullptr
5. 열거체의 범위 지정
6. 자동 타입 변환 : auto
7. 타입 변환 연산자 추가 : explicit
8. 범위 기반 for 문 추가
9. 람다 함수와 람다 표현식 추가

#### C++11/14 표준 라이브러리 변경 사항

1. 확장할 수 있는 난수 생성기의 추가 : random 헤더 파일
2. 튜플 템플릿의 추가: tuple 헤더 파일
3. 정규 표현식의 추가 : regex 헤더 파일
4. 다중 프로그래밍을 위한 스레드의 지원 : thread_local 키워드, automic 헤더 파일
5. 비동기식 프로그래밍 async future 지원
6. 임베디드 프로그래밍을 위한 저수준 프로그래밍 지원


<br/>

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

~~~cpp
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
~~~


##### lambda를 STL container에 저장

~~~cpp
int main() {
    std::vector<std::function<void (void)>> funcs;

    funcs.push_back([]() { std::cout << "Hello" << std::endl; });
    funcs.push_back([]() { std::cout << "lambda!" << std::endl; });

    for(auto& func : funcs) {
        func();
    }
}
~~~


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

~~~cpp
#include <iostream>
#include <tuple>
#include <string>


int main()
{
	// C++ 17 make tuple variable.
	std::tuple<int, std::string, bool> myNumber = std::tuple(10, "Even", true);
	std::tuple<int, std::string, bool> myNumber1 = { 10, "Even", true }; // 엄청 편리!

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

#### 사용 이유

기존 C++ 프로그램에서 new 키워드를 사용하여 동적으로 할당받은 메모리는, 반드시 delete 키워드를 사용하여 해제해야 했습니다. 때문에 언제든 메모리 누수의 위험이 있었습니다.
이런 문제를 줄여주고자 **C++에서는 메모리 누수(memory leak)로부터 프로그램의 안전성을 보장하기 위해 스마트 포인터를 제공하고 있습니다.**

> 스마트 포인터(smart pointer)란 포인터처럼 동작하는 클래스 템플릿으로, 사용이 끝난 메모리를 자동으로 해제해 줍니다.

<br/>

#### 동작 원리

스마트 포인터를 이해하기 위해서는 스마트 포인터는 새로운 포인터가 아니라, 기본 포인터를 소유한 포인터라고 이해하셔야 합니다.
  
기본 동작은 new 키워드를 사용해 기본 포인터(raw pointer)가 실제 메모리를 가리키도록 초기화한 후에 스마트 포인터가 기본 포인터를 소유합니다. 이렇게 정의된 스마트 포인터의 수명이 다하면, 소멸자는 delete 키워드를 사용하여 할당된 메모리를 자동으로 해제합니다.
  
따라서 new 키워드가 반환하는 주소값을 스마트 포인터가 소유하면, 따로 메모리를 해제할 필요가 없어집니다.


> 예제입니다. 

~~~cpp
void UseSmartPointer()
{
    // Declare a smart pointer on stack and pass it the raw pointer.
    unique_ptr<Song> song2(new Song(L"Nothing on You", L"Bruno Mars"));

    // Use song2...
    wstring s = song2->duration_;
    //...

} // song2 is deleted automatically here.
~~~

예제에서와 같이 스마트 포인터는 스택에 선언되고, 힙 할당 객체를 가리키는 기본 포인터를 스마트 포인터가 소유합니다.

<br/>

#### 종류

1. unique_ptr
2. shared_ptr
3. weak_ptr

스마트 포인터는 `<memory>` 헤더 파일의 std 네임스페이스에 정의됩니다.

#### unique_ptr

**unique_ptr은 하나의 스마트 포인터만이 특정 객체를 소유할 수 있도록 구성된 스마트 포인터입니다.**

> 예제입니다.

~~~cpp
#include <iostream>
#include <memory>
#include <string>

using namespace std;

class Person
{
private:
	string name_;
	int age_;
public:
	Person(const string& name, int age); // 기초 클래스 생성자의 선언
	~Person() { cout << "소멸자가 호출되었습니다." << endl; }
	void ShowPersonInfo();
};

Person::Person(const string& name, int age) // 기초 클래스 생성자의 정의
{
	name_ = name;
	age_ = age;
	cout << "생성자가 호출되었습니다." << endl;
}

void Person::ShowPersonInfo() 
{
	cout << name_ << "의 나이는 " << age_ << "살입니다." << endl;
}


void SmartPointerTest()
{
	unique_ptr<Person> hong = make_unique<Person>("길동", 29);
	//	unique_ptr<Person> hong2 = hong; Compile Error
	unique_ptr<Person> hong2 = move(hong);

	//	hong->ShowPersonInfo();		Runtime Error! 소유권을 옮겼기 때문, 치명적임... 이미 아무것도 없는데 사용하려 해서 그런 듯
	hong2->ShowPersonInfo();

	// hong.reset();  . 을 이용한 접근 연산자로 접근,  -> 는 소유 객체에 대한 접근

	// 소멸자를 호출하지 않았지만, 스마트 포인터로 인하여 소멸자가 자동으로 호출 됨! (자동 해제)
	// Stack에 선언 된 unique_ptr가 함수가 끝나 해제되면서, 안에 선언 된 객체를 해제함!	
}

int main(void)
{
	SmartPointerTest();
	cout << "과연 해제 시점은?" << endl; 
	// 소멸자 출력이 이 아웃풋 보다 위인 것으로 인해 unique_ptr는 Stack영역 단위로 포인터를 해제하는 것을 알 수 있다.

	return 0;
}
~~~

##### 특징

* 기본 포인터에 대한 하나의 소유자만 존재하는 포인터
* 때문에 소유자를 추가하는 대입 연산이나 복사 생성자는 컴파일러 에러
* 소유권을 옮길 때는 `move` 사용합니다.
* 미리 해제하는 `reset` 함수도 있습니다.
* 스마트 포인터의 접근은 `.` 을 이용한 접근 연산자로 접근,  `->` 는 소유 객체에 대한 접근
* `make_unique()`는 c++14 이후에 만들어진 함수, 인스턴스를 안전하게 생성할 수 있습니다.
* 보통 대부분의 포인터는 `make_unique()`를 이용한 `unique_ptr`입니다.

<br/>

#### shared_ptr

**shared_ptr은 하나의 특정 객체를 참조하는 스마트 포인터가 총 몇 개인지를 참조하는 스마트 포인터입니다.**  
이렇게 참조하고 있는 스마트 포인터의 개수를 참조 횟수(reference count)라고 합니다. 참조 횟수는 특정 객체에 새로운 shared_ptr이 추가될 때마다 1씩 증가하며, 수명이 다할 때마다 1씩 감소합니다. 따라서 마지막 shared_ptr의 수명이 다하여, 참조 횟수가 0이 되면 delete 키워드를 사용하여 메모리를 자동으로 해제합니다.

> 예제입니다.

~~~cpp
#include "pch.h"

#include <vector>
#include <iostream>
#include <memory>
#include <string>

using namespace std;

class Person
{
private:
	string name_;
	int age_;
public:
	Person(const string& name, int age); // 기초 클래스 생성자의 선언
	~Person() { cout << "소멸자가 호출되었습니다." << endl; }
	void ShowPersonInfo();
};

Person::Person(const string& name, int age) // 기초 클래스 생성자의 정의
{
	name_ = name;
	age_ = age;
	cout << "생성자가 호출되었습니다." << endl;
}

void Person::ShowPersonInfo() 
{
	cout << name_ << "의 나이는 " << age_ << "살입니다." << endl;
}

vector<shared_ptr<Person>> people; //전역 변수 Vector

void SmartPointerTest()
{
	shared_ptr<Person> ptr01 = make_shared<Person>("길동", 29); // int형 shared_ptr인 ptr01을 선언하고 초기화함.

	cout << ptr01.use_count() << endl; // 1
	auto ptr02(ptr01);                 // 복사 생성자를 이용한 초기화
	cout << ptr01.use_count() << endl; // 2
	auto ptr03 = ptr01;                // 대입을 통한 초기화
	cout << ptr01.use_count() << endl; // 3  


	//////////////////////////////////////////////////////////////////////////
	// 만약 이 부분이 없다면, 마찬가지로 현 함수를 끝나면 Stack 영역에서 shared_ptr가 초기화 되기 때문에, 소멸자가 불림
	people.push_back(ptr03);		   // 다 같은 것을 가리키기 때문에 어떤 것을 담아도 상관 없음
	cout << ptr03.use_count() << endl; // 4
	// 하지만 이렇게 벡터에 shared_ptr<person>를 담아 둔다면 
}

int main(void)
{
	SmartPointerTest();
	// 소멸자 출력이 이 아웃풋 보다 위인 것으로 인해 unique_ptr는 Stack영역 단위로 포인터를 해제하는 것을 알 수 있다.

	cout << people[0].use_count() << endl; // 1
	// Vector에 아직 하나가 남아있으므로 힙 영역에 할당된 메모리가 해제되지 않는다.

	while (1);
}
~~~


##### 특징

* 스마트 포인터 자체에 접근하여 `.`(을 통한 접근) `use_count()` 함수를 이용하면 참조 하고 있는 레퍼런스 카운트를 알 수 있습니다.
* 레퍼런스 카운트가 0이되면 힙영역에 할당된 메모리를 해제합니다.
* 복사 생성자를 통한 초기화가 가능합니다. (레퍼런스 카운트 + 1)
* 대입 연산자를 통한 초기화가 가능합니다.. (레퍼런스 카운트 + 1)
* 마찬가지로 해제는 스마트 포인터 자체에 접근하여 `.`(을 통한 접근) `reset()` 함수를 이용하거나, 해당 Stack 영역에 할당된 스마트 포인터가 스택에서 빠지면서 해제됩니다.  <br/>**그것은 레퍼런스 카운트가 -1이 됨을 의미합니다.**



<br/>

#### weak_ptr

`weak_ptr`은 하나 이상의 `shared_ptr` 인스턴스가 소유하는 객체에 대한 접근을 제공하지만, 소유자의 수에는 포함되지 않는 스마트 포인터입니다.
  
> 왜 이런것을 만들었을까?

**접근은 가능!, 소유자는 하나(레퍼런스 카운트가 증가하지 않음)** <br/>
`shared_ptr`은 참조 횟수(reference count)를 기반으로 동작하는 스마트 포인터입니다. 만약 서로가 상대방을 가리키는 `shared_ptr`를 가지고 있다면, 참조 횟수는 절대 0이 되지 않으므로 메모리는 영원히 해제되지 않습니다. 이렇게 서로가 상대방을 참조하고 있는 상황을 순환 참조(circular reference)라고 합니다. 이러한 순환 참조로 인해 스마트 포인터가 소유한 메모리가 해제되지 않는 것을 방지하는 용도로 사용합니다. <br/><br/>

**즉! 상호 참조, 순환 참조를 제거하는 용도로 사용!**


> 예제입니다.

~~~cpp
#include "pch.h"

#include <vector>
#include <iostream>
#include <memory>
#include <string>

using namespace std;

class Money;

class Person
{
private:

public:
	Person(); // 기초 클래스 생성자의 선언
	~Person() { cout << "Person 소멸자가 호출되었습니다." << endl; }
	shared_ptr<Money> pMoney;
};

Person::Person() // 기초 클래스 생성자의 정의
{
	cout << "Person 생성자가 호출되었습니다." << endl;
}

class Money
{
private:

public:
	Money(); // 기초 클래스 생성자의 선언
	~Money() { cout << "Money 소멸자가 호출되었습니다." << endl; }

	weak_ptr<Person> pPerson;
};

Money::Money() // 기초 클래스 생성자의 정의
{
	cout << "Money 생성자가 호출되었습니다." << endl;
}


int main(void)
{
	shared_ptr<Person> pPerson = make_shared<Person>();
	shared_ptr<Money> pMoney= make_shared<Money>();

	// 상호 참조!
	pPerson->pMoney = pMoney;
	pMoney->pPerson = pPerson;

	// Money의 pPerson은 weak_ptr 이기 때문에 정상적으로 메모리 해제가 일어나
	// 양쪽의 소멸자가 모두 호출된다.
}
~~~


##### 특징

* 순환 참조, 상호 참조시에 레퍼런스 카운트가 0이 되지 않는 경우를 방지하기 위해 사용
* 나머지 경우는 잘 모르겠음..


<br/>

#### Smart Pointer to Nomal Pointer

1. `std::shared<int> intPointer : int *ipVal`

~~~cpp
std::shared_ptr<int> intPointer1 = std::make_shared<int>(3);
int *ipVal = intPointer1.get();

int a = 4;
int *b = &a;
// 즉 get() 함수는 주솟 값을 리턴해준다.
~~~

2. `std::shared<int> intPointer : int iVal`

~~~cpp
std::shared_ptr<int> intPointer2 = std::make_shared<int>(3);
int iVal = *intPointer1;
~~~


<br/>

#### 참고

* <https://msdn.microsoft.com/ko-kr/library/hh279674.aspx>
* <http://tcpschool.com/cpp/cpp_template_smartPointer>




<br/><br/>

### 확장할 수 있는 Random 헤더 파일

쉽다.

> 예제입니다.

~~~cpp
#include <random>  
#include <iostream>  

using namespace std;

int main()
{
	/* 1. 기본 난수 */
	random_device rd;  
	mt19937 gen(rd());  // to seed mersenne twister.
	for (int i = 0; i < 5; ++i) {
		cout << gen() << " ";
	}
	cout << endl;

	/* 2. 범위 난수 */
	//random_device rd;  // 위에 것 사용
	//mt19937 gen(rd()); // 위에 것 사용
	uniform_int_distribution<> dist(1, 6); // distribute results between 1 and 6 inclusive.  
	for (int i = 0; i < 5; ++i) {
		cout << dist(gen) << " ";
	}
	cout << endl;
}
~~~



<br/><br/>

### Thread

* 일단 기달...



<br/><br/>

### async future

* 일단 기달...
* <http://snowdeer.github.io/c++/2017/08/20/cpp11-future-and-async/>




<br/><br/>

### boost:: asio

* 네트웍 프로그래밍
	* I/O와 같이 시간이 걸리는 처리를 OS의 비동기 기능과 스레드를 사용하여 처리
* C++20에 들어온다고 하는 얘기가 있음
* <https://www.slideshare.net/jacking/kgc-2012boostasio>




<br/><br/>

### boost:: multi_index

* 하나의 객체를 여러 키로 적용하여 사용하고 싶을 때
	* 예를 들어 캐릭터의 ID로 검색하거나, 캐릭터의 이름으로 검색하고 싶을 때
* <https://www.slideshare.net/OnGameServer/sdc-3rd>




