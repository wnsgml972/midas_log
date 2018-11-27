## go home! [:house_with_garden:](https://github.com/wnsgml972/midas_log)

<br/><hr/>

## C++ 17 문법 공부

### 사용 법

__Visual Studio 2017 기준에서 C++17 컴파일러를 이용하려면 <br/> C/C++ > 언어 > C++ 언어 표준에서 다음과 같이 선택하면 된다.__

![c++17](/media/BasicEducation/cpp17.png)



<br/>

### if-Init

if 문 하나에서 초기화 작업과 검증 과정을 동시에 할 수가 있음!

~~~
#include <iostream>
#include <vector>

int main()
{
	std::vector<int> *v;

//	if(initializing; Validation) 한번에 가능
	if (v = nullptr; v->size() == 0)
	{
		// Using v
	}
}
~~~


<br/>

### Structured Bindings

위 tuple에서 데이터를 얻은 방법으로서 ```[]``` 를 이용한 문법이다.


#### 예시 1

밑의 코드를 보면 i에는 x, s에는 str이 받아진다.
struct 변수를 직접 ```auto [i, s]```를 통해 변수를 받고 변경하면, 결과값은 바뀐 결과값으로 출력된다.  
i와 s가 값이 바뀌는 것은 참조와 관련이 있기 때문이다. (& 는 없지만), 허나 참조와 완전히 같지는 않고 비슷한 방식으로 동작한다고 한다.

~~~
struct Foo
{
	int x = 0;
	std::string str = "world";


	~Foo() { std::cout << str; }
};

int main()
{
	auto[i, s] = Foo();
	std::cout << "hello ";
	s = "structured bindings";

	// 결과로 hello world가 아닌
	// hello structured bindings  출력
}
~~~

#### 예시 2

위의 코드에서 헷갈렸던 참조를 이 예제로 정리할 수 있다!  
case 1과 case 2를 보면 된다. x를 참조가 아닌 값으로 __객체를 생성하면서 받지 않고!__ 받을 경우 a를 변화시켜도 실제 값인 x의 i값은 변하지 않는다.  
허나 case 2를 보면 객체를 생성하면서 값을 받을 경우 b의 값이 곧 생성된 객체의 값과 같다.  
이제 case 5를 보면 참조로 받은 경우 밖에서 변화된 값이 x.i의 값과 같은 것을 확인할 수 있다.  
case 3은 const와 관련이 있다.  
case 4는 참조를 통해 새로 생성된 형태의 객체를 받을 수 없다.

~~~
#include <iostream>

int main()
{
	struct X { int i = 0; };

	X x;

	// case 1  O
	auto[a] = x;
	a++;
	std::cout << a << x.i << std::endl;
	// a = 1, x.i = 0

	// case 2  O
	auto[b] = X();
	b++;
	std::cout << b << std::endl;
	// b = 1

	// case 3  △
	auto const[c] = X(); // Build Warning!  i가 const가 아니기 때문에
	//c++; Copile Error

	// case 4  X
	//auto &[d] = X(); Compile Error
	//d++;

	// case 5  O
	auto &[e] = x;
	e++;
	std::cout << e << x.i << std::endl;
	// e = 1, x.i = 1

	// case 6  △
	auto const &[f] = X(); // Build Warning!  i가 const가 아니기 때문에
	//f++;  Compile Error
}
~~~

#### 예시 3

배열에도 다음과 같이 사용할 수 있다.

~~~
#include <iostream>

int main()
{
	int arr[4] = { 1, 2, 3, 4 };
	auto[a, b, c, d] = arr;
	//auto&[a, b, c, d] = arr;

	a = 3; //마찬가지로 주석의 방법을 사용하지 않을 경우 값이 바뀌지 않음

	for (auto p : arr)
	{
		std::cout << p;
	}
}
~~~


<br/>

### Deduction Guides

이것으로 인해 지금까지 템플릿에서 타입을 명시해서 생성해줘야 했던 것들이 아래와 같이 사용 가능하다.(컴파일러가 Type을 추정 함)  
막 귀찮게 make_tuple 이런 거 안해도 됨

~~~
#include <iostream>
#include <tuple>
#include <string>



int main()
{
	std::tuple<int, std::string> is1 = std::tuple(17, "hello");
	auto is2 = std::tuple(17, "hello"); // !! pair<int, char const *>
	auto is3 = std::tuple(17, std::string("hello"));
	auto is4 = std::tuple(17, "hello");
}
~~~

<br/>

### template auto

다음과 같이 Type을 auto로 지정할 수 있다.

~~~
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

### Fold Expressions

