---
layout: post
title: "고급 자료구조: 해시맵, 셋, 트리, 힙 완벽 가이드"
date: 2025-09-21 15:02:00 +0900
categories: [Tech Interview, Data Structure]
slug: data-structures-advanced
tags: [data structure, hashmap, set, tree, heap, binary search tree, hash table, priority queue]
---

## 📌 학습 목표
- 중급 자료구조의 특성과 동작 원리 이해
- 해시 기반, 트리 기반, 힙 기반 구조의 차이점 학습
- 각 자료구조의 시간/공간 복잡도 분석
- 실무에서의 적절한 자료구조 선택 기준 습득

---

## 📝 개념 정리

### 1. 해시맵 (Hash Map)

**핵심 원리:**
- 해시 함수를 통해 키를 인덱스로 변환하여 키-값 쌍 저장
- **평균 시간복잡도**: O(1) - 탐색, 삽입, 삭제
- **최악 시간복잡도**: O(n) - 모든 키가 같은 해시값을 가질 때
- **공간복잡도**: O(n)

**해시 충돌 해결 방법:**

1. **체이닝 (Chaining)**:
```cpp
class HashMapChaining {
private:
    vector<list<pair<int, string>>> table;
    int size;
    
    int hashFunction(int key) {
        return key % size;
    }
    
public:
    HashMapChaining(int s) : size(s) {
        table.resize(size);
    }
    
    void insert(int key, string value) {
        int index = hashFunction(key);
        
        // 기존 키 업데이트 확인
        for (auto& pair : table[index]) {
            if (pair.first == key) {
                pair.second = value;
                return;
            }
        }
        
        // 새 키-값 쌍 추가
        table[index].push_back({key, value});
    }
    
    string search(int key) {
        int index = hashFunction(key);
        for (auto& pair : table[index]) {
            if (pair.first == key) {
                return pair.second;
            }
        }
        return "";  // 키를 찾지 못함
    }
};
```

2. **개방 주소법 (Open Addressing)**:

```cpp
class HashMapOpenAddressing {
private:
    vector<pair<int, string>> table;
    vector<bool> deleted;
    int size;
    
    int hashFunction(int key) {
        return key % size;
    }
    
    int linearProbe(int key) {
        int index = hashFunction(key);
        while (table[index].first != -1 && table[index].first != key) {
            index = (index + 1) % size;
        }
        return index;
    }
    
public:
    HashMapOpenAddressing(int s) : size(s) {
        table.resize(size, {-1, ""});
        deleted.resize(size, false);
    }
    
    void insert(int key, string value) {
        int index = linearProbe(key);
        table[index] = {key, value};
        deleted[index] = false;
    }
};
```

### 2. 셋 (Set)

**핵심 원리:**
- 중복을 허용하지 않는 원소들의 집합
- 일반적으로 트리 기반(Red-Black Tree) 또는 해시 기반 구현
- **시간복잡도**: O(log n) - 트리 기반, O(1) - 해시 기반

**C++ STL 예제:**
```cpp
#include <set>
#include <unordered_set>

// 트리 기반 set (정렬된 상태 유지)
set<int> orderedSet;
orderedSet.insert(3);
orderedSet.insert(1);
orderedSet.insert(4);
// 결과: {1, 3, 4}

// 해시 기반 unordered_set (정렬되지 않음, 더 빠름)
unordered_set<int> hashSet;
hashSet.insert(3);
hashSet.insert(1);
hashSet.insert(4);
// 결과: 순서 보장 안됨

// 집합 연산
set<int> setA = {1, 2, 3, 4};
set<int> setB = {3, 4, 5, 6};
set<int> result;

// 교집합
set_intersection(setA.begin(), setA.end(),
                 setB.begin(), setB.end(),
                 inserter(result, result.begin()));
// 결과: {3, 4}
```

### 3. 트리 (Tree)

**이진 탐색 트리 (Binary Search Tree)**:
- 왼쪽 자식 < 부모 < 오른쪽 자식 속성 유지
- **평균 시간복잡도**: O(log n)
- **최악 시간복잡도**: O(n) - 편향 트리일 때

