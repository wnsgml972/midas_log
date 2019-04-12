# Alice UI와 그 역사(?)

# GLUI v1 (내용 by. hsjo)

## GLUI ?

* 화면 상에서 그려지는 OpenGL로 그리는 UI들의 명칭 이었다가 GLUI라는 이름이 되어버림.
* 원래는 View 상에서 UI를 직접 그려줘서 마우스 동선 최소화, Direct Modeling을 위해 만든 UI. 
* 최대한 간단한 기능 정보만 보여주는 것이 좋음. 복잡한 기능은 대화상자에 다 밀어 넣는 것으로

## GLUI의 구성

* `MGLUIHandler` : UI의 생성 삭제 이벤트 관리 등등 잡다한 것을 다 처리하는 매우 복잡한 기능
* `MGLUI` : GLUI의 기본 베이스 클래스. 여기서 UI의 Data를 다루도록 한다. (Doc 역할)
* `MViewActor` : GLUI의 베이스 클래스. `MGLUI`와 묶여서 같이 움직이며 주로 View에 대한 이벤트 처리를 함. (View 역할)
* `MGLUINotifier` : 실체 View(`CView`)에서 `MViewActor`로 View 이벤트를 전달하는 클래스. (View의 `OnCreate`에서 `AttachNotifier`, `OnDestroy`에서 `DestroyNotifier` 불러서 연결)
* `MIDGLUIRndrB_*` : 각각의 GLUI에 대한 렌더링 코드가 담긴 곳. GLUI 타입별로 나눠져있음.
* `MGLUIHelper` : GLUI를 다룰 때 유용한 함수를 묶은 덩어리. 원래 Handle 단위로 묶어서 다루려고 했다가 나온 결과물... (Object 단위로 바꿔야할 필요가 있음)
* `MGLUICommandBase` : UI에서 각각의 Command를 사용할 필요가 있을 때 사용하는 클래스. 
* `MGLUIBaseDef.h` : 기본적으로 필요한 Define들을 몽땅 담고 있은 곳.

## View 이벤트와 GLUI 연결 방법

* `MGLUINotifier`를 사용해서 `MGLUIHandler`의 `HandleViewEvents`를 통해 전달이 되도록 설계됨
* `XGViewBase`에서 `SEND_GLUI_NOTIFY` 매크로를 사용해서 각각의 `GLUIHandler`로 전달함
* 이벤트를 받아서 처리할 기능이 필요하면 `ViewActor`에서 처리한 후에 Event 내용을 변경 후 반환하는 방식 (NOTIFY_* 로 들어간 후 반환된 파라미터가 NOTIFY_GLUI*로 들어가는 방식)
* 마우스 이벤트 처리 순서는 Focused GLUI -> 마우스 Hit된 GLUI -> Thief GLUI 순서로 진행
* 키보드 이벤트 처리 순서는 Focused GLUI -> Edit Focus GLUI 순서로 진행
* Focus는 주로 바로 직전에 클릭했던 GLUI이지만, 예외 사항이 있음 (`MViewActor`의 LButtonDown -> LButtonUp 에서 `SetFocus`를 사용하여 처리됨)
* Thief GLUI는 Focus도 아니고 Hit된 것도 아니지만 마우스 이벤트를 받아야하는 것들. Grip GLUI ? 만 사용하는 중

## UI와 명령 연결 방법 (`MGLUICommandBase` 사용법)

* 원래는 GLUI가 처리하는 이벤트는 Opn에서 처리하고 있었음 (`OpnView`의 `OnGLUI*` 계열 함수들)
* 일부 Command만 GLUI에 할당해주면 GLUI가 내부적으로 처리하는 인터페이스
* 진행하다 말은 상태

## GLUI Rendering

* MFC와 3D OpenGL 2가지로 나눠져있음
* 3D 공간상에서 붙어있는 기능은 OpenGL을 사용함 (Widget, Grip, Dimension 등)
* MFC는 윈도우상에서 제공해야하는 기능들 (`DynCtrl`, `ToolbarCtrl` 등)
* GLUI의 종류에 따라 Rendering 상태를 바꾸고 있음
* 엄청 느려진다면 속도 개선 필요 -> 예) 엄청 많은 Dimension

## UISet

* 중립모드에서 나타나야하는 UI들로 주로 Grip 에서 사용

## GLUI 목록

* `DynCtrl` `ToolBarCtrl` : Button, Edit, Combo 등 Mini Toolbar 계열들이 여기에 속함
* `Widget` : 화살표 등 Move, Rotate에서 사용하는 것들
* `Legend` : 후처리에서 사용하는 기능
* `Grip` : 1D/2D를 움직이는데 사용하는 UI들
* `Dimension` : Line이나 Measure에서 사용하는 기능
* `Bubble` : Grid에 붙어서 텍스트를 출력하는 부분
* `TextEditor` : 폐기된거라 신경 안쓰면 됨