다음과 같이 sum(a,b,c,etc...) 같은 형태를 다음과 같이 표현할 수 있다.

~~~
template <typename... Args>
auto sum(Args&&... args) {
	return (args + ... + 0);
}


int main()
{
	sum(4, 5, 6, 7);
}
~~~



<br/>

### Nested Namespaces

중복 namespace를 다음과 같이 표현할 수 있다.

~~~
namespace A::B::C {
   struct Foo { };
   //...
}
~~~



<br/>

### Single Param static_assert

static_assert를 하나의 인자로 사용할 수 있다.

~~~
static_assert(sizeof(short) == 2)
~~~



<br/>

### Inline Variables

다음과 같은 문법도 사용 가능하다.

<table>
<tr>
<th>
C++14
</th>
<th>
C++17
</th>
</tr>
<tr>
<td  valign="top">

<pre lang="cpp">
// foo.h
extern int foo;

// foo.cpp
int foo = 10;
</pre>
</td>
<td  valign="top">

<pre lang="cpp">
// foo.h
inline int foo = 10;
</pre>
</td>
</tr>
</table>



<table>
<tr>
<th>
C++14
</th>
<th>
C++17
</th>
</tr>
<tr>
<td  valign="top">

<pre lang="cpp">
// foo.h
struct Foo {
   static int foo;
};

// foo.cpp
int Foo::foo = 10;
</pre>
</td>
<td  valign="top">

<pre lang="cpp">
// foo.h
struct Foo {
   static inline int foo = 10;
};
</pre>
</td>
</tr>
</table>


<br/>

### 참고

* <https://medium.com/@snghojeong/c-17-%EC%83%88%EB%A1%9C%EC%9A%B4-%EA%B8%B0%EB%8A%A5%EB%93%A4-558f323c27d1>
* <https://github.com/tvaneerd/cpp17_in_TTs/blob/master/ALL_IN_ONE.md>




<br/>

### std::optional<A>

**결과와 반환 값을 한번에 컨트롤 할 수 있게 해준다.**

~~~
#include <string>
#include <iostream>
#include <optional>

// optional can be used as the return type of a factory that may fail
std::optional<std::string> create(bool b) {
	if (b)
		return "Godzilla";
	return {};
}

// std::nullopt can be used to create any (empty) std::optional
auto create2(bool b) {
	return b ? std::optional<std::string>{"Godzilla"} : std::nullopt;
}

// std::reference_wrapper may be used to return a reference
auto create_ref(bool b) {
	static std::string value = "Godzilla";
	return b ? std::optional<std::reference_wrapper<std::string>>{value}
	: std::nullopt;
}

int main()
{
	// example 1. 
	std::cout << "create(false) returned "
		<< create(false).value_or("empty") << '\n';


	// example 2.
	// optional-returning factory functions are usable as conditions of while and if
	if (auto str = create2(true)) {
		std::cout << "create2(true) returned " << *str << '\n';
	}
	else {
		std::cout << "create(false) returned "
			<< create2(false).value_or("empty") << '\n';
	}

	// example 3.
	if (auto str = create_ref(true)) {
		// using get() to access the reference_wrapper's value
		std::cout << "create_ref(true) returned " << str->get() << '\n';
		str->get() = "Mothra";
		std::cout << "modifying it changed it to " << str->get() << '\n';
	}
}
~~~



<br/>

### std::variant<A,B,C,...>

**`형식에 안전한(typesafe) union`, `variant`**

* 밑의 그림과 같이 컴파일 시점에 틀린 부분을 점검해주기 때문에 형식에 안전한 공용체라 불린다.

~~~
void f()
{
    variant<int, double, string> v = 123;

    cout << v << endl; // 123

    cout << std::get<int>(v) << endl; // 명시적으로 int를 요청.
                                      // 앞의 행과 같은 결과를 낸다.

    cout << std::get<0>(v) << endl;   // 첫 번째 형식(int)의 값을 요청.
                                      // 역시 같은 결과를 낸다.

    auto f = std::get<float>(v);      // (1) 컴파일 오류

    auto cond = std::get<string>(v);  // (2) 실행 시점 예외 발생
}
~~~

<br/>

> visit? <br/>
이 부분은 variant의 활용에 중요하지만 지면 제한 때문에 간단하게만 언급했는데, 언제 std::variant와 방문자 패턴, 그리고 '꼬리표 달린 공용체(tagged union)'에 관해 보충 글을 써보겠습니다.

* 진짜 핵 어려움..

~~~
#include <iomanip>
#include <iostream>
#include <string>
#include <type_traits>
#include <variant>
#include <vector>

// the variant to visit
using var_t = std::variant<int, long, double, std::string>;

