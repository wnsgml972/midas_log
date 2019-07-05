# OBB와 AABB?

## Bounding Box

* 3D world에서 교차 검사는 매우 중요한 이슈이다.
* 게임을 예로 들자면, 캐릭터끼리 충돌 검출도 해야하고 마우스 picking도 해야되고 타격 판정도 해야되기 때문이다.
* 그래서 캐릭터 모델(메쉬) 나 특정 범위, 지형 구조물 등에 충돌 판정을 위한 박스나 단순한 모양의 bounding volume을 씌운다.
* 그렇게 해서 복잡한 메쉬끼리 교차 검사를 하는 것이 아니라 단순한 volume들 끼리 교차 검사를 하는 것이다.
* 만약 메쉬를 이루는 모든 삼각형끼리 충돌 검사를 한다면 너무 비용이 커질 것이다.
* 가장 많이 쓰이는 모양이 box인데, 이 box에는 2종류의 타입이 있으니 그것이 바로 aabb와 obb이다.


<br/>

## AABB

![aabb](/media/2019/07/aabb1.png)
* AABB는 Axis-Aligned Bounding Box의 줄임말로 기저 축에 정렬된 충돌 박스이다.
* 박스를 이루는 면의 노말 벡터들이 곧 X Y Z축과 일치하는 모양이다.
* 모델을 이루는 다각형의 x, y, z 좌표의 최소 최대를 각 박스의 버텍스로 해서 생성한다.
* 따라서 회전함에 따라 크기가 계속 변하게 된다.
* 축에 일치된 모양이기때문에 AABB끼리의 충돌 검출은 매우 간편하다.

![aabb](/media/2019/07/aabb2.png)

### 충돌 검출 코드
~~~cpp
void CheckAABB(BoundingBox targetBox)
{
 if (boundingBox.minPos[0] <= targetBox.maxPos[0] && boundingBox.maxPos[0] >= targetBox.minPos[0] &&
 boundingBox.minPos[1] <= targetBox.maxPos[1] && boundingBox.maxPos[1] >= targetBox.minPos[1] &&
 boundingBox.minPos[2] <= targetBox.maxPos[2] && boundingBox.maxPos[2] >= targetBox.minPos[2])
 {
  aabbCollide = true;
  return;
 }
 aabbCollide = false;
}
~~~


<br/>

## OBB
![aabb](/media/2019/07/aabb3.png)
* OBB는 AABB와 마찬가지로 박스를 이루는 세 면은 서로 수직이지만
* 해당 면의 노말 벡터가 X Y Z와 일치하지 않는 박스이다.
* X Y Z 와 일치하지 않기 때문에 AABB보다 충돌 검출의 시간 복잡도가 높지만, 
* 충돌 박스가 XYZ 좌표를 기준으로 최대, 최소로 계속 변하는 AABB보다
* 항상 좀 더 fit한 바운딩 박스를 만들 수 있다.