```cpp
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class BST {
private:
    TreeNode* root;
    
    TreeNode* insert(TreeNode* node, int val) {
        if (!node) return new TreeNode(val);
        
        if (val < node->val) {
            node->left = insert(node->left, val);
        } else if (val > node->val) {
            node->right = insert(node->right, val);
        }
        return node;
    }
    
    TreeNode* search(TreeNode* node, int val) {
        if (!node || node->val == val) return node;
        
        if (val < node->val) {
            return search(node->left, val);
        }
        return search(node->right, val);
    }
    
    TreeNode* findMin(TreeNode* node) {
        while (node->left) node = node->left;
        return node;
    }
    
    TreeNode* deleteNode(TreeNode* node, int val) {
        if (!node) return node;
        
        if (val < node->val) {
            node->left = deleteNode(node->left, val);
        } else if (val > node->val) {
            node->right = deleteNode(node->right, val);
        } else {
            // 삭제할 노드 발견
            if (!node->left) {
                TreeNode* temp = node->right;
                delete node;
                return temp;
            } else if (!node->right) {
                TreeNode* temp = node->left;
                delete node;
                return temp;
            }
            
            // 두 자식 모두 있는 경우
            TreeNode* temp = findMin(node->right);
            node->val = temp->val;
            node->right = deleteNode(node->right, temp->val);
        }
        return node;
    }
    
public:
    BST() : root(nullptr) {}
    
    void insert(int val) {
        root = insert(root, val);
    }
    
    bool search(int val) {
        return search(root, val) != nullptr;
    }
    
    void remove(int val) {
        root = deleteNode(root, val);
    }
};
```

**자가 균형 트리 (Self-Balancing Trees)**:
- **AVL 트리**: 모든 노드의 좌우 서브트리 높이 차이 ≤ 1
- **Red-Black 트리**: 색상 규칙으로 균형 유지 (C++ STL map/set 내부 구현)

### 4. 힙 (Heap)

**핵심 원리:**
- 완전 이진 트리 기반의 자료구조
- **최대 힙**: 부모 노드 ≥ 자식 노드
- **최소 힙**: 부모 노드 ≤ 자식 노드
- 우선순위 큐 구현에 주로 활용

```cpp
class MinHeap {
private:
    vector<int> heap;
    
    void heapifyUp(int index) {
        if (index == 0) return;
        
        int parent = (index - 1) / 2;
        if (heap[parent] > heap[index]) {
            swap(heap[parent], heap[index]);
            heapifyUp(parent);
        }
    }
    
    void heapifyDown(int index) {
        int left = 2 * index + 1;
        int right = 2 * index + 2;
        int smallest = index;
        
        if (left < heap.size() && heap[left] < heap[smallest]) {
            smallest = left;
        }
        
        if (right < heap.size() && heap[right] < heap[smallest]) {
            smallest = right;
        }
        
        if (smallest != index) {
            swap(heap[index], heap[smallest]);
            heapifyDown(smallest);
        }
    }
    
public:
    void insert(int val) {
        heap.push_back(val);
        heapifyUp(heap.size() - 1);
    }
    
    int extractMin() {
        if (heap.empty()) throw runtime_error("Heap is empty");
        
        int root = heap[0];
        heap[0] = heap.back();
        heap.pop_back();
        
        if (!heap.empty()) {
            heapifyDown(0);
        }
        
        return root;
    }
    
    int peek() {
        if (heap.empty()) throw runtime_error("Heap is empty");
        return heap[0];
    }
    
    bool empty() {
        return heap.empty();
    }
};
```

---

## 💻 실무 활용 예제

### 1. 단어 빈도 계산 (해시맵 활용)
```cpp
unordered_map<string, int> countWords(vector<string>& words) {
    unordered_map<string, int> frequency;
    
    for (const string& word : words) {
        frequency[word]++;
    }
    
    return frequency;
}

// 사용 예시
vector<string> document = {"apple", "banana", "apple", "cherry", "banana", "apple"};
auto freq = countWords(document);
// 결과: {"apple": 3, "banana": 2, "cherry": 1}
```

### 2. 우선순위 작업 스케줄러 (힙 활용)
```cpp
struct Task {
    int priority;
    string description;
    
    // 우선순위가 높을수록 먼저 처리 (최대 힙)
    bool operator<(const Task& other) const {
        return priority < other.priority;
    }
};

class TaskScheduler {
private:
    priority_queue<Task> taskQueue;
    
public:
    void addTask(int priority, string description) {
        taskQueue.push({priority, description});
    }
    
    Task getNextTask() {
        if (taskQueue.empty()) {
            throw runtime_error("No tasks available");
        }
        
        Task nextTask = taskQueue.top();
        taskQueue.pop();
        return nextTask;
    }
    
    bool hasTasks() {
        return !taskQueue.empty();
    }
};
```

### 3. 중복 제거와 정렬 (셋 활용)
```cpp
vector<int> removeDuplicatesAndSort(vector<int>& nums) {
    set<int> uniqueNums(nums.begin(), nums.end());
    return vector<int>(uniqueNums.begin(), uniqueNums.end());
}

// 사용 예시
vector<int> input = {3, 1, 4, 1, 5, 9, 2, 6, 5};
vector<int> result = removeDuplicatesAndSort(input);
// 결과: {1, 2, 3, 4, 5, 6, 9}
```

---

## 🎯 연습 문제

