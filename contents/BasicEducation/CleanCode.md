# Clean Code

## 깨끗한 코드

### 소프트웨어의 5S 철학

1. 정리   : 적절한 명명법을 사용하자
2. 정돈   : 코드는 전부 제자리에 가있어야 한다.
3. 청소   : 과거 이력이나 미래 바람을 위한 주석, 혹은 지워야할 코드 주석 -> 모두 제거한다.
4. 청결   : 그룹 내의 일관적인 구현 스타일과 기법
5. 생활화: 관례를 따르고 자신의 작품을 자주 돌아보고, 기꺼이 변경해야 한다.

### 르블랑의 법칙 

* 나중에 고쳐야지~~ ->  나중은 절대 오지 않는다.

### 코드 감각을 얻어야 한다.

* 코드 감각이 있는 프로그래머는 나쁜 모듈을 알아보는 것에 더해서 좋은 모듈이 될 수 있는 개선 방안을 떠올린다.
* 즉 깨끗한 코드를 작성하는 프로그래머는 빈 캠퍼스를 우아한 그림으로 바꿀 능력이 있는 화가와 같다.

### 문학적인 코드를 작성해라

* 인간이 읽기 쉬운 코드를 작성해라, 5S 철학 중 정리에 관한 것

### 깨끗한 코드

* 중복 줄이기, 표현력 높이기, 초반부터 간단한 추상화 고려하기





<br/>


## 좋은 이름

### 의미 있게 구분하라

* 관용어를 넣지 마라. 
* a, the, data, Info 다 안 좋은 예이다.

### 읽기 쉽게 짜라, 검색하기 쉽게 짜라

### 인코딩을 피해라

* 헝가리 식 표기법, 옛날 방식
* 이제는 변수의 선언 위치와 사용 위치가 멀지 않음
* IDE가 모든 것을 알려주고 하이라이팅 해 줌
* 함수의 타입을 바꾸기가 힘들다.

### 이름 설정

* 클래스 이름은 명사구
* 메소드 이름은 동사구
* 의미 있는 이름 : get, set, is, valid

### 해법 영역에서 가져온 이름을 사용하라

* AFactory, AVisitor
* 디자인 패턴의 공식을 활용

### 좋은 이름을 선택하려면 설명 능력이 뛰어나야 하고 문화적인 배경이 같아야 한다.
### 이름만 잘 지어도 코드 가독성이 높아진다  ->   리팩토링이 쉽다.





<br/>

## 함수

### 작게 만들어라

* 함수는 작게 만들어라 (3줄, 4줄 이라도 작을수록 좋다.)

### 한 가지만 해라

**한 함수는 한 가지 일만, 여러 가지 일을 하는 함수는 작은 함수로 나눠서 만든다.**

* 어떤 것이 한 가지??
   * 지정된 함수의 이름 아래 추상화 수준이 하나인 것.  
   * 즉 함수 이름이 여러 개의 일을 지칭한다 해도 지정된 함수 이름 내에서의
    추상화 수준이 하나인 것을 준수한다.  (이름과 하는 일이 같아야 한다.)
* 함수 내에서 의미 있는 이름으로 다른 함수를 추출한다면 한 가지 일이 아니다.
이름 설정에 유의하자.
* 부수 효과를 일으키지 마라
   * 함수의 이름에 한 가지 일만 한다고 해놓고 다른 일도 하는 경우가 많다. -> 오류를 초래한다.

### 내려가기 규칙

<mark>**코드는 위에서 아래로 문학, 이야기처럼 읽혀야 좋다.**</mark>

### Switch 문

* 정말 사용하면 안 되는 방법 중 하나. 이거 하나로 이미 여러 가지 일을 함을 암시한다.
* switch문을 사용할 경우 상속, 구성 관계로 숨긴 뒤에(Factory) 다른 코드에 노출하지 않는다.

### 서술적인 이름을 함수의 이름으로

* 함수의 이름이 길어지는 것을 두려워하지 말자.
* 짧고 어려운 것보다 길고 간단한 것이 훨씬 낫다.

### 함수의 인수