## 새 GLUI 만드는 법

1. `MGLUI` `MViewActor`를 상속받아서 새로운 class를 만듦
2. Renderer 구현
3. GLUI에 해당 Type을 새로 추가
4. 각종 이벤트 `ViewActor` `Command`등을 구현하여 사용

## TODO

* 공통 기능 묶기, Dependency 줄이기
* `GLUIHelper` 삭제 필요
* 현재 GLUI의 생성을 Handler에서 Runtime Class를 바탕으로 생성하는데, Factory나 Template 등으로 좀 더 간단한 구조 필요
* `Command`를 종류별로 다 만들어야하는 문제가 있어서 좀 더 간단하게 생성할 방법 필요
* GLUI의 Selection 구조가 OpenGL Picking API로 구현되어 있음. Color Code 방식으로 개편 필요
* `Dimension`이 DB를 쓰는 것과 안 쓰는 것이 구분되어 있음. 하나로 합친 후 DB를 사용하는 경우 Command 등으로 처리할 필요가 있음
* `Widget`은 다 고쳐야함
* UI를 드래그하는 상황에서 3D OpenGL로 그린 영역과 MFC로 그린 영역이 쪼개짐. 수정 필요

## 기타

* 아이콘 리소스 관리 : Texture 관리하는 문제. 데이터 로드 & 그리기 등 문제
* Handle 기반 -> Object 기반으로 해야할듯

# GLUI v2

## Intro

* nGen 출시쯤 Frame 형태로 GLUI를 개편하는 작업이 진행됨

![image](/media/aui/alice-ui0.png) 

* 당시 개발중인 Alice UI(nGLUI)의 일부 코드와 컨셉을 가져와서 GLUI v1을 개선한 형태 - 현재 nGen에서 사용중

## 변경된 사항

* 기존 Opn에서 `GLUIHelper`를 사용해서 개발자가 직접 UI를 생성해주는 방식에서 `FrameUI`라는 모듈형 방식으로 개선됨
* `FrameUI`에서 UI 형태 구성 & 이벤트 처리 코드를 구현하면 자동으로 개편된 형태의 UI가 나오는 방식
* 테두리를 렌더링하기 위해 Nine-Patch 렌더링을 구현 -> Predefine된 크기에 대한 이미지만 사용 가능함

## 주요 기능

* `Frame` : 현 AliceUI의 LinearLayout과 비슷한 개념으로 각종 UI들을 한줄 형태로 누적하는 기능을 가짐. `Frame`에 `Frame`를 중첨해서 사용도 가능함 -> 세로로 배치되는 형태의 UI 구현
* `FrameHolder` : `Frame`들의 가장 Root가 되는 곳으로 화면상에 특정 위치를 고정시키는 `Anchor` 기능을 제공함
* `Anchor`는 화면의 '좌상단/상단중심/우상단/좌하단/하단중심/우하단/자유'을 기준으로 고정을 시킬 수 있는 기능. 주로 화면 하단 중심에 고정시키기 위해서 사용됨
* `Frame`/`FrameHolder`에서 offset 값이 AliceUI의 Margin 값과 비슷하게 동작함
* `FrameBuilder` `FrameManual` : 미리 정의한 UI들을 상황에 따라 위치를 변경하여 자동으로 재조립시키기 위한 기능. 현재 midasCIM의 `PanelUI`의 기원이된 코드
* `MUISignal` `MUISlot` : 초기 AliceUI에서 사용했던 Signal/Slot 코드. 현재와 달리 Connect/Disconnect를 수동으로 해주지 않으면 Crash가 발생하고 Signal ID값을 통해서 관리됨

## 해결하지 못한 부분

* `ToolBar` 계열 : `Frame`과 비슷한 컨셉을 사용하였지만 대부분의 코드는 사라지고 1~2개의 Legacy 코드만 남겨져 있어서 남아있는 상태
* `Grip` 계열 : 1~2개 기능만 남아있는 상태

# Alice UI (nGLUI, GLUI v3)

## [기본 설명 발표 자료](https://docs.google.com/presentation/d/1xjVN31jFQlz6NajTDVCkhelPd1uh0j_T_RauRCoSUrY/edit?usp=sharing)

## 기원

### GLUI v1에 대한 개선 방안 고안

* GLUI v1의 약점인 Canvas Draw 기능에 대한 사전 조사 작업을 진행함
  * OpenGL의 Triangulation을 기반으로한 렌더링은 너무 Low-Level한 기능만 제공하고 있음
  * 실제 2D UI를 렌더링할 때 사용하는 API들을 보면 GDI, Javascript 등이 있는데 이들은 OpenGL보다 High-Level Feature를 제공하고 있음
  * Android OS의 경우 2D 렌더링 코어를 Skia라는 오픈소스에 기반하고 있음
