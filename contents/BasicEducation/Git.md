# Git Branch 전략

## Git Flow 그림



<br/>

## 각 Branch merge시 Code Quality를 위한 CI 연동사항

크게 3가지가 존재한다.

* 정적분석
* 리그레션 (단위 테스트)
  * Lev1 : 엔진
  * Lev2 : 기능
  * Lev3 : Full Test
* 코드리뷰


<br/>

## 각 Branch 설명

### 상위 branch

#### master

* 고객 브랜치
* 실제 제품 출시 버전
* 고객의 요구사항을 반영하기 위한 `hotfix` 브랜치는 여기서 생성 후 병합
* 팀장 급이 merge를 진행
* master 브랜치에 merge 되는 경우 정적분석, 리그레션 Lev3, 코드리뷰를 진행한다.

#### release

* 기획자 브랜치
* 현 버전에 제품에 들어갈 기능에 대한 첫 기획의 완성
* dev 브랜치에서 파트장 or 팀장이 현 기능의 완성을 release 브랜치에 merge 
* merge된 진행 사항은 기획자에게 테스트와 기능 확인을 요구
* 이후 발생하는 추가 요구사항 및 버그는 release 브랜치에서 새로운 Issue 브랜치를 이용하여 개발 완료 후 지속적인 merge
* release 브랜치에 merge되는 경우 정적분석, 리그레션 Lev3, 코드리뷰를 진행한다.


#### dev

* 개발자 브랜치
* 현재 개발되는 모든 기능을 여기서 merge하며 진행한다.
* 현 버전에 필요한 기능은 추 후에 release 브랜치에 merge 될 것이고,
* 그렇지 않은 부분은 dev 브랜치에서 계속 진행한다.
* dev에서 개발을 진행하는 것이 아니라, dev의 하위 파트 별(기능 별) 브랜치에서 개발을 각자 자유롭게 진행하다가, dev에 파트장 급이 merge를 진행한다.
* dev 브랜치에 merge되는 경우 정적분석, 리그레션 Lev1 or Lev2 를 진행한다.



<br/>

### 하위 branch

#### dev/X
* 각 파트 브랜치
* dev/A, dev/B, dev/C 방식이 존재한다.
* dev/X 브랜치에 merge 되는 경우 정적분석을 진행한다.
* dev/X에서 dev에 merge 시에 각 다른 파트 별 merge를 진행해야 하는 경우가 생긴다. 이는 파트 장 급이 담당하는데 자기가 만든 로직이 아니므로 해당 담당자를 불러 충돌을 해결해야 하는 문제가 있다.  ->   이를 해결하기 위해 겹칠 것 같은 부분은 미리 모두에게 공지하도록 한다.

#### dev/X/1476-ifc
* 각 개발자 개인의 자유로운 브랜치
* 어떤 merge request나 pull request의 방식이 아닌, 각자 자유롭게 Commit을 관리하며 각자 branch의 개발을 진행한다.
* 그냥 개발하며 다음과 같은 프로세스를 지키며 된다.
   1. Commit
   2. Pull dev/X
   3. Push dev/X
* dev/X 와의 충돌은 각자의 책임이 된다.

#### master/hotfix
* 고객의 요구사항에 의해 발생한 긴급 브랜치
* 출시했는데 추후에 발생한 심각한 문제를 빠르게 해결하는 브랜치
* release에서 발생하는 테스트 후 발생되는 버그는 hotfix가 아니라, Issue에 대한 fix 사항이라는 것만 구별한다.


<br/><br/><hr/>

## Git Scenario1

### Overview
[![Video Label](http://img.youtube.com/vi/hKJ7TBd7G9E/0.jpg)](https://www.youtube.com/watch?v=hKJ7TBd7G9E)

### Git Code
* [Git Scen1](/contents/BasicEducation/GitScen1.md)





<br/><br/>

## 참고

* <https://learngitbranching.js.org/?demo>
* <https://github.com/wnsgml972/learnGitBranching>
