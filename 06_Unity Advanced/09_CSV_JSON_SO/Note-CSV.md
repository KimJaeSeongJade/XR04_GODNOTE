# CSV 뭔데?

> **C**omma **S**eparated **V**alues

```csv
ID,HP,ATK
1,100,10
2,200,20
```

👉🏻 엑셀 -> 텍스트로 저장

---
# CSV 쓰는 이유?

> "몬스터 스탯 -> 필드 변수 다 선언"

[문제]
- 기획자 접근(협업) 불가
- 하드 코딩
- 데이터 변경 용이X -> 나만 알아봐

[CSV 이점]
- 코드(게임 로직) <-> 데이터 분리 -> `기획자`가 데이터 값 수정 용이
- 버전 관리 & 협업 용이

[CSV 단점]
- **계층적 구조**

```text
Weapon
 └─ Sword
     └─ RareSword
```

```csv
Weapon,Sword,RareSword
```


- **중첩 데이터**

```text
Monster
 └─ Skills
     ├─ Fire
     ├─ Ice
```

```csv
ID,Skill1,SKill2 ....
1,Fire,Ice
2,
```


[CSV 한계]

- 타입 정보 없음 (int, float, bool)
- 구조적 표현이 불가
- 런타임 수정 불편
