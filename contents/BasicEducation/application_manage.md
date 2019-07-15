# 응용 프로그램 파일 관리

## 응용프로그램의 설정 파일 등의 저장 위치

### AppData 폴더 

> AppData 폴더에는 일반적으로 앱 설정, 파일 및 PC의 앱 관련 데이터가 포함되어 있다. 3개의 폴더로 구성되어 있는데 `Local`, `Roaming`, `LocalLow`로 구성되어 있다. (C:\Users\사용자이름\AppData)

* `Local` : 단일 컴퓨터의 특정한 데이터가 저장된다. 도메인간에 로그인하더라도 컴퓨터간에 절대 동기화 되지 않으며, 이 곳에는 일반적으로 고유해야 하는 데이터나 매우 큰 파일들이 포함된다. 각 사용자별 설정을 저장하는데 용이하다. (`C:\Users\사용자이름\AppData\Local`)
* `Roaming` : PC가 로밍 프로필이 있는 도메인에 연결되어 있는 경우(회사, 공공기간처럼 제공되는 도메인에 가입된 컴퓨터) 컴퓨터와 컴퓨터간에 사용자 계정으로 로밍 할 수 있는 데이터가 존재하게 된다. 구글, 파이어폭스 등과 같이 계정에 연결하여 관련 설정을 받아와 이 곳에 저장하여 북마크 및 기타 검색 데이터가 PC에서 PC로 이동하도록 한다. 
* (`C:\Users\사용자이름\AppData\Roaming`)
* `LocalLow` : `Local`폴더와 동일, 제한된 보안 설정으로 실행되는 “low intergrity” 응용 프로그램을 위해 설계되었다. 예로 보호 모드로 실행되는 경우 `LocalLow` 폴더에만 접근할수 있다. 
* (`C:\Users\사용자이름\AppData\LocalLow`)
* 파일 저장 시 `Local`, `Roaming` 여부는 프로그램 개발자에게 달려있다.
 
### Temp 폴더
> Temp 폴더는 UAC(사용자 계정 컨트롤)에 관계없이 파일을 쓰거나 변경이 가능 한 곳이다. (`C:\Users\사용자이름\AppData\Local\Temp`)
 
### 내 문서
> Personal 폴더이기 때문에 사용자 개별 관리에 적합하다. 내 문서에 위치한 폴더 및 파일들은 읽기/쓰기 모두 가능하며 필요에 따라 외부에 연동이 가능하다. 사용자가 쉽게 인지 할 수 있다는 것을 잊지 말아야 한다. (`C:\Users\사용자이름\Documents`)


<br/><br/>

## 프로그램 구동에 필요한 파일(폴더)의 구분

### 목적
**각 파일(폴더)를 올바르게 관리하고자 하는 구분 요소**

제목 | 목적
---------|----------
 임시파일(폴더) | 프로그램에서 일시적으로 대용량 데이터/이미지를 저장할 버퍼 , Cache와 같은 역할 
 기본 설정 파일(폴더) | 프로그램에서 폰트, 색상 등 프로그램의 기본적인 것들을 저장 
 사용자 설정 파일(폴더) | 프로그램에서 폰트, 색상 등 프로그램의 기본적인 것들을 사용자 별로 개별 설정 후  저장 
 복구 파일(폴더) | 프로그램이 강제 종료되었을 때 작업하던 상황을 저장하기 위해 <br/> 원본 데이터가 파손되었을 때 대체용(원본 데이터 저장시 생성) 
 공유 파일(폴더) | 2개 이상의 프로그램이 폴더(파일)을 공유하여 작업을 진행해야 하는 경우
 템플릿 파일(폴더) | 프로그램에 사용되는 템플릿들을 관리하기 쉽게 정리하기 위해


<br/>

### 사용자 관리

제목 | 사용자 구분
---------|----------
 임시파일(폴더) | 모든 사용자 동일하게 관리
 기본 설정 파일(폴더) | 모든 사용자 동일하게 관리 <br/> (하지만 사용자 설정 파일이 따로 생성되지 않고 기본 설정 파일에 덮어쓴다면 사용자 설정 파일과 동일하게 적용해야 할지는 고민해봐야 한다.)
 사용자 설정 파일(폴더) | 개별 사용자 관리
 복구 파일(폴더) | 모든 사용자 동일하게 관리
 공유 파일(폴더) | 개별 사용자 관리
 템플릿 파일(폴더) | 모든 사용자 동일하게 관리


<br/>

### 인스톨 시 배포 / 재시작시 필요 여부

제목 | 사용자 구분
---------|----------
 임시파일(폴더) | X / X
 기본 설정 파일(폴더) | X / X
 사용자 설정 파일(폴더) | X / O
 복구 파일(폴더) | X / 때에 따라 필요
 공유 파일(폴더) | O / X
 템플릿 파일(폴더) | O / O


<br/>

### 각 파일(폴더)별 예시 및 저장 위치