### 충돌 검출 코드
~~~cpp
void CheckOBB(BoundingBox targetBox)
{
 double c[3][3];
 double absC[3][3];
 double d[3];
 double r0, r1, r;
 int i;
 const double cutoff = 0.999999;
 bool existsParallelPair = false;
 D3DXVECTOR3 diff = boundingBox.centerPos - targetBox.centerPos;
 
 
 for (i = 0; i < 3; ++i)
 {
  c[0][i] = D3DXVec3Dot(&boundingBox.axis[0], &targetBox.axis[i]);
  absC[0][i] = abs(c[0][i]);
  if (absC[0][i] > cutoff)
   existsParallelPair = true;
 }
 
 
 d[0] = D3DXVec3Dot(&diff, &boundingBox.axis[0]);
 r = abs(d[0]);
 r0 = boundingBox.axisLen[0];
 r1 = targetBox.axisLen[0]* absC[0][0] + targetBox.axisLen[1]* absC[0][1] + targetBox.axisLen[2]* absC[0][2];
 if (r > r0 + r1)
 {
  obbCollide = false;
  return;
 }
 for (i = 0; i < 3; ++i)
 {
  c[1][i] = D3DXVec3Dot(&boundingBox.axis[1], &targetBox.axis[i]);
  absC[1][i] = abs(c[1][i]);
  if (absC[1][i] > cutoff)
   existsParallelPair = true;
 }
 d[1] = D3DXVec3Dot(&diff, &boundingBox.axis[1]);
 r = abs(d[1]);
 r0 = boundingBox.axisLen[1];
 r1 = targetBox.axisLen[0] * absC[1][0] + targetBox.axisLen[1] * absC[1][1] + targetBox.axisLen[2] * absC[1][2];
 if (r > r0 + r1)
 {
  obbCollide = false;
  return;
 }
 for (i = 0; i < 3; ++i)
 {
  c[2][i] = D3DXVec3Dot(&boundingBox.axis[2], &targetBox.axis[i]);
  absC[2][i] = abs(c[2][i]);
  if (absC[2][i] > cutoff)
   existsParallelPair = true;
 }
 d[2] = D3DXVec3Dot(&diff, &boundingBox.axis[2]);
 r = abs(d[2]);
 r0 = boundingBox.axisLen[2];
 r1 = targetBox.axisLen[0] * absC[2][0] + targetBox.axisLen[1] * absC[2][1] + targetBox.axisLen[2] * absC[2][2];
 if (r > r0 + r1)
 {
  obbCollide = false;
  return;
 }
 r = abs(D3DXVec3Dot(&diff, &targetBox.axis[0]));
 r0 = boundingBox.axisLen[0] * absC[0][0] + boundingBox.axisLen[1] * absC[1][0] + boundingBox.axisLen[2] * absC[2][0];
 r1 = targetBox.axisLen[0];
 if (r > r0 + r1)
 {
  obbCollide = false;
  return;
 }
 
 
 r = abs(D3DXVec3Dot(&diff, &targetBox.axis[1]));
 r0 = boundingBox.axisLen[0] * absC[0][1] + boundingBox.axisLen[1] * absC[1][1] + boundingBox.axisLen[2] * absC[2][1];
 r1 = targetBox.axisLen[1];
 if (r > r0 + r1)
 {
  obbCollide = false;
  return;
 }
 
 
 r = abs(D3DXVec3Dot(&diff, &targetBox.axis[2]));
 r0 = boundingBox.axisLen[0] * absC[0][2] + boundingBox.axisLen[1] * absC[1][2] + boundingBox.axisLen[2] * absC[2][2];
 r1 = targetBox.axisLen[2];
 if (r > r0 + r1)
 {
  obbCollide = false;
  return;
 }
 
 
 if (existsParallelPair == true)
 {
  obbCollide = true;
  return;
 }
 
 
 r = abs(d[2] * c[1][0] - d[1] * c[2][0]);
 r0 = boundingBox.axisLen[1] * absC[2][0] + boundingBox.axisLen[2] * absC[1][0];
 r1 = targetBox.axisLen[1] * absC[0][2] + targetBox.axisLen[2] * absC[0][1];
 if (r > r0 + r1)
 {
  obbCollide = false;
  return;
 }
 
 
 r = abs(d[2] * c[1][1] - d[1] * c[2][1]);
 r0 = boundingBox.axisLen[1] * absC[2][1] + boundingBox.axisLen[2] * absC[1][1];
 r1 = targetBox.axisLen[0] * absC[0][2] + targetBox.axisLen[2] * absC[0][0];
 if (r > r0 + r1)
 {
  obbCollide = false;
  return;
 }
 
 
 r = abs(d[2] * c[1][2] - d[1] * c[2][2]);
 r0 = boundingBox.axisLen[1] * absC[2][2] + boundingBox.axisLen[2] * absC[1][2];
 r1 = targetBox.axisLen[0] * absC[0][1] + targetBox.axisLen[1] * absC[0][0];
 if (r > r0 + r1)
 {
  obbCollide = false;
  return;
 }
 
 
 r = abs(d[0] * c[2][0] - d[2] * c[0][0]);
 r0 = boundingBox.axisLen[0] * absC[2][0] + boundingBox.axisLen[2] * absC[0][0];
 r1 = targetBox.axisLen[1] * absC[1][2] + targetBox.axisLen[2] * absC[1][1];
 if (r > r0 + r1)
 {
  obbCollide = false;
  return;
 }
 
 
 r = abs(d[0] * c[2][1] - d[2] * c[0][1]);
 r0 = boundingBox.axisLen[0] * absC[2][1] + boundingBox.axisLen[2] * absC[0][1];
 r1 = targetBox.axisLen[0] * absC[1][2] + targetBox.axisLen[2] * absC[1][0];
 if (r > r0 + r1)
 {
  obbCollide = false;
  return;
 }
 
 
 r = abs(d[0] * c[2][2] - d[2] * c[0][2]);
 r0 = boundingBox.axisLen[0] * absC[2][2] + boundingBox.axisLen[2] * absC[0][2];
 r1 = targetBox.axisLen[0] * absC[1][1] + targetBox.axisLen[1] * absC[1][0];
 if (r > r0 + r1)
 {
  obbCollide = false;
  return;
 }
 
 
 r = abs(d[1] * c[0][0] - d[0] * c[1][0]);
 r0 = boundingBox.axisLen[0] * absC[1][0] + boundingBox.axisLen[1] * absC[0][0];
 r1 = targetBox.axisLen[1] * absC[2][2] + targetBox.axisLen[2] * absC[2][1];
 if (r > r0 + r1)
 {
  obbCollide = false;
  return;
 }
 
 
 r = abs(d[1] * c[0][1] - d[0] * c[1][1]);
 r0 = boundingBox.axisLen[0] * absC[1][1] + boundingBox.axisLen[1] * absC[0][1];
 r1 = targetBox.axisLen[0] * absC[2][2] + targetBox.axisLen[2] * absC[2][0];
 if (r > r0 + r1)
 {
  obbCollide = false;
  return;
 }
 
 
 r = abs(d[1] * c[0][2] - d[0] * c[1][2]);
 r0 = boundingBox.axisLen[0] * absC[1][2] + boundingBox.axisLen[1] * absC[0][2];
 r1 = targetBox.axisLen[0] * absC[2][1] + targetBox.axisLen[1] * absC[2][0];
 if (r > r0 + r1)
 {
  obbCollide = false;
  return;
 }
 
 
 obbCollide = true;
 return;
}
~~~


<br/>

## 코드 구동 예시

두 박스를 AABB로 충돌시킬 경우에는 다음처럼 눈에 보기엔 충돌하지 않았는데도 충돌함

![aabb](/media/2019/07/aabb4.png)


## Ref
* https://hoodymong.tistory.com/8 [3초 2분 1년]
