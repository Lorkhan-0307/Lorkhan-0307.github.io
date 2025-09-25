---
layout: post
title: "기본 자료구조 마스터하기 - 배열, 연결리스트, 스택, 큐"
date: 2025-09-21 15:02:00 +0900
categories: [Tech Interview, Data Structure]
tags: [data-structure, array, linked-list, stack, queue, algorithm, big-o, memory]
slug: data-structure-basic
---

## 📌 학습 목표
- 기본 자료구조 4개 완전 이해 및 구현  
- 각 자료구조의 장단점과 활용 사례 파악  
- 시간/공간 복잡도 분석 능력 향상

---

## 📝 개념 정리

### 1. 배열 (Array)
**연속된 메모리에 동일한 타입의 데이터를 저장**

#### 특징
- **인덱스 기반 접근**: O(1)
- **크기 고정**: 선언 시 크기 결정
- **삽입/삭제 비용 큼**: O(n)

```cpp
// C++ 배열 예제
int arr[5] = {1, 2, 3, 4, 5};
arr[2] = 10;  // O(1) 접근

// 중간 삽입은 비효율적
for(int i = 4; i > 2; i--) {
    arr[i] = arr[i-1];  // 요소들을 한 칸씩 이동
}
arr[2] = 99;
```

#### 장점 vs 단점

| 장점 | 단점 |
|------|------|
| 빠른 접근 속도 | 크기 고정 |
| 캐시 친화적 | 삽입/삭제 비용 |
| 메모리 효율적 | 타입 제한 |

### 2. 연결리스트 (Linked List)
**노드(값 + 포인터)들이 연결된 구조**

#### 특징
- **동적 크기**: 런타임에 크기 변경 가능
- **삽입/삭제**: O(1) (위치를 안다면)
- **순차 접근**: O(n)

```cpp
// 단일 연결리스트 구현
struct Node {
    int data;
    Node* next;
    
    Node(int val) : data(val), next(nullptr) {}
};

class LinkedList {
private:
    Node* head;
    
public:
    LinkedList() : head(nullptr) {}
    
    // 앞쪽에 삽입 O(1)
    void insertFront(int val) {
        Node* newNode = new Node(val);
        newNode->next = head;
        head = newNode;
    }
    
    // 특정 값 삭제 O(n)
    void remove(int val) {
        if (!head) return;
        
        if (head->data == val) {
            Node* temp = head;
            head = head->next;
            delete temp;
            return;
        }
        
        Node* current = head;
        while (current->next && current->next->data != val) {
            current = current->next;
        }
        
        if (current->next) {
            Node* temp = current->next;
            current->next = current->next->next;
            delete temp;
        }
    }
};
```

### 3. 스택 (Stack)
**LIFO (Last In, First Out) 구조**

#### 핵심 연산
- `push()`: 최상단에 요소 추가
- `pop()`: 최상단 요소 제거
- `top()`: 최상단 요소 확인

```cpp
#include <stack>
#include <iostream>

// STL 스택 사용
void stackExample() {
    std::stack<int> s;
    
    s.push(1);
    s.push(2);
    s.push(3);
    
    while (!s.empty()) {
        std::cout << s.top() << " ";  // 3 2 1
        s.pop();
    }
}

// 배열로 스택 구현
class ArrayStack {
private:
    int* arr;
    int capacity;
    int topIndex;
    
public:
    ArrayStack(int size) : capacity(size), topIndex(-1) {
        arr = new int[capacity];
    }
    
    ~ArrayStack() { delete[] arr; }
    
    void push(int val) {
        if (topIndex < capacity - 1) {
            arr[++topIndex] = val;
        }
    }
    
    int pop() {
        if (topIndex >= 0) {
            return arr[topIndex--];
        }
        return -1; // 에러 처리 필요
    }
    
    bool isEmpty() { return topIndex == -1; }
};
```

### 4. 큐 (Queue)
**FIFO (First In, First Out) 구조**

#### 핵심 연산
- `enqueue()`: 뒤쪽에 요소 추가
- `dequeue()`: 앞쪽 요소 제거
- `front()`: 앞쪽 요소 확인

