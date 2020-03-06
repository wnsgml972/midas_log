
# In Memory Database : Pluto Todo

> Dev Log, 크게 5가지로 구성<br/>
 Core Engine 개발로서 사실 상 어떠한 부분도 작은 영역의 개발은 아님, 조그마한 코드를 변경해도 막대한 책임이 따름, 확실한 검증 방법을 거쳐야만 할 것을 명심할 것 (__Test에 많은 시간을 쏟아야 함__)



> mit_core와 mit_db 2개만 가지고 database를 형성할 수 있도록 확실히 모듈화 해보자

## 개발한 것
### DB Core Code Quality (Code Level Micro Tuning)
   1) Migrate to modern cpp 17
   2) Macro to Constexpr(const), Enum Class(Enum)
   3) Use Override, Final
   4) Clean Comments, Remove Code Comments
   5) Delete MFC Type, Hungarian (USHORT, UINT, DWORD, WORD...)
   6) Remove Duplicate Code
   7) Delete Output Param, Use Various const, const reference Param
   8) Convert the variable name to a readable variable name
   9) Struct Packing (+ Run Time, Compile Time Checker)

### Class abstraction, encapsulation and separation of responsibilities
   1) High Risk Function Encapsulation
   2) DB Reflection IO Util API Class
   3) DB Table IO Util API Class
   4) MIterator Util API Class
   5) Refactor MHashContainer 및 Hash Macro
   6) MStruct (Base Struct)
   7) DB Session(Storage) Layering (mit, mdk, cim layer), (interface, base, project, partial) : Child Callback is Used UI in Upper Layer

### Software Architecture (Critical Decision)
   1) MQueryControler API (for MIDAS Bash 기반)
   2) Inheritance Table
   3) `MHistory Class ~ing`
   4) `MQueryParser ~ing`
   5) `MFile IO API Util Class ~ing`
   6) `Third-Party DB System Compatibility ~ing`
   7) MTableTypeObject

### DB Core Validation
   1) DB Table Type Match
   2) DB Insert Duplicate Key
   3) DB Insert No Key
   4) DB No Declare Type
   5) DB Reflection : Compile Time Human Error Checker
   6) DB Core : Run Time Error Checker
   7) Modernized Example Table & Data Object
   8) `Implement DB Core Regression Code ~ing`

### DB Core Usability Enhancement
   1) Provide many convenient APIs
   2) Documentation (Architecture, `Philosophy`, API Document, Study History)
   3) Declare Table Type API
   4) Presentation & Education
   5) `MIDAS DB Wizard ~ing`
   6) DB Session Factory
   7) DB Session Manager
   8) DB Session Monitor
   9) Unit Ctrl Interface API

<br/><br/>

## 남은 것
### Study OpenSource Reference (보고 많이 배울 것)
* [cubrid](http://www.cubrid.com/cubrid) : 설명 진짜 잘 나와 있음!
* [altibase](http://kr.altibase.com/product/)

### DB Core Code Quality (Code Level Micro Tuning)
   1) MDataPool
~~~
DataType의 선언 구별화 (그냥 GetType이 아니라 진짜 명확화 할 수 있는 구조...?)
아니 그리고 DataType 선언부가 Common 프로젝트에 있음 왜…?
DPOOL_SLOT 리팩토링
OBJECT_TYPE_SLOT : SetUserDPoolSlot처럼 만들까…?  → 고민
MDataPool 다 뜯어야 함, MCustomDataPool 만들고 final 다 붙일 것, 그리고 getter에 const 하위 클래스 봐가면서 하나하나 다 붙일 것
~~~
   2) Delete BOOL
   3) Delete CString
   4) Bulk Memory



<br/>

### Class abstraction, encapsulation and separation of responsibilities
   1) Refactor MHashContainer
~~~
MHashContainer 현재 충돌 알고리즘 단순 체이닝인데, 현재 최신 자버 버젼의 HashMap 처럼 충돌 크기가 일정 갯 수를 넘어갈 경우 RB Tree로 전환할 수 있도록 개선
Hash Memory 검증
Hash Performance 검증
3배 이하의 변수 충돌 검증
2진수 근접 검증
~~~
   2) Util Class Doc Safe



<br/>

### Software Architecture (Critical Decision)
   1) MQuery Class (for MIDAS Bash)
~~~
Transaction 전후 비교 bash 보여주는 함수
~~~
   2) Third-Party DB System Compatibility ~ing
~~~
다른 DB System과 상호작용 하는 모듈 레이어링이 필요(현재는 Parasolid와 결합성 제거가 필요)
~~~
   3) MHistory Class ~ing
~~~
Core Transaction, Core Task 통합 개선
~~~
   4) MQuery Class ~ing
   5) MFile IO API Util Class ~ing
~~~
File IO API
Save / Load
Export / Import API
~~~
   6) Database Sharding
~~~
퍼포먼스 향상을 위한  데이터베이스 샤딩 https://medium.com/@jeeyoungk/how-sharding-works-b4dec46b3f6
~~~
   7) Visualize DataBase By Class Diagram
~~~
개발자 뿐만 아니라, 엔지니어도 DB를 사용하여 설계할 때 퍼포먼스를 생각하며 기능을 완성할 수 있도록 DB를 Visual 하는 기능
찾아보니까 유니티에는 이런 게 있음
~~~
   8) Process Relation
~~~
   https://midasitdev.atlassian.net/wiki/spaces/~313676491/pages/46170117/CIM+DB
~~~
   9) 위의 이슈와도 비슷하겠지만 DataPool 단순 조회의 관한 것들
~~~
enable if, if constxepr, queue 등을 활용하면 단순 조회하며 필요 없는 부분들도 모든 테이블 조회하던 것들에 대한 것을 전체적으로 퍼포먼스를 향상시킬 수 있을 듯?
~~~
   10) Data 무결성 판별 규칙 및 시점 추가
~~~
https://coding-factory.tistory.com/221
~~~
   11) 내부 알고리즘 병렬 처리 구현, 제일 중요!
~~~
알티베이스, 레디스 등 상용 DB 참고할 것
~~~



<br/>

### DB Core Validation
   1) Casing과 DataType이 다르게 매칭됐을 시의 미스매칭 검사
   2) DB에 선언할 수 없는 다른 Type 넣었을 때 컴파일 타임 검증
~~~
#define에 대한 일반 심볼릭 상수가 안좋은 이유 :
https://boycoding.tistory.com/156
~~~
   3) Core DB : RegDataPool 순서 오류 시 나타나는 휴먼 에러 잡기
   4) DB Reflection : Compile Time Human Error Checker
      - Struct Standard Layout Compile Time Checker
   5) Implement DB Core Regression Code ~ing   

### DB Core Usability Enhancement
   1) Provide many convenient APIs
   2) Documentation (Architecture, Philosophy, API Document, Study History)
~~~
DB Reflection 참고할만한 자료 : https://github.com/veselink1/refl-cpp
~~~
   3) Presentation & Education
   4) `MIDAS DB Wizard ~ing`
~~~
Def나 DPool 생성 시 자동 소스 생성 (MIDAS DB 마법사)
~~~