* 0항, 1항, 2항, 3항, 4항
   * 0항이 제일 좋고, 그 다음 1항, 그 다음 2항, 3항은 피해야 한다. 4항은 특별한 일이 있을 때만 사용해야 한다.(거의 안 쓰는 게 좋다.)
* 단항
    * 인수에 질문을 던지는 경우(ex isEmpty(?))
    * 인수를 무언가로 변환해 리턴하는 경우
    * 플래그 인수 -> 추하다, 이 함수가 여러 일을 할 것이다! 를 알려버리는 인수
* 이항
    * 최대한 기피하고, 단항 함수로 짤 수 있도록 하자.
*  삼항
    * 마찬가지로 더 어려워진다.
    * 인수 객체의 가능성을 짚어보자. <br/>
    : makeCircle(int x, int y, int radius) -> makeCircle(Point center, int radius)
    * 함수 이름에 키워드를 넣자.  인수의 순서에 관한 키워드를 넣어도 좋음
* 부수 효과를 일으키지 마라
    * 넘어 온 인수가 변경 되던가, 다른 클래스를 수정하는 경우가 있다. -> 오류를 초래한다.


### 출력 인수를 피해야 한다.

* `appendFooter(report)  ->  report.appendFooter()`
* 객체지향이기 때문에 가능하다.

### 명령과 조회를 분리해라

~~~cpp
if(junheeIsExist())
  setJunHee()
~~~

### 오류 코드를 리턴하는 방식보다는 예외 처리를 이용한다.

* 오류 코드를 반환하는 방식은 명령/조회 분리의 규칙을 미묘하게 위반한다.
* try / catch를 이용하되 주의사항이 있다.

### Try / Catch 블록 뽑아내기

* 정상 동작과 오류 동작을 분리해야 한다.  ->  throws를 이용한다.
* 오류도 함수다. 한 가지의 오류를 처리하고 한가지의 일을 처리하도록 구성한다.
* throws를 이용해 Exception 클래스를 생성해 오류를 처리하는 방법을 이용한다.

### 함수를 짜는 방법

<mark>**논문을 쓰는 것과 같다.**</mark>

먼저 생각을 하고 기록한 후 읽기 좋게 다듬는다. 함수도 마찬가지다. 처음에는 길고 복잡하다. 이름은 즉흥적이고 중복된다. 하지만 그 서투른 코드를 빠짐없이 테스트 코드를 작성한다. 그런 다음 코드를 다듬고, 함수를 만들고, 이름을 바꾸고, 중복을 제거한다. <br/>
메소드를 줄이고 순서를 바꾼다. 때로는 전체 클래스를 쪼개기도 한다. 이 와중에도 코드는 계속 테스트 케이스를 통과한다. 최종적으로 이 장에서 설명한 함수가 만들어진다. <br/>
**처음부터 딱 짜내지 않는다. 그게 가능한 사람은 없으니까.**




<br/>

## 주석

### 주석은 실패다.

* 코드로 의도를 표현할 표현력이 부족해, 주석으로 더하는 것이다. 실패라는 표현이 마땅하다.
* 주석은 거짓말을 한다. 코드는 변하지만 주석은 항상 변할 수 없다. 때로는 거짓말을 한다. 그렇기에 주석을 코드를 고칠 때 같이 고치는 노력보다 **코드로서 의도를 명확히 표현할 방법을 모색하는 것이 낫다.**

### 좋은 주석

*  의도를 표현하는 주석
    * 스레드를 대량 생성하는 방법으로 어떻게든 경쟁 상태를 만들려 시도한다.
* 결과를 경고하는 주석
    * 시간이 많을 경우에만 사용하시오.
* TODO Comment
    * 대부분의 IDE가 ToDo 주석에 대한 기능을 제공한다. 앞으로의 작업 목록을 ToDo로 주석해놓고 Visual Studio 기준 `Ctrl + \, Ctrl + t` -> ( Alt + T ) 를 이용하면 찾을 수 있다.
* 중요성을 강조하는 주석
    * ex) 여기서의 trim은 정말 중요!


### 나쁜 주석

* 대부분의 주석은 나쁜 주석이다. 주석보다는 코드로서 의미를 전달하려 노력하자. 
    * 즉 코드의 표현력을 기르는 연습이 필요하다. 
