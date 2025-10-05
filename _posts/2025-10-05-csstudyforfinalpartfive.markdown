---
layout: post
title: "기술면접 대비 CS 공부 - 04"
date: 2025-10-05 16:00:00 +0900
categories: [Tech Interview, Study Plan]
tags: [c++, c-sharp, computer-science, algorithm, data-structure, operating-system, network, database, design-pattern, unity, unreal]
slug: csstudyforfinalpartfive
---

# 면접 대비 사전 QnA 정리 - 자료구조/알고리즘


# 🔷 자료구조/알고리즘 면접 예상 질문 50선 – 모범답변

---

<details>
<summary><strong>1️⃣ 빅-오(Big-O), 빅-오메가(Ω), 빅-세타(Θ)의 차이를 설명해주세요.</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **Big-O:** 최악의 경우 시간복잡도  
- **Big-Ω:** 최선의 경우 시간복잡도  
- **Big-Θ:** 평균적인(상한과 하한이 동일한) 시간복잡도  

---

<strong>🔹 상세설명</strong>  
- **Big-O (Upper Bound)** : 알고리즘이 수행되는 *최대 실행 시간*의 상한을 의미합니다.  
  예: O(n²) → 입력이 커져도 n²보다 빠르다.  
- **Big-Ω (Lower Bound)** : 알고리즘이 보장하는 *최소 실행 시간*입니다.  
  예: Ω(n) → 최소 n번은 수행된다.  
- **Big-Θ (Tight Bound)** : 상한과 하한이 일치하는 경우, 평균적인 수행 시간입니다.  
  예: Θ(n log n) → 평균적으로 n log n 정도의 성능을 가진다.  

---

<strong>💬 면접식 답변</strong>  
Big-O는 “최악의 경우”, Big-Ω는 “최선의 경우”, Big-Θ는 “평균적인 경우”를 의미합니다.  
일반적으로 우리는 Big-O를 가장 많이 사용하며, 이는 알고리즘의 상한 복잡도를 통해  
성능을 보수적으로 평가하기 위함입니다.

</details>

---

<details>
<summary><strong>2️⃣ 재귀(Recursion)와 반복(Iteration)의 차이는 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- 재귀는 **함수 자신을 호출**하는 방식,  
- 반복은 **루프(while, for)** 를 통해 반복 수행하는 구조입니다.

---

<strong>🔹 상세설명</strong>  
- **재귀:**  
  - 함수 호출 스택을 사용  
  - 가독성 높지만 오버헤드가 큼  
  - 종료 조건(Base Case) 필수  
- **반복:**  
  - 변수만으로 루프 제어  
  - 메모리 효율적  
  - 상태를 명시적으로 관리해야 함  
- **꼬리 재귀 최적화:**  
  - 컴파일러가 마지막 호출을 반복문처럼 처리하여 스택 사용을 줄이는 최적화 기법.  

---

<strong>💬 면접식 답변</strong>  
재귀는 함수가 자기 자신을 호출하면서 문제를 분할하는 방식이고,  
반복은 명시적인 루프를 통해 상태를 갱신하며 해결합니다.  
재귀는 코드가 간결하지만 스택 오버플로우 위험이 있고,  
반복은 안정적이지만 구조가 다소 복잡할 수 있습니다.

</details>

---

<details>
<summary><strong>3️⃣ 분할 정복(Divide and Conquer)이란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
문제를 **작은 부분으로 분할(Divide)** → 각 부분을 **재귀적으로 정복(Conquer)** →  
**결합(Combine)** 하는 알고리즘 설계 기법입니다.

---

<strong>🔹 상세설명</strong>  
- **대표 알고리즘:**  
  - Merge Sort, Quick Sort, Binary Search, FFT 등  
- **복잡도:** 대부분 O(n log n) 형태  
- **핵심 아이디어:**  
  - 동일한 하위 문제로 쪼개면 효율적  
  - 독립적인 부분 문제는 병렬화 가능  

---

<strong>💬 면접식 답변</strong>  
분할 정복은 큰 문제를 여러 개의 작은 문제로 나누고,  
각각을 해결한 뒤 결과를 합치는 방식입니다.  
대표적인 예로 Merge Sort나 Quick Sort가 있습니다.

</details>

---

<details>
<summary><strong>4️⃣ 재귀 호출의 공간 복잡도는 왜 높을까요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
재귀는 호출될 때마다 **스택 프레임이 새로 생성**되기 때문에,  
함수 호출 깊이만큼의 추가 메모리를 사용합니다.

---

