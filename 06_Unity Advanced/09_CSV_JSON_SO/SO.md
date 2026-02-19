# 경량 패턴(Flyweight Pattern) 형태 비슷

![]( <Resources/Pasted image 20260219141111.png> )

```csharp
public class TreeModel : MonoBehaviour
{
	Mesh _mesh;
	Texture _bark;
	Texutre _leaves;
}
```

```csharp
public class Tree
{
	TreeModel _model;
	
	Vector3 _position;
	double _height;
	double _width;
	
	Color _barkTint;
	Color _leafTink;
}
```

![]( <Resources/Pasted image 20260219141309.png> )
 
## ScriptableObject

- 초기 설정값이 되는 것들
	- 몬스터 종류
	- 프리팹
	- HP/MP
	- 이동속도
	- 연출 애니메이션
	- SFX
	  등등


---
## CSV - JSON - ScriptableObject

### CSV
- 수치 테이블
	- 밸런스 기획

### JSON
- 직렬화 포맷
	- 데이터를 저장, 통신

### ScriptableObject
- 유니티에서 제공하는 초기 데이터 설정
	- 초기 데이터 타입

