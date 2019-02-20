
## Visual Studio

### Visual Studio User Macro 정의

각종 Path 정리에 유용한 User Macro를    `*.vcxproj.user`     에 정의할 수 있다.
예를 들어 여기에 사용하려는 Lib의 path를   `<LIB_PATH>C://</LIB_PATH>`   로 정의해놓고 프로젝트 속성상에서 `$(LIB_PATH)`   로 사용할 수 있다.
IFC++를 Build하는 과정에서도 다음과 같은 방법을 사용했다.

### Visual Studio 프로젝트 속성 중, 외부 라이브러리 import

**Visual Studio 기본 설정**<br/>
VC++ 디렉터리 > 포함 디렉터리 <br/>
VC++ 디렉터리 > 라이브러리 디렉터리

**추가 include 설정** <br/>
C/C++ > 일반 > 추가 포함 디렉터리

**추가 lib 설정**<br/>
링커 > 일반 > 추가 라이브러리 디렉터리 <br/>
링커 > 입력 > 추가 종속성

**추가 dll 설정**<br/>
디버깅 > 환경


### IDE 구성

* 텍스트 에디터   : 말 그대로 텍스트 에디터   (`*.c, *.cpp` 등등 을 읽음)
* 컴파일러 	   : 어셈블러 코드로 번역해줌  (`*.s` 파일 생성)
* 어셈블러	   : 기계어로 바꾸어 줌          (`*.obj` 파일 생성)
* 디버거	   : 디버깅을 위한 일을 함
* 링커	   : obj 파일을 모두 묶어 하나의 `exe` 파일로 만들어준다.
를 모두 포함한다.

### Build, Compile

* `Compile` : 소스의 문법을 분석해 기계어로 번역
* `Build` : 번역된 것과 그 외 것들을 모아서 실행 가능한 파일로 만듦 (Compile + 그 외)

**VS Compile 후 생성 파일** <br/>
* `obj` 파일 : 기계어로 컴파일 된 파일
* `pdb` 파일 : Program Debug Database의 약자, 디버깅, dll 파일 위치 등 여러 정보가 저장되어 있는 파일
* `idb` 파일 : Incremental Debug Database의 약자, 재 컴파일을 위한 파일

**VS Build 후 생성 파일** <br/>
* `exe` 파일 : 실행 파일
* `ilk` 파일  :  Incremental Linker File의 약자, 재 빌드를 위한 파일

### 실행 파일 만드는 과정

프로그램 작성 -> 전처리 -> 컴파일 -> 링크 -> 실행

### Debug, Release 모드

* `Debug` 모드는 개발 시에 사용
* `Release` 모드는 최적화를 해서 상용화 시험을 할 때 사용
* `Debug와` `Release` 모드를 따로 분리하는 전처리 기능이 있음!



<br/>

## OS

### 64 비트 시스템
* CPU가 처리할 수 있는 작업의 단위


### 프로세스, 스레드
**프로세스** <br/>
* 수행 중인 프로그램의 인스턴스
* 스케줄링의 대상이 되는 작업

**스레드** <br/>
* 프로세스의 작업 실행 흐름 단위
* 프로세스 하나에는 하나 이상의 스레드가 있음

### 커널
* 운영체제의 핵심 부분
* 하드웨어의 조작을 막기 위해 있음
* 시스템 콜을 통해 유저들은 커널 모드로 접근

### ASCII
* 정보 교환을 위한 미국 표준 부호
* ASCII는 1바이트를 사용하며 최상위 비트는 0으로 표시하고 나머지 7비트를 사용



<br/>

## C

### Macro 문법
* <https://ruvendix.blog.me/220902724381?Redirect=Log&from=postView>
* `#@`  문자로 표현해줌
* `##`   붙여줌
* `\` 한 칸 띄고 작성 가능


### L-value,  R-value
* `L-value`는 객체를 참조할 수 있는 값, 객체의 주소를 알아낼 수 있는 값, 식별자
* `R-value`는 상수