<strong>🔹 상세설명</strong>  
- 각 호출은 독립적인 **지역 변수, 매개변수, 반환 주소**를 포함한 스택 프레임을 생성  
- 깊은 재귀 → 스택 메모리 초과(Stack Overflow) 위험  
- 꼬리 재귀 최적화를 통해 일부 언어(C#, Scheme 등)는 이를 반복문으로 변환 가능  

---

<strong>💬 면접식 답변</strong>  
재귀 호출은 함수마다 스택에 새로운 프레임을 할당하기 때문에  
호출 깊이에 따라 공간 복잡도가 증가합니다.  
그래서 반복문으로 대체하거나 꼬리 재귀 최적화를 활용해 메모리 사용을 줄이기도 합니다.

</details>

---

<details>
<summary><strong>5️⃣ 슬라이딩 윈도우(Sliding Window) 알고리즘이란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
배열이나 문자열에서 **연속된 구간(subarray)** 을 효율적으로 탐색하기 위한  
O(n) 시간 복잡도의 최적화 알고리즘입니다.

---

<strong>🔹 상세설명</strong>  
- **기본 원리:**  
  - 두 포인터를 사용해 윈도우의 시작과 끝을 관리  
  - 한쪽 포인터만 이동시켜 중복 계산 제거  
- **적용 사례:**  
  - 최대/최소 구간 합, 문자열 내 중복 문자 검사, 부분합 문제 등  
- **복잡도:**  
  - 시간 O(n), 공간 O(1) 또는 O(k)

---

<strong>💬 면접식 답변</strong>  
슬라이딩 윈도우는 배열 내의 연속된 부분을 효율적으로 탐색하는 기법으로,  
두 포인터를 이용해 이전 계산을 재활용하면서 불필요한 연산을 줄입니다.  
예를 들어, 최대 부분합이나 문자열 내 유효 윈도우 탐색 문제에서 자주 사용됩니다.

</details>

---

<details>
<summary><strong>6️⃣ 투 포인터(Two Pointers)와 이진 탐색(Binary Search)의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- 투 포인터: **선형적 탐색 기반의 구간 확장 기법**  
- 이진 탐색: **정렬된 배열에서 중간값 기준 절반씩 제거하는 탐색 기법**

---

<strong>🔹 상세설명</strong>  
- **Two Pointers:**  
  - 정렬/연속 데이터 기반  
  - 시작점과 끝점을 조절해 조건 만족  
  - O(n)  
- **Binary Search:**  
  - 정렬된 데이터 전제  
  - 중간값 기준으로 반씩 나누어 탐색  
  - O(log n)  

---

<strong>💬 면접식 답변</strong>  
투 포인터는 선형 탐색 기반으로 연속된 범위를 다루는 반면,  
이진 탐색은 정렬된 데이터에서 중간값을 기준으로 절반을 제거하는 방식입니다.  
문제 조건이 “연속 구간”이면 투 포인터,  
“정렬된 데이터의 특정 값 탐색”이면 이진 탐색을 씁니다.

</details>

---

<details>
<summary><strong>7️⃣ KMP 알고리즘은 어떤 방식으로 문자열 검색을 빠르게 하나요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
KMP는 불일치가 발생했을 때 **이전 비교 정보를 활용하여 불필요한 재탐색을 피하는 문자열 탐색 알고리즘**입니다.

---

<strong>🔹 상세설명</strong>  
- **핵심:** Prefix-Suffix 배열(Partial Match Table)을 이용해 점프  
- **시간복잡도:** O(n + m)  
- **장점:** 불일치 발생 시 이미 일치한 접두사 정보를 활용  
- **비교:** Brute-Force는 O(nm)  

---

<strong>💬 면접식 답변</strong>  
KMP는 문자열 검색 시 접두사/접미사 일치 정보를 미리 계산해두고,  
불일치가 발생하더라도 중복 비교를 피하는 방식으로 동작합니다.  
결과적으로 모든 문자를 한 번씩만 비교하므로 O(n + m) 시간에 동작합니다.

</details>

---

<details>
<summary><strong>8️⃣ 단일 연결 리스트에서 중간 노드를 삭제하려면 어떻게 하나요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
중간 노드를 삭제하려면 **다음 노드의 데이터를 현재 노드에 복사하고, 다음 노드를 건너뛰는 방식**을 사용합니다.

---

<strong>🔹 상세설명</strong>  
- **이유:** 단일 리스트는 이전 노드를 알 수 없음  
- **과정:**  
  1. next 노드의 값 복사  
  2. next 노드를 삭제  
- **복잡도:** O(1)  

---

<strong>💬 면접식 답변</strong>  
단일 연결 리스트에서는 이전 노드를 모를 수 있기 때문에,  
삭제하려는 노드에 다음 노드의 값을 덮어쓰고,  
그 다음 노드를 삭제하는 방식으로 중간 노드를 제거할 수 있습니다.

</details>

---

<details>
<summary><strong>9️⃣ BFS와 DFS의 차이점은?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- BFS: **큐를 이용한 너비 우선 탐색**  
- DFS: **스택(또는 재귀)을 이용한 깊이 우선 탐색**

---

<strong>🔹 상세설명</strong>  
- **BFS:**  
  - 가까운 노드부터 방문  
  - 최단 경로 탐색에 적합  
  - 큐 사용  
- **DFS:**  
  - 한 경로를 끝까지 탐색  
  - 경로 탐색/백트래킹에 유용  
  - 재귀 또는 스택 사용  

---

<strong>💬 면접식 답변</strong>  
BFS는 가까운 노드부터 탐색하며 큐를 사용하고,  
DFS는 한 경로를 끝까지 탐색하며 스택을 사용합니다.  
그래서 BFS는 최단 경로, DFS는 경로 탐색 문제에 적합합니다.

</details>

---

<details>
<summary><strong>🔟 힙 정렬(Heap Sort)과 우선순위 큐의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
둘 다 힙을 기반으로 하지만,  
**힙 정렬은 전체 데이터를 정렬**하고,  
**우선순위 큐는 필요한 원소만 순차적으로 관리**합니다.

---

<strong>🔹 상세설명</strong>  
- **힙 정렬:**  
  - 전체 정렬 수행 O(n log n)  
  - 불안정 정렬  
- **우선순위 큐:**  
  - 삽입/삭제 중심 O(log n)  
  - 전체 정렬은 아님  
- **차이점:**  
  - 목적의 차이 (전체 정렬 vs 실시간 우선순위 관리)

---

<strong>💬 면접식 답변</strong>  
힙 정렬은 전체 데이터를 힙 구조로 정렬하는 알고리즘이고,  
우선순위 큐는 삽입과 삭제 시 우선순위를 유지하는 자료구조입니다.  
즉, 정렬 전체를 목적으로 하느냐, 실시간 관리가 목적이냐의 차이입니다.

</details>

---

<details>
<summary><strong>11. 이진 탐색 트리(BST)와 균형 트리(AVL, Red-Black Tree)의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
BST는 기본적으로 정렬된 트리 구조이지만,  
삽입 순서에 따라 편향될 수 있습니다.  
균형 트리는 삽입/삭제 시 높이를 자동으로 조정해 항상 O(log n) 복잡도를 유지합니다.

---

<strong>🔹 상세설명</strong>  
- **BST (Binary Search Tree)**  
  - 왼쪽 < 루트 < 오른쪽  
  - 평균 O(log n), 최악 O(n)  
- **AVL Tree**  
  - 높이 균형 트리, 회전으로 항상 균형 유지  
  - 탐색 빠름, 삽입/삭제는 다소 느림  
- **Red-Black Tree**  
  - 약한 균형 유지, 삽입/삭제 효율적  
  - STL의 map, set이 내부적으로 사용  

---

<strong>💬 면접식 답변</strong>  
BST는 정렬 탐색에 유용하지만 삽입 순서에 따라 편향될 수 있습니다.  
AVL과 Red-Black은 자동 균형 조정을 통해 항상 O(log n)을 보장하며,  
특히 Red-Black 트리는 C++ STL map과 set에서 사용됩니다.

</details>

---

<details>
<summary><strong>12. 세그먼트 트리와 펜윅 트리(Fenwick Tree)의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
둘 다 구간 합이나 최소/최대값을 빠르게 계산하기 위한 자료구조지만,  
세그먼트 트리는 구조가 일반적이고, 펜윅 트리는 더 단순하고 메모리 효율적입니다.

---

<strong>🔹 상세설명</strong>  
- **세그먼트 트리:**  
  - 완전 이진 트리 기반  
  - 구간 합, 최소/최대 등 다양한 연산  
  - 공간 복잡도: O(4n)  
- **펜윅 트리:**  
  - 비트 연산 기반 (LSB 활용)  
  - 누적 합만 처리 가능  
  - 공간 복잡도: O(n)  

---

<strong>💬 면접식 답변</strong>  
세그먼트 트리는 범용적이며 구간 합, 최소값 등 다양한 연산에 쓰이고,  
펜윅 트리는 합계 전용이지만 구현이 간단하고 공간 효율이 좋습니다.

</details>

---

<details>
<summary><strong>13. 그래프 탐색에서 BFS와 DFS는 각각 언제 유리한가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
BFS는 최단 거리 탐색에,  
DFS는 깊은 탐색이나 백트래킹 문제에 유리합니다.

---

<strong>🔹 상세설명</strong>  
- **BFS:**  
  - 큐 사용, 너비 우선  
  - 최단 거리 탐색 (가중치 없음)  
- **DFS:**  
  - 스택/재귀 사용, 깊이 우선  
  - 백트래킹, 순열 생성에 적합  
- **복잡도:** 둘 다 O(V + E)

---

<strong>💬 면접식 답변</strong>  
BFS는 레벨 단위로 탐색해 최단 경로 문제에 적합하고,  
DFS는 경로 탐색, 순열, 백트래킹 문제에 자주 활용됩니다.

</details>

---

<details>
<summary><strong>14. 다익스트라 알고리즘과 벨만-포드 알고리즘의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
둘 다 최단 경로 알고리즘이지만,  
다익스트라는 음수 가중치를 처리하지 못하고,  
벨만-포드는 음수 가중치까지 처리할 수 있습니다.

---

<strong>🔹 상세설명</strong>  
- **다익스트라:**  
  - O(E log V) (우선순위 큐)  
  - 음수 간선 불가  
- **벨만-포드:**  
  - O(VE)  
  - 음수 가중치/사이클 탐지 가능  
- **플로이드-워셜:**  
  - 모든 정점 쌍 최단경로 (O(V³))

---

<strong>💬 면접식 답변</strong>  
다익스트라는 양수 간선의 최단 경로를 빠르게 구할 수 있지만,  
음수 간선이 있다면 벨만-포드 알고리즘을 사용해야 합니다.  
벨만-포드는 음수 사이클 탐지도 가능합니다.

</details>

---

<details>
<summary><strong>15. 최소 신장 트리(MST)에서 Kruskal과 Prim 알고리즘의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
Kruskal은 간선 중심 접근, Prim은 정점 중심 접근입니다.

---

<strong>🔹 상세설명</strong>  
- **Kruskal:**  
  - 간선을 가중치 순으로 정렬  
  - Union-Find로 사이클 방지  
  - 희소 그래프에 유리  
- **Prim:**  
  - 임의의 정점에서 시작, 인접 최소 간선 선택  
  - 밀집 그래프에 유리  

---

<strong>💬 면접식 답변</strong>  
Kruskal은 간선 위주로 최소 간선을 선택하고,  
Prim은 정점을 확장하며 최소 연결을 유지합니다.  
그래프가 희소하면 Kruskal, 밀집하면 Prim이 효율적입니다.

</details>

---

<details>
<summary><strong>16. 그리디 알고리즘이 항상 최적해를 보장하기 위한 조건은?</strong></summary>

<strong>🧠 핵심 요약</strong>  
그리디 알고리즘은 “매 순간 최선의 선택이 전체적으로도 최선”일 때만 최적해를 보장합니다.

---

<strong>🔹 상세설명</strong>  
- **필요 조건:**  
  - **탐욕적 선택 속성(Greedy Choice Property)**  
  - **최적 부분 구조(Optimal Substructure)**  
- **예시:**  
  - 거스름돈 문제(단위 일정할 때)  
  - 크루스칼/프림(MST), 허프만 코딩  

---

<strong>💬 면접식 답변</strong>  
그리디 알고리즘은 매 단계의 지역 최적해가 전역 최적해로 이어질 때만 성립합니다.  
그래서 탐욕적 선택 속성과 최적 부분 구조를 반드시 만족해야 합니다.

</details>

---

<details>
<summary><strong>17. 동적 계획법(DP)과 분할 정복(D&C)의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
둘 다 문제를 나누는 방식이지만,  
DP는 **중복 부분 문제를 메모이제이션**으로 최적화합니다.

---

<strong>🔹 상세설명</strong>  
- **분할 정복:**  
  - 부분 문제 독립적  
  - 재귀 기반, 중복 계산 많음  
- **DP:**  
  - 부분 문제 중복 존재  
  - 결과를 저장(메모이제이션, 테이블화)  
  - 예: 피보나치, 배낭 문제  

---

<strong>💬 면접식 답변</strong>  
분할 정복은 문제를 나누어 해결하고 결합하는 구조지만,  
DP는 중복되는 계산을 캐싱하여 효율을 높입니다.  
즉, “분할 정복 + 저장”이 DP입니다.

</details>

---

<details>
<summary><strong>18. LIS(최장 증가 부분 수열)의 효율적인 풀이법은?</strong></summary>

<strong>🧠 핵심 요약</strong>  
이진 탐색을 활용해 O(N log N) 복잡도로 풀이할 수 있습니다.

---

<strong>🔹 상세설명</strong>  
- **기본 DP 방식:** O(N²)  
- **개선 방식:**  
  - 이진 탐색으로 부분 수열의 끝 원소 갱신  
  - `lower_bound`를 활용  
- **복원:** 추적 배열로 실제 수열 복원 가능  

---

<strong>💬 면접식 답변</strong>  
LIS는 단순 DP로는 O(N²)이지만,  
이진 탐색을 이용하면 O(N log N)으로 줄일 수 있습니다.  
C++의 `lower_bound`를 활용한 방식이 대표적입니다.

</details>

---

<details>
<summary><strong>19. STL vector, list, deque의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **vector:** 연속된 메모리, 접근 빠름  
- **list:** 양방향 연결 리스트  
- **deque:** 양 끝 삽입/삭제가 빠름

---

<strong>🔹 상세설명</strong>  
| 컨테이너 | 내부 구조 | 랜덤 접근 | 삽입/삭제 | 특징 |
|-----------|------------|-------------|-------------|--------|
| vector | 배열 | O(1) | O(n) | 캐시 효율 우수 |
| list | 연결 리스트 | X | O(1) (노드 기준) | 양방향 순회 가능 |
| deque | 블록 메모리 | O(1) | O(1) (양 끝) | 양 끝 삽입 빠름 |

---

<strong>💬 면접식 답변</strong>  
vector는 접근이 빠르고, list는 중간 삽입이 빠릅니다.  
deque는 양쪽 끝 삽입/삭제에 최적화되어 있습니다.  
상황에 따라 접근 빈도나 삽입 패턴을 고려해 선택합니다.

</details>

---

<details>
<summary><strong>20. map과 unordered_map의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **map:** 내부적으로 Red-Black Tree  
- **unordered_map:** 내부적으로 Hash Table  

---

<strong>🔹 상세설명</strong>  
| 구분 | map | unordered_map |
|------|-----|----------------|
| 내부 구조 | Red-Black Tree | Hash Table |
| 정렬 여부 | Key 정렬 | 순서 없음 |
| 탐색 복잡도 | O(log n) | 평균 O(1), 최악 O(n) |
| 메모리 사용 | 적음 | 많음 (버킷 관리) |

---

<strong>💬 면접식 답변</strong>  
map은 키가 자동 정렬되는 트리 기반 컨테이너로,  
탐색과 삽입이 O(log n)입니다.  
unordered_map은 해시 기반으로 평균 O(1) 속도를 가지지만,  
충돌이 많으면 성능이 저하될 수 있습니다.

</details>

---

---

<details>
<summary><strong>21. 해시 충돌(Hash Collision)은 왜 발생하고, 어떻게 해결하나요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
서로 다른 키가 같은 해시 인덱스로 매핑될 때 충돌이 발생합니다.  
대표적인 해결 방식은 **체이닝(Chaining)** 과 **개방 주소법(Open Addressing)** 입니다.

---

<strong>🔹 상세설명</strong>  
- **충돌 원인:**  
  - 해시 함수의 불균일 분포  
  - 해시 테이블 크기 부족  
- **해결 방식:**  
  - **체이닝:** 같은 버킷에 연결 리스트로 저장  
  - **오픈 어드레싱:** 충돌 시 다른 슬롯 탐색 (선형, 제곱, 이중 해싱 등)  
- **성능:**  
  - 평균 O(1), 최악 O(n)

---

<strong>💬 면접식 답변</strong>  
해시 충돌은 서로 다른 키가 동일한 인덱스로 매핑될 때 발생합니다.  
보통 체이닝 방식으로 연결 리스트를 두거나,  
오픈 어드레싱으로 빈 슬롯을 찾아 해결합니다.

</details>

---

<details>
<summary><strong>22. 체이닝(Chaining)과 오픈 어드레싱(Open Addressing)의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
체이닝은 버킷마다 리스트를 두는 구조,  
오픈 어드레싱은 빈 슬롯을 찾아 재배치하는 구조입니다.

---

<strong>🔹 상세설명</strong>  
| 구분 | 체이닝 | 오픈 어드레싱 |
|------|--------|----------------|
| 구조 | 버킷마다 연결 리스트 | 한 배열 내에서 재배치 |
| 공간 효율 | 낮음 | 높음 |
| 삭제 | 쉬움 | 어려움 (빈 공간 관리 필요) |
| 캐시 효율 | 낮음 | 높음 |
| C++ STL | unordered_map은 기본적으로 체이닝 사용 |

---

<strong>💬 면접식 답변</strong>  
체이닝은 충돌된 키를 리스트로 묶는 방식이고,  
오픈 어드레싱은 빈 슬롯을 탐색해 재배치하는 방식입니다.  
C++ unordered_map은 체이닝을 사용해 삭제를 단순화합니다.

</details>

---

<details>
<summary><strong>23. 해시 함수(Hash Function)는 어떤 특성을 가져야 하나요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
좋은 해시 함수는 **균등성, 결정성, 계산 효율성**을 만족해야 합니다.

---

<strong>🔹 상세설명</strong>  
- **균등성(Uniformity):** 데이터가 버킷에 고르게 분포  
- **결정성(Deterministic):** 같은 입력 → 항상 같은 출력  
- **효율성(Efficiency):** 빠른 계산  
- **충돌 최소화:** 비트 마스킹, 소수 모듈로 연산 등 사용  

---

<strong>💬 면접식 답변</strong>  
좋은 해시 함수는 입력이 고르게 퍼지고, 계산이 빠르며,  
같은 입력에 항상 같은 결과를 내야 합니다.  
특히 해시 충돌을 줄이는 게 핵심입니다.

</details>

---

<details>
<summary><strong>24. 정렬 알고리즘 중 Introsort는 왜 사용되나요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
Introsort는 **퀵정렬의 평균적 빠름 + 힙정렬의 안정된 최악 성능**을 결합한 하이브리드 정렬입니다.

---

<strong>🔹 상세설명</strong>  
- **구성:**  
  - 퀵정렬 사용  
  - 재귀 깊이가 일정 수준을 넘으면 힙정렬로 전환  
  - 소규모 데이터는 삽입정렬로 처리  
- **시간복잡도:** O(n log n)  
- **C++ STL의 std::sort에서 사용**

---

<strong>💬 면접식 답변</strong>  
Introsort는 평균적으로 빠른 퀵정렬을 사용하지만,  
편향 분할이 감지되면 힙정렬로 전환해 O(n log n)을 보장합니다.  
C++의 std::sort가 이 방식을 사용합니다.

</details>

---

<details>
<summary><strong>25. 삽입 정렬(Insertion Sort)은 언제 유리한가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
데이터 크기가 작거나, 이미 대부분 정렬된 경우 삽입 정렬이 효율적입니다.

---

<strong>🔹 상세설명</strong>  
- **시간복잡도:** O(n²), 최선 O(n)  
- **장점:** 구현 간단, 안정적(stable)  
- **적용:**  
  - 소규모 데이터 정렬  
  - Introsort, Merge Sort의 보조 정렬로 사용  

---

<strong>💬 면접식 답변</strong>  
삽입 정렬은 데이터가 거의 정렬되어 있거나 크기가 작을 때 효율적입니다.  
그래서 하이브리드 정렬(Introsort)에서도 부분 정렬에 사용됩니다.

</details>

---

<details>
<summary><strong>26. Stable Sort(안정 정렬)이란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
같은 값의 원소가 정렬 후에도 **입력 순서를 유지**하는 정렬입니다.

---

<strong>🔹 상세설명</strong>  
- **예시:** Merge Sort, Insertion Sort → 안정적  
- **비안정 정렬:** Quick Sort, Heap Sort  
- **필요한 경우:**  
  - 다중 키 정렬 (예: 이름, 점수)  
  - 정렬 후 순서 보존이 중요한 경우  

---

<strong>💬 면접식 답변</strong>  
안정 정렬은 동일한 값의 원소가 기존 순서를 유지하는 정렬입니다.  
대표적으로 Merge Sort와 Insertion Sort가 안정 정렬입니다.

</details>

---

<details>
<summary><strong>27. 외부 정렬(External Sorting)은 언제 필요한가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
데이터가 메모리에 한 번에 올라가지 않을 때,  
디스크 I/O를 고려한 정렬 알고리즘이 필요합니다.

---

<strong>🔹 상세설명</strong>  
- **적용 상황:** 대용량 로그, DB 파일  
- **구성:**  
  1. 데이터를 여러 블록으로 나누어 정렬  
  2. 외부 병합(External Merge)  
- **대표 알고리즘:** External Merge Sort  
- **복잡도:** O(n log n), I/O 최적화 중심  

---

<strong>💬 면접식 답변</strong>  
외부 정렬은 데이터가 메모리에 한 번에 들어오지 않을 때 사용합니다.  
보통 데이터를 여러 블록으로 나누어 각각 정렬 후,  
병합 단계에서 합치는 외부 병합 정렬을 사용합니다.

</details>

---

<details>
<summary><strong>28. 그리디와 DP의 차이점은 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
그리디는 순간의 최적해를, DP는 전체 구조의 최적해를 구합니다.

---

<strong>🔹 상세설명</strong>  
| 구분 | 그리디 | 동적 계획법 |
|------|--------|--------------|
| 접근 방식 | 순간의 최선 선택 | 전체 상태 저장 |
| 중복 문제 | 없음 | 존재 |
| 최적 구조 | 항상 아님 | 항상 존재 |
| 예시 | 거스름돈, 허프만 코딩 | 배낭 문제, LIS |

---

<strong>💬 면접식 답변</strong>  
그리디는 순간의 최적해가 전체 최적해로 이어질 때만 유효하지만,  
DP는 모든 경우의 수를 저장하며 전체 최적해를 구합니다.

</details>

---

<details>
<summary><strong>29. 백트래킹과 DFS의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
DFS는 깊이 탐색 자체이고,  
백트래킹은 DFS 중 조건을 만족하지 않으면 **조기 탈출(pruning)** 하는 전략입니다.

---

<strong>🔹 상세설명</strong>  
- **DFS:** 단순한 전체 탐색  
- **백트래킹:** 조건 불만족 시 탐색 중단  
- **예시:** N-Queen, 조합, 순열 생성  
- **복잡도 감소:** 가지치기를 통해 효율 상승  

---

<strong>💬 면접식 답변</strong>  
DFS는 전체 탐색을 의미하지만,  
백트래킹은 탐색 도중 조건을 만족하지 않으면 가지를 잘라 효율을 높이는 기법입니다.  
즉, DFS + 가지치기가 백트래킹입니다.

</details>

---

<details>
<summary><strong>30. Union-Find(Disjoint Set)의 핵심 아이디어는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
집합의 연결 여부를 빠르게 확인하기 위한 자료구조로,  
대표 노드를 기준으로 병합(Union)과 탐색(Find)을 수행합니다.

---

<strong>🔹 상세설명</strong>  
- **핵심 연산:**  
  - `Find(x)` : 루트 노드 탐색 (Path Compression)  
  - `Union(a, b)` : 루트 병합 (Union by Rank)  
- **적용:** Kruskal MST, 네트워크 연결  
- **복잡도:** 거의 O(1)

---

<strong>💬 면접식 답변</strong>  
Union-Find는 노드들이 같은 그룹에 속하는지 빠르게 판단하는 자료구조입니다.  
Path Compression과 Rank 병합을 적용하면 거의 O(1)에 수렴합니다.

</details>

---

<details>
<summary><strong>31. Topological Sort(위상 정렬)이란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
유향 비순환 그래프(DAG)의 정점을 **선행 관계에 맞게 정렬**하는 알고리즘입니다.

---

<strong>🔹 상세설명</strong>  
- **전제:** 사이클이 없어야 함  
- **방식:**  
  - 진입 차수가 0인 노드 큐에 삽입  
  - 큐에서 꺼내며 연결 간선 제거  
- **복잡도:** O(V + E)  

---

<strong>💬 면접식 답변</strong>  
위상 정렬은 선행 관계가 있는 작업을 순서대로 나열하는 알고리즘입니다.  
대표적으로 빌드 순서, 과목 이수 조건 같은 문제에서 활용됩니다.

</details>

---

<details>
<summary><strong>32. 플로이드-워셜 알고리즘의 특징은?</strong></summary>

<strong>🧠 핵심 요약</strong>  
모든 정점 쌍 간의 최단 경로를 구하는 DP 기반 알고리즘입니다.

---

<strong>🔹 상세설명</strong>  
- **동작 원리:**  
  - 중간 정점을 하나씩 고려하면서 거리 갱신  
- **복잡도:** O(V³)  
- **특징:** 음수 간선 가능 (사이클은 불가)  

---

<strong>💬 면접식 답변</strong>  
플로이드-워셜은 모든 정점 쌍의 최단 거리를 구하는 알고리즘으로,  
중간 정점을 점진적으로 고려하며 거리 행렬을 갱신합니다.

</details>

---

<details>
<summary><strong>33. 다중 우선순위 큐를 구현하려면?</strong></summary>

<strong>🧠 핵심 요약</strong>  
여러 개의 큐를 우선순위별로 두거나,  
하나의 힙에 `(priority, value)` 튜플로 저장해 관리합니다.

---

<strong>🔹 상세설명</strong>  
- **방법 1:** 계층형 큐 (queue of queues)  
- **방법 2:** pair 기반 힙 (`priority_queue<pair<int, T>>`)  
- **응용:** 작업 스케줄러, 프로세스 우선순위  

---

<strong>💬 면접식 답변</strong>  
다중 우선순위 큐는 우선순위가 다른 작업을 한 구조에서 관리할 때 사용합니다.  
보통 (priority, value) 튜플을 가진 힙으로 구현합니다.

</details>

---

<details>
<summary><strong>34. 트라이(Trie) 자료구조의 장점은?</strong></summary>

<strong>🧠 핵심 요약</strong>  
문자열 검색에서 중복 비교를 줄여,  
O(문자열 길이) 시간에 탐색을 수행할 수 있습니다.

---

<strong>🔹 상세설명</strong>  
- **특징:**  
  - 각 문자 단위로 노드 구성  
  - 공통 접두사 공유  
- **장점:** 빠른 검색, 자동 완성, 사전 구현  
- **단점:** 메모리 사용량 많음  

---

<strong>💬 면접식 답변</strong>  
트라이는 문자열을 문자 단위로 저장해 공통 접두사를 공유합니다.  
검색은 문자열 길이에 비례하므로 매우 빠르지만, 메모리를 많이 사용합니다.

</details>

---

<details>
<summary><strong>35. 세그먼트 트리에서 Lazy Propagation이란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
대량의 구간 업데이트를 효율적으로 처리하기 위한 지연 갱신 기법입니다.

---

<strong>🔹 상세설명</strong>  
- **문제점:** 구간 단위로 값 변경 시 매번 전체 트리 갱신 필요 → 비효율적  
- **해결:** 갱신을 즉시 반영하지 않고, 필요한 시점에만 적용  
- **복잡도:** 구간 업데이트/쿼리 모두 O(log n)

---

<strong>💬 면접식 답변</strong>  
Lazy Propagation은 구간 업데이트를 한꺼번에 처리하지 않고,  
필요할 때만 갱신하는 방식입니다.  
이로써 세그먼트 트리의 성능을 O(log n)으로 유지할 수 있습니다.

</details>

---

<details>
<summary><strong>36. STL에서 lower_bound와 upper_bound의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **lower_bound:** 주어진 값 이상(≥)의 첫 위치  
- **upper_bound:** 주어진 값 초과(>)의 첫 위치

---

<strong>🔹 상세설명</strong>  
- **기반:** 이진 탐색  
- **적용 컨테이너:** vector, set, map  
- **활용:** 구간 내 원소 개수, LIS 등  

---

<strong>💬 면접식 답변</strong>  
lower_bound는 주어진 값 이상인 첫 원소의 위치,  
upper_bound는 초과하는 첫 원소의 위치를 반환합니다.  
두 값을 빼면 해당 구간 내 개수를 알 수 있습니다.

</details>

---

<details>
<summary><strong>37. STL의 erase-remove idiom이란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
vector에서 `erase()`와 `remove()`를 조합해 요소를 안전하게 제거하는 패턴입니다.

---

<strong>🔹 상세설명</strong>  
- **remove():** 값만 재배열, 실제 삭제 X  
- **erase():** 특정 구간을 실제로 제거  
- **패턴:**  
  ```cpp
  vec.erase(remove(vec.begin(), vec.end(), value), vec.end());
  ```  

---

<strong>💬 면접식 답변</strong>  
remove는 단순히 값들을 앞으로 당길 뿐 실제 삭제를 하지 않습니다.  
따라서 erase와 함께 써서 실제 메모리에서 요소를 제거해야 합니다.

</details>

---

<details>
<summary><strong>38. C++ STL sort, stable_sort, partial_sort의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **sort:** 전체 정렬 (Introsort)  
- **stable_sort:** 순서 유지 정렬  
- **partial_sort:** 상위 N개만 정렬  

---

<strong>🔹 상세설명</strong>  
| 함수 | 특징 | 시간복잡도 |
|-------|--------|-------------|
| sort | 전체 정렬, 불안정 | O(n log n) |
| stable_sort | 순서 유지, 메모리 사용↑ | O(n log² n) |
| partial_sort | 상위 N개만 정렬 | O(n log k) |

---

<strong>💬 면접식 답변</strong>  
sort는 전체 정렬, stable_sort는 순서를 유지한 정렬,  
partial_sort는 상위 N개만 정렬하는 함수입니다.  
데이터 양에 따라 적절히 선택합니다.

</details>

---

<details>
<summary><strong>39. unordered_map의 평균과 최악 복잡도는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- 평균 시간복잡도: **O(1)**  
- 최악 시간복잡도: **O(n)** (해시 충돌이 심할 때)  
- 성능 유지 키포인트: **적절한 load_factor**, **리해시(rehash)**, **reserve**

---

<strong>🔹 상세설명</strong>  
- **평균 O(1)**: 해시 함수가 키들을 균등하게 분산시키고, 버킷 수가 충분할 때 평균적으로 삽입/탐색/삭제 모두 O(1)에 수렴합니다.  
- **최악 O(n)**: 충돌이 몰리면 특정 버킷의 체인이 길어져 선형 시간까지 악화될 수 있습니다.  
- **load_factor (부하율)**: `size() / bucket_count()` 로 계산되며, `max_load_factor()`를 초과하면 자동으로 리해시가 발생합니다.  
- **rehash / reserve**:  
  - `rehash(n)`: 최소 **n**개 이상의 버킷을 갖도록 재배치(강제 리해시).  
  - `reserve(n)`: **n**개의 요소를 수용할 수 있도록 필요한 버킷 수를 확보(보통 더 직관적).  
- **커스텀 해시**: 균등 분포를 위해 키 특성에 맞는 해시 함수를 정의하면 충돌을 줄일 수 있습니다.  
- **메모리 트레이드오프**: 버킷을 넉넉히 잡으면 시간은 빨라지지만 메모리 사용량이 증가합니다.

---

<strong>💬 면접식 답변</strong>  
`unordered_map`은 평균적으로 삽입/탐색/삭제가 O(1)이지만, 해시 충돌이 심하면 최악의 경우 O(n)까지 악화될 수 있습니다.  
그래서 `max_load_factor`를 적절히 설정하고, 대량 삽입 전에 `reserve`로 버킷을 넉넉히 확보해 리해시 비용을 줄이는 편이 좋습니다.  
또한 키 특성에 맞는 커스텀 해시를 사용하면 충돌 확률을 낮출 수 있습니다.

</details>

---

<details>
<summary><strong>40. 불균일 분포 데이터에서 BST와 해시 중 무엇을 선택할까?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **해시**: 평균 O(1) 성능이지만 **키 분포/충돌**에 민감하고 **순서성이 없음**.  
- **균형 BST(map)**: 항상 **O(log n)** 보장, **정렬/범위 쿼리/순회**에 강함.  
- **결론**: 순서/범위가 필요하거나 키 분포가 나쁠 때는 **균형 BST**,  
  단순 키→값 조회가 대부분이고 분포가 괜찮다면 **해시**.

---

<strong>🔹 상세설명</strong>  
- **해시(unordered_map)**  
  - 장점: 평균 O(1) 접근, 간단한 키→값 조회에 매우 빠름.  
  - 단점: 순서 없음(정렬·범위 질의 불리), 충돌·리해시 비용, 부하율 관리 필요.  
  - 불균일(스키유) 분포: 특정 패턴의 키가 충돌을 유발하면 최악 O(n)로 떨어질 위험이 존재.  
- **균형 BST(map, Red-Black Tree)**  
  - 장점: **항상 O(log n)**, **정렬 유지**, **lower_bound/upper_bound**, **범위 반복(range iteration)** 지원.  
  - 단점: 상수계수가 커서 해시 대비 단순 조회에서는 느릴 수 있음.  
- **의사결정 체크리스트**  
  1) **정렬/순서/범위 쿼리**가 필요한가? → **map**  
  2) 데이터 분포가 **스키유**하거나 키가 해싱에 **불리한 패턴**인가? → **map** 고려  
  3) 대부분이 **정확한 키 조회**이고, 메모리를 더 써도 괜찮은가? → **unordered_map**  
  4) **메모리/캐시 효율**이 중요한가? → 연속 메모리 구조(vector + 이진탐색) 대안도 검토  

