## 1. 유니티 오디오 3요소

1. Audio Listener (귀)

2. Audio Source (스피커)

3. Audio Clip (음원)

---
## 2. 목차

- Audio Clip
- AudioSource
- Audio Pool Manager

---
## Audio Clip

### Load type

- **Deconpress on Load(👍🏻)**
	- 짧은 효과음(SFX)
	- 로드 시 압축품

- **Compressed In Memory**
	- 일반적인 소리
	- 메모리에 압 올리고 -> 재생시 압축 풀림

- **Streaming**
	- 배경음(BGM)
	- 메모리에 올리지 않고 씀

### Compression Format (압축 포맷)

- **PCM**
	- 무압축 고음질 데이터
	- 용량이 큼 -> 짧은 소리에 유용

- **Vorbis(👍🏻)** 
	- 압축률이 굿 -> 실무에서 자주 사용

- **ADPCM**
	- PCM보다 용량 작음 
	- 노이즈 섞인 짧은 소리 유용

- **Force To Mono**
	- 채널이 하나로 합쳐짐 -> 용량이 절반

---
## AudioSource

### 핵심 속성

- **Priority**
	- 0~256설정 가능 -> 주로 배경음은 0 설정

- **Spatial Blend**
	- 0~1 (2D~3D)
		- 0 (2D - BGM, UI)
		- 1 (3D - 발소리, 효과음)

- **Volume & Pitch**
	- Pitch: 0.9~1.1 사이로 조정할 때 사운드 풍부

- **Play On Awake**
	- 체크 시, 게임 시작과 동시에 시작

### 거리 감쇠 (FallOff)

- **Min Distance / Max Distance**: 소리 거리 설정
  
- **Rolloff Mode**
	- **평시**: Logaritmic
	- **거리감을 일정하게 주고싶을 때**: Linear

---
## 오디오 실행 함수

- **Play()**
	- 기존 소리 끊고 새로 시작
	- BGM 교체

- **PlayOneShot()**
	- 기존 소리에 **중첩**해서 재생
	- 대부분 효과음
