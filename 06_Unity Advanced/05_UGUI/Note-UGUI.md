## 1. Image & RawImage

### 1-1. Image

#### Image Type (4가지)

- **Simple**: 이미지를 있는 그대로 출력 (배경, 아이콘)

- **Sliced**: 테두리는 유지하고 중심만 늘리는 방식 (버튼)
  
![](https://blog.kakaocdn.net/dna/AJQ2T/btqNqY9j365/AAAAAAAAAAAAAAAAAAAAAPXd8x84pwk1zE-22x0c7pY7Vr1dtspFlKKPwHgEPMNY/img.png?credential=yqXZFxpELC7KVnFOS48ylbz2pIh7yKj8&expires=1772290799&allow_ip=&allow_referer=&signature=mBQ0AuqLEMwKNhgJw%2Fa%2B59N%2FAhk%3D)

- **Tiled**: 이미지 (Center) 타일 형태로 반복. 언제? -> 이미지 늘릴때

- **Filled**: 이미지 일부를 노출 (체력바, 스킬 쿨타임)
	- Fill Amount (0~1)
	- Radial 모드


### Image(Sprite) vs Raw Image(Texture)

##### Image
- 버튼
- 배경
- 체력바
- 아이템
- 아이콘

단점: Sprite 변환 과정 필요 (자원 소모가 상대적으로 있음)

##### Raw Image
- CCTV 화면
- 저격총 Scope
- 영상
- 미니맵 

단점: 각 텍스처당 Draw Call 발생

---
## 2. Slider

- Value 값 (0~1)

### 사용 사례

[환경 설정]

![Unity Volume Slider Example](http://johnleonardfrench.music/wp-content/uploads/fader-gif.gif)

[체력바 - 플레이어, 적, 보스 등등]
![](https://velog.velcdn.com/images/yarogono/post/77c3032c-9c9d-43aa-9258-317670fc2ac8/image.gif)

[과열기능 - 게임적 허용]
![](https://miro.medium.com/v2/resize:fit:1400/1*nQF3UIxfhqh-jLjzl_6a6g.gif)


---
## 3. Scroll View

- **Content** : 실제 아이템들이 담기는 도화지
  
- **Movement Type**
	- Elastic
	- Clamped

### 사용 사례
![](https://miro.medium.com/v2/resize:fit:1200/1*dIbuSqiuWBchxKVfMq216w.gif)


### 실습

- **Scroll Rect 컴포넌트**: 'Content', 'Movement Type' 
- **Content 파트**
	- Layout 컴포넌트 (Horizontal, Vertical, Grid)
	- Content 자식으로 UI 컴포넌트 막 넣어서 어떻게 동작하는지 테스트

---
## 4. TextMeshPro (TMP)

### Q. 왜 Text 대신 TMP를 사용할까?

> "렌더링 방식에서 최적화가 일어났기 때문에" 
> Unity6 기본으로 사용중