---

<strong>💬 면접식 답변</strong>  
키 분포가 불균일하면 `unordered_map`의 충돌로 인해 최악 성능이 나올 수 있어요.  
정렬 유지와 범위 질의가 필요하거나 안정적인 성능 보장이 중요하다면 **균형 BST(map)**가 더 적합합니다.  
반대로 단순 조회가 대부분이고 평균 성능 극대화가 목표라면 **unordered_map**이 유리하죠.  
상황에 따라 **lower/upper_bound** 같은 트리 기반 연산과, **부하율 관리 + reserve**를 갖춘 해시의 트레이드오프를 비교해 결정합니다.

</details>

---

<details>
<summary><strong>41. merge sort와 quick sort의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- Merge Sort: **O(n log n)** 안정적, 분할 후 **병합 단계** 중심.  
- Quick Sort: 평균 **O(n log n)**, 최악 **O(n²)**, **분할 중심** 정렬.  
- Quick Sort는 **in-place**로 공간 효율적, Merge Sort는 **안정성**이 뛰어남.

---

<strong>🔹 상세설명</strong>  
- **Merge Sort**  
  - 분할(half split) → 정렬된 서브 리스트 → 병합 단계로 완성.  
  - 항상 O(n log n) 보장, 하지만 추가 메모리 공간 필요.  
  - 안정 정렬(stable)이라 데이터 순서를 유지함.  
