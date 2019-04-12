# MIDAS Filter

![filter](/media/aui/filter.png)

## 목적
* 계층적인 프로젝트군으로 만듬    
* 순환참조의 위험을 막고 명시적인 참조관계를 가짐  
* 프로젝트/클래스 네이밍으로 직관적인 파악 가능
* 계층별 목적(모델링, 해석, 설계, ... 등)을 명확히 하여 관리 용이

## 참고
* 모듈화 구성 (편의상 확장자로 설명)
  * `*.dll`
    * 필수 Component는 프로젝트 군별 참조 최소화 및 분명한 역할 구분으로 필요시에 분리 용이하도록 함
  * `*.ext`
    * 동적인 로딩 및 plugin 인터페이스를 가지는 dll
    * 제품별 Component로 제품별 구현을 모듈화하여 제품별 분기 제거
    * 선택적 Component는 필요에 따라 동적 로딩 가능
  * `*.exe`
    * 시작 프로젝트는 제품별로 동일한 코드, 리소스만 제한적으로 구현
 
* 프로젝트 이름의 `post-fix` 의미
  * c : 제품별, 선택적 component를 의미하며 출시제품에 따라 기본으로 제공 될 모듈
  * p : Plug-in을 의미하며 사용자가 필요에 의해 추가하는 기능
   ~~~cpp
   MDKCADc_modeling.prj   //MDKCAD 레벨까지만 참조하는 modeling 기능 관련 모듈
   ~~~


## 구성
그림과 같이 제일 바깥 쪽은 크게 5가지로 나누어진다.
1. `Core`
2. `CIM`
3. `Component`
4. `Extra`
5. `Global Setting Files`

## 세부 구성

### Core
* `MIT (Midas IT)`
  * 라이브러리 프로젝트이다.
  * MDK, CIM 참조 불가능하다.
  * 안의 해당 요소들은 다른 제품과 연관없이 구현된 원자적인 모듈로 이루어져 있다.
  * ex) CIM과 연관되지 않은 부분의 Alice UI
* `MDK (Midas development Kit)`
  * 기본적인 제품 구현이 가능한 프로젝트군이다.
  * MIT만 참조 가능하다.
  * 다른 것과 연관하여 이루어지는 모듈이다.
  * ex) MFC와 연동하여 화면에 구성되는 ViewUI, PanelUI 등등
* `MAIN`
  * Main Module이 들어간다.
* `Common`
  * 모든 프로젝트에 공통으로 들어가는 모듈이다
  * `*.ini` 파일의 메시지 정보가 있다.
  * 프로젝트의 버젼 정보와 Import된 헤더파일들이 있다.
  * DATYTYPE의 범위가 설정되어 있다.
  * 상품 정보가 설정되어 있다.


### CIM
* 제품 프로젝트군이다. 
* MIT, MDK 참조 가능하다.
* CIM에서만 들어가는 모듈들을 모아 놓은 필터이다.


### Component
* 특정 제품과 관계없는 프로젝트들, 주로 말단의 구현 위주이다.

### Extra
* Extra에는 Google Test로 테스트 하는 Test 모듈이 들어간다.


### Global Setting Files
각각의 설정 파일을 모아놓는 필터이다. 크게 3가지가 들어간다.
1. `.editorconfig`
   * 소스 코드를 일관성있게 편집할 수 있게 해준다.
   * <https://docs.microsoft.com/ko-kr/visualstudio/ide/create-portable-custom-editor-options>
2. `mitcommon.props`
   * User Macro 파일이다.
   * 컴파일러 설정을 한다.
   * lib, dll, *.h 파일 등의 Path 설정을 한다.
   * Input, Output File Path 설정을 한다.
3. `skia.props`
   * User Macro 파일이다.
   * skia의 dll, lib 파일 Dependency 설정을 한다.
   * 마찬가지로 Path 설정을 한다.

### 그 외

* `CAD` : CAD 관련 모듈이다.
* `CAE` : 해석 관련 모듈이다.
* `frx` : MIT Lib의 서로 상호 작용하는 원자적인 모듈들을 모아주는 역할을 하는 모듈이다.

### 정리 그림!

![filter2](/media/aui/filter1.png)