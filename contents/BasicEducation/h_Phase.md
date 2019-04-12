
# Phase

> 1차 분석

> 크게 Phase Structure, Phase History, Phase Async, Phase Common Interface로 나누어 설명하였습니다. 

## Phase Structure

>   Why `M V VM` Model?

먼저 구조 설명에 앞서, Phase란 무엇인지 한 마디로 말하면 : Phase는 CIM의 기반 프레임워크입니다.

기존 MFC는 MVC 패턴의 FrameWork로 로직, 즉 사용자가 원하고자 하는 바는 같지만 UI의 모양은 다른 것을 원할 때 많은 코드를 수정해야 하는 일이 빈번하였습니다. 

현재 시대의 소프트웨어 방법론에서는 **사용자의 요구사항은 항상 변한다** 라는 것을 가정합니다. 그렇기에 더 발전된 방식인 UI와 Logic을 분리하여 원하고자 하는 요구사항(로직)은 같지만 UI만 바꾸고 싶을 때 간단하게 바꿀 수 있는 `M V VM` 모델을 Phase는 지향하게 되었습니다.

**Phase는 Component 프로그래밍을 지향하며, 개발자간 확실한 작업의 분리를 지향합니다.** 예를 들어 Group Phase를 상속받아 Feature Solid 기능을 구현할 시, 타 어떤 로직과도 영향받지 않고 구현할 수 있으며, 완성된 Feature Solid는 하나의 Component로 완성되어 필요할 때 해당 기능을 사용할 수 있도록 구현되어 있습니다. 

### 정리

* `M V VM` Model
* UI와 Logic의 분리 
* Component Programming
* 개발자 간 확실한 작업의 분리
* 제작된 Phase는 다른 페이즈의 구성요소로서 사용이 가능합니다.
* UI 코딩에 대한 부담이 거의 없이 로직만 구현하면 됩니다.

  


## Phase History

페이즈의 동작은 History로 관리됩니다. 페이즈의 동작은 사용자 단위의 동작으로서, 사용자가 Undo/Redo 하는 단위입니다.

페이즈 히스토리는 여러 페이즈들의 커밋된 순서를 하나의 시퀀스 큐로 저장하고, 시퀀스 큐를 태스크(Task) 큐로 나누어 관리하는 역할을 맡고 있습니다.


### CIM Transaction
* 밖에 Phase TransAction으로 감싸고 안에서 DB TransAction이 불립니다.

#### TransAction 모양

~~~
// One Task
[Phase TransAction Begin
DB TransAction{}
DB TransAction{}
DB TransAction{}
Phase TransAction End]
~~~

#### TransAction 코드

~~~cpp
// CIM TransAction Start
void IfcFileManager::beginTransaction() noexcept
{
	if (!pDBTr->IsAppeared())
	{
		auto bootstrap = std::make_shared<mphase::MPhaseBootstrap>();
		bootstrap->BootPhase(pDBTr);
	} //여기까지 Phase TransAction Begin


	if (!pDBTr->DBTranscationBegin(_LSXW(Import IFC))) { assert(0); return; } 
       //Phase TransAction 안의 DB TransAction Begin
}
~~~

~~~cpp
// 중간의 DB TransAction commit, begin
void IfcBaseConverter::doDBTrBeginCommit(mit_interface::IDBSession * pDBSession)
{
	pDBTr->DBTransactionCommit();
	if (!pDBTr->DBTranscationBegin(_LSXW(Import IFC))) { ASSERT(0); return; }
}
~~~

~~~cpp
// CIM TransAction End
void IfcFileManager::endTransaction() noexcept
{
	if (!pDBTr->DBTransactionCommit()) { assert(0); return; }
        //Phase TransAction 안의 DB TransAction Commit

	pDBTr->Commit();
	pDBTr->MakeTask(_T("MIDAS CIM, ifc read/load")); 
        // Task End
        //여기까지 Phase TransAction Commit
}
~~~



#### 각 TransAction은 뭔지?
* Phase TransAction은 사용자의 Undo, Redo 단위입니다.
* DB TransAction은 해당 Entity를 가진 애들을 Commit 시 한꺼번에 우리 DB에 등록을 시킵니다. (GKey에 따른 Key 값을 연결)
* TransAction이기 때문에 도중에 문제가 있어서 실패하면 다 Rollback 하게됩니다.

