## go home! [:house_with_garden:](https://github.com/wnsgml972/midas_log)


<br/><br/>


## gtest

### 특징

* Google에서 개발된 C++ 테스트 프레임 워크
* Git에서 다운받아서 헤더파일 추가해서 사용
* 간단한 사용 방식은 다운 받아서, 검증해야할 부분을 검증하면 됨
* 사용하기 편하게 라이브러리를 덮어서 만들면 더 좋음, Test Fixture
* 구글에서도 실제로 사용 중 (Chrom)
* 항상 맞다고 생각하는 부분은 꼭 테스트 코드를 작성해야 함 (DB, Socket 통신 등등)
* 테스트 코드를 다 통과하더라도 버그가 없는 건 아니다.
* 사실 상 및에 참고 자료 정독하면 다 이해 됨

### 참고

* [gtest 1 (c++ 17 문법은 아님!)](https://www.slideshare.net/jinhwason/ss-69528881)
* [gtest 2 (c++ 17 문법은 아님!)](https://www.slideshare.net/zone0000/c-7522148?next_slideshow=1)

<br/>

## C++ Style Casting




<br/>

# C++ 17 문법 공부

## If-Init

## 익명 함수 문법(Lambda)

<pre>
  auto bar = []() -> float { return 3.14f; }; // arrow function을 이용하면 타입 명시 가능
  [a,&b] a를 복사로 캡처, b를 참조로 캡처.
  [this] 현재 객체를 참조로 캡처.
  [&] 몸통에서 쓰이는 모든 변수나 상수를 참조로 캡처하고 현재 객체를 참조로 캡처.
  [=] 몸통에서 쓰이는 모든 변수나 상수를 복사로 캡처하고 현재 객체를 참조로 캡처.
  [] 아무것도 캡처하지 않음.  
</pre>

## 참고

* [C++ 17](https://medium.com/@snghojeong/c-17-%EC%83%88%EB%A1%9C%EC%9A%B4-%EA%B8%B0%EB%8A%A5%EB%93%A4-558f323c27d1)
