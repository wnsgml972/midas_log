## go home! [:house_with_garden:](https://github.com/wnsgml972/midas_log)

<br/><hr/>

# Commit Message Rule

## 왜 좋게 작성해야 할까?

* 더 좋은 커밋 로그 가독성
* 더 나은 협업과 리뷰 프로세스
* 더 쉬운 코드 유지보수

## 좋은 커밋 메시지를 위한 8가지 약속

1. 제목과 본문을 한 줄 띄워 분리하기
2. 제목은 영문 기준 50자 이내로
3. 제목 첫글자를 대문자로
4. 제목 끝에 `.` 금지
5. 제목은 `명령조`로
6. Github - 제목(이나 본문)에 이슈 번호 붙이기
7. 본문은 영문 기준 72자마다 줄 바꾸기
8. 본문은 `어떻게`보다 `무엇을`, `왜`에 맞춰 작성하기

## 예제

* 제목은 명령문, (대문자)
* 본문은 평서문  (소문자)  -> 왜 했는지?

### 제목 예제

제목을 작성할 때 다음 예문을 통해 문장이 어울리나 확인하면, 그 문장이 올바른 명령문이 되었나 확인할 수 있다.

`If applied, this commit will {제목}`

~~~
Refactor subsystem X for readability
Test company class and its public methods
Update getting started documentation
Fix occasionally test failure
Remove deprecated methods
Release version 1.0.0
Merge pull request #123 from user/branch
~~~

### Issue 자동 종료 시키기

* close는 일반 계열 Issue
* fix는 hotfix Issue
* resolve는 문의나 요청 사항에 대한 Issue

~~~
close #1336
fix #1336
resolve #1336
~~~

### 기획자에게 참고 날리기

* ref를 걸어 Issue에 알림을 넣을 수 있다.

~~~
ref #1476
~~~



<br/><br/>

## Commit Message Convention

커밋 메시지를 작성할 때는 원칙을 정하고 일관성 있게 작성해야 한다.

### Structure

~~~
Type : Title

Body

Footer
~~~

### Commit Type
* `feat` : 새로운 기능 추가
* `remove` : 기능 삭제
* `release` : 제품 출시
* `fix` : 버그 수정
* `docs` : 문서 수정
* `style` : 코드 포맷팅, 세미콜론 누락, 코드 변경이 없는 경우
* `refactor` : 코드 리펙토링
* `test` : 테스트 코드, 리펙토링 테스트 코드 추가
* `chore` : 빌드 업무 수정, 패키지 매니저 수정

### Title
* 명령문으로 작성한다.
* 50자를 넘기지 않는다.

~~~
Refactor subsystem X for readability
Test company class and its public methods
Update getting started documentation
Fix occasionally test failure
Remove deprecated methods
Release version 1.0.0
Merge pull request #123 from user/branch
~~~

### Body
* 선택사항이기 때문에 모든 커밋에 본문내용을 작성할 필요는 없다.
* 부연설명이 필요하거나 커밋의 이유를 설명할 경우 작성해준다.
* 72자를 넘기지 않고, 제목과 구분되기 위해 한칸을 띄워 작성한다.

### Footer
* 선택사항이기 때문에 모든 커밋에 꼬리말을 작성할 필요는 없다.
* issue tracker id를 작성할 때 사용한다.

<br/>

### Example

~~~
feat: Summarize changes in around 50 characters or less

More detailed explanatory text, if necessary. Wrap it to about 72
characters or so. In some contexts, the first line is treated as the
subject of the commit and the rest of the text as the body. The
blank line separating the summary from the body is critical (unless
you omit the body entirely); various tools like `log`, `shortlog`
and `rebase` can get confused if you run the two together.

Explain the problem that this commit is solving. Focus on why you
are making this change as opposed to how (the code explains that).
Are there side effects or other unintuitive consequenses of this
change? Here's the place to explain them.

Further paragraphs come after blank lines.

 - Bullet points are okay, too

 - Typically a hyphen or asterisk is used for the bullet, preceded
   by a single space, with blank lines in between, but conventions
   vary here

If you use an issue tracker, put references to them at the bottom,
like this:

resolve: #123 (or ref #123)
see also: #456, #789
~~~

### Reference

* <https://github.com/googlesamples/android-architecture/issues/300#issue-213830486>