* 주석으로 처리한 코드
    * 정말 최악의 주석이다! 다른 사람이 지우지도 못하고 코드만 어지럽힌다.




<br/>

## 형식 맞추기

### 코드를 열었을 때 독자들이 깔끔하고, 일관적이며, 꼼꼼하다고 감탄했으면 좋겠다.

* 코드 형식은 의사소통의 일환이다. 원활한 의사소통을 장려할 수 있는 코드 형식을 이용하자.

### 신문 기사처럼 작성하라.

* 마치 보고서를 쓰는 것처럼 형식에 맞춰 작성해라.

### 세로 밀집도

* 서로 밀접한 코드는 세로로 가까이에 두어야 한다.
* 서로 연관 있는 함수는 세로로 가까이에 두어야 한다.
* 일반적인 변수는 사용 위치 근처에 선언을 해야 하고, 인스턴스 변수는 서로 모아두어 제일 위나 아래쪽에 선언을 해야 한다.

### 결국 관련 있는 코드는 근처에 두고 작성한다.

### 마치 보고서처럼 함수의 종속성이나 여러 서로 관련 있다, 서로가 서로를 필요로 한다, 등등을 고려하여 작성하여야 한다.



<br/>

## 객체와 자료구조

<mark>**코드를 열었을 때 독자들이 깔끔하고, 일관적이며, 꼼꼼하다고 감탄했으면 좋겠다.**</mark>

### 자료 추상화

* 객체는 자료를 숨기고, 함수를 공개해야 한다.

### 기차 충돌 코드를 피하라

* 객체를 리턴하는 함수의 경우, 한 줄에 모든 것을 작성하면 안 된다. (하나의 객체가 다 일을 하는 것처럼 보임)

~~~
ex) a.a().b().c()    X 
a.a(); b.b(); c.c();  O
~~~

### 꼭 객체지향적인 것이 절대적으로 좋은 것은 아니다.

* 새로운 자료 타입을 추가하는 유연성이 필요하면 객체지향구조가 적합하고,
* 새로운 동작을 추가하는 유연성이 필요하면 절자지향적인 구조가 더 적합하다.
* 다만, 둘이 섞여 잡종 구조가 되는 것은 반드시 피해야 한다!


<br/>

## 오류 처리

### 오류 처리는 중요하다. 하지만 오류 처리로 인하여 프로그램의 논리를 이해하기 어려워진다면 깨끗한 코드라고 할 수 없다.


### 오류 코드보다는 예외 처리를 사용하라.  (throws)

* 객체는 자료를 숨기고, 함수를 공개해야 한다.

### 예외에 의미를 담아 전달해라

* 메시지나 레벨 등 예외에 힌트를 부여하라

### 특수 사례 패턴

* 특정 조건에 따른 일을 행하고자 할 때, 예외를 처리하지 말고 클래스를 만들어 반환하게 해라

### null을 반환하지 마라!

* 호출자에게 일거리를 던지는 행위이다. 누구하나라도 까먹을 시 문제가 발생한다!
* 예외를 던지거나 특수 사례 객체를 반환해라.
* 나 진짜 null 반환 많이 하는데... 줄여야 할 듯

~~~cpp
ex)
List<Employee> employees = getEmployees();
if (employees == null)
  이런 거 안 됨!
~~~

### 보통 STL에는 빈 배열을 검사해주는 함수가 존재한다. 빈 배열을 반환하고 빈 배열을 검사하는 함수를 사용하자.




<br/>

## 경계

### 모든 소프트웨어를 직접 개발하는 경우는 드물다. 외부 코드를 깔끔하게 정리하는 방법이 필요하다.

### 소프트웨어의 경계를 깔끔하게 처리하는 기법이 필요하다.


### 필요한 부분만 캡슐화하고 구성하여 나만의 라이브러리를 만들어 사용하자.

* 프레임워크, 인터페이스 제공자는 최대한 많은 부분을 제공하려 애쓴다. 때문에 필요하지 않은 기능과 사용해선 안 되는 기능도 같이 사용하게 된다. 적절히 캡슐화하고 구성하여 사용하자.

