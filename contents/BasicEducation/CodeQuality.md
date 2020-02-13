# 코드 품질

> 블로그에 최신 버젼 있음~


## 정적 코드 분석

정적 코드 분석 과정은 개발자가 눈으로 코드를 따라가면서 코드의 결함을 찾는 것과 유사하다. 다만 코드 분석기는 개발자보다 꼼꼼하고, 휴리스틱하다. 


### 소나큐브나, Git Lab 유료 버전을 이용한 코드 정적분석 자동화

![help](/media/q_all.png)

1. IDE에서 코드수정을 하고 remote 저장소에 commit & push를 한다.
2. 그 다음 github에서 master(혹은 stable한 branch)에 대해 작업 branch를 PullRequest 올린다.
3. 미리 등록한 github의 web-hook에 의해 PullRequest 정보들을 jenkins에 전송한다.
4. 전달받은 정보를 재 가공하여 SonarQube로 정적분석을 요청한다.
5. SonarQube에서 분석한 정보를 다시 jenkins로 return 해준다.
6. SonarQube으로부터 return 받은 정보를 해당 PullRequest의 댓글에 리포팅을 해준다.

**좋지 않은 품질의 코드들이 Merge 되는 것을 막을 수 있다.**

* 참고 <https://taetaetae.github.io/2018/02/08/jenkins-sonar-github-integration/>


<br/>

# Visual Stdio Code Quality

> 일반적으로 디버깅을 잘하는 것은 정말 코드 실력에 도움이 많이된다. 다음과 같은 MicroSoft Docs에 정말 잘 나와있다!<br/>
<https://docs.microsoft.com/ko-kr/visualstudio/debugger/?view=vs-2019>

## 1. 컴파일러 경고 강화

* `프로젝트 속성 -> C/C++ -> 일반 -> SDL 검사 (예 체크)`, `(Security Development Lifecyle)`


<br/>

## 2 (1). 직접 코드 분석 돌리기
* `분석 -> 코드 분석`을 통해 코드 분석을 실행

## 2 (2) 빌드시마다 정적 코드 분석하기
* `프로젝트 속성 -> 코드 분석 -> 빌드에 코드 분석 사용 (체크) -> 원하는 코드 분석 규칙 집합 선택`을 이용하면 빌드시에도 코드 분석을 할 수 있다.
* Cpp Core Check라는 규칙으로도 Ckeck 가능
* 근데 뭔가 잘 안되는 듯...?  ->  규칙을 한 3가지 넣고 빌드해봤는데, 안 걸렸음 규칙이 부족한 건지, 모두 적합했는지는 확인 안 해봄

### 코드 분석 시 나온 도움 창

* 출력 창에 저 부분을 클릭하면

![help](/media/help1.png)

* 다음과 같은 설명이 나온다.

![help](/media/help2.png)

### 사용해보고 쓸만한 부분

* 버퍼 오버 플로우, 초기화, null or nullptr 참조  이정도..?

### 조금 귀찮은 부분과 그에 따른 해결방법?

* 쓸데 없이 나오는 경고들 `#programa`로 해결


<br/>

## 3. 디버거를 사용한 예외 관리

> 시스템 단의 예외도 체크 방식으로 걸리게 만들 수 있다.

### 사용환경
가끔 프로그램이 갑자기 어떠한 경고창도 없이 출력창에 메시지만 남기고 죽어버리는 경우가 있다. 
그럴 때 해당 기능을 사용하여 원하는 예외처리 범위를 늘려 예외를 관리한다.<br/>

### Microsoft Docs
* <https://docs.microsoft.com/ko-kr/visualstudio/debugger/managing-exceptions-with-the-debugger?view=vs-2019>

같은 것들은 기본적으로 익혀두도록 한다.


<br/>

## 4. 디버거 사용자 지정 시각화

> 일반적으로 모든 정보를 표시하는 디버거를 원하는 모양으로 볼 수 있게 시각화 할 수 있다.

### 사용환경
일반적으로 나오는 디버그 모드의 자동이나 조사식 등의 정보들이 너무 복잡하게 형성되어 있을 수 있다. 허나 어떤 특정 UI의 Root 같은 경우, 아니면 Java의 Object 같은 객체는 일반적으로 사용자가 보고 싶은 형태로 디버거 변수들을 시각화 할 수 있다.