// helper type for the visitor #3
template<class T> struct always_false : std::false_type {};

// helper type for the visitor #4
template<class... Ts> struct overloaded : Ts... { using Ts::operator()...; };
template<class... Ts> overloaded(Ts...)->overloaded<Ts...>;

int main() {
	std::vector<var_t> vec = { 10, 15l, 1.5, "hello" };
	for (auto& v : vec) {

		// 1. void visitor, only called for side-effects (here, for I/O)
		std::visit([](auto&& arg) {std::cout << arg; }, v);

		// 2. value-returning visitor, demonstrates the idiom of returning another variant
		var_t w = std::visit([](auto&& arg) -> var_t {return arg + arg; }, v);

		// 3. type-matching visitor: a lambda that handles each type differently
		std::cout << ". After doubling, variant holds ";
		std::visit([](auto&& arg) {
			using T = std::decay_t<decltype(arg)>;
			if constexpr (std::is_same_v<T, int>)
				std::cout << "int with value " << arg << '\n';
			else if constexpr (std::is_same_v<T, long>)
				std::cout << "long with value " << arg << '\n';
			else if constexpr (std::is_same_v<T, double>)
				std::cout << "double with value " << arg << '\n';
			else if constexpr (std::is_same_v<T, std::string>)
				std::cout << "std::string with value " << std::quoted(arg) << '\n';
			else
				static_assert(always_false<T>::value, "non-exhaustive visitor!");
		}, w);
	}

	for (auto& v : vec) {
		// 4. another type-matching visitor: a class with 3 overloaded operator()'s
		std::visit(overloaded{
			[](auto arg) { std::cout << arg << ' '; },
			[](double arg) { std::cout << std::fixed << arg << ' '; },
			[](const std::string& arg) { std::cout << std::quoted(arg) << ' '; },
			}, v);
	}
}
~~~

<br/>

### std::any<A,B,C,...>

**`형식에 안전한(typesafe) void*`, `any`**

* 복사가 가능한 형식만을 담을 수 있다.
* `any_cast`
* optional과 같이 빈 객체를 생성하는 기본 생성자가 있다. 즉 `has_value() false, true 가능하다`


~~~
#include <any>
#include <iostream>

int main()
{
    std::cout << std::boolalpha;
 
    // any type
    std::any a = 1;
    std::cout << a.type().name() << ": " << std::any_cast<int>(a) << '\n';
    a = 3.14;
    std::cout << a.type().name() << ": " << std::any_cast<double>(a) << '\n';
    a = true;
    std::cout << a.type().name() << ": " << std::any_cast<bool>(a) << '\n';
 
    // bad cast
    try
    {
        a = 1;
        std::cout << std::any_cast<float>(a) << '\n';
    }
    catch (const std::bad_any_cast& e)
    {
        std::cout << e.what() << '\n';
    }
 
    // has value
    a = 1;
    if (a.has_value())
    {
        std::cout << a.type().name() << '\n';
    }
 
    // reset
    a.reset();
    if (!a.has_value())
    {
        std::cout << "no value\n";
    }
 
    // pointer to contained data
    a = 1;
    int* i = std::any_cast<int>(&a);
    std::cout << *i << "\n";
}
~~~

<br/>

### std::string_view

변하지 않는 문자열 값을 전달할 때 우리는 `const string& val`을 이용했다. 참조 const를 이용하므로서 메모리를 새로 할당하지 않는 것이다. 하지만 string을 전달하지 않고 `"Hello World"`로 전달할 때는 const char*를 받는 생성자를 호출함으로서 메모리를 새로 할당한다. 

그에 대한 대체제로 `std::string_view`가 나왔다. 이제 우리는 `const string_view& val`을 통해 값을 전달하면 된다.

~~~
void println(const string_view& str)
{
    cout << str << endl;
}
~~~


<br/>

### 기타 새로 추가 된 알고리즘

**C++17에서 새로 추가된 알고리즘들을 간략하게만 소개하겠다. 이들은 모두 std 이름공간에 속한다.**

* clamp: 주어진 값이 하한보다 작으면 하한을, 상한보다 크면 상한을 돌려준다. 그 외에는 주어진 값을 돌려준다. 비교 함수를 지정할 수 있다. 필요한 헤더는 `<algorithm>`이다.

* for_each_n: for_each와 같되 [first, last)가 아니라 [first, first+n)을 입력 범위로 사용하며, first+n을 돌려준다(참고로 for_each의 직렬 버전은 사용자 지정 함수를 돌려주고 병렬 버전은 아무것도 돌려주지 않는다). 필요한 헤더는 `<algorithm>`이며, 병렬 버전도 있다.