- **Quick Sort**  
  - pivot을 기준으로 분할(partition) → 재귀 정렬.  
  - 평균은 O(n log n), 하지만 pivot이 편향되면 O(n²).  
  - 공간 효율 높고, 캐시 친화적(cache-friendly).  
  - 불안정 정렬(unstable).  
- **실제 구현에서** Quick Sort는 평균적으로 빠르지만, 안정성과 worst-case 보장이 필요한 경우 Merge Sort가 선호됨.

---

<strong>💬 면접식 답변</strong>  
Merge Sort는 항상 O(n log n)이고 안정적이지만 메모리 사용량이 많습니다.  
반면 Quick Sort는 메모리 효율이 좋고 평균적으로 빠르지만 pivot 선택이 나쁘면 O(n²)까지 떨어질 수 있습니다.  
그래서 STL의 `std::sort`는 Quick, Heap, Insertion을 조합한 Introsort 방식을 사용합니다.

</details>

---

<details>
<summary><strong>42. 힙(Heap) 자료구조의 특징과 사용처는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- 완전이진트리 기반의 **우선순위 큐 구조**.  
- 삽입/삭제 **O(log n)**.  
- **최대/최소값 추출에 효율적**.  

---

<strong>🔹 상세설명</strong>  
- **Heap의 핵심 특성**: 부모 노드가 항상 자식보다 크거나 작다는 **heap property** 유지.  
- **최대힙(Max-Heap)**: root가 항상 가장 큼.  
- **최소힙(Min-Heap)**: root가 항상 가장 작음.  
- **배열 기반 구현**으로 index 0부터 시작,  
  - `parent = i/2`, `left = 2i+1`, `right = 2i+2`.  
