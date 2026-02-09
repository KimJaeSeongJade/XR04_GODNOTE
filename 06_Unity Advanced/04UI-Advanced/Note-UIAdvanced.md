## 1. UI의 기본 좌표: RectTranform

- **Anchor**: 부모 해상도가 변할 때, 부모기준으로 상대위치를 결정
  
- **Pivot**: 내가 회전하거나 커질 때, 어디를 기준으로 움직 것인가 (자기 기준)


---
## 2. Canvas UI 처리 최소 단위

### 2-1. Canvas Render Mode

- **Screen Space-Overlay**
  카메라와 상관없이 화면 가장 위 레이어에 그림.
  
- **Screen Space-Camera**
  특정 카메라 앞에 일정한 거리에 배치.
  👉🏻 연출용 UI, 파티클이 섞인 UI
  
- **World Space**
  씬 내 3D 오브젝트처럼 활용하는 UI
  👉🏻 오브젝트 체력, 데미지 등등

### 2-2. Canvas Scaler

- **Constant Pixel Size**
  해상도가 바뀌어도 UI가 사용하는 픽셀 수를 그대로 유지

- **Scale With Screen Size**⭐⭐⭐
   👉 모바일 게임 표준
  
- **Constant Physical Size**
  화면 해상도와 상관없이 cm/inch 크기를 유지하려고함.

---
## 주의 컴포넌트

1. Layout Group

2. Text
   대안 -> Text Mesh Pro 
   사용하는 이유? -> UI가 변화하는 상황에 Text에 비해서 최적화 되어 있다.

---
## Profiler 에서 UI 최적화 잡는법

- **Layout(파란색)**
  👉🏻 UI 위치/크기 계산하는 시간
  
- **Render(노란색)**
  👉🏻 실제 메쉬 생성 시간
    
> Canvas.SendWillRenderCanvases

---
## Keyword

1. **Anchor vs Pivot 개념**
2. **UI는 RectTransform**
3. **Canvas Render Mode**
4. **Canvas Scale Mode**
5. (실습)Device Simulator로 UI 가지고 놀아보기 (해상도 대응)

(+추가지식)Profiler로 UI 병목현상 잡는법
