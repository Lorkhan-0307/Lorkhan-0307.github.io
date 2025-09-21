---
layout: post
title: "C++ vtable 메커니즘: 가상 함수 동작 원리 완벽 분석"
date: 2025-09-21 15:04:00 +0900
categories: [Tech Interview, C++]
tags: [C++, vtable, virtual function, polymorphism, vptr, memory layout]
---

## 📌 학습 목표
- vtable(Virtual Function Table)의 구조와 동작 원리 이해
- 가상 함수 호출 시 메모리 레벨에서의 동작 과정 분석
- 다형성 구현을 위한 vtable의 역할 완벽 이해
- vtable과 RTTI의 관계 학습

---

## 📝 개념 정리

### vtable이란?

**핵심 개념:**
- 가상 함수의 주소들을 저장하는 함수 포인터 테이블
- 클래스마다 하나씩 존재 (객체마다가 아님)
- 컴파일 타임에 생성되어 실행 시 다형성 보장
- vptr(virtual pointer)이 이 테이블을 가리킴

**메모리 구조:**
```cpp
class Base {
public:
    virtual void func1() { cout << "Base::func1" << endl; }
    virtual void func2() { cout << "Base::func2" << endl; }
    void nonVirtualFunc() { cout << "Base::nonVirtual" << endl; }
};

class Derived : public Base {
public:
    void func1() override { cout << "Derived::func1" << endl; }
    virtual void func3() { cout << "Derived::func3" << endl; }
};

// Base 클래스의 vtable:
// [0] Base::func1의 주소
// [1] Base::func2의 주소

// Derived 클래스의 vtable:
// [0] Derived::func1의 주소 (오버라이드됨)
// [1] Base::func2의 주소 (상속됨)
// [2] Derived::func3의 주소 (새로 추가됨)
```

---

## 💻 동작 흐름 상세 분석

### 1. 객체 생성과 vptr 초기화
```cpp
void demonstrateVtableCreation() {
    cout << "=== 객체 생성과 vptr 초기화 ===" << endl;
    
    Base* basePtr = new Base();
    // 1. Base 객체 메모리 할당
    // 2. vptr이 Base 클래스의 vtable을 가리키도록 초기화
    
    Base* derivedPtr = new Derived();
    // 1. Derived 객체 메모리 할당
    // 2. vptr이 Derived 클래스의 vtable을 가리키도록 초기화
    
    cout << "Base 객체 크기: " << sizeof(Base) << " bytes" << endl;
    cout << "Derived 객체 크기: " << sizeof(Derived) << " bytes" << endl;
    // vptr 때문에 크기가 증가함 (보통 8바이트 추가, 64비트 시스템)
    
    delete basePtr;
    delete derivedPtr;
}
```

### 2. 가상 함수 호출 과정
```cpp
void demonstrateVirtualCall() {
    cout << "=== 가상 함수 호출 과정 ===" << endl;
    
    Base* ptr = new Derived();
    
    // ptr->func1() 호출 시 내부 동작:
    // 1. ptr이 가리키는 객체의 vptr을 읽음
    // 2. vptr이 가리키는 vtable에서 func1의 인덱스(0번) 위치 찾음
    // 3. 해당 위치에 저장된 함수 주소를 읽음
    // 4. 그 주소로 점프하여 함수 실행
    
    ptr->func1(); // "Derived::func1" 출력
    ptr->func2(); // "Base::func2" 출력
    
    delete ptr;
}
```

### 3. vtable과 메모리 레이아웃
```cpp
#include <iostream>
#include <cstdint>

class VtableDemo {
public:
    virtual void func1() { cout << "VtableDemo::func1" << endl; }
    virtual void func2() { cout << "VtableDemo::func2" << endl; }
    
    void printVtableInfo() {
        // vptr은 객체의 첫 번째 멤버
        uintptr_t* vptr = *reinterpret_cast<uintptr_t**>(this);
        
        cout << "객체 주소: " << this << endl;
        cout << "vptr 주소: " << vptr << endl;
        cout << "vtable[0] (func1): " << reinterpret_cast<void*>(vptr[0]) << endl;
        cout << "vtable[1] (func2): " << reinterpret_cast<void*>(vptr[1]) << endl;
    }
};

class DerivedDemo : public VtableDemo {
public:
    void func1() override { cout << "DerivedDemo::func1" << endl; }
    
    void compareVtables() {
        VtableDemo base;
        DerivedDemo derived;
        
        cout << "=== Base 객체의 vtable 정보 ===" << endl;
        base.printVtableInfo();
        
        cout << "\n=== Derived 객체의 vtable 정보 ===" << endl;
        derived.printVtableInfo();
    }
};
```

---

## ⚡ 주의점과 성능 고려사항

### 1. 메모리 오버헤드
```cpp
class WithoutVirtual {
    int data;
public:
    void func() { cout << "Non-virtual" << endl; }
};

class WithVirtual {
    int data;
public:
    virtual void func() { cout << "Virtual" << endl; }
};

void compareMemoryUsage() {
    cout << "WithoutVirtual 크기: " << sizeof(WithoutVirtual) << " bytes" << endl; // 4
    cout << "WithVirtual 크기: " << sizeof(WithVirtual) << " bytes" << endl;    // 16 (8 for vptr + 4 for data + padding)
}
```