### Boost 라이브러리를 통한 예제
* <https://www.kdata.or.kr/info/info_04_view.html?field=&keyword=&type=techreport&page=5&dbnum=188505&mode=detail&type=techreport>

### Microsoft Docs
* <https://docs.microsoft.com/ko-kr/visualstudio/debugger/create-custom-views-of-native-objects?view=vs-2019>


<br/>

## 10. 그 외 여러가지

### 자동
* 자동 적으로 변수 표시
* 함수의 반환 값 표시

### 로컬
* 해당 지역 변수 내 디버깅 가능

### 조사식
* this
* 주솟값을 직접 조사식에 입력하여 한 변수 계속 추적

### 여러가지 중단 팁
* 중단 조건에 예외 추가하기
* 데이터 중단점으로 주솟값을 이용하여 해당 변수 변경하면 중단하기 설정
* 데이터 중단점으로 마우스 우클릭하여 -> 해당 값 바뀌면 중단하기 설정

### 예외 설정에서 전체 예외 그냥 이것저것 다 걸리게 해놓고 돌리기
* 다시 원상태로 복구하기 버튼 있음!

### 호출 스택
* 호출 스택 시각화 (엔터프라이즈만 되는 듯ㅠ) [link](https://docs.microsoft.com/ko-kr/visualstudio/debugger/map-methods-on-the-call-stack-while-debugging-in-visual-studio?view=vs-2019)





<br/>

## 5. .editorconfig file

> 편집기나 IDE에 관계없이 **일관된 코딩 스타일**을 유지할 수 있게 해준다.

### 지원되는 설정
* indent_style
* indent_size
* tab_width
* end_of_line
* 문자 집합
* trim_trailing_whitespace
* insert_final_newline
* 루트

### 예시

~~~
# 소스 코드 일관성 편집툴
# https://docs.microsoft.com/ko-kr/visualstudio/ide/create-portable-custom-editor-options
#

# 루트 설정 파일
root = true

# 모든 파일이 대해
#   줄바꿈 문자 기본값을 \r\n 으로
#   텍스트 인코딩을 utf-8로
[*]
end_of_line: crlf

# 웹 관련 기능들은 utf-8이 기본
[*.{json,md,js,html,htm}]
charset = utf-8

# C/C++ 소스 및 헤더파일에 대해
# 1.  Intent Style은 Space 한 Indent당 Space 4개 : 즉 Tap 누르면 Space 4개로
# 2.  빈 공백문자를 없애기
# 3.  소스 마지막줄에 빈줄 추가하기
[*.{cpp,c,h,hpp}]
indent_style = space
indent_size = 4
# 2. #trim_trailing_whitespace = true
# 3. #insert_final_newline = true
~~~

### Reference
* <https://docs.microsoft.com/ko-kr/visualstudio/ide/create-portable-custom-editor-options?view=vs-2017>





<br/><br/>

## 여러 정적 코드 분석기

### CPP Check

무료 정적 분석기로 그냥 분석해서 로깅만 해준다. GitLab 유료 버전과 연동하고, CI를 이용하여 파이프 라인에 붙여 사용하면 좋을 듯 하다.

1. 파이프라인에 붙여, (Git Lab 담당장의 선택적 분석 or 항상 분석) 사용하기
2. GitLab 유료 버전을 통해, 분석 결과 자동 `PR`
3. 개발자는 보고 피드백하면 됨

### SonarQube

* SonarQube는 기업 단위에서는 다 유료이고, 개인 개발자 단위에서도 C/C++은 유료이다...
* SonarQube 또한 `Github`에서는 `Web Hooking?`을 이용하면, `GitLab`에서는 그에 따른 연동 방식으로 분석 결과를 `PR`로 보낼 수 있는 듯 하다.
* `GitLab` 말고도 `Jenkis` 같은 CI와 많은 개발자들이 연동해서 사용하는 듯 하다.

### 결과

* 혼자 개발하는 조그마한 개인 프로젝트에만 한번씩 위에있는 VS 정적 코드 분석기를 돌리면 될 것 같다.





<br/><br/>

## 개발자 간 코드 리뷰 시스템 도입

* [Code Review](/contents/BasicEducation/CodeReview.md)