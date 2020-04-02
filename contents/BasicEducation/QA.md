# QA

## [5 Questions Every Unit Test Must Answer](https://medium.com/javascript-scene/what-every-unit-test-needs-f6cd34d9836d)

대부분의 개발자들은 단위 테스트를 작성해야 하는 사실은 잘 알고 있지만, 많은 경우 단위 테스트의 필수 요소들은 무엇인지 잘 알지 못한다는 점이다. 해당 글에서는 저자 자신의 실제 경험을 기반으로 실패하지 않는 좋은 테스트를 작성하기 위해 스스로 질문해 봐야 하는 다음의 5가지 항목들을 소개한다.

* What are you testing? (무엇을 테스트 했습니까?)
* What should it do? (그것은 무엇을 합니까?)
* What is the actual output? (결과는 무엇입니까?)
* What is the expected output? (예상되는 결과는 무엇입니까?)
* How can the test be reproduced? (어떻게 테스트를 재현할 수 있습니까?)

### 단위 테스트 케이스의 명확한 장점

테스트는 소프트웨어 결함에 대한 최초의, 최상의 방어선입니다. 정적 분석 (실제 프로그램 로직의 문제가 아닌 하위 클래스만 찾을 수 있음) 보다 테스트가 더 중요합니다.

* **디자인 지원** : 테스트 작성은 먼저 이상적인 API 디자인에 대한 명확한 관점을 제공합니다.
* **기능 문서 (개발자 용)** : 구현 된 기능 요구 사항마다 코드에 테스트 설명이 포함되어 있습니다.
* **개발자의 이해도 테스트** : 개발자 는 모든 중요한 구성 요소 요구 사항을 코드로 표현할 수있을 정도로 문제를 이해하고 있습니까?
* **품질 보증** : 수동 QA는 오류가 발생하기 쉽습니다. 내 경험상 개발자가 리팩터링, 새 기능 추가 또는 기능 제거로 변경 한 후 테스트가 필요한 모든 기능을 기억하는 것은 불가능합니다.
* **지속적인 납품 지원** : 자동화 된 QA를 통해 파손 된 빌드가 생산에 배포되는 것을 자동으로 방지 할 수 있습니다.

### 그래서 좋은 테스트란?

* **설계 지원** : 구현 전에 설계 단계에서 작성 합니다. (TDD.. 잘 모르겠음, 팀 상황에 따라, 하지만 대부분의 모든 구현에는 테스트가 존재해야 한다고 생각) 
* **기능 문서 및 개발자 이해 테스트** : 테스트는 테스트 중인 기능에 대한 명확한 설명을 제공해야합니다. (더 깔끔해야 하고, 누군가에게 가이드 코드가 될 수 있어야 하고)
* **QA / 연속 전달** : 테스트는 실패시 전달 파이프 라인을 중단하고 실패시 양호한 버그 보고서를 작성해야합니다. (너무 당연한 소리)


### Good Failer Report Example

~~~cpp
/*
title: `abc` should return a function
    ---
      operator: equal / ASSERT_TRUE
      expected: `function`
      actual: `object`
      at: Test.<anonymous> (/path/path)
    ...
*/
~~~

### Reference

* <https://github.com/naver/fe-news/blob/master/issues/2020-04.md>
* <https://medium.com/javascript-scene/what-every-unit-test-needs-f6cd34d9836d>



<br/><br/>

## 실무 테스트 Log

### 한 프로젝트 전체 통합 테스트를 설계할 경우