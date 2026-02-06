## 기존 Input 방식의 한계

![]( <Resources/Pasted image 20260117005241.jpg> )

![]( <Resources/Pasted image 20260117005250.jpg> )



- PC
- Console
- Mobile

플랫폼 대응의 한계!

---
## 기존 Input 시스템 한계 

```csharp
if(Input.GetKey(KeyCode.W))
{
// W방향 이동
}
```

- 키 변경 시, 코드 수정
- 플랫폼별 추가 코드 작성 및 대응

---
## Input System 핵심 구성 요소


```mathematica
Input Action Asset
└─ Action Map (예: Player)
   ├─ Action (Move)
   │  ├─ Type: Value (Vector2)
   │  └─ Binding: WASD / Left Stick
   ├─ Action (Jump)
   │  ├─ Type: Button
   │  └─ Binding: Space / Gamepad A
```


### 구성 요소

![]( <Resources/Pasted image 20260205090835.png> )

#### 1. Action Map

- 입력을 상황 단위로 묶는 그룹
	- Player -> 캐릭터 조작
	- UI -> 메뉴 조작

#### 2. Action

- 하나의 행동을 의미
	- Move, Jump, Attack

#### 3. Binding

- Action을 실행하는 실제 입력 장치
	- WASD
	- Gamepad stick
	- Touch

#### 4. Control Type

- 실제 데이터 타입
	- Vector2
	- Button
	- float

---
## Player Input - Behavior 속성

1. SendMessage / Broadcast
	1. 쓰지 않는 걸 추천합니다. (공부용, 테스트용)

2. **Invoke Unity Events**
	1. 프로젝트 협업 시 많이 활용된다.
	2. 대체로 이펙트나 사운드처럼 기획자가 직접 타이밍을 잡아야 하는 요소에만 부분적으로 사용하기

3. **Invoke C# Events**
	1. 캐릭터 이동, 전투 등 핵심 로직은 무조건 이 방식. 코드는 길어지지만 디버깅과 성능에서 압승.
