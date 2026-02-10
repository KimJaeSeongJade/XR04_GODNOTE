## [기본 실습] UI-Button 클릭 MVP

### **동작 결과 (결과물 모습)**

1. 화면 중앙의 버튼을 누를 때마다 숫자가 올라갑니다(+1).
    
2. **첫 번째 실험:** 숫자가 텍스트(0, 1, 2, 3...)로 표시되는 화면을 연결해 봅니다.
    
3. **두 번째 실험:** 텍스트 화면을 빼고, 게이지(Slider)가 차오르는 화면으로 갈아 끼웁니다.
    
4. **핵심:** 화면을 교체할 때, 클릭 횟수를 계산하는 **중심 코드(Presenter/Model)는 단 한 줄도 수정하지 않고 그대로 작동**해야 합니다.

<br>

### 2. 필수 스크립트 & 이벤트

#### 2-1. Model (ClickModel.cs)
- **데이터**: int count (현재 클릭 수)

- **핵심 이벤트**: Action<int> OnCountChanged

- **역할**: 숫자가 바뀔 때마다 **"변경된 숫자"** 를 담아 외침

<br>

#### 2-2. View (ClickView_Text.cs)
- **핵심 이벤트**: Action OnButtonClicked

- **역할**: 버튼 클릭 시 **"버튼이 눌렸어요!"** 출력

- **핵심 함수**: void SetDisplay(int value)

- **역할**: 전달받은 숫자로 **텍스트(TMP)** 를 갱신

<br>

#### 2-3. Presenter (ClickPresenter.cs)
- **연결**: ClickView_Text를 직접 참조.

- **로직**: View의 보고를 받아 Model을 깨우고, Model의 외침을 들어 View를 갱신.

---
## [응용 실습] "보스 레이드" - 다중 연동 체력 시스템

### 1. 동작 결과 (결과물 모습)

- 입력: '공격' 버튼을 누르면 보스의 체력이 일정량 깎입니다.

- 출력 (3가지 동시 반응):

1. 메인 체력바: 상단의 커다란 슬라이더(Slider)가 줄어듭니다.

2. HP 수치: 텍스트(TMP)로 현재 체력을 "80 / 100" 형태로 표시합니다.

3. 위험 감지: 체력이 30% 이하가 되면 "위험!" 팝업이나 빨간 경고창을 띄웁니다.

<br>


### 2. 필수 스크립트 & 이벤트

#### 2-1. Model (BossStatModel.cs)

- 데이터: int currentHp, int maxHp

- 이벤트: Action<float> OnHpRatioChanged (0.0~1.0 비율 전달)

- 역할: 체력을 계산하고, 값이 변할 때마다 **"현재 체력 비율"**을 외칩니다.

<br>

#### 2-2. View (독립적인 3종 요정)

- BossHPBarView: SetGauge(float ratio) 함수로 슬라이더 갱신.

- BossHPTextView: SetText(int current, int max) 함수로 숫자 표시.

- BossWarningView: SetWarning(bool active) 함수로 경고창 On/Off.

※ 주의: 각 View는 서로의 존재를 절대 몰라야 하며, 본인한테 할당된 UI 컴포넌트 역할만 수행합니다.

<br>

#### 2-3. Presenter (BossHPPresenter.cs)
- 핵심 로직 (1:N 연결):

   1. 공격 버튼 클릭 보고 수신 ➔ Model의 체력을 깎음.

   2. Model의 이벤트 하나에 3개의 View 업데이트 함수를 모두 연결(+=)함.

 <br>

### 3. 체크리스트
- **독립성**: HPBarView를 삭제해도 HPTextView는 에러 없이 정상 작동하는가?

- **효율성**: Presenter가 Update()에서 매 프레임 체크하지 않고, 오직 이벤트가 터질 때만 UI를 갱신하는가?

- **확장성**: 새로운 UI(예: 보스 표정 변화)를 추가할 때 Presenter의 이벤트 연결 코드 외에 Model을 수정하지 않는가?