- **주요 사용처**  
  - 우선순위 큐(priority_queue)  
  - 다익스트라(Dijkstra), A* 탐색  
  - K번째 원소 찾기 문제  

---

<strong>💬 면접식 답변</strong>  
힙은 완전이진트리 기반의 우선순위 큐 구조로, 삽입/삭제가 O(log n)입니다.  
가장 큰 값이나 작은 값을 빠르게 얻고 싶을 때 유용하며, STL에서는 `std::priority_queue`로 쉽게 구현할 수 있습니다.

</details>

---

<details>
<summary><strong>43. STL의 map과 unordered_map 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- `map`: **정렬**된 Red-Black Tree 기반, **O(log n)**  
- `unordered_map`: **해시 기반**, **평균 O(1)**  
- `map`은 **순서 필요**, `unordered_map`은 **속도 우선**

---

<strong>🔹 상세설명</strong>  
- `map`:  
  - 내부적으로 **Red-Black Tree** 구조.  
  - 키가 항상 정렬되어 유지.  
  - `lower_bound`, `upper_bound`, `begin~end` 순회 가능.  
  - 삽입/탐색/삭제 O(log n).  
- `unordered_map`:  
  - **해시 테이블** 기반.  
  - 평균 O(1), 최악 O(n).  
  - 순서가 없고, 키 중복 불가.  
  - `reserve()`를 통해 리해시 방지 가능.  

