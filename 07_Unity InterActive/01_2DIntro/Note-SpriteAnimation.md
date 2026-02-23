# 1. 개요

## Sprite Animation

> 이미지를 시간 순서로 변화시키는/바꾸는 것

---
# 2. Sprite Sheet란 무엇인가?

> 여러 개의 Sprite 이미지를 “한 장의 이미지”에 모아둔 것

## 2-1. 왜 사용하는가?

애니메이션의 본질은:
> 이미지를 시간 순서대로 교체하는 것


## 2-2. 왜 한 장으로 모으는가?

1) 관리가 편하다
2) 로딩이 효율적이다


## 2-3. Unity에서 사용법

1. `Texture Type` → Sprite (2D and UI)
2. `Sprite Mode` → Multiple
3. `Sprite Editor` → Slice (Grid by Cell Size)
4. Apply

이 과정을 거치면  
Sprite Sheet가 “개별 Sprite들”로 분리된다.

---
# 3. Sprite Mode = Multiple 

> 이미지 하나 = 여러 Sprite


## 3-1. Pixels Per Unit (PPU)

![[Pasted image 20260220014637.png|500]]


> “PPU는 픽셀을 월드 단위로 환전하는 기준”
>  “1유닛 = 몇 픽셀인가?”

즉,
```ini
64px = 1 Unity Unit
```

- PPU 64
- 이미지 64px  
    → 월드에서 1유닛

[예시]
- 이미지 크기: 96px  
- PPU: 64

계산:
```kotlin
96 ÷ 64 = 1.5 Unit
```
즉,
씬에서 높이 1.5 유닛으로 보임

### Q. 왜 중요한가?

> "카메라 기준이 Unity Unit이기 때문"

예:
```csharp
orthographicSize = 5;
```
→ 카메라 중점 기준, 위로 5 유닛

---
# 4. Mesh Type 설명 (중요)

## 4-1. Full Rect

> "사각형 메쉬 전체 사용"

[장점]
- 안정적
- 타일맵, 캐릭터에 안전

[단점]
- 투명 영역도 포함


## 4-2. Tight

> "투명 영역 제외하고 메쉬 생성"

[장점]
- 오버드로 감소

[단점]
- 경계가 불규칙
- 타일 경계 어긋날 수 있음
- 픽셀 아트 흔들림 발생 가능


[실무 추천]

|용도|추천|
|---|---|
|캐릭터|Full Rect|
|타일맵|Full Rect|
|파티클|Tight 가능|

---
# 5. Wrap Mode

## 5-1. Repeat
- 텍스처 반복
- 타일처럼 무한 반복 가능


## 5-2. Clamp
- 끝 픽셀 유지
- 대부분 2D 게임 기본값


### Q. 언제 Repeat?
- 배경 패턴
- 무한 스크롤

---
# 6. Filter Mode (매우 중요)

## 6-1. Point (no filter)
- 픽셀 그대로 표현  
- 픽셀 아트 필수

## 6-2. Bilinear
- 부드럽게 보간  

## 6-3. Trilinear
- 3D에서 사용  
- 2D에서는 거의 안 씀

> “픽셀 게임이면 무조건 Point.”



---
# (+추가) Sprite Atlas란 무엇인가?

> 여러 Sprite를 하나의 큰 텍스처로 묶어서 성능을 올리는 시스템

![f:id:kan_kikuchi:20190425143824j:plain](https://cdn-ak.f.st-hatena.com/images/fotolife/k/kan_kikuchi/20190425/20190425143824.jpg "f:id:kan_kikuchi:20190425143824j:plain")

---
### Q. 왜 쓰는가?
`GPU`는 **같은 텍스처를 쓰는 오브젝트를 한 번에 그릴 수 있음**
텍스처가 다르면 → Draw Call 증가 → 성능 하락

### Q. 언제 쓰는가?
- 모바일
- UI 많을 때
- 캐릭터 수 많을 때
