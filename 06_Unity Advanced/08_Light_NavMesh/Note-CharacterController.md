
## Character Controller 속성

- **Slope Limit**
	- Default: 45
	- 이 수치를 넘으면 이동 상쇄

- **Step Offset**
	- Default: 0.3
	- 계단 높이. 되도록 0.3을 넘기지 않음

- **Skin Width**
	- 충돌 계산의 완충 지대
	- 수치가 작으면: 벽에 낌
	- 수치가 크면: 캐릭터 공중에 뜬 것처럼 보임

 - **Min Move Distance**
	 - Default: 0.001
	 - 지터링 방지를 위해 기본값(0.001) 유지

---
## 이동 방식 3가지

### 1. Transform

- 이동❌ -> 순간이동
- **사례**: 충돌이 필요 없는 투사체

```csharp
transform.position += direction * speed * Time.deltaTime();
```

### 2. Rigidbody

- 물리 법칙의 지배
- velocity / AddForce()

### 3. Character Controller

- 단독 캐릭터 조작할 때 많이 사용

---
## 이동 방식 3가지 실사용 사례

### 1. Transform

- 배경 연출
- 아이템 획득 UI (주로 UI 연출)
- 카메라 연출

### 2. Rigidbody

- 레이싱 게임
- 앵그리 버드
- 게팅오버잇
- 폴가이즈

### 3. Character Controller

- 바이오하자드
- 오버워치
- 소울류
- FPS류
- RPG 주인공

#### 지터링(Jittering)

> Character Controller + Rigidbody 같이 쓸 때 나타나는 현상.
> 두 컴포넌트가 서로 좌표 잡으려고함.

