# Object Pooling Pattern 구현해보기

- 어떤 자료구조로 **오브젝트 풀**을 구현할지 직접 구현해 본다.

- 생성 및 On/Off 될 게임 오브젝트는 유니티 기본 지원 오브젝트(Cube)와 같이 간단한 오브젝트로 해도 테스트 해봐도 충분!

> 이후 영역 확장: **Audio Prefab**, **Particle Prefab**

---
# Audio

## Unity 오디오 핵심 3요소

1. Audio Listener (귀)

2. Audio Source (스피커)

3. Audio Clip (음원)

> 풀링으로 BGM, SFX 상황별 설정해서 활용해보기

---
# Particle System

- 파티클 속성(Duration, Loop, Start Lifetime, Stasrt Speed, Simulation Space, Max Particles, Stop Action, Culling Mode, Texture Sheet, Emission, Shape, Color over Lifetime, Renderer)들 가지고 놀아보기

- Asset Store 에서 파티클 Import해서 가지고 놀아보기
