## modern c+ framework 분석


## 목차

* `*.hpp`, `*.c`, `*.h` 파일
* noexcept 선언
* explicit 선언
* constexpr 선언
* 위치에 따른 const 선언의 차이
* default, delete 키워드
* templete auto
* C++14 decltype(auto) 반환
* override 선언
* final 선언
* Fold Expressions


<br/>
<hr/>

### `*.hpp`, `*.c`, `*.h` 파일







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




<br/>

### constexpr 선언





<br/>

### 위치에 따른 const 선언의 차이





<br/>

### default, delete 키워드





<br/>

### templete auto





<br/>

### override 선언





<br/>

### final 선언




<br/>

### Fold Expressions





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