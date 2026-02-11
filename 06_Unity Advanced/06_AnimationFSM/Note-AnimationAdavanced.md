## 1. Blend Tree

> 한 노드 안에 여러 클릭을 넣고 수치에 따라 실시간으로 적용시킴 (블렌딩)


### Parameter

- **1D Blend Type**: 주로 이동(속도)과 같은 카테고리로 제어할 때 사용
	- **Threshold (임계값)**: float형 수치로 조절해서 두 동작 사이 전환 가능

- **2D Simple Directional (2D 단순 방향)**
	- 상하좌우(위, 아래, 왼쪽, 오른쪽)와 같은 방향성 애니메이션
	- **사용 사례**: 4방향 혹은 8방향 걷기/달리기.

- **2D Freeform Directional (2D 자유 형식 방향)**
	- 방향은 같지만 속도(걷기/달리기)가 다른 애니메이션을 혼합할 때 유용
	- **사용 사례**:  같은 방향으로의 걷기/달리기 속도 차이가 있는 경우

---
## 2. Avatar Mask

> 애니메이션이 영향을 미칠 **'부위'** 를 선택하는 필터

![]( <Resources/Pasted image 20260211155143.png> )

### 2-1. 실사용 예시

[Base Layer: Idle, Move(Walk, Run)]

![]( <Resources/img.gif> )

[Shield Layer: 다리 부분을 제외 -> 방패막기와 방패밀치기 Animation]

![]( <Resources/img 1.gif> )


[Avatar Mask: Base Layer 기반 -> Upper Layer Override/Additive]

![]( <Resources/Pasted image 20260211154021.png> )

