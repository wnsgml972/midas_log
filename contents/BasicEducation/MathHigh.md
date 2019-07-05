# UI 엔진 업무 실무

## CIM 좌표계

![xyz](/media/xyz.png)




<br/>

## 3차원 좌표계 충돌 구현
[aabb와 oob 알고리즘](./OBB_AABB.md)




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

## Point Widget 동작 구현

> 해당 점으로 부터 나오는 Dir 방향 마우스 클릭에 따라 바꾸기

### Point Widget 정보 정의
![math3](/media/point_widget.gif)



<br/>

## Distance Widget 동작 구현

> 마우스 클릭 시 기존 위치에서 마우스가 움직인 거리로 Widget 움직이기

### Distance Widget 정보 정의
![math3](/media/2019/distance_info.png)


### 기본 동작
![math3](/media/math3.png)


### Ref
* [내적을 이용한 점과 직선 사이의 거리 구하기](https://m.blog.naver.com/PostView.nhn?blogId=yh6613&logNo=221212680685&proxyReferer=https%3A%2F%2Fwww.google.com%2F)


### 구현 
![math3](/media/distance_widget.gif)




<br/>

## Angle Widget 동작 구현

> 마우스 클릭 시 기존 위치에서 마우스가 움직인 거리로 Widget 움직이기, 위의 Distance Widget에서 응용 + 심화

### Angle Widget 정보 정의
![math3](/media/2019/angle_info.png)

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




<br/>

## Diameter Widget 동작 구현

> 마우스 클릭 시 기존 원 안에서 Measure Line 돌리기 + 원 밖을 벗어나면 Distance Widget 처럼 동작

### Diameter Widget 정보 정의
![math3](/media/2019/diameter_info.png)

### 기본 동작
1. 기본 모양은 Distance Widget의 Length Dimension과 같음
2. 측정의 중심은 원의 중심
3. 원으로 부터 2차원의 판을 그림
4. 중심과 Current Pos간의 거리를 원 밖을 벗어났을 시 거리로 지정하고
5. 원 안에 있을 시에는 Current Pos를 원 중심에서 평행한 벡터를 그려야 함
6. 즉 `Current Pos - Center Pos` 벡터에서 2차원의 판의 Z축 방향의 Normal Vector를 곱하면 중심선으로 부터 평행한 Dir을 구할 수 있음


### 구현 
![math3](/media/diameter_widget1.gif)




<br/>

## Ellipse Diameter Widget 동작 구현

> Major Diameter, Minor Diameter 당 Length Dimension 표시하기

### 기본 동작
1. 기본 모양은 Distance Widget의 Length Dimension과 같다.
2. Major Dir이나 Minor Dir을 현재 Current Dir으로 정하고 Distance Widget과 같은 공식을 취함.
3. 현재 Currrent 좌표의 위치 공식은 Distance Widget의 내적을 이용한 점과 직선사이의 거리 구하기 공식을 통해 법선 벡터 상의 거리를 비교해 마우스의 좌표를 구하고
4. 현재 구해야 할 위치를 구했으면 다시 점과 직선사이의 거리 구하기 공식을 통해 거리를 세팅해주면 된다.

### 구현 
![math3](/media/ellipse_diameter_widget.gif)