#### 사용 이유
* DB에 저장하는 작업을 계속 하면 너무 비효율적이기 때문에 DB TransAction으로 관리합니다
* `DB TransAction`은 우리 DB에 값들을 저장하기 위해 사용하고 
* `Phase TransAction`은 사용자 단위의 Undo/Redo 단위로 사용합니다.



  


## Phase Async

Phase는 비동기 프로그래밍을 지원합니다.

> Why Phase `Async`?

Phase에서 비동기 프로그래밍을 지원하는 이유는 멀티스레드 프로그래밍을 지원하되 멀티스레드 프로그래밍 이라는 느낌을 서비스 사용자가 느끼지 못하게 하기 위함입니다. **(Thread Safe)**



  

## Phase Common Interface

Phase는 `PCI`를 통해 cpp외 타 언어간 프로그래밍이 가능합니다. 

> Why PCI?

그래픽 작업을 할 시 cpp 뿐만 아니라 타 언어를 사용할 때 훨신 효과적일 때가 있습니다. 그러한 그래픽 언어를 도입하기 위하여 `PCI`가 생겨나게 되었습니다.


<br/><br/><hr/>

> 2차 분석

## Part 2. Phase의 구성요소

### Phase
Phase는  Notifier 이자 Observer 이고, MVVM의 Model이라고 할 수 있습니다.

### Phase History
페이즈 히스토리는 여러 페이즈들의 커밋된 순서를 하나의 시퀀스 큐로 저장하고, 시퀀스 큐를 태스크(Task) 큐로 나누어 관리하는 역할을 맡고 있습니다.

### Phase Bootstrap
페이즈 부트스트랩은 페이즈의 실행과 종료를 관리합니다. 

### Phase UI Slot
페이즈 UI슬롯은 실행된(Booted) 페이즈 부트스트랩의 UI를 생성해 줍니다. 하나의 UI Slot은 한번에 하나의 bootstrap만 세팅될 수 있습니다.

### Phase Helper
페이즈 헬퍼는 페이즈가 실행중 필요한 유틸리티들과 각종 라이브러리에 대한 매니저를 담아 둘 수 있습니다. 기본적으로는(MIT_frx 기준), DB Session 과 Geom Manager, Snap Core 등의 Get 함수를 갖고 있습니다.

### Phase UI
* 페이즈 UI는 가상화된 UI의 핵심 개념입니다.
* 각 페이즈의 입력은 페이즈 UI를 통해서 구현되며, 원할 경우 UI는  교체가 가능합니다.
* 특정 UI 가 선언되어 있는 페이즈를 상속받은 페이즈의 경우, 기본적으로 부모 페이즈의 UI를 사용 하게 됩니다. 만약, 독자적인 UI를 사용하고 싶다면 스스로의 UI 를 따로  정의 할 수 있습니다. 이 경우, 새로운 UI 는 부모의 UI를 상속받아서 만들 수도 있고, 전혀 새로운 UI를 만들 수도 있습니다.

### Phase Manager
* 페이즈 UI 매니저는 페이즈 UI의 타입별로 생성함수를 등록해 두고 있습니다.
* 페이즈 UI의 삭제시에 Asynchronous Update 를 제공하고 있습니다. 이것은 이러지 않을 경우 페이즈 UI 의 그룹 관계및 이벤트 처리에 문제를 야기할 수 있기 때문입니다.
* 페이즈UI의 포커싱 기능을 제공하고 있습니다. 위의 삭제시 이슈에 맞물려, 포커싱관리 또한 Asynchronous Update 로 이루어지고 있습니다.




<br/><br/>







# Note

## Filter

![filter](/media/aui/filter.png)

### 목적
* 계층적인 프로젝트군으로 만듬    
* 순환참조의 위험을 막고 명시적인 참조관계를 가짐  
* 프로젝트/클래스 네이밍으로 직관적인 파악 가능
* 계층별 목적(모델링, 해석, 설계, ... 등)을 명확히 하여 관리 용이

### 참고
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


### 구성
제일 바깥 쪽은 크게 5가지로 나누어진다.
1. Core
2. CIM
3. Component
4. Extra
5. Global Setting Files

### 세부 구성

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
