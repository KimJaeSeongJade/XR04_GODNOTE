
### 협업 프로세스

1. VFX/이펙트 아티스트 or Assets
	1. 파티클 모양, 색상, 텍스처 제작

2. 클라 개발자
		1. 제작된 파티클 프리팹 -> 오브젝트 풀 등록
		2. 게임 로직에 맞춰서 호출되는 코드 작성\

---
## 기본 속성

- **Main Module:** `Start Lifetime`, `Start Speed` 등 파티클의 초기 설정을 결정
	- **Simulation Space**: `World` = 잔상 효과
    
- **Emission:** 파티클 생성 빈도를 결정
	- 단발성 타격 효과는 `Rate over Time`❌ `Bursts`(한꺼번에 뿜어내기) 사용 

- **Shape:** 파티클이 방출되는 모양(구체, 원뿔, 박스 등)을 정의
    
- **Color over Lifetime:** 시간이 지남에 따라 파티클의 색상이나 투명도(Alpha)를 변화
	- ex: 연기가 서서히 투명해지는 효과

- **Size over Lifetime:** 입자가 크기를 변화를 줌 (역동성)

- **Renderer:** 파티클의 시각적 외형을 결정

---
## 파티클 초기화 함수

> 파티클 OFF -> 초기화x -> 파티클 켜진 상태 잔상 남은 상태에서 어색하게 Play 됨

```csharp
ps.Clear();
ps.Stop(true, ParticleSystemBehavior.StopEmittingAndClear); // Clear() 함수 포함

ps.Play();
```
