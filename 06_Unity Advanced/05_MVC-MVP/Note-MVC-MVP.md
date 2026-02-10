
> "게임 로직은 건들지 않고 UI만 갈아끼우는 설계"

### MVC (Model - View - Controller)
### MVP (Model - View - Presenter)

---
## 1. 왜 UI와 로직을 분리해야 하나?

```csharp
using TMPro;
using UnityEngine;

public class Enemy : MonoBehaviour
{
    public int hp = 100;
    public TextMeshProUGUI hpText;  // UI와 강한 결합

    public void OnDamge(int damage)
    {
        hp -= damage;

        // 로직 스크립트 안에서 UI를 건든다..? ->  UI와 강한 결합
        hpText.text = "HP:" + hp.ToString();

        if(hp <= 0)
        {
            Debug.Log("Enemy 죽음..");
        }
    }
}
```

- **유지보수 지옥**
	- 프로젝트 커질 시 추적 불가 (어딨어..?)

---
## 2. MVC vs MVP

![](https://velog.velcdn.com/images%2Fkerri%2Fpost%2F156a6ecf-c46b-4757-9b8f-322d54de79bf%2Fimage.png)

### 1. MVC
- 사용자 입력이 Controller로 전달.
- **의존성**: View가 Model을 직접 참조


### 2. MVP
- 사용자의 입력이 View를 통해 들어와서 Presenter로 전달.
- **의존성**: View와 Model 사이에 참조가 전혀 없음 (의존 안함)

---
## 3-1. MVC (Model - View - Controller) 예시

[Model - 데이터 관리]
```csharp
public class PlayerModel_MVC
{
    public int hp = 100;
}
```

[View - 화면에 출력 - Model 직접 참조중]
```csharp
using UnityEngine;
using UnityEngine.UI;

public class PlayerView_MVC : MonoBehaviour
{
    public PlayerModel_MVC playerModel;
    public Slider hpSlider;

    public void UpdateView()
    {
        hpSlider.value = playerModel.hp / 100f;
    }
}
```

[Controller - 입력 처리 및 Model(데이터) 수정]
```csharp
using UnityEngine;

public class PlayerController_MVC : MonoBehaviour
{
    PlayerModel_MVC playerModel = new PlayerModel_MVC();
    public PlayerView_MVC playerView;

    public void OnDamage(int damage)
    {
        playerModel.hp -= damage;       // Model(데이터) 수정
        playerView.UpdateView();        // View(UI - Slider) 갱신
    }
}
```


---
## 3-2. 같은 코드로 MVP 패턴 구현

[Model 파트 MVC와 동일]

[View - 오로지 그려주기만 한다]
```csharp
using UnityEngine;
using UnityEngine.UI;

public class PlayerView_MVP : MonoBehaviour
{
   [SerializeField] Slider slider;

    public void SetFillAmount(float ratio)
    {
        slider.value = ratio;    
    }
}

// Model을 직접 참조하고 있지 않다
// 데이터 가공도 하지 않는다
// 오로지 그려주기만 한다
```

[Presenter - Model(데이터) 와 View(화면) 중재]
```csharp
using UnityEngine;

public class PlayerPresenter_MVP : MonoBehaviour
{
    PlayerModel_MVC model = new PlayerModel_MVC();
    [SerializeField] PlayerView_MVP playerView;     // 조립할 녀석

    public void TakeDamge(int damage)
    {
        model.hp -= damage;

        float ratio = model.hp / 100f;
        playerView.SetFillAmount(ratio);
    }
}
```

---
## 4. MVP 패턴

### 4-1.역할

- **Model**
	- 데이터 관리 (값만 가짐, 순수 C#)
	- MonoBehaviour 상속받지 않음

- **View**
	- 그리기만 하는 녀석 -> UI 컴포넌트 수정/갱신
	- MonoBehaviour 상속받음

- **Presenter**
	- Model(데이터)의 변화를 감지해서 View(화면)에 전달/명령내리는 녀석


### 사고 방식

- **Model** : 어떤 데이터를 관리하지? (선언된 데이터)

- **View** : 화면에 뭘 그릴건데?

- **Presenter** : Model(데이터) 업데이트와 그려줄 View(화면)를 누가 중재/관리하니?

---
## 5. 실전 예제

### 1) Basic - 단일 연동 (1:1)

[model]
```csharp
public class StatModel
{
    public float hp = 100f;
    public float maxHP = 100f;
    public float GetHpRatio() => hp / maxHP;
}
```

[View]
```csharp
using TMPro;
using UnityEngine;
using UnityEngine.UI;

public class SliderBarView : MonoBehaviour
{
    public Slider slider;
    public void SetValue(float ratio) => slider.value = ratio;
}

public class TextPercentView : MonoBehaviour
{
    public TMP_Text hpText;
    public void SetProgress(float ratio)
    {
        hpText.text = $"HP; {ratio * 100}%";
    }
}
```

[Presenter]
```csharp
using UnityEngine;

public class StastPresenter : MonoBehaviour
{
    StatModel model = new StatModel();
    [SerializeField] SliderBarView viewA;
    [SerializeField] TextPercentView viewB;

    public void OnDamage(float damage)
    {
        model.hp -= damage;
        viewA.SetValue(model.GetHpRatio());
        viewB.SetProgress(model.GetHpRatio());
    }
}
```


### 2) Advanced - 다중 연동 (1:N)

[Model]
```csharp
public class ScoreModel
{
    int score;
    
    public event System.Action<int> OnScoreChanged;     // 전광판

    public void AddScore(int amount)
    {
        score += amount;
        OnScoreChanged?.Invoke(score);
    }
}
```

[Presenter]
```csharp
using UnityEngine;

public class ScorePresenter : MonoBehaviour
{
    [SerializeField] ScoreView scoreView;
    [SerializeField] TargetView targetView;
    [SerializeField] RankingView rankingView;
    [SerializeField] AchievementPopupView popupView;

    ScoreModel model = new ScoreModel();

    void OnEnable()
    {
        model.OnScoreChanged += scoreView.UpdateScoreText;
        model.OnScoreChanged += targetView.UpdateTargetText;
        model.OnScoreChanged += CheckAchievement;  // Q. 왜 View쪽이 아닌 여기에 구현할까?  
    }

    void OnDisable()
    {
        // 구독 해지 안해준다면??  
    }

    void CheckAchievement(int score)
    {
        if(score >= 100) popupView.Show("팝업 떳습니다");
    }
}
```

---
## 6. 안전한 구독: OnEnable / OnDisable

- **OnEnable (구독 시작)**

- **OnDisable (구독 해제)**
  👉🏻 메모리 누수/죽은 참조 호출  방지 핵심, 

>  밥먹듯이 습관적으로 해주기

---
## 7. 정리

- **MVC**: 프로젝트가 커지면, **View**와 **Model**이 꼬이는(참조가 많이 일어나는) 스파게티 위험이 있다.

- **MVP**: Presenter가 **Model**과 **View**를 중재해서 깔끔하게 관리
  👉🏻 대규모 프로젝트로 가도 문제가 발생할 확률이 적다.
