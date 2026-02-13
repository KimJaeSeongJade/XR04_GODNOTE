## NavMesh 뭐야?

> 이동 로직을 코드로 짜지 않고 사용하는 시스템
> 내부적으로 **(Graph)** 로 데이터화 해서 성능-안정성 우수함.

![]( <Resources/Pasted image 20260203092513.jpg> )

![]( <Resources/Pasted image 20260203092524.png> )

> **NavMesh** 는 Bake 시점에서 계산이 끝남.


---
## Bake 단계 (사전 준비)

**컴포넌트**: NavMesh Surface -> Bake 버튼 클릭!

### 1. 공간 판별
Unity 씬을 훑음

- 이 표면 Walking 가능?
- 경사 각도 올라갈 수 있음? [Navigation 탭 - Max Slope]
- Agent가 지나갈 수 있는 충분한 부피/반지름임? [Navigation 탭 - Radius]

### 2. 공간을 폴리곤/면(Polygon)으로 분해/생성

![]( <Resources/Pasted image 20260203092524.png> )

- 다각형(Polygon)으로 쪼갬
- 이동에 필요한 정보만 남김

### 3. 폴리곤/면(Polygon) 간 연결

- 폴리곤/면 간 생기는 선 엣지(Edge)

### 4. Area - Cost 저장

![]( <Resources/Pasted image 20260213172317.png> )


- 이동 우선순위를 정함.

---
## Runtime 단계 (이동 요청 시)

```csharp
agent.SetDestination(target.position);
```

1. **현재 위치**가 속한 폴리곤/면 찾기 -> 내 위치 찾기
   
2. **목적지** 폴리곤/면 찾기
   
3. 폴리곤 그래프(Graph)에서 최적 경로 검색 (경로 O/X ❌ -> 경로 선택)
	1. A* 계열, 상대적으로 노드 수가 적음
	   
4. 폴리곤 경로를 따라 이동 경로 생성


### NavMesh가 내부에 가지고 있는 정보

```css
[폴리곤 A] ── 연결 ── [폴리곤 B]
[폴리곤 B] ── 연결 ── [폴리곤 C]
```

- 좌표 ❌
- 텍스처 ❌
- 렌더링 정보 ❌

👉🏻 이동 정보만 가짐
👉🏻 그래프 기반 이동이기 때문에 이미 검증된 길만 탐색함

---
## A* vs NavMesh

### A*
- **노드 생성**: 런타임(Runtime)
- **그래프 구성**: 직접
- **비용 계산**: 항상

### NavMesh
- **노드 생성**: Bake
- **그래프 구성**: 엔진
- **비용 계산**: 최소화

---
## NavMesh 기본 구성

### 1. NavMesh Surface - 길 만들기

![[]]
![]( <Resources/Pasted image 20260213173949.png> )


- 걸어다닐 수 있는 공간인지 판별해주는 매니저 역할

### 2. NavMesh Agent

![]( <Resources/Pasted image 20260213174149.png> )

#### 역할
- 이동
- 회전
- 충돌 회피

#### 프로퍼티
- speed
- angular speed
- Stopping Distance

#### 필수 함수
```csharp
agent.SetDestination(target.position);
```


#### 유의사항
- **transform.position** 으로 이동❌
- **Rigidbody** 동시 제어❌


### 3. NavMesh Obstacle - 동적 장애물

![]( <Resources/Pasted image 20260213174551.png> )


#### 필수 기능
- Carving [ON] -> 못가는 길을 만듦.

#### 유의사항
- 많이 쓰면 성능 하락
- 많이 움직일 필요가 없는 물체 정도만 사용

---
##  Agent Settings

![]( <Resources/스크린샷 2026-02-02 175430.png> )


- **Raidus/Height** -> 캐릭터 부피/크기
- Setp Height/Max Slope -> 계단-경사 허용치

👉🏻 Agetn(AI) 몸체

## Areas

![]( <Resources/스크린샷 2026-02-02 175440.png> )

- 0~31번까지 있음
- **Cost = 선호도** (낮을수록 우선순위)

---
## NavMesh Link

![]( <Resources/Pasted image 20260202180631.png> )

- 끊긴 길 연결
	- 점프
	- 드롭(낙하)
	- 사다리

---
## NavMesh Modifier Volume

![]( <Resources/Pasted image 20260202180724.png> )

### 역할
- 특정 영역의 길을 설정

#### 예시
- 늪지대
- 함정 구역
