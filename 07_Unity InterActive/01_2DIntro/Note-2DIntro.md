# 3D  환경과 차이점?

> **Depth Test** -> 기본적으로 해당 단계를 비활성화/생략

👉🏻 2D 렌더링은` Sorting Layer / Order in Layer`를 사용해 그려지는 순서를 결정

---
# 유니티 2D를 제작 전 사고 방식 (게임 센스)

- Sprite 그림(연출)
- Collider 감지

👉🏻 결국엔 게임 규칙을 따름

---
# 유니티 2D 컴포넌트

- Collider 2D
	- Circle
	- Box
	- Capsule
	- Edge
	- Polygon
	- Composite (⭐)

- Rigidbody 2D

- Sprite Renderer

[+추가]
- PHysics Material 2D
- Sorting Layer 설정하는 법
- 2D Physics 설정 (Project Settings)


## Edge Collider 2D

- 선 하나 시작 -> 원하는 콜라이더를 직접 지정 가능


## PolygonCollider 2D

- **복잡한 모양** 자동 생성
- 성능이 상대적으로 무거움

## Composite Collider 2D

- 하위 객체 콜라이더를 하나로 관리해주는 녀석

