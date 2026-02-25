### Q. AI 어디 위에서 길을 찾을까요?

> `그래프` 위에서 움직인다

---
# Node 설계

### Q. 노드(Node) 하나가 가지고 있을 정보는?

- `walkable`: 지나갈 수 있는지 없는지 판별 - **bool**
- `position`: 좌표 - **Vector2Int**
- `gCost`: 시작점부터 현재 위치까지 - **int**
- `hCost (A*)`: 현재 위치에서 목표까지 - **int**
- `FCost (A*)`: gCost + hCost - **int**
- `parent` - **Node**

![]( <Resources/Pasted image 20260224171307.png> )

**Node를 알면, Grid는 Node들의 집합체임을 알 수 있음**

---
### Q. Node 하나로 길찾기 가능함?

**No. 노드들 간 연결이 필요함** (이웃노드)


### Q. 현재 노드 어디 방향으로 갈 수 있음?

**상/하/좌/우**
**상/하/좌/우/우상/좌상/우하/좌하**

---
# 다익스트라

1. 시작점
2. 주변 확장
3. 거리 기록

---
### Q. Node가 퍼져 나가려면 코드에서 있어야 될 데이터는?

- OpenList  - List
- Closed - HashSet


**List** 성격
- 순회 가능
- 인덱스 접근이 용이

**Closed** 왜 **HashSet** 일까?
- 빠른 검사를 위함 -> O(1)
- 중복 처리 방지

---
### Q. Step 함수에서 무슨 일이 일어나야 될까?

**Step()**
1. **OpenList** 에서 어떤 걸 꺼내야하지?
2. **꺼낸 노드**는 어디로 가는데?
3. **이웃 노드**는 어떻게 처리하는데?
4. 이동 비용은 어떻게 계산하는데?

---
## 다익스트라 정리

- `gCost`만 사용
- 전부 탐색

[문제점]
- 목표를 찾는 방식 -> 비효율적 -> 그래서 나온 게 `휴리스틱(hCost)`을 포함한 `A* 알고리즘`


[다익스트라 vs A* 차이]

- **다익스트라**
	- 시작점(S)  기준으로 퍼져 나감 -> 전부 탐색

- A*
	- 목표(T) 방향으로 퍼짐 -> 필요 영역 우선적 탐색

---
# A*

![]( <Resources/Pasted image 20260224171307.png> )

- **gCost**: 시작점 노드와 현재 노드 사이의 거리
- **hCost**: 현재 노드와 타겟 노드 사이의 거리
- **FCost**: 타겟 노드까지 얼마나 걸리는지에 대한 예상값

---
## hCost 

[예시]
```c
S ... T
```

S = (0, 0)
T = (4, 0)

현재 노드 좌표 (1, 0) -> 타겟까지 우측 3칸
👉🏻 `hCost`: 목표까지 몇 칸 남았는지를 계산한 숫자


### Q. 왜 맨해튼 거리?

현재 위치 (1, 2)
타겟 위치 (4, 5)

```C
|4 - 1| + |5 - 2| = 3 + 3 = 6
```

👉🏻 `hCost` = 6

---
A Node - 남은거리 (**3**)
B Node - 남은거리 (**7**)

A Node와 B Node `gCost` 같다.

---
## parent - 경로 역추적

> "parent 노드에 오기 직전 노드"


```C
S -> a -> b -> c -> T
```

```C
T.parent = c
c.parent = b
b.parent = a
a.parent = S
```

 👉🏻 거꾸로 올라감
 `isPath = true` 순회했다고 표시를 남김

---
```csharp
int newCost = currentNode.gCost + 1;

// 더 좋은 길을 찾았거나 || 이웃 노드를 처음 발견했거나
if (newCost < neighbour.gCost || !_openList.Contains(neighbour))
{
	   // 비용 갱신
       neighbour.gCost = newCost;
       neighbour.hCost = GetHeuristic(currentNode, _targetNode);
       // 부모(이전) 노드를 기록
       neighbour.parent = currentNode;
		
       if (!_openList.Contains(neighbour))
	  //리스트에 탐색 후보를 넣는다
          _openList.Add(neighbour);
}
```

**👉🏻 다익스트라 + 휴리스틱 = `A*`**

---
# 정리

```C
// #01
Node - 가지고 있는 정보가 무엇이,고 의미하는바가 무엇인지?
	1) walkable
	2) position
	3) gCost
	// A*용
	   3-1) hCost
	   3-2) FCost
	4) parent
	5) isPath

// #02
Node 데이터를 처리할 자료구조는 무엇이고, 왜 필요한데?
- OpenList
- ClosedSet
  
// #03
Node를 꺼내는 방식과 비용 갱신 방식 방식
- 다익스트라 
	- gCost로만 판단
	- StartNode

- A* 
	- FCost 로 판단 || FCost 같다면 hCost로 판단
	- StartNode, TargetNode
```

