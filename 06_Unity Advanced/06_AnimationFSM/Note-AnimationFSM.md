## 1. FSM (유한 상태 머신) 무엇인가?

- 객체가 한 번에 하나의 상태만 가질 수 있도록 **제어하는 모델**.

**[구조]**
> **현재 상태(State) + 조건(Parameter) -> 상태 전이(Transition)**

Idle <-> Walking
Walking <-> Running

---
## 2. Unity Animation 구성 요소

### 2-1. 컴포넌트

- **Animation (Legacy)**
	- 단순한 클립 재생 (단발성 연출)
	- .anim 파일
![[Pasted image 20260211030308.png]]


- **Animator (New)**
	- 상태 전이가 활발한데 제어도 필요할 때
	- .controller 파일
	- Parameter 탭 수치를 조절
![[Pasted image 20260211030255.png]]


Animtion과 Animator가 둘 다 사용하는 것 -> **Animation Clip** (녹화본)

> Animation은 단순한 연출에 활용된다.

---
### 1. SkinnedMeshRenderer 컴포넌트

> **뼈의 움직임**에 따라 변형되는 **메시(Mesh)** 를 화면에 그리는 컴포넌트

### 2.Animator 컴포넌트

> 애니메이션 설계도 (Controller)를 읽어 뼈(Bone)을 실제로 움직이는 엔진

### 3. Animator Controller

> 애니메이션의 흐름을 정의하는 FSM 설계도

#### 3-1. State (노드)

> 캐릭터가 취할 수 있는 각각의 행동 단위

![[Pasted image 20260211102910.png]]

#### 3-2. Transition (상태 전이 표시, 화살표)

> **상태(State)** <-> **상태(State)** 를 잇는 통로

![[Pasted image 20260211103607.png|500]]
![[Pasted image 20260211103549.png|500]]

#### 3-3. Parameter

> 코드에서 애니메이터로 보내는 데이터
![[Pasted image 20260211104154.png]]

- **Transition**이 일어나기 위한 조건문 (Float, Int, Bool, Trigger)
	- **Float**: 이동 속도처럼 **부드러운 변화**

	- **Int**: 무기 스왑이 필요한 것들
	
	- **Bool**: 상태 체크
	
	- **Trigger**: Attack처럼 단발성 이벤트

> 개발자는 위 4가지 Parameter 들을 스크립트로 제어하여 State(노드) 간 Transition(상태 전이)을 제어함.

---
## 3. 추가 속성 기능

- **Loop Time**
	- 동작 반복 여부 결정

- **Animation Type**
	- 인간형이라면 반드시 **Humanoid**로 설정해야 다른 캐릭터의 애니메이션을 재사용 가능

- **Avatar (Configure)**
	- 유니티가 캐릭터의 관절(머리, 팔, 다리)을 올바르게 인식하도록 지도를 매핑하는 단계

- **Any State**
	- 현재 어떤 상태에 있든 즉시 특정 액션(공격, 피격)으로 이동하는 **만능 통로**
    
- **Has Exit Time**
	- 체크하면 현재 애니메이션이 끝날 때까지 기다렸다가 이동하고, 해제하면 즉시 이동합니다. 

- **Can Transition To Self**
	- 공격 중에 다시 공격 신호를 받았을 때 처음부터 다시 재생할지 결정
	- **Any State** 활용 시 무한 루프 방지를 위해 반드시 해제
    
- **Transition Duration**
	- 두 동작이 섞이는 시간
	- 반응 속도를 높이려면 이 값을 0.1 정도로 줄이기 (보통 0.1~0.25 가 적당)
    
- **Transition Priority**
	- 멈춤(Idle)과 걷기(Walking) 조건이 동시에 만족될 때, 어떤 화살표를 먼저 검사할지 결정하는 리스트 순서