* 2D 렌더링 라이브러리 조사
  * Skia : Google에서 개발한 렌더링 라이브러리. BSD 라이센스로 상용 프로그램에서 사용함에 무리가 없으며 Chrome Browser, Android OS 등 다양한 프로그램에서 검증된 라이브러리.
  * Cairo : XWindow 시스템의 기본 렌더링 라이브러리. WebKit 기반의 브라우저에서 기본으로 사용하던 렌더링 라이브러리.
  * GDI / GDI+ : Microsoft에서 개발한 렌더링 라이브러리. Windows에서 기본으로 사용하고 있다.
  * Direct2D : Microsoft에서 개발한 렌더링 라이브러리. GDI를 대체하기 위해 개발되었으며 Windows Vista 이상 DirectX 10 이상의 요구사항이 필요하다.
  * QPainter : Qt Framework의 2D 렌더링 라이브러리. 주로 GDI, Cairo 등을 Wrapping한 기능을 제공하는듯 하다.
  * CoreGraphics / Quartz : macOS의 렌더링 라이브러리. 
* Handler를 사용한 방식이 아닌 OOP 구조를 활용한 객체 단위의 묶음으로 개발이 가능하도록 개선
* GLUI v1에서는 UI의 Type에 따라 렌더링 순서가 정해져 있는 문제
  * 실제 UI는 Tree의 상하관계에 따라 렌더링 순서가 정해져야함
  * 새로운 Type이 생기면 렌더링 순서를 잘 맞춰주지 않으면 이상하게 나타남
* GLUI v1에서는 CEdit과 같은 MFC와 GLUI 객체가 공존하는 문제
  * 두가지가 동시에 Focus를 받거나, 가상으로 Focus를 처리하는 편법 등에 의한 버그가 많았음
* GLUI v1에서는 UI 의 이벤트 처리를 밖에서 처리하기 힘든 구조
  * `CommandBase`를 활용할 수는 있지만, 리소스 관리 등의 문제로 복잡하긴 마찬가지였음
  * 범용 이벤트 처리기를 사용하여 비즈니스로직을 밖에서 접근시킬 방법이 필요
* UI의 애니메이션 처리가 어려움
  * 2D 엔진을 사용하여 렌더링 갱신을 조절하면 가능해보임
* GLUI v1에서 GLUI의 불명확한 생명주기
  * 객체 단위로 움직일 수 있고, 개발자가 생명주기를 관리할 수 있도록 개선 필요

### Skia

* Skia Inc. 라는 회사에서 개발하여 2005년 Google이 인수함
* 주요 기능
  * Open Source 2D Vector Graphics Engine
  * Front End와 Back End가 구분되어 있어 엔진 부분과 개발 인터페이스 부분이 나눠져있음
  * Front End
    * Raster
    * SVG
    * PDF
    * PostScript
  * Back End
    * CPU Software Rasterization (SIMD Optimized)
    * PDF
    * GPU Accelerated Rasterization (OpenGL, Vulkan)
  * 그리는 행위/명령을 `*.skp`라는 파일 형태로 export가 가능함
* 주요 사용처
  * Chrome, Firefox 브라우저 렌더링 엔진
  * Android OS의 시스템 렌더링 엔진
* 라이선스 : New BSD 라이선스로 상용 프로그램에서 문제가 없음


# Alice UI v2.0 (with Mosaic)

## 개발 동기

* 타겟 사용자의 변화
  * 초기 Alice UI는 개발자가 직접 Widget을 상속받아 구현하는 컨셉이었지만, 현재 CIM에서는 PanelUI에서 자동 생성시키는 모듈로 용도가 변경됨.
  * 지금은 비개발자가 Panel UI에 각종 Constraint와 Style Sheet 정보로 UI를 생성하는 방식으로 사용함.
  * 주요 사용자가 비개발자이다보니 복잡한 Constraint를 구현할 필요가 생김. (예: 가변 크기에서 중앙 정렬, A 객체 왼쪽으로 n 픽셀만큼의 위치 등)
* v1.0의 Widget 구조적 한계
  * Widget의 생성 구조 특성상 자신의 영역 Boundary를 벗어날 수 없는 문제.<br>
    아래와 같은 Slider Widget에서 슬라이드시 나타나는 Thumb을 표기하기 위해 Thumb를 포함하는 Bounding Box로 Widget이 생성되어야만 함<br>
    ![image](/media/aui/alice-ui1.png) ![image](/media/aui/alice-ui2.png) 