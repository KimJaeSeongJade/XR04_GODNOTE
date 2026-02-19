# 사용처

> 게임 설정 -> 인게임에 크게 영향 받지 않는 곳에 활용됨

⭕
- **설정창**
- **튜토리얼 완료 여부**

❌
- 중요한 정보들 -> 변동성이 심한 데이터

---
# 사용법

```csharp
        PlayerPrefs.SetFloat("키", value);
        PlayerPrefs.SetInt("키", value);
        PlayerPrefs.SetString("키", value);

        PlayerPrefs.GetFloat("키", value);
        PlayerPrefs.GetInt("키", value);
        PlayerPrefs.GetString("키", value);
```


---
# 유니티 PlayerPrefs 저장 장소

| 플랫폼         | 저장 위치 및 형식                                                                                                                  |
| ----------- | --------------------------------------------------------------------------------------------------------------------------- |
| **Windows** | **레지스트리**에 저장됩니다. 레지스트리 편집기에서 아래 경로를 확인하세요.  <br>`HKEY_CURRENT_USER\Software\[CompanyName]\[ProductName]`                   |
| **macOS**   | **plist 파일**로 저장됩니다. Apple 공식 가이드에 따라 라이브러리 폴더를 확인하세요.  <br>`~/Library/Preferences/unity.[CompanyName].[ProductName].plist` |
| **Android** | **SharedPreferences** 시스템을 사용하며 XML 파일로 저장됩니다. (루팅 필요)  <br>`/data/data/[PackageName]/shared_prefs/[PackageName].xml`       |
| **iOS**     | 앱의 **Library/Preferences** 폴더 내 `.plist` 파일에 저장됩니다.                                                                         |
| **Linux**   | **config 파일**로 저장됩니다.  <br>`~/.config/unity3d/[CompanyName]/[ProductName]`                                                  |