---

<strong>💬 면접식 답변</strong>  
`map`은 정렬된 탐색이 필요한 경우, `unordered_map`은 빠른 해시 기반 조회가 필요한 경우 사용합니다.  
예를 들어 순차 순회가 필요하면 map을, 단순한 key-value 조회면 unordered_map을 사용합니다.

</details>

---

<details>
<summary><strong>44. BFS와 DFS의 차이점은?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- BFS: **너비 우선 탐색**, Queue 사용  
- DFS: **깊이 우선 탐색**, Stack/재귀 사용  
- BFS: **최단거리 탐색**, DFS: **모든 경로 탐색**

---

<strong>🔹 상세설명</strong>  
- **BFS (Breadth First Search)**  
  - 큐를 사용하며, 가까운 노드부터 탐색.  
  - 가중치 없는 그래프에서 최단거리 탐색에 적합.  
- **DFS (Depth First Search)**  
  - 재귀나 스택을 이용.  
  - 경로 존재 여부 확인, 백트래킹 기반 탐색에 적합.  
- **복잡도**: 두 알고리즘 모두 O(V+E).  

---

<strong>💬 면접식 답변</strong>  
BFS는 Queue 기반으로 가까운 노드부터 탐색하며, 최단 경로를 찾을 때 사용됩니다.  
DFS는 Stack 기반으로 깊이 탐색을 하며, 백트래킹 문제나 미로 탐색에 적합합니다.