### 경계 살피고 익히기

* 외부 코드를 적용하여 사용하기 전에(내 코드와 섞기 전에), 외부 코드만 적용하고 테스트하여 먼저 외부 코드를 익히자! 이를 “학습 테스트”라 한다.

### 학습 테스트

* 학습 테스트를 작성하고, 외부 코드를 익히는 과정은 어떠한 것보다 도움이 많이 된다!
* 나중에 버전이 바뀌었을 때도 잡아 낼 수 있음.


### 즉 테스트를 통해 외부 코드를 익히고, 캡슐화 하여 독자적인 클래스를 만든다, 아니면 어뎁터 패턴을 이용하는 방법도 좋다!

* 외부 API 호출을 줄임으로서 경계를 관리하자.
* 외부 경계 인터페이스를 몰라도 이제 사용할 수 있게 된다.



<br/>

## 단위 테스트

### 우리 분야는 현재 눈부신 성장을 이뤘다. 테스트 자동화, TDD 등등. 하지만 우리 분야에 테스트를 추가하려고 급하게 서두르다가 중요한 테스트 케이스를 작성하지 못 한다. 제대로 된 테스트 케이스를 작성해야 한다.


### 깨끗한 테스트 코드 유지하기

* 테스트 코드이니까. 테스트 코드는 일단 빨리. 라는 생각을 할지 모르겠다.
* 테스트 코드는 실제 못지않게 중요하다!, 제일 중요한 건 가독성! 가독성! 가독성!
* 한번 사용하고 버릴 테스트 코드는 없느니만 못 하다. 실제 코드와 같이 구조적으로 깨끗한 코드로 작성하자.

### 테스트는 유연성, 유지 보수성, 재사용성을 제공한다.

* 단위 테스트 코드가 있다면, 변경에 유연해진다.

### 이중 표준

* 테스트 코드는 실제 못지않게 중요한 것은 맞다. 하지만 실제 코드만큼 효율적일 필요는 없다.
* 각각의 코드 작성 표준이 존재하는 것도 좋은 방법이다.
* 가독성은 유지하되 간결하고 편한 방법.

### 테스트 당 Assert 하나?, No 테스트 당 개념 하나

* 절대적인 것은 아니나, 테스트 당 Assert 하나는 가혹한 부분이 많다.
* 테스트 당 개념 하나가 더 낫겠다.


### 깨끗한 테스트를 위한 규칙, F.I.R.S.T

* Fast
    * 테스트는 빠르게 돌아야한다. 테스트가 느리게 돌면 테스트 돌리는 것을 겁내게 된다.
* Independent
    * 각 테스트는 서로 의존하면 안 된다.
* Repeatable
    * 테스트는 반복 가능해야 한다. (테스트가 돌아가다가 끊기면 이어서 진행되어야 한다.)
* Self-Validating
    * 테스트는 Bool 값으로 결과가 나와야 한다. 성공 or 실패이다. 통과 여부를 알기 위해 로그 값을 읽게 하는 행위는 매우 좋지 못 한 방법이다.
* Timely
    * 테스트는 적시에 작성해야 한다. 단위 테스트는 실제 코드 구현하기 직전에 작성한다. 
    * (난 잘 동의할 수 없음...) 실제 코드 구현 전에 해야 테스트 코드 작성이 더 쉽데...




<br/>

## 클래스

### 클래스는 작아야 한다.

* 함수와 마찬가지로 클래스는 작아야 한다. 함수는 물리적인 행 수로 크기를 정했지만, 클래스는 맡은 책임을 센다.
* 클래스의 이름은 해당 클래스의 책임을 의미해야 하며, 물리적인 숫자가 아닌! 책임을 세는 것을 명시해야한다.

### SRP(Single Responsibility Principle), 단일 책임의 법칙

나는 설계 패턴을 배우며, 너무 쓸데없이 많은 것을 다 쪼개어 이것이 과연 좋은가? 라는 생각을 한 적이 있다. 클래스를 나타내는 단일 책임에서 이 **책임은 클래스의 이름을 의미한다!** 클래스의 이름, 책임을 잘 정의하고 클래스를 구성하자. 클래스에게 책임을 줄 때는 변경을 유의하며 만들어야 한다. 하나의 책임이 변경되면 하나의 클래스만 수정해야 한다. 때문에 강조한다. **큰 클래스 몇 개가 아니라 작은 클래스 여럿으로 이뤄진 시스템이 더 바람직하다. 잘 캡슐화하고 잘 구성해보자.**

