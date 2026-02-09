## 1. RT(Render Texture) 왜 배우는데?

- CCTV, 저격총 Scope, MiniMap 등 활용
  
👉🏻 근데 많이 쓰면 과부하 일어남.. (이유는 URP 교과목에 충분 설명)

---
## 흐름 도식화

```scss
[Sub Camera] (촬영)
    ↓ Target Texture 설정
[Render Texture] (저장소/필름)
    ↓ 에셋 참조
[Material / UI -> RawImage] (출력/액자)
```

---

## 사용 예시

### [RT - Minimap]
![]( <Resources/1_ZqhWCrhAQLXnCRTocCYyTw.gif> )

### [RT - 저격총 Scope]
https://www.reddit.com/r/Unity3D/comments/6ylhz9/realistic_scope_demo_2_parallax_shadow_and/


### [RT - CCTV]
https://bbs.ruliweb.com/etcs/board/300780/read/51008124
