---
layout: post
title: "스마트 포인터(Smart Pointer) - C++/C#/CS 기초"
date: 2025-09-22 15:00:00 +0900
categories: [Tech Interview, Study Plan]
tags: [c++, c#, computer-science, algorithm, data-structure, operating-system, network, database, design-pattern]
slug: whatis-smartpointer
---

# 스마트 포인터(Smart Pointer)

## 📌 개념 정리
- **스마트 포인터**: 포인터처럼 동작하지만, 객체의 소멸 시 자동으로 메모리 해제  
- C++11 이후 도입된 자동 메모리 관리 도구
- C++11 이후 표준 제공: `unique_ptr`, `shared_ptr`, `weak_ptr`  
- 핵심 아이디어: 참조 카운트, 소유권 개념을 이용하여 `delete`를 자동화  

## 📌 학습 목표
- C++에서 [스마트포인터]({{ site.baseurl }}/posts/whatis-smartpointer/)의 필요성과 원리 이해  
- `unique_ptr`, `shared_ptr`, `weak_ptr`의 차이 학습  
- 메모리 누수 방지와 RAII와의 관계 이해  


---

## 📝 개념 정리
- **스마트 포인터**: 포인터처럼 동작하지만, 객체의 소멸 시 자동으로 메모리 해제  
- C++11 이후 표준 제공: `unique_ptr`, `shared_ptr`, `weak_ptr`  
- 핵심 아이디어: 참조 카운트, 소유권 개념을 이용하여 `delete`를 자동화  

### 1. `unique_ptr`
- 단일 소유권 → 복사 불가, 이동만 가능  
- 가장 가볍고 안전  

### 2. `shared_ptr`
- 참조 카운팅 기반  
- 여러 개의 포인터가 하나의 자원 공유 가능  
- 참조 카운트가 0이 되면 자동 해제  

### 3. `weak_ptr`
- `shared_ptr`의 순환 참조 문제를 해결하기 위해 사용  
- 참조 카운트를 증가시키지 않음  

---

## 💻 예제 코드
```cpp
#include <iostream>
#include <memory>
using namespace std;

void UniquePtrDemo() {
    unique_ptr<int> p1 = make_unique<int>(42);
    // unique_ptr<int> p2 = p1; // ❌ 복사 불가
    unique_ptr<int> p3 = move(p1); // ✅ 이동
}

void SharedPtrDemo() {
    shared_ptr<int> sp1 = make_shared<int>(100);
    shared_ptr<int> sp2 = sp1; // 참조 카운트 증가
    cout << "use_count: " << sp1.use_count() << endl; // 2
}

void WeakPtrDemo() {
    shared_ptr<int> sp = make_shared<int>(200);
    weak_ptr<int> wp = sp; // 순환 참조 방지
    if (auto locked = wp.lock()) {
        cout << *locked << endl;
    }
}
```

---

## 🎯 연습 문제
1. `unique_ptr`로 동적 배열을 관리하는 예제를 작성하세요.  
2. `shared_ptr` 순환 참조 문제를 코드로 시뮬레이션하고, `weak_ptr`로 해결해보세요.  
3. `unique_ptr`과 `shared_ptr`의 성능 차이를 비교해보세요.  

---

## 🔎 심화 학습
- `shared_ptr`의 제어 블록 구조  
- `enable_shared_from_this` 사용법  
- `scoped_ptr`(Boost)와 표준 스마트 포인터 비교  

---

## 🪞 회고
- 왜 여전히 `raw pointer`가 필요한 경우가 있을까?  
- 스마트 포인터 사용 시 주의해야 할 점은 무엇일까?  

---

## 🔗 관련 페이지
- [C++ 메모리 관리]({{ site.baseurl }}/posts/cpp-memory-management/)  
- [RAII]({{ site.baseurl }}/posts/whatis-raii/)  