# 문제 코드

```csharp
public enum StateList
{
   Idle, Move, Jump, Attack
}

public class PlayerState : MonoBehaviour
{
   private int _direction;
   [SerializeField] private StateList CurrentState; 

   private void Update() 
   {
     // 키 입력 에 따른 각 상태 변화
     
   }

   private void Idle()
   {
     // 기능 로직
     // 애니메이션 변화 로직
     // 예외처리 등
   }

   private void Move()
   {
     // 기능 로직
     // 애니메이션 변화 로직
     // 예외처리 등
   }

   private void Jump()
   {
     // 기능 로직
     // 애니메이션 변화 로직
     // 예외처리 등
   }

   private void Attack()
   {
     // 기능 로직
     // 애니메이션 변화 로직
     // 예외처리 등
   }
}
```

- `상태(State)`가 늘어나면, 조건 분기 수(if) 기하급수적으로 늘어남.

---
# 수강 시 꼭 기억해야할 점

> **게임 기능**이 늘어나면 `조건문`을 늘리는 게 아니라 `상태(State)` 늘린다.

---
### Q. 그래서 여기서 말하는 상태 (State)가 뭔데?

> 어떤 객체가 **특정 상황에서 어떻게 행동해야 하는지를 결정**하는 **행동 규칙**

---
# 코드 문제점

```csharp
void Update()
{
    HandleInput();
    ApplyGravity();
    CheckGround();

    if (_state == PlayerState.Idle)
    {
        ...
    }
    else if (_state == PlayerState.Move)
    {
        ...
    }
    else if (_state == PlayerState.Jump)
    {
        ...
        if (CanDoubleJump()) ...
        if (InputDash()) ...
        if (InputAttack()) ...
        if (IsHit()) ...
    }
    else if (_state == PlayerState.AirAttack)
    {
        ...
        if (ComboWindowOpen()) ...
        if (IsHit()) ...
        if (IsGrounded()) ...
    }
    else if (_state == PlayerState.Hit)
    {
        ...
        if (RecoverTimeOver()) ...
    }
    else if (_state == PlayerState.Die)
    {
        ...
    }
    // 계속 증가
}
```

- `상태(State)`가 서로를 안다 (엮어있다)
- **책임**이 `Player`한테 몰리는 현상이 일어남..
- 수정 시 리스크 폭발

---
# State Pattern 구조

| 요소                | 책임          | 하면 안 되는 것   |
| ----------------- | ----------- | ----------- |
| **Player**        | 상태 실행       | 상태 로직 직접 구현 |
| **StateMachine**  | 상태 교체       | 입력 처리       |
| **ConcreteState** | 자기 상태 행동 전달 | 다른 상태 내부 접근 |


[UML]
![]( <Resources/Pasted image 20260222022055.png> )

[동작 흐름]
```powershell
User Input -> Player -> StateMachine -> State.Upate()

입력 → 코드 FSM → 물리 → Animator
```

- **게임**은 `코드 FSM` 위에서 동작
  `Animator`는 그 위에 얹힌 표현 계층
  

[전환 흐름]
```powershell
CurrentState?.Exit()
↓
NewState.Enter()
```

---
# Unity에서 상태패턴을 가지는 임의의 객체 (Player)

## PlayerController.cs 틀 예시
```csharp
using UnityEngine;

// 입력 처리 / 물리 판정 / 애니메이션 결정
public class Player : MonoBehavoiur
{
	Rigidbody2D _rb;
	Animator _animator;
	
	StateMachine _stateMachine;
	
	// Player가 사용할 Stete들
	IdleState Idle;
	MoveState Move;
	JumpState Jump;
	
	void Start()
	{
		_stateMahcine = new StateMachine();
		Idle = new IdleState(this)
		Move = new MoveState(this);
		Jump = new JumpState(this);
		
		_stateMachine.ChangeState(Idle);
	}
	
	void Update()
	{
		_stateMachine.Update();
	}
	
	public void ChangeState(IState state)
	{
		_stateMachine.ChangeState(state);
	}
}

```

---
# 정리

[Player가 모든 상태를 관리하는 코드 문제점]
> 상태 수 x 전환 조건 수
> = Update 복잡도 폭증

👉🏻 `상태(State)`가 늘어날수록 Player가 모든 상태 관련 것들을 알아야함


[상태(State) 역할]
> `상태(게임 로직 + 애니메이션)` 모두 Player 안에서 처리


[상태(State) 분리로 일어난 이점]
- Update 함수 코드 구현이 없어진 것이 아니라, 책임이 `상태(State)` 객체로 이동한 것

---
## (선택 실습) 메이플스토리 - 나이트워커

![](https://upload2.inven.co.kr/upload/2018/01/11/bbs/i13832203955.gif?MW=800 "원본 크기로 보시려면 이미지를 클릭하세요.")

 
 [상황]
 > 점프하다가 공격 버튼을 누름 -> 투사체 던짐


 [공중 공격]
1. `JumpState`에서 **공격 입력 감지**
2. `(Air)AttackState`로 **ChangeState**
3. `(Air)AttackState`가:
    - 공격 판정 실행
    - 공중 이동 허용 여부 결정
    - 착지 시 Idle 또는 Move로 전환