</details>

---

<details>
<summary><strong>45. 다익스트라 알고리즘이란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- 가중치가 **양수**인 그래프에서 **최단 경로 탐색** 알고리즘.  
- **우선순위 큐(Heap)** 기반 구현.  

---

<strong>🔹 상세설명</strong>  
- 시작 노드에서 다른 모든 노드까지의 최소 비용 계산.  
- 음수 가중치 불가 (Bellman-Ford 필요).  
- 매 반복마다 최소 거리 정점을 선택해 거리 갱신.  
- **시간복잡도**: O(E log V).  
- **활용 예시**: 네비게이션, 라우팅 알고리즘, 경로 탐색.

---

<strong>💬 면접식 답변</strong>  
다익스트라는 양수 가중치 그래프의 최단 경로 탐색 알고리즘입니다.  
우선순위 큐를 사용해 매번 가장 짧은 경로를 가진 정점을 선택하고 인접 노드의 거리를 갱신합니다.

</details>

---

<details>
<summary><strong>46. Union-Find(Disjoint Set)는 언제 쓰나요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- 서로소 집합 관리 자료구조.  
- 대표 사용처: **네트워크 연결, MST(크루스칼)**  
- 핵심 연산: **Find**, **Union**, **Path Compression**

---

<strong>🔹 상세설명</strong>  
- **Union(x, y)**: 두 집합을 합침.  
- **Find(x)**: x가 속한 집합의 루트를 찾음.  
- **Path Compression**: 재귀를 통해 루트를 갱신하여 시간 단축.  
- **시간복잡도**: 거의 O(1) (Inverse Ackermann).  

