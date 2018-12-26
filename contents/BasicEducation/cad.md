## CAD 용어 정리

### Tool & Target
* Tool : “결과물을 만들기 위해 필요한 재료”
* Target : “결과물, 대상”

#### 반대 관계
* Tool <-> Target   “위와 같은 관계”
* Source <-> Target   “위와 같은 관계”
* IN <-> OUT   “위와 같은 관계”



<br/>

### Feature

**형상만 가지고 있는 것** <br/>
* 단순히 면이다. 선이다. Solid다. 이런 것


단일 | 묶음 
---------|----------
 Face | Sheet 
 Edge | Wire 
 Vertex | Minimum 

* Face, Sheet : “Face 들의 묶음 Sheet(Body)”
* Edge, Wire : “Edge 들의 묶음 Wire(Body)”
* Vertex, Minimum: “Vertex 들의 묶음 Minimum(Body)”



<br/>

### Member

**부재라고도 하며, 형상 뿐만 아니라 용도를 가지고 있는 것**<br/>
* Ex) Beam (바닥을 지지하는 기둥), 바닥, 벽 등등<br/>
* 보통 해석을 위한 중심이 주어짐!




<br/>

### Make Solid (Profile, Surface)
* Surface : 표면, Solid 하기 위해 기본으로 알아야 할 것
* Profile : Surface를 만들기 위한 기본 곡선, (단면:aProfileFaces)
* Revolve(회전) : 어떤 Profile을 회전시켜 Solid
* Loft(로프트) : 여러 개의 Profile을 이용하여 연결하여 Solid
* Sweep(스윕) : Profile을 일정 **경로**를 따라 밀어서 Solid
* Extrude(돌출) : Profile을 **직선** 방향으로 밀어서 Solid


