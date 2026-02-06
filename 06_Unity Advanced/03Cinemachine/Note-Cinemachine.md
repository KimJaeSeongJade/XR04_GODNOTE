
# 시네머신이란?

> 카메라 선택과 전환을 자동화한 기능이다.

---
# 이점은?

1. 상황별 연출을 코드 없이 구현 가능
   
2. 플레이어 이동이나 상태 변화에 카메라에 자연스럽게 적용 가능

---
# 카메라 추적 코드

```csharp
void LateUpdate()
{
	Camera.main.tranform.position = player.position + offset;
	
	Camera.main.tranform.LookAt(player);
}
```

## Q. 왜 이 방식을 잘 사용하지 않을까?

1. 하드코딩 로직

2. 예외 처리 지옥

3. 상태 전환 불가능

## Q. 그래서 왜 시네머신 배움?

> 카메라에 어떤 규칙을 부여해서 활용하는 시스템/기능이다.
> 안 그러면 코드로 다 짜야됨...... (지옥)

1. 카메라 로직의 분리

2. 수학적 지식이 크게 요구되지❌

3. 카메라 상태 관리 가능

---
# 시네머신 구성

1. Brain (브레인)
   
2. Virtual (브이캠, VC)


---
## 1. Cinemachine Brain (감독)

- 어떤 가상 카메라(Virtual Camera)를 이용할지 결정.
- 블렌딩(전환 효과) 방식을 결정함.

![]( <Resources/Pasted image 20260206012057.png> )

> **실제 카메라는Brain만 보고, Brain은 가상 카메라(Virtual Camera, 브이캠) 중 하나를 고름.**

---
## 2. Virtual Camera
다른말로 **VC**, **브이캠**

### 2-1. 하는 일
- 누구를 따라갈건데? (Follow)
- 어디를 볼건데? (Look At)
- 어느 거리/각도/부드러움 규칙을 설정

[계층 구조]
```css
Main Camera (실제 카메라)
 └─ Cinemachine Brain (결정 및 전환)
     ├─ Virtual Camera A (상태 1: 평상시)
     ├─ Virtual Camera B (상태 2: 조준 시)
     └─ Virtual Camera C (상태 3: 이벤트 컷신)
```

### 2-2. 브이캠 속성

#### 2-2-1. Follow & LookAt (대상 설정)

- **Follow** : 어떤 대상의 몸체(Body)을 매칭
  
- **LookAt**: 기획에 따라 다르지만 따로 빈 게임 오브젝트로 빼서 설정함


### 브이캠 속성 핵심
- **Damping**: 이동을 부드럽게
- **Dead/Soft Zone**: 회전 안정성
- **Priority**: 브이캠 간 전환을 제어


---
## 3. Blend List Camera

![]( <Resources/1_xinSNuxSsuwQ0P9yg2JrBA.gif> )

![]( <Resources/20190705002334.gif> )

### 3-1. 사례
- 인트로 연출
- 컷신 시작
- 보스 등장

### 3-2. 특징
- 여러 브이캠을 리스트로 묶음

> **연출용 카메라**


---
## 4. Clear shot Camera (시야 확보 전문가)

![]( <Resources/20181115204335.gif> )

![]( <Resources/20181115210725.gif> )


### 4-1. 사례
- 실내 던전
- 좁은 복도

👉🏻 장애물(기둥, 벽) 많은 환경

> **시야 확보 전문가**

---
## 5. Dolly Camera

![]( <Resources/Pasted image 20260118204418.png> )

![]( <Resources/Pasted image 20260118204425.png> )


### 5-1. 사례
- 특정 구간 연출
- 추척할 때

### 5-2. 구성 & 특징
- Dolly Track (경로)
- Dolly Camera (트랙 따라 이동)

> **연출용 카메라** -> **정서적인 몰입감**과 **시각적 역동성**에 특화된 카메라

---
## 6. Free Look Camera (3인칭 조작의 표준)

![]( <Resources/20190706122422.gif> )

![]( <Resources/img 11.gif> )


### 6-1. 사례
- TPS
- 오픈월드
- 액션 게임


### 6-2. 특징
- 3인칭(TPS) 조작 카메라의 표준
- 사용자 주도 하에 회전 가능/제어
- 자동 보간, 구도 유지 -> 구현하려면 수학적 지식 요구함.

---
## 7. Target Group

![]( <Resources/1_dPVMsNx4cNORYNFrTAW-4A.webp> )

![]( <Resources/1_fOuPJ0iZnNA6Je7Flht6hw.gif> )

![]( <Resources/1_ywEjDLvpj9zgX8gwbgb1aA.gif> )


### 7-1. 사례
- 보스 + 플레이어
- 파티 플레이


### 7-2. 특징
- 여러 Target을 하나의 그룹으로 묶음
- 거리, 비율 자동 조절
