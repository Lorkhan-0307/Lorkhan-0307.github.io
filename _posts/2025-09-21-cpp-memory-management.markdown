---
layout: post
title: "C++ 메모리 관리 완전 정복 - 스택 vs 힙, 스마트 포인터, RAII"
date: 2025-09-21 15:01:00 +0900
categories: [Tech Interview, C++]
tags: [c++, memory-management, smart-pointer, raii, stack, heap, unique_ptr, shared_ptr, weak_ptr]
slug: cpp-memory-management
---

## 📌 학습 목표
- 스택과 힙의 차이를 명확히 이해  
- 스마트 포인터(`unique_ptr`, `shared_ptr`, `weak_ptr`) 사용법 학습  
- RAII 개념과 활용  

---

## 📝 개념 정리

### 1. 스택(Stack)
- 함수 호출 시 **자동으로 할당/해제**되는 메모리 영역
- **지역 변수, 함수 인자** 저장
- **LIFO 구조**, 빠르지만 크기 제한이 있음

```cpp
void function() {
    int localVar = 42;  // 스택에 저장
    char arr[1024];     // 스택에 저장
} // 함수 종료 시 자동 해제
```

### 2. 힙(Heap)
- `new` / `delete` 를 통해 **동적 할당**되는 메모리
- 크기가 유연하지만 **관리 책임이 프로그래머**에게 있음
- 잘못 관리 시 **메모리 누수** 발생

```cpp
int* ptr = new int(42);  // 힙에 할당
delete ptr;              // 수동 해제 필요
```

### 3. 스마트 포인터
C++11에서 도입된 **자원 관리 도구**로, 메모리 누수를 방지합니다.

#### `unique_ptr` - 단일 소유권
```cpp
std::unique_ptr<int> ptr = std::make_unique<int>(42);
// 자동으로 delete 호출, 복사 불가, 이동만 가능
```

#### `shared_ptr` - 참조 카운팅 기반 공유
```cpp
std::shared_ptr<int> ptr1 = std::make_shared<int>(42);
std::shared_ptr<int> ptr2 = ptr1;  // 참조 카운트 증가
// 마지막 참조가 사라질 때 자동 해제
```

#### `weak_ptr` - 순환 참조 방지
```cpp
std::weak_ptr<Node> weakPtr = sharedPtr;
if (auto locked = weakPtr.lock()) {
    // 안전하게 접근
}
```

### 4. RAII (Resource Acquisition Is Initialization)
- **객체의 생성 시 자원 획득, 소멸 시 자원 해제**
- C++의 핵심 원칙 중 하나
- 예: 파일 핸들, 뮤텍스, 스마트 포인터 등

---

## 💻 실전 예제

### 스마트 포인터 활용
```cpp
#include <iostream>
#include <memory>
using namespace std;

class FileHandler {
public:
    FileHandler() { cout << "파일 열기\n"; }
    ~FileHandler() { cout << "파일 닫기\n"; }
    void process() { cout << "파일 처리 중...\n"; }
};

void smartPointerExample() {
    // unique_ptr 사용
    auto filePtr = make_unique<FileHandler>();
    filePtr->process();
    
    // shared_ptr 사용
    auto sharedFile = make_shared<FileHandler>();
    {
        auto anotherRef = sharedFile;  // 참조 카운트: 2
        anotherRef->process();
    } // 참조 카운트: 1
    
    // 함수 종료 시 자동 해제
}
```

### RAII 패턴 구현
```cpp
class LockGuard {
    std::mutex& mtx;
public:
    LockGuard(std::mutex& m) : mtx(m) {
        mtx.lock();
        cout << "뮤텍스 잠금\n";
    }
    
    ~LockGuard() {
        mtx.unlock();
        cout << "뮤텍스 해제\n";
    }
};

void threadSafeFunction() {
    static std::mutex mtx;
    LockGuard guard(mtx);  // RAII를 활용한 자동 관리
    
    // 임계 영역 코드
    cout << "스레드 안전 작업 수행\n";
    
    // 함수 종료 시 자동으로 뮤텍스 해제
}
```

---

## 🎯 연습 문제

1. **스마트 포인터 비교**
   - `unique_ptr`와 `shared_ptr`의 차이를 설명하고, 순환 참조 문제를 `weak_ptr`로 해결하는 코드를 작성하세요.

2. **RAII 구현**
   - 파일을 자동으로 관리하는 `FileManager` 클래스를 RAII 패턴으로 구현하세요.

3. **메모리 분석**
   - 다음 코드의 메모리 사용 패턴을 분석하고 개선점을 제시하세요:
   ```cpp
   void problematicCode() {
       int* arr = new int[1000];
       // 작업 수행
       // delete[] 누락!
   }
   ```

---

## 🔗 관련 링크

### 공식 문서
- [C++ 스마트 포인터 - cppreference](https://en.cppreference.com/w/cpp/memory)
- [RAII - cppreference](https://en.cppreference.com/w/cpp/language/raii)

### 추천 자료
- [Effective Modern C++ - Scott Meyers](https://www.amazon.com/Effective-Modern-Specific-Ways-Improve/dp/1491903996)

---

## 🔎 심화 학습

- `malloc/free`와 `new/delete` 차이점
- `shared_ptr`의 내부 동작(제어 블록)
- `std::scoped_lock`과 같은 RAII 활용 사례
- 메모리 풀(Memory Pool) 개념
- 커스텀 알로케이터 구현

---

## 💡 실무 적용 팁

1. **항상 스마트 포인터 우선 고려**
   - `new/delete` 대신 `make_unique/make_shared` 사용

2. **RAII 패턴 적극 활용**
   - 자원 관리가 필요한 모든 곳에 적용

3. **성능 고려사항**
   - `unique_ptr`은 일반 포인터와 동일한 성능
   - `shared_ptr`은 참조 카운팅 오버헤드 존재

---

## 🪞 회고 질문

- 스택과 힙의 차이를 **메모리 관점**에서 설명할 수 있는가?
- 실무에서 스마트 포인터를 **언제 선택**해야 하는지 구분할 수 있는가?
- RAII 개념이 없는 언어(예: C)에서는 어떻게 자원 관리를 해야 할까?
- 순환 참조가 발생할 수 있는 **실제 상황**을 예시로 들 수 있는가?

---

## 🚀 다음 학습 주제

다음으로는 **C# 메모리 관리**에서 가비지 컬렉터의 동작 원리와 C++의 수동 메모리 관리와의 차이점을 알아보겠습니다!