## 1. LayerMask 배우는 이유

### 1-1. Layer vs LayerMask

[Layer]
![[Pasted image 20260207005734.png]]

![[Pasted image 20260207005739.png]]

- 오브젝트는 단 1개의 레이어(Layer)만 가질 수 있다.
- `LayerMask.NameToLayer("Player")`    이렇게 이름을 통해서 **레이어 인덱스(0~31)** 를 가져옴


[LayerMask]

- 사용 예시
  ```csharp
  
    // 특정 레이어 선택
	int layerMansk = 1 << LayerMask.NameToLayer("Enemy");
  
	// 여러 레이어 선택
	LayerMask mask = LayerMask.GetMask("Enemy", "Obstacle");
  
	if(Physics.Raycast(transform.position, transform.forward, out hit, 10f, mask))
	{
		
	}

  ```

[LayerMask 함수 종류]

1. LayerMask.GetMask
	
	레이러 이름을 묶음을 생성하는 함수
	 **Enemy 가 8번 레이어(Layer)**
	
	```csharp
	LayerMask.GetMask("Enemy", "Obstacle");
	// Enemy 가 8번 레이어(Layer)라면?
	// 1 << 8 (값 256)
	```

2. LayerMask.NameToLayer
	레이어의 인덱스(int)를 반환
	
```csharp
LayerMask.NameToLayer("Enemy");
// int 반환값 = 8
```


3. LayerMask.LayerToName
   레이어의 이름(string)을 반환

```csharp
LayerMask.LayerToName(8);
```

4. LayerMask.value
   비트마스크 값을 나타냄. 주로 비트 연산 수행
   
   ```csharp
   int bitMask = enemyLayer.value | obstacleLayer.value; (OR 연산)
   ```

---
## 2. 비트 연산의 원리

- **1 << n** : n번 레이어의 스위치를 켠다(ON)
  
- **| (OR)**: 필터에 레이어를 추가한다.
  
- **& (AND)**: 이 오브젝트가 포함되어 있어 (필터 역할)
  
- **~ (NOT)**: 지정한 레이어 제외한 나머지 다 선택


```csharp
layerMask = 7;
// 이진수 111 -> 0, 1, 2
```

layerMask = 7

	0000 0000 0000 0000 0000 0000 0000 0000 0111

layerMask = 1 << 7;

	0000 0000 0000 0000 0000 0000 0000 1000 0000

---

### 특정 레이어 포함 여부 체크 (& AND)

```csharp
public static class Extensions
{
	public static bool Contains(this LayerMask mask, int layer)
	{
		return (mask.value & (1 << layer)) != 0;
	}
	
	public static bool Contains(this LayerMask maks, GameObject obj)
	{
		return (mask.value & (1 << obj.layer)) != 0;
	}
}

if(mask.Contains(obj, layer)) {}
```


#### 시나리오 A. Monster/NPC를 때렸을 때, 공격 가능 여부 체크

```csharp
(this LayerMask maks, GameObject obj)

if(mask.Contains(Enemy 마스크, 몬스터/NPC obj)) { // 공격하기 }
```

	Enemy는 8번 레이어

mask             0000 0000 0000 0000 0000 0000 0001 0000 0000
1 << NPC        0000 0000 0000 0000 0000 0000 0000 1000 0000 &
                                         8 7654 3210
           0000 0000 0000 0000 0000 0000 0000 0000 0000        **false (공격 무시)**                      


---
### ~ (NOT) 연산자

```csharp
~(1 << PlayerLayer)

// UI 레이어까지 포함
```

---
### 왜 비트마스크 방식을 사용할까?

```csharp
if(taget =="" || target == "" || target == "" || .....)


if(mask.Contains(target, layer)) {}
```


---

[Mission 1]

1. 오브젝트 Cube, Sphere 생성.
2. LayerMask 선언. (Cube, Sphere)
3. Capsule(Player) 가 Ray를 forward 방향으로 쐈을 때, Cube , Sphere 맞는 대상을 1개만.


[Mission 2]

1. ProjectSetting - Physics Settings 가지고 놀아보기


[(심화) Mission3] 선택

큐브 2개, 원 2개 / Layer는 CubeLayer, SphereLayer 설정

- 키보드 1번을 누르면 CubeLayer와 SphereLayer 레이어가 붙은 녀석들한테 독뎀 부여 (디버깅)
- 키보드 2번을 누르면 CubeLayer 레이어가 붙은 녀석만 독이 풀린다.
