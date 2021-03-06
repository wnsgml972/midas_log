
# 빌드 속도 개선

__빌드란 소스코드가 프로그램이 되는 과정__

## 여러가지 컴파일러

### C++ 컴파일러 (Compile Language)

* 바로 기계어로 번역되어 실행
* 실행 속도가 빠르고 보안에도 유리
* 소스코드를 조금만 수정해도 다시 컴파일 해야함..
* 개발 생산성이 낮아짐
* 즉 개발 속도는 낮고, 실행 속도는 빠르다.

### 자바 컴파일러 (Byte Code Language)

* 바이트 코드는 가상 기계어 개념
* 자바는 컴파일 시 결과물이 실행 파일이 아니라 class 라는 바이트 코드 파일이 생김
* 자바 같은 코드언어는 직접적인 하드웨어 제어 불가
* 하지만 자바가 C++ 보다 개발 생산성이 뛰어남

### Javascript, Python (Interpreter Language)

* 컴파일 언어와 다르게 한 줄 씩 바로 언어를 번역해서 실행
* 실시간 분석 가능 -> 개발 생산성이 높음
* 실행 속도는 낮고, 개발 속도는 높다.

<br/>

## Precompiled Header 사용

* C++ 소스파일은 많은 파일을 include 하는 경향이 있다.
* 이런 include들의 문제점은 그 과정이 모든 소스파일에서마다 반복된다는 것이다.
windows.h 와 같이 덩치가 무지 큰 헤더 파일을 #include하는 모든 파일을 매번 컴파일하면 시간이 상당히 걸린다.
* 헤더파일은 사이즈와 상관없이 많은 프로젝트에서 모든 소스파일이 같은 컴파일 시간만큼을 가지는 아주 큰 C++ 컴파일 시간을 가진다.
* 특정 헤더 파일을 미리 파싱한 결과물을 .pch 파일로 덤프시킨다
* 다른 파일들은 이 .pch를 단순히 사용함으로써 중복된 파싱 작업을 없앨 수 있다.
* 그 .pch 파일 중 하나가 stdafx.h이다.

### stdafx.h

* 덩치가 크거나 거의 바뀌지 않는 정의 같은 것을 모두 stdafx.h에 넣는다.
* 기본적인 아이디어는 하나의 cpp파일을 PCH를 만드는 역할로 정하고, 다른 cpp파일 을 그 PCH파일을 사용하는 역할로 정하는 것이다. (stdafx.cpp)
* 하나의 소스파일만이 PCH파일을 만드므로 전체 rebuild에서 한번만 build되는 것을 보장한다.
* 우선 프로젝트의 모든 소스파일이 include할 헤더파일이 하나 필요하다. (stdafx.h)
* PCH파일의 include가 첫줄/제일위에 있는지 확인해보길 바란다.

<br/>

## Incredi-Build

* 분산 빌드 시스템
* 여러 컴퓨터에서 분산해서 빌드
