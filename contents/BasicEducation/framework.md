## modern c+ framework 분석


## 목차

* *.hpp, *.c, *.h 파일
* noexcept 선언
* explicit 선언
* template 시 사용할 constexpr, static_assert
* 위치에 따른 const 선언의 차이
* default, delete 키워드
* template auto
* C++14 decltype(auto) 반환
* override 선언
* final 선언
* Fold Expressions


<br/>
<hr/>

### *.hpp, *.c, *.h 파일

정해진 것은 없다. C의 헤더 파일과 C++의 헤더 파일을 구분하기 위해 *.hpp라는 것이 있다고 한다. 실제 적용 모습을 보니, __헤더 파일에 선언부와 구현부를 모두 넣은 파일을 *.hpp 파일로 지정한다.__



<br/>

### noexcept 선언

**예외를 방출하지 않을 함수는 noexcept로 선언하라**

* C++11 이후에서는 모든 메모리 해제 함수와 모든 소멸자 함수는 암묵적으로 `noexcept`다.
* 예외를 던지는 것을 올바르게 사용해야한다.
* 예외를 던져서는 안 되는 장소에서 사용하는 키워드이다.

~~~cpp
int f(int x) noexcept; // f는 예외를 던지지 않음 : C++11 방식
~~~



<br/>

### explicit 선언

**컴파일러가 자동 형변환 하는 것을 막아주는 키워드**

* 밑 예제에서 `int가 -> Foo로` 자동 형변환 되는 것을 막아준다.

~~~cpp
class Foo{
public:
    int m_foo;
    explicit Foo (int foo) : m_foo (foo) {}
};

void printM_foo (Foo foo){
    cout << foo.m_foo << endl;
}

int main (){
    int num = 43;
    printM_foo(num);
}
~~~




<br/>

### template 시 사용할 constexpr, static_assert

* KeyWord는 밑의 2가지이다.
* **컴파일 타임(정적) vs 런타임(동적)**
* 위의 두 키워드는 **컴파일 타임**에 동작한다.

만약 사용자의 입력 값을 받는 것이 아닌, 컴파일 타임에 알 수 있는 컴파일 상수 값은, 굳이 프로그램이 실행하여, 런타임 시간에 값을 검사할 필요가 없다. <br/>
더 나아가 검사할 필요가 없는게 아니라 검사를 그 시기에 하는 것은 맞지 않다. 미리 검사를 함으로서 예외처리 속도도 높일 수 있고, 프로그램 안정성도 높일 수 있다. <br/>

#### if constexpr

* 밑의 함수는 포인터면 포인터가 가리키는 변수 값을, 아니면 그냥 자신의 값을 리턴하는 함수이다.
* if constexpr을 사용하면, template으로 넘어온 컴파일 상수를, 분기 처리에 활용 할 수 있다.
* 원래 기존 if는 template으로 넘어온 컴파일 상수를 활용할 수 없음. 

~~~cpp
#include <iostream>

template <typename T>
auto get_value(T t) {
	if constexpr (std::is_pointer_v<T>)
		return *t;
	else
		return t;
}

int main()
{
	int a = 3;
	int *pVal = &a;
	std::cout << get_value(pVal) << std::endl;
}
~~~

<br/>

* `static_assert()`를 이용해 실행하지 않고도, 컴파일 타임에 넘어온 컴파일 상수 값을 체크하는 것도 좋은 방법이다.
* 이 외에도 if constexpr은 컴파일 상수 값을 분기 처리하는데 좋다.

~~~cpp
template <typename T>
void do_something() {
     if constexpr (std::is_arithmetic<T>) {
         //do some maths
     }
     else {
       static_assert(false); //invalid for all specializations
     }
}
~~~

<br/>

### 위치에 따른 const 선언의 차이

**const 선언 바로 뒤의 값을 상수화한다.**<br/>
크게 2가지로 나누어서 볼 수 있다.


#### const int * pointer

`int` 즉 변수 값을 상수화한다.<br/>
pointer의 주솟 값 자체는 변경이 가능하지만 pointer가 가리키는 값은 상수화 되어 있다.

#### int const * pointer

`*` 즉 포인터를 상수화한다.<br/>
pointer의 주솟 값 자체를 상수화하여 주솟 값 변경이 불가능하게 만든다.

#### const int const * pointer

변수 값과, 주솟 값 모두 상수화 한다.<br/>
둘다 컴파일 오류가 난다.

~~~cpp
int value = 42;

const int* const p = &value;
p = &value;    //Error
*p = 10;       //Error
~~~



<br/>

### default, delete 키워드

#### default 키워드

* C++11에서부터는 default 키워드를 이용하여 명시적으로 default 생성자를 선언할 수 있습니다.
* C++11 이전부터 C++ 클래스는 내부를 작성하지 않아도 기본적으로 기본생성자, 복사생성자, 대입연산자, 소멸자 네 가지 멤버함수를 생성합니다. 
* 생성자를 자동으로 만들어 줌, 복사 생성자는 깊은 복사는 안 됨

#### delete 키워드

* 대표적으로 복사 및 대입이 불가능한 클래스를 만들고 싶을 때를 예로 들 수 있습니다. 
* 이전에는 생성을 원하지 않는 방식의 생성자를 private에 선언했지만 이제는 delete 키워드를 이용하여 해당 방식의 생성을 막을 수 있다.
* 생성자 뿐만 아니라 어떤 일반 함수도 사용할 수 있다.

~~~cpp
class NoInteger
{
// 아래와 같이 함수를 작성하는 경우 매개변수를 int로 암시적 형변환하여 실행하는 것을 막게됩니다.
// 따라서 foo(13)을 실행하는 경우 에러가 발생합니다.
    void foo(double dparam);
    void foo(int iparam) = delete;
};
  
struct OnlyDouble
{
// 아래의 경우는 double외에는 모든 타입에 대해서 막게됩니다.
    void foo(double d);
    template<typename T> void foo(T) = delete;
};
~~~



<br/>

### template auto

**다음과 같이 Type을 auto로 지정할 수 있다.**

~~~cpp
template <auto v>
struct integral_constant
{
	static constexpr auto value = v;
};
int main()
{
	integral_constant<2048>::value;
	integral_constant<'a'>::value;

}
~~~





<br/>

### C++14 decltype(auto) 반환

* 후행 반환을 이용할 시 C++14 이후로부터는 타입을 `->`을 통하여 명시하지 않아도 된다.
* `decltype(auto)`를 이용해서
* 복잡하게 생각할 것 없이, 함수에서 auto 타입 반환을 하려거든 이것을 이용하면 된다.

~~~cpp
template <typename T, typename U>  
decltype(auto) add_template(T&& x, U&& y)
{
    return std::forward<T>(x) + std::forward<U>(y);    
}
 
decltype(auto) add_function(int a, int b)
{
    return a + b;
}
 
template <typename TBuilder>
decltype(auto) MakeAndProcessObject(const TBuilder& builder)
{
    auto val = builder.makeObject();
    // process...
    return val;
}
~~~





<br/>

### override 선언

**override 선언을 통해 override 된 함수를 표시할 수 있다.**



<br/>

### final 선언

**final 선언을 통해 상속을 막을 수 있다.**



<br/>

### Fold Expressions

다음과 같이 sum(a,b,c,etc...) 같은 형태를 다음과 같이 표현할 수 있다.

~~~cpp
template <typename... Args>
auto sum(Args&&... args) {
	return (args + ... + 0);
}


int main()
{
	sum(4, 5, 6, 7);
}
~~~