제목 | 예시 / 저장 위치
---------|----------
 임시파일(폴더) | (예시) <br/> Project의 계산서 emf 파일  <br/><br/> (위치) <br/> `C:\Users\사용자이름\AppData\Local\Temp\Company\프로그램이름`
 기본 설정 파일(폴더) | (예시) <br/> Project 기본 설정 파일 <br/> `BDSCOLOR_BBK.color, BDSCOLOR_WBK.color` <br/><br/>(위치) <br/> 프로그램 폴더 내에 위치
 사용자 설정 파일(폴더) | (예시) <br/>Project의 사용자 설정 파일 <br/><br/>(파일명) <br/> `*.color`<br/><br/>(위치)<br/>`C:\Users\사용자이름\Documents\Company\프로그램이름\(파일명).color`
 복구 파일(폴더) | (예시) <br/>Project 프로그램 원본 복사 모델 저장<br/><br/>(위치) <br/>강제 종료시 : 해당 모델이 존재하는 폴더 <br/>원본 데이터 저장시 백업 파일 :<br/>`C:\Users\사용자이름\Documents\Company\프로그램이름`<br/><br/>(예시)<br/>`Ms Office 백업 파일`<br/><br/>(위치)<br/>`C:\Users\사용자이름\AppData\Local\Microsoft`
 공유 파일(폴더) | (예시) <br/>`Project Grid.txt`<br/><br/>(위치)<br/>`C:\Users\사용자이름\Documents\Company\프로그램이름`
 템플릿 파일(폴더) | 프로그램 폴더 내에 위치

### Project 프로그램 폴더 관리 현재상황
Project의 경우 사용자 설정 파일이 내 문서에 관리되고 있다. <br/>
내 문서의 특성 상 사용자가 쉽게 인지가 가능하여 수정 및 삭제가 용이하다. <br/>
따라서 자신에게 맞게 수정한 사용자 설정 파일을 삭제하여 데이터를 손실하거나 프로그램 외부에서 수정하여 데이터를 파손시켜 프로그램 구동 오류에 원인이 될 수 있다. <br/> 결과적으로는 C:\Users\lbh0731\AppData\Local\Company과 같이 사용자의 접근이 비교적 불편한 Local 폴더 내로 위치하는 것이 바람직하다고 생각한다. <br/> <br/>

복구 파일은 MS Office의 경우 복구 매니저가 있기 때문에 Local 폴더에 저장하는 것이 바람직하다. <br/>
하지만 Project은 복구매니저가 존재하지 않아 Local에 저장하는 것은 효율적이지 않다. <br/>
상황에 따라 저장되는 위치가 현재 나뉘어 지는데 이를 내 문서로 통합하는 것이 효율적이라고 생각한다.


<br/>

### 윈도우의 각종 폴더 경로를 얻는 함수 (`::SHGetKnownFolderPath`)

~~~c
SHGetKnownFolderPath(
REFKNOWNFOLDERID rfid,
DWORD            dwFlags,
HANDLE           hToken,
PWSTR            *ppszPath
)
~~~

rfid 값이 얻고자 하는 폴더를 뜻한다.
예시)

~~~c
FOLDERID_Documents : 내 문서
FOLDERID_LocalAppData : Local 폴더
FOLDERID_LocalAppDataLow : LocalLow 폴더
FOLDERID_Desktop : 바탕화면
~~~

~~~c
#include <stdio.h>
#include <tchar.h>
#include <ShlObj.h>
  
int _tmain(int argc, _TCHAR* argv[])
{
    LPWSTR path;
    SHGetKnownFolderPath(FOLDERID_Documents, 0, NULL, &path);
    wprintf(L"%s\n", path);
    CoTaskMemFree(path);
    return 0;
}
~~~

자세한 사항은 아래 링크 참조

* <https://docs.microsoft.com/ko-kr/windows/desktop/api/shlobj_core/nf-shlobj_core-shgetknownfolderpath>


<br/>

### 결론
* 앞으로 목적에 맞게 파일(폴더)를 구분 하고 통일된 위치에 저장.
* 템플릿, 공유 파일은 쓰임새에 맞게 설정 파일, 임시 파일 등이 될 수 있다.
* 아래는 파일(폴더) 별 저장 위치이다.

#### 임시 파일(폴더)
`C:\Users\사용자이름\AppData\Local\Temp\Company\프로그램이름`

#### 기본 설정 파일(폴더)
프로그램 폴더 내 위치(수정 가능 할 시 사용자 설정 파일과 동일하게 처리 할 지에 대해서는 고민 필요)

#### 사용자 설정 파일(폴더)
`C:\Users\사용자이름\AppData\Local\Company\프로그램이름\Setting`

#### 복구파일(폴더)
복구매니저가 있을 시 <br/>
`C:\Users\사용자이름\AppData\Local\Company\프로그램이름\Restore`

#### 복구매니저가 없을 시
`C:\Users\사용자이름\Documents\Company\Project_JP`