* sample: 주어진 범위의 요소 n개를 주어진 확률 분포에 따라 무작위로 선택한다. 필요한 헤더는 `<algorithm>`이다.

* uninitialized_move와 uninitialized_move_n: 주어진 요소들을 초기화되지 않은 메모리 영역으로 이동한다. 필요한 헤더는 `<memory>`이며, 병렬 버전도 있다.

* reduce: 분산 처리나 병렬 처리 관련 프레임워크 또는 언어에서 흔히 볼 수 있는 Map-Reduce(사상-축약) 패턴의 Reduce 단계에 해당하는 알고리즘이다. 필요한 헤더는 `<numeric>`이며, 병렬 버전도 있다. 참고로 Map에 해당하는 표준 라이브러리 알고리즘은 transform이다. 기존 accumulate 알고리즘에 병렬 버전이 추가되지 않았는데, 대신 reduce의 병렬 버전을 사용하면 된다.

* transform_reduce: Map-Reduce에 해당하는 알고리즘으로, 요소들을 먼저 변환한 후에 축약한다. 필요한 헤더는 `<numeric>`이며, 병렬 버전도 있다. 기존 inner_product 알고리즘에 병렬 버전이 추가되지 않았는데, 대신 이 transform_reduce의 병렬 버전을 사용하면 된다.

* inclusive_scan과 exclusive_scan: 요소들의 구간 합(prefix sum; 또는 부분합)들을 구한다. inclusive_scan은 i번째 요소를 i번째 부분합에 포함하고, exclusive_scan은 포함하지 않는다. 예를 들어 덧셈과 초기치 0을 사용한다고 할 때 {1, 1, 0, 2, 3}의 inclusive_scan 결과는 {1, 2, 2, 4, 7}이고 exclusive_scan 결과는 {0, 1, 2, 2, 4}이다. 덧셈 이외의 합산 함수를 지정할 수 있으며, 부분합의 초기치도 다르게 지정할 수 있다(기본은 0). 필요한 헤더는 `<numeric>`이며, 병렬 버전도 있다. 기존 partial_sum 알고리즘에 병렬 버전이 추가되지 않았는데, 대신 inclusive_scan의 병렬 버전을 사용하면 된다.

* transform_inclusive_scan과 transform_inclusive_scan: 요소들을 먼저 변환한 후 구간 합을 구한다. 필요한 헤더는 `<numeric>`이며, 병렬 버전도 있다.

**그 외에 C++17 표준 라이브러리의 변경 사항을 정리하자면 다음과 같다.**

* map과 unordered_map에 try_emplace와 insert_or_assign이라는 새로운 멤버 함수가 추가되었다. 이들은 주어진 키가 컨테이너에 없는 경우에만 새 요소를 생성 또는 삽입한다.
* 컨테이너 멤버 함수 size, empty, data의 비멤버 함수 버전인 std::size, std::empty, std::data가 추가되었다(<iterator> 헤더).
* 메모리를 구성하는 바이트byte의 개념을 좀 더 명시적으로 표현하기 위해 std::byte라는 형식이 추가되었다. 내부적으로 std::byte는 하나의 열거형 클래스(enum class)인데, 바탕 자료 형식은 unsigned char이다. unsigned char와는 달리 std::byte는 문자 형식으로도, 수치(산술) 형식으로도 간주되지 않는다. 개념적으로 std::byte는 단지 비트들의 집합일 뿐이며, 산술 연산자들은 지원하지 않고 비트별 논리 연산자들과 자리이동(shift) 연산자들만 지원한다. 임의의 정수 n을 std::byte 객체로 변환하려면 std::byte{n} 형태의 표현식을 사용하고(C++17부터는 이런 식으로 열거형 객체를 생성할 수 있게 되었다), 그 반대의 변환은 std::to_integer 함수(역시 C++17에서 추가되었다)를 사용하면 된다.
* 컴파일 시점에서 형식 특질들의 논리합, 논리곱, 부정을 산출하는 메타 함수 conjunction, disjunction, negation이 추가되었으며, is_aggregate, is_invocable, is_swappable 등 다양한 컴파일 시점 형식 판정 함수가 추가되었다(`<type_traits>` 헤더).
* 최대공약수와 최소공배수를 돌려주는 수학 함수 gcd와 lcm이 추가되었으며(`<numeric>` 헤더), 타원적분, 베셀 함수, 르장드르 함수, 노이만 함수, 리만 제타 함수 등 다양한 특수 함수가 추가되었다(<cmath> 헤더). 표 A.4에 특수 함수들이 나열되어 있다.