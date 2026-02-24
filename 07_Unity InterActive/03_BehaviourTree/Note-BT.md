# 오늘 수업 흐름도

> **FSM 복습 (한계점) -> State 복습 (한계점) -> 코드 기반 BT (한계점) -> BT Graph - Unity6**

- Player
- Enemy -> 일정거리 안에 있으면 -> **Player 추적** -> 공격 범위 안에 있으면 공격
		  Patrol 기능 실행

**EnemyAI 가 어떻게 Player를 추적하고 동작하게 할지?**
+ Patrol 기능 추가해보기 (선택 실습)

---
# 1. FSM 복습

## 기본 구조
```csharp
switch(currentState)
{
	case State.Idle:
		// 동작
		break;
		
	case State.Move:
		// 동작
		break;
	...
}
```

[ex]
- `상태(State)` 3개일 때는?
근데 만약에 `상태(State)` 10개 이상이면?

👉🏻 `상태(State)` 수가 늘수록 코드 복잡도 폭증

---
# 2. State Pattern 복습

## 구조

- **IState.cs (interface)**
	- IdleState
	- MoveState
	- JumpState
	  ...
- **StateMachine.cs**

## 상태(State) 구성

- Idle
- Move
- Jump

+`상태(State)`
- Attack
- Dash
- Air 관련 (Jump 비슷한 State)
- Crouch

**"전이(Transition) 연결"**

`상태(State)` 가 많아질수록 
- 비슷한 `상태(State)` 간, **조건 중복**이 많이됨
- `상태(State)`들간 결합도가 커질 수 있음

---
# 3. Behaviour Tree (BT)

> **루트 노드** -> 자식 노드를 순서대로 조건부 판단

### Q. State Pattern 과 차이?
- `상태(State)`는 선택된 현재 상태(State)만 실행 -> 그 상태(State)가 가진 조건부 판별만함
- `BT`는 조건을 매 프레임 단위로 루트 노드에서부터 판단

---
## 3-1. BT의 구조

> **Node**들로 이뤄져 있다

```csharp
public enum NodeState { Success, Failure, Running }

public abstract class Node
{
	public abstract NodeState Update();
}
```

> **Evaluate()** 실행 결과를 반환

## 3-2. 다룰 Node

- **Selector (OR)**
- **Sequence (AND)**
- **Leaf (Condition / Action)**

## 3-3. Node 종류

### 1. Composite Node

- **자식 노드들의 실행 흐름을 제어한다**
  
	- `시퀀스(Sequence)` - **AND 구조**
		- 순서대로 실행 
		- 하나라도 실패하면 즉시 **Failure** 반환
		- 모두 성공하면 **Success** 반환
	  
	- `셀렉터(Selector)` - **OR 구조**
		- 자식 중 하나라도 성공하면 **Success** 반환
		- 모두 실패하면 **Failure** 반환

### 2. Leaf Node

- **트리의 가장 하위 노드로** 자식을 가지지 않는다
- **조건을 판단(`Condition`)** 하거나 **실제 행동( `Action`)을 수행**

---
## (유니티 실습) BT 구조

> Enemy가 Player 추적 및 공격 구현중

```csharp
           [Root]
              |
          [Selector] - OR구조
           /       \
      [Attack]    [Chase] - Sequence (And 구조)
                     |           
                [IsNearTarget] - Condition(Leaf)
	                |               
	            [MoveToTarget]  - Action(Leaf) 
```

### Selector (OR 구조)

- `AttackAction`: 공격이 가능한가? -> 가능하다면 공격 **(Success / Failure)**
	- **Failure**: Chase(추적) 함? **(Success / Failure)**
		- **Success**: Target 추적중
		- **Failure**: IdleState

---
## (+추가) BT 구조 짜실 때

1. 동작할 흐름 `모식도`로 그려보기
2. **노드들 이해하기 (Composite , Leaf Node)**
3. 코드 짜기

---
## 3-4. 한계점

- 코드 복잡해짐
- 구조 파악 어려움
- 디자이너(기획자) 수정 불편 (못함...)

---
## 정리

`FSM`
 - **정의**: 상태를 분기
 - **한계점**: 코드가 복잡해짐

`State Pattenr`
- **정의**: 상태 책임 분리
- **한계점**: 전이 폭증 현상

`Behaviour Tree`
- **정의**: 상태 조건 판별 트리 구조
- **한계점**: 구조 파악 어려움, 협업 활용 못함

`BT Graph`
- **정의**: 코드 기반 BT 시각화
- **한계점**: Good at English
