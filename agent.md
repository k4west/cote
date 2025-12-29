# agent.md — 코딩테스트용 C/C++ 코드베이스 리팩터링 AI Agent 가이드

이 저장소의 목적은 **코딩 테스트에서 바로 복붙해서 쓸 수 있는 “짧고 안전한” C/C++ 템플릿**을 만드는 것입니다.  
현재 첨부된 코드 모음은 교육/예제 스타일(전역 배열, 테스트케이스 출력 포맷, 고정 크기, 불필요한 I/O 등)이 섞여 있어, 코테 환경(BOJ/Programmers/SWEA 등)에 맞춰 **재사용 가능한 라이브러리 형태**로 정리하는 게 핵심입니다. fileciteturn0file0 fileciteturn0file1

---

## 0) 리팩터링 원칙 (코테 최적화 규칙)

### A. “문제 풀이 코드”와 “템플릿 코드” 분리
- 템플릿은 **입출력 / 테스트케이스 루프 / printf("#%d")** 같은 문제별 포맷을 포함하지 않음.
- 템플릿은 `namespace ds`, `namespace algo` 아래 **함수/클래스만 제공**.
- 문제 풀이 파일(`main.cpp`)에서 템플릿을 호출.

### B. 전역 고정 배열 제거
- `#define MAX_*` 기반 고정 크기 → `vector`, `string`, `array` 등으로 전환.
- 다만 성능이 중요한 경우:
  - “상한이 문제에서 주어지는” 경우에만 `static vector<int> buf(maxN);` 형태로 재사용 버퍼 허용.

### C. 표준 라이브러리 적극 활용
- 정렬: `sort`, `stable_sort`, `nth_element`
- 우선순위 큐: `priority_queue`
- BFS/DFS: `vector<vector<int>>` + `queue/stack`
- 해시: `unordered_map`, `unordered_set` (필요 시 커스텀 해시)
- 코테에서 직접 구현이 필요한 건 주로:
  - DSU, 세그트리/Fenwick, LCA, Dinic, MinCostMaxFlow, SCC, Trie 등

### D. “안전한 기본값”을 갖춘 형태로
- 오버플로: 거리/비용은 기본 `long long`, INF는 `4e18` 수준.
- 0/1-index 혼용 금지: 템플릿은 기본 0-index, 문제 입력만 보정.
- 함수는 **side-effect 최소화**: 인자로 입력 받고 결과 반환.

---

## 1) Agent 역할 정의 (Workflow)

AI Agent는 아래 3단계로 움직입니다.

### Step 1) Inventory (현 코드 목록화)
- 파일에서 구현된 알고리즘/자료구조를 항목화
- “코테에서 표준 라이브러리로 대체 가능” / “직접 템플릿 필요”를 분류

### Step 2) Refactor Plan (템플릿 스펙 설계)
각 항목에 대해:
- 인터페이스(함수 시그니처/클래스 API)
- 시간복잡도/공간복잡도
- 사용 예시(3~10줄)
를 정의

### Step 3) Implementation (템플릿 코드 생성)
- `include/` 또는 `template/` 아래에 단일 헤더(`cp.hpp`)로 합치거나
- 분야별(`graph.hpp`, `dsu.hpp`)로 분리
- 컴파일 옵션/매크로/빠른 I/O 포함

---

## 2) 추천 디렉토리 구조

```
cp/
  include/
    cp.hpp              # 올인원 템플릿 (최종 배포용)
  src/
    verify/             # 로컬 검증용 main들 (문제별)
  snippets/
    notes.md            # 자주 까먹는 포인트/패턴
  agent.md              # (이 문서)
```

> 처음엔 “올인원 cp.hpp”로 가는 걸 추천합니다. 코테 실전에선 파일 하나가 제일 편합니다.

---

## 3) 코테용 공통 템플릿 (cp.hpp 베이스)

반드시 포함:
- `#include <bits/stdc++.h>`
- `using namespace std;`
- Fast I/O: `ios::sync_with_stdio(false); cin.tie(nullptr);`
- 공통 타입: `using ll = long long;`
- INF 상수: `const ll INF = (ll)4e18;`

선택 포함:
- 디버그 매크로(로컬에서만)
- `chmin/chmax` 유틸

---

## 4) 기존 코드 모음에서 우선 리팩터링 대상 (권장 순서)

첨부 코드 기준, 아래가 “코테 템플릿 가치가 높은 것”입니다.

### 그래프 기본
- BFS/DFS: 인접 리스트 기반 (현재는 2D 맵/고정 큐 방식도 섞여있음) fileciteturn0file0
- Dijkstra: `priority_queue` 버전(현재는 O(V^2) 구현) fileciteturn0file0
- Floyd: 그대로 두되, n<=500 등 제약 체크 주석
- MST: Prim보다 DSU+Kruskal 템플릿 추천(간선 리스트 기반)

### DS
- Stack/Queue/PQ: STL로 대체(직접 구현은 학습용으로 보관) fileciteturn0file1
- Hash/Map/Set: STL로 대체(충돌/성능 이슈는 커스텀 해시 옵션 제공) fileciteturn0file1

### 고급(필요하면)
- Max Flow: Dinic으로 교체(현재 구현은 고정 MAX_V=10 + 버그 가능성) fileciteturn0file0
- Bipartite Matching: Hopcroft–Karp로 교체(현재 O(VE)) fileciteturn0file0
- Plane Sweeping: 목적이 명확한 경우에만 유지 (코테 빈도 낮음) fileciteturn0file0

---

## 5) Agent에게 줄 프롬프트 템플릿 (복붙용)

### (1) 파일/섹션 리팩터링 지시
- 입력: `{원본 코드 블록}`
- 출력: `cp.hpp`에 들어갈 “템플릿 코드” + “사용 예시” + “주의점”

**Prompt**
- 목표: 코딩테스트에서 바로 사용 가능한 C++17 템플릿으로 리팩터링
- 요구:
  1) 전역 고정 배열 제거(필요 시 vector로)
  2) 0-index 기반
  3) 함수/클래스 형태로 제공 (main 제거)
  4) 시간/공간 복잡도 주석
  5) 사용 예시(짧게)
- 추가:
  - 입력 범위가 크면 long long 사용
  - 경쟁 프로그래밍 스타일로 짧고 명확하게

### (2) “표준 라이브러리 대체 여부” 판단 지시
**Prompt**
- 다음 구현은 코테 템플릿으로 유지할까, STL로 대체할까?
- 판단 기준: 실전에서 실수 가능성, 구현 시간, 성능, 빈도

---

## 6) 품질 체크리스트 (Agent 출력 검수)

- [ ] `main()` 없음
- [ ] `#include <bits/stdc++.h>` + C++17 호환
- [ ] 입력 크기에 맞는 타입(`ll`, `int`) 선택
- [ ] 오버플로/INF 안전
- [ ] 0-index 기준 명확
- [ ] 경계 조건(빈 그래프, 단일 노드 등) 처리
- [ ] 사용 예시가 실제로 컴파일/동작 가능한 수준

---

## 7) 다음 액션 (추천)

1) `algo: dijkstra`를 **priority_queue 기반**으로 교체  
2) `graph: BFS/DFS`를 인접 리스트/방문 배열로 단순화  
3) `max flow`를 Dinic 템플릿으로 교체  
4) 최종적으로 `cp.hpp` 올인원 생성 + 로컬 검증(main들)

원하면, 내가 `cp.hpp`의 “뼈대”부터 만들고, 항목을 하나씩 채워 넣는 방식으로 진행해도 돼.
