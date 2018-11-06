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

* [gtest 1 (c++ 17 문법은 아님!)](https://www.slideshare.net/jinhwason/ss-69528881)
* [gtest 2 (c++ 17 문법은 아님!)](https://www.slideshare.net/zone0000/c-7522148?next_slideshow=1)

<br/>

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



<br/>

### tuple

원래는 boost 문법인데 C++11이후로부터 표준으로 채택되었다.<br/>
2개 이상의 값을 반환하거나 전달할 때 사용하면 유용하다.

~~~
#include <tuple>
#include <string>
#include <iostream>

int main()
{
    // make tuple variable.
    typedef std::tuple<int, std::string, bool> OddOrEven;
    OddOrEven myNumber = std::make_tuple(10, std::string("Even"), true);

    // get tuple size
    std::cout << "size : " << std::tuple_size<decltype(myNumber)>::value << std::endl;

    // get each value and get type using std::tuple_element, auto keyword.
    std::tuple_element<0, decltype(myNumber)>::type nNum = std::get<0>(myNumber);
    auto szVal = std::get<1>(myNumber);
    bool bEven = std::get<2>(myNumber);

    std::cout << nNum << ", " << szVal << ", " << std::boolalpha << bEven << std::endl;

    return 0;
}
~~~

<br/><hr/>

## C++ 17 문법 공부

### 사용 법

__Visual Studio 2017 기준에서 C++17 컴파일러를 이용하려면 <br/> C/C++ > 언어 > C++ 언어 표준에서 다음과 같이 선택하면 된다.__

![c++17](/media/cpp17.png)

### If-Init




### 익명 함수 문법(Lambda)

<pre>
  auto bar = []() -> float { return 3.14f; }; // arrow function을 이용하면 타입 명시 가능
  [a,&b] a를 복사로 캡처, b를 참조로 캡처.
  [this] 현재 객체를 참조로 캡처.
  [&] 몸통에서 쓰이는 모든 변수나 상수를 참조로 캡처하고 현재 객체를 참조로 캡처.
  [=] 몸통에서 쓰이는 모든 변수나 상수를 복사로 캡처하고 현재 객체를 참조로 캡처.
  [] 아무것도 캡처하지 않음.  
</pre>

### 참고

* [C++ 17](https://medium.com/@snghojeong/c-17-%EC%83%88%EB%A1%9C%EC%9A%B4-%EA%B8%B0%EB%8A%A5%EB%93%A4-558f323c27d1)
