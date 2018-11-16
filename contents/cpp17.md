## go home! [:house_with_garden:](https://github.com/wnsgml972/midas_log)

<br/><hr/>

## C++ 17 문법 공부

### 사용 법

__Visual Studio 2017 기준에서 C++17 컴파일러를 이용하려면 <br/> C/C++ > 언어 > C++ 언어 표준에서 다음과 같이 선택하면 된다.__

![c++17](/media/cpp17.png)


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

### td::variant<A,B,C,...>

작성중...