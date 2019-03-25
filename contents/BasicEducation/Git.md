# Git Branch 전략

## Git Flow 그림

![workflow](/media/cim_workflow.png)


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

> 전체 프로젝트가 하나의 Git Repository로 관리 될 때, CI의 연동을 통해 dev branch에 merge한 내용은 모든 코드가 Git에 올라가며, release branch에 merge한 내용은 따로 자동 Build 후, 실행 파일만 올라간다.

#### master

* 고객 브랜치
* 실제 제품 출시 버전
* 고객의 요구사항을 반영하기 위한 `hotfix` 브랜치는 여기서 생성 후 병합
* 팀장 급이 `merge`를 진행
* master 브랜치에 `merge` 되는 경우 정적분석, 리그레션 Lev3, 코드리뷰를 진행한다.

#### release

* 기획자 브랜치
* 현 버전에 제품에 들어갈 기능에 대한 첫 기획의 완성
* dev 브랜치에서 파트장 or 팀장이 현 기능의 완성을 `release` 브랜치에 `merge` 
* `merge`된 진행 사항은 기획자에게 테스트와 기능 확인을 요구
* 이후 발생하는 추가 요구사항 및 버그는 `release` 브랜치에서 새로운 `Issue` 브랜치를 이용하여 개발 완료 후 지속적인 `merge`
* `release` 브랜치에 `merge`되는 경우 정적분석, 리그레션 Lev3, 코드리뷰를 진행한다.


#### dev

* 개발자 브랜치
* 현재 개발되는 모든 기능을 여기서 `merge`하며 진행한다.
* 현 버전에 필요한 기능은 추 후에 `release` 브랜치에 `merge` 될 것이고,
* 그렇지 않은 부분은 `dev` 브랜치에서 계속 진행한다.
* `dev`에서 개발을 진행하는 것이 아니라, `dev`의 하위 파트 별(기능 별) 브랜치에서 개발을 각자 자유롭게 진행하다가, `dev`에 파트장 급이 `merge`를 진행한다.
* `dev` 브랜치에 merge되는 경우 정적분석, 리그레션 Lev1 or Lev2 를 진행한다.



<br/>

### 하위 branch

#### dev/X
* 각 파트 브랜치
* `dev/A`, `dev/B`, `dev/C` 방식이 존재한다.
* `dev/X` 브랜치에 `merge` 되는 경우 정적분석을 진행한다.
* `dev/X`에서 `dev`에 `merge` 시에 각 다른 파트 별 `merge`를 진행해야 하는 경우가 생긴다. 이는 파트 장 급이 담당하는데 자기가 만든 로직이 아니므로 해당 담당자를 불러 충돌을 해결해야 하는 문제가 있다.  ->   이를 해결하기 위해 겹칠 것 같은 부분은 미리 모두에게 공지하도록 한다.

#### dev/X/1476-ifc
* 각 개발자 개인의 자유로운 브랜치
* 어떤 `merge request`나 `pull request`의 방식이 아닌, 각자 자유롭게 C
`commit`을 관리하며 각자 branch의 개발을 진행한다.
* 그냥 개발하며 다음과 같은 프로세스를 지키며 된다.
   1. `commit`
   2. `pull dev/X`
   3. `push dev/X`
* `dev/X` 와의 충돌은 각자의 책임이 된다.

#### master/hotfix
* 고객의 요구사항에 의해 발생한 긴급 브랜치
* 출시했는데 추후에 발생한 심각한 문제를 빠르게 해결하는 브랜치
* `release`에서 발생하는 테스트 후 발생되는 버그는 `hotfix`가 아니라, 한 `Issue`에 대한 fix 사항이라는 것만 구별한다. 굳이 말하자면 `bugfix`?





<br/><br/><hr/>

## Git Scenario1

### Overview
[![Video Label](http://img.youtube.com/vi/hKJ7TBd7G9E/0.jpg)](https://www.youtube.com/watch?v=hKJ7TBd7G9E)

### Git Code
* [Git Scen1](/contents/BasicEducation/h_GitScen1.md)
* <https://github.com/wnsgml972/learnGitBranching>





<br/><br/>

## Github Project Management

* Github 기반 프로젝트 관리법 정리!
* <https://github.com/cheese10yun/github-project-management>
* <https://developer.github.com/>

Work | Name 
---------|----------
 Version Control | [Git](/contents/BasicEducation/Git.md)
 Communication Tool | [Slack](https://slack.com/), [Mattermost](https://mattermost.com/) : Linked Github 
 CI/CD | [Travis CI](https://travis-ci.org/), [Coveralls](https://coveralls.io/)
 Issue Tracking, Planning | [ZenHub](https://www.zenhub.com/)
 Docs Cooperation | Wiki
 Static Analysis | [Sonar Qube](https://www.sonarqube.org/)
 Code Review | Github pull request





<br/><br/>

## 참고

* <https://learngitbranching.js.org/?demo>\
* [우아한 형제들 : Git Flow 도입](http://woowabros.github.io/experience/2017/10/30/baemin-mobile-git-branch-strategy.html)

