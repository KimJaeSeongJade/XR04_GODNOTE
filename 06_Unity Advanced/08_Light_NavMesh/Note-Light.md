### Q. RP 어디 단계?

👉🏻 픽셀/프래그먼트 셰이더
👉🏻 **GPU 연산**이 일어난다? -> 과부하 요소가 있다 -> 최적화 할 필요가 있다

### Q. Unity6 는 어떤 RP 방식?

👉🏻 Forward+ RP 방식 (Default)

---
## 목차

1. Unity에서 Light 종류는?

2. Light Setting 세팅 - 알아야할 속성

---
## Unity가 지원해주는 빛 컴포넌트

### Light Mode

1. **Realtime**
	- 매 프레임 객체에 빛과 그림자를 실시간으로 계산하는 녀석
	- 부하가 높음
	- 빛 연산이 민감하게 처리되야 하는 모든 곳 (횃불)
	- 그림자 연산 처리도 신경써줘야한다 (특히 모바일)

2. **Mixed**
	- 정적 객체 -> lightmap으로 굽기
	  동적 객체 -> Realtime으로 굽기
	  👉🏻 하이브리드 방식
	  
	- **Lighting Settings**
		- **Shadowmask(표준)**:  실시간 그림자 + Baked된 그림자 섞음
		- **Subtractive(저사양 모바일)**: 가볍고 퀄리티가 낮음
		- **Baked Indirect(고상양)**

3. **Baked**
	- 빛의 정보를 미리 계산함 -> lightmap (Texture) 형식으로 저장함
	- 런타임 연산이 거의 없다
	- 부하가 매우 낮음
	- 변하지 않는 환경에 세팅된 물체

### 1. Directional Light

👉🏻 모든 씬에서 태양광 역할
   
2. **Point Light**
- **모양**: 구
- **특징**: 전방향 발사
- **사례**: 횃불, 랜턴, 모닥불, 마법 효과, 반딧불

![]( <Resources/img 2.gif> )

3. **Spot Light**
- **모양**: 원뿔형 조명
- **특징**: 원뿔형 발사
- **사례**: 손전등, 가로등

![]( <Resources/img.gif> )

4. **Area Light**
- 이 친구만 **Baked** -> 왜? 연산 비용 비싸서
- **사례**: 실내 형광등, 창문 채광


5. **Light Probe Group**
	- 동적 오브젝트에게 라이트맵(lightmap)을 입히는 기능
	  = **Baked**된 Texture를 입히는 작업 
   
   
6. **Reflection Probe**
![](https://blog.kakaocdn.net/dna/ndhdu/btr6nWYvOyw/AAAAAAAAAAAAAAAAAAAAABSOOrDbXSqMzd8gBn5H7a6m1W8mb1z2Ijz8GcC9izTO/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1772290799&allow_ip=&allow_referer=&signature=dLKjaZK8WEMKODUtjTp4X1MUwtU%3D)![](https://blog.kakaocdn.net/dna/AxItC/btr53DeSjpY/AAAAAAAAAAAAAAAAAAAAAEcQP-jDt83eJXk5BUdgBygh9K80tJrFvJOz9o-gnG8r/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1772290799&allow_ip=&allow_referer=&signature=2VzFatJFVpTNP1J6rTElYmbn4VE%3D)
---
## Light Setting

![]( <Resources/Pasted image 20260127105624.png> )

- **Lighting Settings Asset**

- **Lightmapper**
	- **Progressive GPU** 권장

- **Lightmap Resolution**
	- **모바일**: 5~15 (권장)
	- **PC/콘솔**: 20~40

- **Max Lightmap Size**
	- 텍스처 해상도
	- 표준 1024  (되도록 건들지 않기)

- **Direct/Indirect Samples**
	- 수치가 높을수록 Noise가 줆 -> 근데 Baked 시간 엄청 튐 (되도록 건들지 않기)