### 응집도

* 클래스는 인스턴스 변수가 적아야 한다.
* 응집도를 유지하면 작은 클래스가 여럿이 나온다. 

### 응집도

* 클래스는 인스턴스 변수가 적아야 한다.

### TODO 진행중....








<br/><br/>

# 실제 개발을 진행하며 내가 생각한 규칙

## 리팩토링 시 주의할 것

### 공통

* 꼭 단위 테스트를 작성한 뒤! 리팩토링을 할 것

### 함수 (함수 쪼개기)

* 함수는 작게, 매개변수 목록은 짧게 법칙을 따른다.
* 명령과 조회를 구분해라.
* 출력 인수를 되도록 사용하지 말자.
* 부수 효과를 일으키지 마라.  (함수의 이름이 행하는 일만 해라.)
* 때에 따른 구분도 좋은 방법이다. begin, end 등등
* valid, is, get, set, convert, begin, end

### 클래스 (클래스 쪼개기)

* SRP를 지키고 있는가?



<br/>

## 결과 값 규칙 (가독성을 위해, 출력 인자를 없애자.)

1. TestCode의 반환 값은 True or False로 정하도록 한다.
   - 완성된 결과 값이 필요하다면 매개변수를 이용한다.
2. 일반 코드의 결과 값은 반환 값으로 받는 방식을 이용한다.
   - 대부분의 방식은 반환 값을 통해 결과 값을 받는다. (출력 인수를 사용하지 않는다.)
        * 예외 사항
        - convert Class의 convert 함수, a의 정보를 이용해서 b에 대입하여 완성된 b를 만드는 함수.
        명명 규칙을 convert를 붙인 후, 매개변수 참조를 이용하여 결과를 받는다.
        - swap 함수, 매개 변수를 통해 값을 받는다.
3. 사용자의 선택에 의해 성공과 실패가 갈리는 함수의 명령은 bool 타입을 이용해 결과 값을 받는다.
   - 왜 bool로 처리해야 하는가?
      1. 사용자가 올바르지 않은 값을 넘겨줬다고 해서 프로그램이 죽는 것은 올바르지 못 하다.
      2. 올바르지 못한 값을 넘겨줌으로서 읽었던 코드 부분의 Transaction 부분을 RollBack 해야한다.  (그래서 Solid 부분은 bool 결과 값을 이용했다.)
   - pCADSession 부분에도 나눌 수 있겠지만, 일단 내가 짠 부분이 아니다. bool 타입으로 결과 값을 처리한다. 다행히 CAD의 대부분은 사용자가 넘겨 준 값에 의해 성공과 실패가 결정된다. 즉 결과 값을 bool로 처리하는 방식이 옳다!
4. 성공 해야만 하는 부분의 실패가 나올 수 있는 부분은 assert를 이용하여 처리한다. 만약 성공해야 하지만 구현상 불가능한 경우 사용자에게 알려줘야 할 때는 Msg Dlg 같은 것을 이용한다.
   - Import 코드는 무조건 성공해야 하는 부분이다. 당연히 bool 타입으로 결과를 배출하는 방식은 옳지 못 하다.
5. 만약 성공, 실패의 반환 값과 결과의 반환 값이 둘다 필요한 경우 Optional을 이용한다.
   - optional과 c++17 문법인 If-Init을 이용하면 정말 깔끔하게 코드를 짤 수 있다.
~~~cpp
if(auto resultPath = readUserSelectedFilePath(); resultPath)
{ path_in = *resultPath; }
~~~
6. Get 함수 중, 반환 값이 존재하지 않는 경우를 표현하고 싶을 때 {}를 이용한다.
7. 쓸데없이 nullptr 반환을 하지마라. 일을 미루는 행위다.  위에가 결국 null이긴 하지만, 꼭 필요한 경우에만 사용한다.