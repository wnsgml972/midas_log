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
* Ex) Beam (바닥을 지지하는 기둥), 바닥, 벽 등등
* CIM에선 해석을 위한 중심이 주어짐




<br/>

### Wireframe Modeling System
* 점과 선으로 물체의 외양만을 표현하는 방법

### Surface Modeling System
* 여러 개의 곡면으로 물체의 바깥 모양을 표현하는 방법
* Wireframe Model에 face 정보를 추가한 형태
* face 사이의 상호관계는 저장하지 않고, face의 list만 저장

### Solid Modeling System
* 모양만이 아닌 물체의 다양한 성질을 좀 더 정확하게 표현하기 위해 고안된 방법
* 크게 `CSG, B-Rep, Decomposition`으로 나누어짐
* `CSG`
    * Primitive(경계가 잘 정의되는 단위형상)의 조합으로 Solid를 표현
    * Primitive에 대한 Boolean Operation을 수행하는 과정을 Tree 구조로 저장.
    * 부드러운 곡면을 표현하기에는 한계가 있음. 허나 단순
* `B-Rep`
    * 방향성과 경계가 있는 곡면들로 Solid를 표현
    * `B-Rep`에선 방향성과 경계가 있는 곡면을 face라 함
    * `B-Rep`에서 face, edge, vertex들의 상호 이웃 관계를 같이 저장하며 이것을 `Topology`(위상관계)라 함, 또 각각의 요소를 `topology entity`라 함
    * 이들 3가지에 대응하는 `geometry entity`(기하요소)는 `surface`(곡면), `curve`(곡선), `point`(점)이다.
* `Decomposition`
    * Solid model을 간단한 형상의 집합으로 approximation시켜서 표현하는 방식


### Make Solid (Profile, Surface)

#### 알아야 할 것
* 완성된 형상 자체를 Solid로 만들 경우 폐구간을 이룬 형상만 가능
* `Primitive` : 경계가 잘 정의되는 단위형상
* `Surface` : 곡면, Solid 하기 위해 기본으로 알아야 할 것
* `Profile` : Surface를 만들기 위한 기본 곡선, (단면:aProfileFaces)
<br/>또는 그냥 이거 자체를 단면으로 부름


#### Create Solid 연산
* `Revolve`(회전) : 어떤 Profile을 회전시켜 `Solid`
* `Loft`(로프트) : 여러 개의 Profile을 이용하여 연결하여 `Solid`
* `Sweep`(스윕) : Profile을 일정 **경로**를 따라 밀어서 `Solid`
* `Extrude`(돌출) : Profile을 **직선** 방향으로 밀어서 `Solid`
* `Sew`(바느질) : topology적으로 붙어있다고 정의됐지만 실 좌표에서는 그렇지 않아 Sew(바느질)하여 `Solid`
* `Tessellated Solid`(삼각망) : 삼각망으로 나누어 `Solid`



<br/>

### 그 외 용어

#### manifold
* 모양이 막 이상한 애들, 얘는 뭐다! 이렇게 정의내릴 수 없는 애들
* `Manifold B-Rep` 형상을 Tesselation하여 여러개의 Face들로 나누고 Sew하여 Solid를 만듬