```cpp
#include <queue>

// STL 큐 사용
void queueExample() {
    std::queue<int> q;
    
    q.push(1);  // enqueue
    q.push(2);
    q.push(3);
    
    while (!q.empty()) {
        std::cout << q.front() << " ";  // 1 2 3
        q.pop();  // dequeue
    }
}

// 원형 큐 구현
class CircularQueue {
private:
    int* arr;
    int capacity;
    int front, rear, size;
    
public:
    CircularQueue(int cap) : capacity(cap), front(0), rear(-1), size(0) {
        arr = new int[capacity];
    }
    
    ~CircularQueue() { delete[] arr; }
    
    void enqueue(int val) {
        if (size < capacity) {
            rear = (rear + 1) % capacity;
            arr[rear] = val;
            size++;
        }
    }
    
    int dequeue() {
        if (size > 0) {
            int val = arr[front];
            front = (front + 1) % capacity;
            size--;
            return val;
        }
        return -1;
    }
    
    bool isEmpty() { return size == 0; }
    bool isFull() { return size == capacity; }
};
```

---

## 🎯 실전 연습 문제

### 1. 연결리스트 뒤집기
```cpp
Node* reverseLinkedList(Node* head) {
    Node* prev = nullptr;
    Node* current = head;
    
    while (current) {
        Node* next = current->next;
        current->next = prev;
        prev = current;
        current = next;
    }
    
    return prev;
}
```

### 2. 스택으로 괄호 유효성 검사
```cpp
bool isValidParentheses(string s) {
    stack<char> st;
    
    for (char c : s) {
        if (c == '(' || c == '[' || c == '{') {
            st.push(c);
        } else {
            if (st.empty()) return false;
            
            char top = st.top();
            st.pop();
            
            if ((c == ')' && top != '(') ||
                (c == ']' && top != '[') ||
                (c == '}' && top != '{')) {
                return false;
            }
        }
    }
    
    return st.empty();
}
```

### 3. 큐로 BFS 구현
```cpp
void bfs(vector<vector<int>>& graph, int start) {
    vector<bool> visited(graph.size(), false);
    queue<int> q;
    
    q.push(start);
    visited[start] = true;
    
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        
        cout << node << " ";
        
        for (int neighbor : graph[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push(neighbor);
            }
        }
    }
}
```

---

## 📊 복잡도 비교표

| 자료구조 | 접근 | 검색 | 삽입 | 삭제 | 공간복잡도 |
|----------|------|------|------|------|------------|
| 배열 | O(1) | O(n) | O(n) | O(n) | O(n) |
| 연결리스트 | O(n) | O(n) | O(1)* | O(1)* | O(n) |
| 스택 | O(n) | O(n) | O(1) | O(1) | O(n) |
| 큐 | O(n) | O(n) | O(1) | O(1) | O(n) |

*위치를 아는 경우

---

## 🔗 관련 링크

### 문제 풀이 사이트
- [LeetCode Array Problems](https://leetcode.com/tag/array/)
- [백준 스택 문제](https://www.acmicpc.net/problem/tag/71)
- [프로그래머스 큐 문제](https://programmers.co.kr/learn/courses/30/parts/12081)

### 시각화 도구
- [VisuAlgo - 자료구조 시각화](https://visualgo.net/)

---

## 🔎 심화 학습 주제

### 고급 변형들
- **동적 배열**: `std::vector`, `std::deque`
- **이중 연결 리스트**: 양방향 탐색 가능
- **원형 큐**: 메모리 효율적 활용
- **덱(Deque)**: 양쪽 끝에서 삽입/삭제 가능

### 메모리 관점
- **배열 vs 연결리스트**: 캐시 친화성 차이
- **메모리 지역성**: 배열이 더 유리한 이유
- **메모리 단편화**: 연결리스트의 단점

---

## 💡 실무 적용 팁

1. **적절한 자료구조 선택**
   - 빈번한 접근 → 배열
   - 빈번한 삽입/삭제 → 연결리스트
   - 후입선출 → 스택
   - 선입선출 → 큐

2. **STL 적극 활용**
   - `std::vector`, `std::list`, `std::stack`, `std::queue`
   - 직접 구현보다는 검증된 라이브러리 사용

3. **성능 측정**
   - 이론적 복잡도와 실제 성능은 다를 수 있음
   - 프로파일링으로 병목지점 확인

---

## 🪞 회고 질문

- 각 자료구조의 장단점을 **실제 예시**와 함께 설명할 수 있는가?
- **언제 어떤 자료구조**를 선택해야 하는지 판단 기준이 있는가?
- 최근 프로젝트에서 **어떤 자료구조**를 주로 사용했고, 그 이유는?
- 메모리 제약이 있는 환경에서는 **어떤 자료구조**가 더 적합할까?

---

## 🚀 다음 학습 주제

다음으로는 **고급 자료구조**인 해시맵, 셋, 트리, 힙에 대해 알아보겠습니다. 더 복잡하지만 강력한 도구들을 만나보세요!