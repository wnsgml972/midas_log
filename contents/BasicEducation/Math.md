
# Programming Math

## 용어 정리

~~~
Vec1 = (a1, a2, a3)
Vec2 = (b1, b2, b3)
|Vec1| 은 Vec1의 크기를 의미
~~~

영어 | 한글 | 설명
---------|----------|----------
Vector | 벡터 | 크기와 방향을 가진 것
Scalar | 스칼라 | 크기만 가진 것
Normal Vector | 법선 벡터 | 평면에 수직인 벡터
Unit Vector | 단위 벡터 | 크기가 1인 벡터
Inverse Vector | 역벡터 | 벡터에 대해 크기는 같고 방향은 반대인 벡터
Dot | 내적 | 스칼라곱, 주로 점과 직선 사이의 거리를 구하는데 쓰임
Cross | 외적 | 텐서곱, 주로 법선 벡터를 구하는데 쓰임
Normalize | 정규화 | 0과 1이하로 데이터의 범위를 일치시키는 것 

~~~
Dot       :  Vec1·Vec2 = |Vec1|*|Vec2|*cosθ
Cross     :  |Vec1*Vec2| = |Vec1|*|Vec2|*sinθ
Cross     :  `Vec1*Vec2 = (a2b3-a3b2), a3b1-a1b3, a1b2-a2b1)`
Normalize :  벡터의 정규화는 벡터의 각 요소를 벡터의 길이로 나눔
~~~
<br/>

## 어파인 공간

**어파인 공간이란, 점을 마치 벡터처럼 취급함으로써 벡터 공간을 확장한 것이다.**

이러한 어파인 공간일 시 점 Q에서 P를 빼면 PQ-> 라는 벡터가 된다.

어파인 공간에서는 3가지 연산이 가능하다.

* 벡터와 벡터의 덧셈, 뺄셈
* 스칼라와 벡터의 곱셈, 나눗셈
* 점과 벡터의 덧셈, 뺄셈


<br/>

## vec(3, 4, 5)란?

> 고민하던 2가지가 모두 정답이라는 것을 이해하는데 쉽지 않았음.

3차원 좌표 상의 해당 위치를 뜻하는 Position으로서의 의미도 맞고, 방향을 가진 벡터의 시작점의 위치도 맞음!

예를들어 한 커브의 그래프가 있을 때 그 해당하는 커브의 3, 4, 5에 위치에서 그려지고 있는 커브는 방향이 있음 그 때의 vec(3, 4, 5)는 벡터의 시작점이자, 3차원 좌표상의 점도 맞음



<br/>

## 방향 벡터?, 점?, 그냥 벡터?

> 벡터는 크기와 방향을 가진 것이다. 즉 점과 점을 빼면 벡터가 나온다. 그 이유는 원점을 기준으로 벡터라고 정의했을 때 벡터와 벡터를 빼면 해당 그 사이의 벡터가 나옴

* 점은 그냥 3차원 좌표 상의 점이다! 
* 방향 벡터는 0, 0, 0을 기준으로 정의된 벡터이다.
* 그냥 벡터는 해당 Pos 위치에서 위의 설명한 것과 같은 벡터이다.



## 직선의 벡터 표현 (1점을 지나는 벡터)

~~~
Pos = (x1, x2, x3)
Dir-> = (u1, u2, u3)

Pos + kDir-> = Pos를 지나고 Dir과 평행한 벡터 Vec->

즉 
a-> + pB->
는 a를 지나고 B와 평행한 벡터임
~~~

### Reference

* [방향 벡터와 평행하고 점 x, y, z를 지나는 직선의 방정식](https://blog.naver.com/at3650/40203761799)



<br/>

## 공부할 때 참고한 전체 Reference

* <https://lypicfa.tistory.com/298?category=805276>



<br/>

# 실무

## CIM 좌표계

![xyz](/media/xyz.png)

<br/>

## Line2PointDistanceReference 구현

> 점과 직선이 있을 때, 해당 점을 지나고 직선의 법선벡터에서 교차점 구하기

### 알아야 할 것

* 직선의 벡터 표현
* 내적
* 어파인 공간 내에서의 벡터 기본 연산

### 유도 공식!

![math2](/media/math2.jpg)




<br/>

## Distance Widget 동작 구현

> 마우스 클릭 시 기존 위치에서 마우스가 움직인 거리로 Widget 움직이기

![math3](/media/math3.png)


### Ref
* [내적을 이용한 점과 직선 사이의 거리 구하기](https://m.blog.naver.com/PostView.nhn?blogId=yh6613&logNo=221212680685&proxyReferer=https%3A%2F%2Fwww.google.com%2F)


### 구현 
![math3](/media/distance_widget.gif)




<br/>

## Angle Widget 동작 구현

> 마우스 클릭 시 기존 위치에서 마우스가 움직인 거리로 Widget 움직이기

### 기본 동작

1. 현재 찍은 좌표의 공간 구하기 (GetSpace)
2. 정확한 거리 변경을 위한 currentPos와 centerPos의 기준을 중간 디렉션으로 바꿈!
3. centerPos로 부터의 방향과 여러 조건에 따른 거리 크기 구하기
4. 드래그 중 공간이 바뀌면 각도 Reverse
5. GuideDimension 거리 Set

<br/>

### GetSpace 공식 설명

![math3](/media/angle_widget.png)

* 위의 그림에서 Current Pos가 갈 수 있는 위치는 다음과 같다. `IN_STANDARD`, `OUT_STANDARD_EXTENDED`, `OUT_AGAINST_STANDARD`, `OUT_AGAINST_STANDARD_EXTENDED` 알아야할 것은 외적과 내적이다. 
* 검은색 두 선을 벡터 `SV`, `EV`라고 가정한다.
* 밑의 코드가 공식이다.


![math3](/media/angle_widget1.png)

#### IN_STANDARD 예시
1. SV과 CurrentPos을 외적하면 Z축 방향의 법선 벡터가 생긴다. 
1. EV과 CurrentPos을 외적하면 위와 반대 방향의 법선 벡터가 생긴다. 
3. 두 법선 벡터를 내적하면 음수의 값이 나온다.
4. 이제 SV와 EV와 외적한다. 마찬가지로 SV와 EV를 외적한 벡터와 SV와 Current Pos를 외적한 벡터를 내적하면 역시 음수의 값이 나온다.


### 기준 바꿔 거리 구하기 설명

1. 거리를 측정하기 위해 각 Start Angle과 End Angle의 사이의 방향으로 각각 Current Pos와 Center Pos를 구한다. 
2. 그 이후 거리를 위하면 같은 디렉션 방향에 있으므로 정확한 거리가 나오게 된다. 
3. 거리를 구할 시 방향을 고려하지 않은 크기만 나오게 되므로 위의 Distance Widget 공식을 이용하여 정확한 방향까지 고려한 크기를 구한다.
4. 위의 Distance Widget과 마찬가지로 Arc에서도 기준을 바꿨으므로 일정한 거리 d를 구할 수 있다.
5. 일정한 거리 d와 Old Distance를 더하여 최종 거리를 구한다.


### 구현 
![math3](/media/angle_widget.gif)