### 1. LRU 캐시 구현 (해시맵 + 이중 연결 리스트)
```cpp
class LRUCache {
private:
    struct Node {
        int key, value;
        Node* prev;
        Node* next;
        Node(int k, int v) : key(k), value(v), prev(nullptr), next(nullptr) {}
    };
    
    unordered_map<int, Node*> cache;
    Node* head;
    Node* tail;
    int capacity;
    
    void addToHead(Node* node) {
        node->prev = head;
        node->next = head->next;
        head->next->prev = node;
        head->next = node;
    }
    
    void removeNode(Node* node) {
        node->prev->next = node->next;
        node->next->prev = node->prev;
    }
    
public:
    LRUCache(int cap) : capacity(cap) {
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head->next = tail;
        tail->prev = head;
    }
    
    int get(int key) {
        if (cache.find(key) != cache.end()) {
            Node* node = cache[key];
            removeNode(node);
            addToHead(node);
            return node->value;
        }
        return -1;
    }
    
    void put(int key, int value) {
        if (cache.find(key) != cache.end()) {
            Node* node = cache[key];
            node->value = value;
            removeNode(node);
            addToHead(node);
        } else {
            if (cache.size() >= capacity) {
                Node* last = tail->prev;
                removeNode(last);
                cache.erase(last->key);
                delete last;
            }
            
            Node* newNode = new Node(key, value);
            addToHead(newNode);
            cache[key] = newNode;
        }
    }
};
```

### 2. K개의 가장 빈번한 원소 찾기 (힙 활용)
```cpp
vector<int> topKFrequent(vector<int>& nums, int k) {
    unordered_map<int, int> frequency;
    for (int num : nums) {
        frequency[num]++;
    }
    
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> minHeap;
    
    for (auto& [num, freq] : frequency) {
        minHeap.push({freq, num});
        if (minHeap.size() > k) {
            minHeap.pop();
        }
    }
    
    vector<int> result;
    while (!minHeap.empty()) {
        result.push_back(minHeap.top().second);
        minHeap.pop();
    }
    
    return result;
}
```

---

## 🔎 심화 학습

### 고급 해시 기법
```cpp
// Robin Hood Hashing - 더 균등한 분포
// Cuckoo Hashing - 최악의 경우에도 O(1) 보장
// Consistent Hashing - 분산 시스템에서 활용
```

### 고급 트리 구조
```cpp
// B-Tree, B+ Tree - 데이터베이스 인덱스
// Trie (Prefix Tree) - 문자열 검색
// Segment Tree - 구간 쿼리 처리
// Fenwick Tree (Binary Indexed Tree) - 누적합 쿼리
```

### 자료구조 성능 비교표

| 자료구조 | 탐색 | 삽입 | 삭제 | 공간복잡도 | 특징 |
|---------|------|------|------|------------|------|
| 해시맵 | O(1) | O(1) | O(1) | O(n) | 순서 없음, 충돌 가능 |
| 트리맵 | O(log n) | O(log n) | O(log n) | O(n) | 정렬된 순서 유지 |
| 힙 | O(n) | O(log n) | O(log n) | O(n) | 최대/최소값 빠른 접근 |

---

## 🌐 외부 링크
- [cppreference - STL Containers](https://en.cppreference.com/w/cpp/container)
- [GeeksforGeeks - Data Structures](https://www.geeksforgeeks.org/data-structures/)
- [Visualizing Data Structures](https://www.cs.usfca.edu/~galles/visualization/Algorithms.html)

---

## 💡 실무 적용 팁

1. **자료구조 선택 기준**:
   - 빈번한 탐색이 필요하면 해시맵 고려
   - 정렬된 데이터가 필요하면 트리맵 선택
   - 우선순위 처리가 필요하면 힙 활용

2. **성능 최적화**:
   - 해시맵의 로드 팩터 관리 (0.75 이하 유지)
   - 트리의 균형 유지로 최악 성능 방지
   - 힙에서는 배열 기반 구현으로 캐시 효율성 향상

3. **메모리 관리**:
   - 동적 할당 최소화
   - 메모리 풀 사용 고려
   - 참조 지역성 고려한 데이터 배치

---

## 다음 학습 주제
- **고급 트리**: Trie, Segment Tree, Fenwick Tree
- **그래프 자료구조**: 인접 리스트, 인접 행렬, Union-Find
- **문자열 자료구조**: Suffix Array, Suffix Tree
- **확률적 자료구조**: Bloom Filter, Count-Min Sketch

---

## 🪞 회고 질문
- 해시 충돌이 발생했을 때 어떤 해결 방법을 선택할 것인가?
- 힙과 트리의 차이점을 실무 상황에 맞춰 설명할 수 있는가?
- 대용량 데이터 처리 시 어떤 자료구조가 가장 적합할까?
- 메모리 제약이 있는 환경에서는 어떤 자료구조를 우선적으로 고려해야 할까?