---

<strong>💬 면접식 답변</strong>  
Union-Find는 집합의 연결 여부를 빠르게 판단하기 위한 구조입니다.  
예를 들어 크루스칼 알고리즘에서 사이클을 방지하기 위해 두 노드의 루트를 비교할 때 사용합니다.

</details>

---

<details>
<summary><strong>47. Topological Sort(위상 정렬)는 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **방향 비순환 그래프(DAG)**에서 노드 간 **선행 순서 결정**.  
- **진입차수(indegree)** 기반 탐색.

---

<strong>🔹 상세설명</strong>  
- **정의**: A → B 관계가 있을 때, A가 B보다 먼저 나오는 정렬.  
- **알고리즘**:  
  1) 진입차수가 0인 노드를 큐에 삽입  
  2) 꺼내며 간선 제거, 새로 진입차수 0인 노드 추가  
- **활용**: 빌드 순서, 과목 이수, 작업 의존성 분석  

---

<strong>💬 면접식 답변</strong>  
위상 정렬은 DAG에서 선행 관계가 있는 작업 순서를 정리하는 알고리즘입니다.  
예를 들어 "A가 끝나야 B가 시작되는" 빌드 시스템이나 과목 이수 조건 같은 문제에 적용됩니다.

</details>

---

<details>
<summary><strong>48. Trie(트라이) 자료구조란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- 문자열 탐색을 위한 트리 구조.  
- **Prefix(접두사)** 기반 검색 효율화.  

---

<strong>🔹 상세설명</strong>  
- **노드 구조**: 각 문자를 자식 노드로 가짐.  
- 문자열 삽입/탐색: O(L) (L = 문자열 길이).  
- **활용 예시**: 자동완성, 사전(dictionary), 접두사 필터링.  
- **공간 절약형 구현**: HashMap + End-of-Word flag로 관리.  

---

<strong>💬 면접식 답변</strong>  
Trie는 문자열을 접두사 단위로 분기하는 트리입니다.  
검색 시간이 문자열 길이에 비례해 일정하고, 자동완성이나 사전 검색 같은 기능에서 자주 사용됩니다.

</details>

---

<details>
<summary><strong>49. Segment Tree란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- 구간 합, 최솟값/최댓값 등을 빠르게 구하는 트리 구조.  
- **시간복잡도 O(log n)**로 구간 질의와 업데이트 처리.

---

<strong>🔹 상세설명</strong>  
- 완전이진트리 형태로 배열 구간 정보를 저장.  
- 구간 [l, r] 쿼리 처리 시 트리를 따라 부분합 계산.  
- Lazy Propagation으로 대규모 업데이트 최적화.  
- Fenwick Tree보다 구현 복잡하지만 더 유연함.

---

<strong>💬 면접식 답변</strong>  
Segment Tree는 배열의 특정 구간 합이나 최댓값을 빠르게 구할 때 사용됩니다.  
업데이트와 질의가 모두 O(log n)에 가능해 실시간 데이터 처리에 적합합니다.

</details>

---

<details>
<summary><strong>50. STL sort()는 어떤 정렬 알고리즘을 쓰나요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **Introsort**: Quick Sort + Heap Sort + Insertion Sort 혼합  
- 입력 크기, 깊이에 따라 동적으로 선택

---

<strong>🔹 상세설명</strong>  
- 초기에는 Quick Sort로 수행  
- 재귀 깊이가 일정 이상이면 Heap Sort로 전환 (worst-case 방지)  
- 작은 구간은 Insertion Sort로 최적화  
- 즉, 평균 O(n log n), 최악 O(n log n) 보장  
- stable 버전은 `std::stable_sort()` (Merge 기반)

---

<strong>💬 면접식 답변</strong>  
STL의 `std::sort`는 Introsort를 사용합니다.  
처음엔 Quick Sort로 빠르게 정렬하다가 재귀 깊이가 깊어지면 Heap Sort로 바꾸고, 작은 구간은 Insertion Sort를 적용해 전체 성능을 최적화합니다.

</details>
