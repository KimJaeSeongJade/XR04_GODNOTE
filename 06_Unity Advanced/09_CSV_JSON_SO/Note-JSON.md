# 개요

> 객체 -> 텍스트로 바꿔 외부와 교환하는 방법

---
# 실무 가지는 문제들

## 1) 하드코딩된 필드 변수

```csharp
int level = 5;
```


## 2) 메모리 휘발

- Unity 객체는 메모리
- 게임 종료 시, 데이터 삭제


## 3) 서버 데이터

```json
{
	"gold": 500,
	"items": ["sword", "potion"]
}
```

👉🏻 문자열 -> 객체 복원해줘야함

---

```csharp
public class Player : MonoBehaviour
{
	public int _level;
	public int _gold;
	
	public string _path;
	
	public void Save()
	{
		string json = JsonUtility.ToJson(this);
		File.WriteAllText(_path, json);
	}
}
```

👉🏻 Player.cs 게임 로직 담당

[위 코드의 문제점]
- JSON = 데이터를 직접적으로 만짐


[역할 분리]

### 1. Player.cs
```csharp
public class Player : MonoBehaviour
{
	public int _level;
	public int _gold;
}
```

👉🏻 게임 플레이만 담당

### 2. JsonHandler.cs
```csharp
public static class JsonHandler
{
	// 파일 변환
	public static string ToJson(Data data)
	{
		return JsonUtility.ToJson(data);
	}
	
	// 파일 저장
	public static void Save(string path, string json)
	{
		File.WriteAlltext(path, json);
	}
}
```

---
# 유니티에서 제공하는 JSON 2가지

## 1) JsonUtility

[특징]
- Unity 기본 제공
- 빠르다
- 제한 많음

[장점]
- 간단한 구조 적합
- 세이브/설정에 충분히 활용 가능

[한계점]
- Dictionary 미지원
- 프로퍼티(Property) 미지원
- 상속 제한

## 2) Newtonsoft JSON

Package Manager git URL 설치:

> com.unity.nuget.newtonsoft-json


---

- `JsonUtility`는 Unity 저장 규칙 따름 (Inspector에 뜨냐/안 뜨냐)
- `Newtonsoft JSON` Unity 규칙 무관 -> C# 객체를 직접 JSON 바꿈

---
### Q. 무조건 Newtonsoft JSON 쓰면 되지 않을까?

- 기획자랑 협업? -> 데이터 수치를 기획자가 만질 일이 많아? -> `CSV`

- 서버 통신 / 복잡한 데이터 -> `Newtonsoft JSON`

- 간단한 설정 / 캐싱 -> `JsonUtility`


[사용처]
#### CSV
- 아이템 정보 스탯
- 몬스터 정보
- 퀘스트 리스트

#### Newtonsoft JSON
> C# 객체가 JSON 변환해주기 때문에 JsonUtility가 제공하지 않는 녀석들도 제공함
- NPC 대사
- 유저 인벤토리

#### JsonUtility 
> Unity에서 제공하는 직렬화 녀석들만 가능
> = Inspector에서 보이는지 여부
- 설정창
- 업적