### 2. 성능 영향
```cpp
class PerformanceTest {
public:
    // 비가상 함수 - 직접 호출, 인라인 최적화 가능
    void directCall() { /* 빠른 실행 */ }
    
    // 가상 함수 - 간접 호출, 인라인 최적화 어려움
    virtual void virtualCall() { /* 약간의 오버헤드 */ }
};

// 성능 비교 예제
void performanceComparison() {
    const int iterations = 10000000;
    
    PerformanceTest obj;
    PerformanceTest* ptr = &obj;
    
    auto start = chrono::high_resolution_clock::now();
    
    // 직접 호출
    for (int i = 0; i < iterations; ++i) {
        obj.directCall();
    }
    
    auto mid = chrono::high_resolution_clock::now();
    
    // 가상 함수 호출
    for (int i = 0; i < iterations; ++i) {
        ptr->virtualCall();
    }
    
    auto end = chrono::high_resolution_clock::now();
    
    cout << "직접 호출 시간: " 
         << chrono::duration_cast<chrono::microseconds>(mid - start).count() 
         << " us" << endl;
    cout << "가상 호출 시간: " 
         << chrono::duration_cast<chrono::microseconds>(end - mid).count() 
         << " us" << endl;
}
```

---

## 🔎 심화 학습: RTTI와 vtable

### RTTI (Run-Time Type Information)
```cpp
#include <typeinfo>

class RTTIDemo {
public:
    virtual ~RTTIDemo() = default;  // RTTI를 위해 가상 함수 필요
};

class RTTIDerived : public RTTIDemo {
public:
    void uniqueFunction() { cout << "RTTIDerived specific" << endl; }
};

void demonstrateRTTI() {
    RTTIDemo* ptr = new RTTIDerived();
    
    // typeid 연산자 - vtable의 type_info 사용
    cout << "실제 타입: " << typeid(*ptr).name() << endl;
    
    // dynamic_cast - vtable 정보 활용
    RTTIDerived* derivedPtr = dynamic_cast<RTTIDerived*>(ptr);
    if (derivedPtr) {
        cout << "dynamic_cast 성공!" << endl;
        derivedPtr->uniqueFunction();
    }
    
    delete ptr;
}
```

### vtable 구조 확장
```cpp
// 실제 vtable은 다음과 같은 구조를 가짐:
// [-2] offset to top (다중 상속에서 사용)
// [-1] type_info* (RTTI를 위한 타입 정보)
// [0]  첫 번째 가상 함수 주소
// [1]  두 번째 가상 함수 주소
// ...

class VtableStructure {
public:
    virtual ~VtableStructure() = default;
    virtual void func1() = 0;
    virtual void func2() = 0;
    
    void analyzeVtableStructure() {
        uintptr_t* vptr = *reinterpret_cast<uintptr_t**>(this);
        
        // type_info 포인터 접근 (vptr[-1])
        const type_info* ti = reinterpret_cast<const type_info*>(vptr[-1]);
        cout << "타입 정보: " << ti->name() << endl;
        
        // 가상 함수 주소들
        cout << "가상 소멸자: " << reinterpret_cast<void*>(vptr[0]) << endl;
        cout << "func1 주소: " << reinterpret_cast<void*>(vptr[1]) << endl;
        cout << "func2 주소: " << reinterpret_cast<void*>(vptr[2]) << endl;
    }
};
```

---

## 💡 실무 적용 팁

### 1. 가상 함수 사용 가이드라인
```cpp
// 좋은 예: 다형성이 필요한 경우
class GraphicObject {
public:
    virtual void draw() = 0;
    virtual void move(int x, int y) = 0;
    virtual ~GraphicObject() = default;
};

// 나쁜 예: 불필요한 가상 함수
class Point {
public:
    virtual int getX() const { return x; }  // 단순 getter에 virtual 불필요
    virtual int getY() const { return y; }
private:
    int x, y;
};
```

### 2. 가상 소멸자의 중요성
```cpp
class Base {
public:
    Base() { data = new int[100]; }
    
    // 가상 소멸자 없으면 메모리 누수 발생
    virtual ~Base() { 
        delete[] data;
        cout << "Base destructor" << endl;
    }
    
private:
    int* data;
};

class Derived : public Base {
public:
    Derived() { moreData = new int[200]; }
    
    ~Derived() {
        delete[] moreData;
        cout << "Derived destructor" << endl;
    }
    
private:
    int* moreData;
};

void virtualDestructorDemo() {
    Base* ptr = new Derived();
    delete ptr;  // 가상 소멸자 덕분에 Derived 소멸자도 호출됨
}
```

### 3. 성능 최적화 기법
```cpp
// 템플릿을 이용한 정적 다형성 (vtable 오버헤드 없음)
template<typename T>
class StaticPolymorphism {
public:
    void execute() {
        static_cast<T*>(this)->implementation();
    }
};

class FastImplementation : public StaticPolymorphism<FastImplementation> {
public:
    void implementation() {
        cout << "Fast implementation - no vtable overhead" << endl;
    }
};
```

---

## 🌐 외부 링크
- [C++ vtable 내부 구조](https://www.learncpp.com/cpp-tutorial/the-virtual-table/)
- [Itanium C++ ABI](https://itanium-cxx-abi.github.io/cxx-abi/abi.html#vtable)
- [GCC vtable 구현](https://gcc.gnu.org/onlinedocs/gcc-4.9.0/gcc/Vague-Linkage.html)

---

## 다음 학습 주제
- **가상 함수 고급 기법**: vptr 최적화, 함수 포인터 vs 가상 함수
- **다중 상속과 vtable**: 가상 상속, vtable 레이아웃 복잡성
- **최신 C++ 기능**: final, override 키워드 활용
- **성능 최적화**: 템플릿 기반 정적 다형성, CRTP 패턴

---

## 🪞 회고 질문
- vtable의 메모리 구조와 vptr의 관계를 정확히 설명할 수 있는가?
- 가상 함수 호출 시 발생하는 성능 오버헤드의 정확한 원인을 아는가?
- RTTI가 vtable을 어떻게 활용하는지 이해하고 있는가?
- 언제 가상 함수를 사용하고 언제 피해야 하는지 판단할 수 있는가?