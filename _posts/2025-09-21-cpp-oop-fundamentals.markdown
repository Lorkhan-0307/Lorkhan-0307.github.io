---
layout: post
title: "C++ 객체지향 프로그래밍 완벽 가이드: 클래스, 상속, 가상 함수"
date: 2025-09-21 15:03:00 +0900
categories: [Tech Interview, C++]
tags: [C++, OOP, class, inheritance, polymorphism, virtual function, vtable, abstract class]
---

## 📌 학습 목표
- 객체지향 핵심 개념(클래스, 상속, 다형성) 완벽 이해
- 가상 함수와 vtable 메커니즘 심층 분석
- 순수 가상 함수, 추상 클래스, 다이아몬드 상속 문제 해결 방법 학습
- 상속과 컴포지션의 적절한 선택 기준 습득

---

## 📝 개념 정리

### 1. 클래스 (Class)

**핵심 원리:**
- 객체지향 프로그래밍의 기본 단위
- 데이터(멤버 변수)와 동작(멤버 함수)을 하나로 묶는 사용자 정의 타입
- 캡슐화를 통한 데이터 은닉과 인터페이스 제공

**접근 지정자:**
```cpp
class Student {
private:    // 외부에서 접근 불가
    int studentId;
    string name;
    
protected:  // 상속받은 클래스에서만 접근 가능
    float gpa;
    
public:     // 어디서든 접근 가능
    // 생성자
    Student(int id, const string& n) : studentId(id), name(n), gpa(0.0f) {}
    
    // 접근자 메서드 (Getter)
    int getId() const { return studentId; }
    string getName() const { return name; }
    
    // 설정자 메서드 (Setter)
    void setGpa(float newGpa) {
        if (newGpa >= 0.0f && newGpa <= 4.0f) {
            gpa = newGpa;
        }
    }
    
    // 소멸자
    ~Student() {
        cout << "Student " << name << " destroyed" << endl;
    }
};
```

**생성자와 소멸자:**
```cpp
class Resource {
private:
    int* data;
    size_t size;
    
public:
    // 기본 생성자
    Resource() : data(nullptr), size(0) {}
    
    // 매개변수 생성자
    Resource(size_t s) : size(s) {
        data = new int[size];
        cout << "Resource allocated: " << size << " integers" << endl;
    }
    
    // 복사 생성자 (깊은 복사)
    Resource(const Resource& other) : size(other.size) {
        data = new int[size];
        for (size_t i = 0; i < size; ++i) {
            data[i] = other.data[i];
        }
        cout << "Resource copied" << endl;
    }
    
    // 이동 생성자 (C++11)
    Resource(Resource&& other) noexcept : data(other.data), size(other.size) {
        other.data = nullptr;
        other.size = 0;
        cout << "Resource moved" << endl;
    }
    
    // 대입 연산자
    Resource& operator=(const Resource& other) {
        if (this != &other) {
            delete[] data;
            size = other.size;
            data = new int[size];
            for (size_t i = 0; i < size; ++i) {
                data[i] = other.data[i];
            }
        }
        return *this;
    }
    
    // 소멸자
    ~Resource() {
        delete[] data;
        cout << "Resource deallocated" << endl;
    }
};
```

### 2. 상속 (Inheritance)

**기본 상속:**
```cpp
class Vehicle {
protected:
    string brand;
    int year;
    
public:
    Vehicle(const string& b, int y) : brand(b), year(y) {}
    
    virtual void start() {
        cout << "Vehicle starting..." << endl;
    }
    
    virtual void displayInfo() {
        cout << "Brand: " << brand << ", Year: " << year << endl;
    }
    
    virtual ~Vehicle() = default;  // 가상 소멸자
};

class Car : public Vehicle {
private:
    int numDoors;
    
public:
    Car(const string& b, int y, int doors) 
        : Vehicle(b, y), numDoors(doors) {}
    
    void start() override {
        cout << "Car engine starting..." << endl;
    }
    
    void displayInfo() override {
        Vehicle::displayInfo();  // 부모 클래스 메서드 호출
        cout << "Doors: " << numDoors << endl;
    }
    
    void honk() {  // Car만의 고유 메서드
        cout << "Beep beep!" << endl;
    }
};
```

**다중 상속:**
```cpp
class Flyable {
public:
    virtual void fly() = 0;
    virtual ~Flyable() = default;
};

class Swimmable {
public:
    virtual void swim() = 0;
    virtual ~Swimmable() = default;
};

class Duck : public Vehicle, public Flyable, public Swimmable {
public:
    Duck() : Vehicle("Duck", 2023) {}
    
    void start() override {
        cout << "Duck waddles away..." << endl;
    }
    
    void fly() override {
        cout << "Duck flying!" << endl;
    }
    
    void swim() override {
        cout << "Duck swimming!" << endl;
    }
};
```

### 3. 다형성 (Polymorphism)

**컴파일 타임 다형성 (정적 다형성):**
```cpp
// 함수 오버로딩
class Calculator {
public:
    int add(int a, int b) {
        return a + b;
    }
    
    double add(double a, double b) {
        return a + b;
    }
    
    string add(const string& a, const string& b) {
        return a + b;
    }
};

// 템플릿
template<typename T>
class Container {
private:
    T data;
    
public:
    Container(const T& d) : data(d) {}
    
    T getValue() const { return data; }
    
    void setValue(const T& d) { data = d; }
};
```

**런타임 다형성 (동적 다형성):**
```cpp
class Shape {
public:
    virtual double area() const = 0;  // 순수 가상 함수
    virtual double perimeter() const = 0;
    virtual void draw() const {
        cout << "Drawing a shape" << endl;
    }
    virtual ~Shape() = default;
};

class Circle : public Shape {
private:
    double radius;
    
public:
    Circle(double r) : radius(r) {}
    
    double area() const override {
        return 3.14159 * radius * radius;
    }
    
    double perimeter() const override {
        return 2 * 3.14159 * radius;
    }
    
    void draw() const override {
        cout << "Drawing a circle with radius " << radius << endl;
    }
};

class Rectangle : public Shape {
private:
    double width, height;
    
public:
    Rectangle(double w, double h) : width(w), height(h) {}
    
    double area() const override {
        return width * height;
    }
    
    double perimeter() const override {
        return 2 * (width + height);
    }
    
    void draw() const override {
        cout << "Drawing a rectangle " << width << "x" << height << endl;
    }
};
```

### 4. 가상 함수와 vtable

**vtable 동작 원리:**
```cpp
class Base {
public:
    virtual void func1() { cout << "Base::func1" << endl; }
    virtual void func2() { cout << "Base::func2" << endl; }
    void nonVirtualFunc() { cout << "Base::nonVirtualFunc" << endl; }
};

class Derived : public Base {
public:
    void func1() override { cout << "Derived::func1" << endl; }
    // func2는 오버라이드하지 않음
    void nonVirtualFunc() { cout << "Derived::nonVirtualFunc" << endl; }
};

// 메모리 구조 분석
void demonstrateVtable() {
    Base* basePtr = new Derived();
    
    // 가상 함수 호출 - vtable을 통해 실제 타입의 함수 호출
    basePtr->func1();          // "Derived::func1" 출력
    basePtr->func2();          // "Base::func2" 출력 (오버라이드 없음)
    
    // 비가상 함수 호출 - 컴파일 타임에 결정
    basePtr->nonVirtualFunc(); // "Base::nonVirtualFunc" 출력
    
    delete basePtr;
}
```

**가상 소멸자의 중요성:**
```cpp
class Base {
public:
    Base() { cout << "Base constructor" << endl; }
    virtual ~Base() { cout << "Base destructor" << endl; }  // 가상 소멸자
};

class Derived : public Base {
private:
    int* data;
    
public:
    Derived() : data(new int[100]) {
        cout << "Derived constructor" << endl;
    }
    
    ~Derived() {
        delete[] data;
        cout << "Derived destructor" << endl;
    }
};

void demonstrateVirtualDestructor() {
    Base* ptr = new Derived();
    delete ptr;  // 가상 소멸자 덕분에 Derived의 소멸자도 호출됨
    // 출력: Derived destructor, Base destructor
}
```

---

## 💻 실무 활용 예제

### 1. 추상 클래스와 팩토리 패턴
```cpp
class Database {
public:
    virtual void connect() = 0;
    virtual void disconnect() = 0;
    virtual void executeQuery(const string& query) = 0;
    virtual ~Database() = default;
};

class MySQLDatabase : public Database {
public:
    void connect() override {
        cout << "Connecting to MySQL database..." << endl;
    }
    
    void disconnect() override {
        cout << "Disconnecting from MySQL database..." << endl;
    }
    
    void executeQuery(const string& query) override {
        cout << "Executing MySQL query: " << query << endl;
    }
};

class PostgreSQLDatabase : public Database {
public:
    void connect() override {
        cout << "Connecting to PostgreSQL database..." << endl;
    }
    
    void disconnect() override {
        cout << "Disconnecting from PostgreSQL database..." << endl;
    }
    
    void executeQuery(const string& query) override {
        cout << "Executing PostgreSQL query: " << query << endl;
    }
};

// 팩토리 클래스
class DatabaseFactory {
public:
    static unique_ptr<Database> createDatabase(const string& type) {
        if (type == "mysql") {
            return make_unique<MySQLDatabase>();
        } else if (type == "postgresql") {
            return make_unique<PostgreSQLDatabase>();
        }
        return nullptr;
    }
};
```

### 2. 다이아몬드 상속 문제 해결
```cpp
class Animal {
public:
    virtual void eat() {
        cout << "Animal eating..." << endl;
    }
};

// 가상 상속으로 다이아몬드 문제 해결
class Mammal : virtual public Animal {
public:
    void breathe() {
        cout << "Mammal breathing air..." << endl;
    }
};

class WingedAnimal : virtual public Animal {
public:
    void fly() {
        cout << "Flying..." << endl;
    }
};

class Bat : public Mammal, public WingedAnimal {
public:
    void eat() override {
        cout << "Bat eating insects..." << endl;
    }
    
    void echolocate() {
        cout << "Using echolocation..." << endl;
    }
};

void demonstrateDiamondInheritance() {
    Bat bat;
    bat.eat();        // Bat의 eat() 호출
    bat.breathe();    // Mammal의 breathe() 호출
    bat.fly();        // WingedAnimal의 fly() 호출
    bat.echolocate(); // Bat의 고유 메서드
    
    // Animal이 한 번만 상속됨을 확인
    Animal* animalPtr = &bat;
    animalPtr->eat(); // Bat::eat() 호출
}
```

### 3. 컴포지션 vs 상속
```cpp
// 상속 방식
class Engine {
public:
    void start() { cout << "Engine starting..." << endl; }
    void stop() { cout << "Engine stopping..." << endl; }
};

// 잘못된 상속 사용 (is-a 관계가 아님)
class BadCar : public Engine {
public:
    void drive() {
        start();  // Engine의 메서드 직접 호출
        cout << "Car driving..." << endl;
    }
};

// 올바른 컴포지션 사용 (has-a 관계)
class GoodCar {
private:
    Engine engine;  // 컴포지션
    
public:
    void start() {
        engine.start();  // Engine 객체의 메서드 위임
    }
    
    void stop() {
        engine.stop();
    }
    
    void drive() {
        start();
        cout << "Car driving..." << endl;
    }
};
```

---

## 🎯 연습 문제

### 1. 도형 클래스 계층 구조 구현
```cpp
// 추상 기본 클래스
class Shape {
public:
    virtual double area() const = 0;
    virtual double perimeter() const = 0;
    virtual void draw() const = 0;
    virtual ~Shape() = default;
};

// 구체적인 도형 클래스들 구현
class Circle : public Shape {
private:
    double radius;
public:
    Circle(double r) : radius(r) {}
    // area(), perimeter(), draw() 구현
};

class Square : public Shape {
private:
    double side;
public:
    Square(double s) : side(s) {}
    // area(), perimeter(), draw() 구현
};

class Triangle : public Shape {
private:
    double a, b, c;
public:
    Triangle(double a, double b, double c) : a(a), b(b), c(c) {}
    // area(), perimeter(), draw() 구현
};

// 다형성 테스트 함수
void testPolymorphism() {
    vector<unique_ptr<Shape>> shapes;
    shapes.push_back(make_unique<Circle>(5.0));
    shapes.push_back(make_unique<Square>(4.0));
    shapes.push_back(make_unique<Triangle>(3.0, 4.0, 5.0));
    
    for (const auto& shape : shapes) {
        shape->draw();
        cout << "Area: " << shape->area() << endl;
        cout << "Perimeter: " << shape->perimeter() << endl;
        cout << "---" << endl;
    }
}
```

### 2. virtual vs non-virtual 함수 비교
```cpp
class BaseClass {
public:
    virtual void virtualFunc() {
        cout << "Base::virtualFunc" << endl;
    }
    
    void nonVirtualFunc() {
        cout << "Base::nonVirtualFunc" << endl;
    }
};

class DerivedClass : public BaseClass {
public:
    void virtualFunc() override {
        cout << "Derived::virtualFunc" << endl;
    }
    
    void nonVirtualFunc() {
        cout << "Derived::nonVirtualFunc" << endl;
    }
};

void compareVirtualNonVirtual() {
    BaseClass* ptr = new DerivedClass();
    
    ptr->virtualFunc();    // "Derived::virtualFunc" - 런타임에 결정
    ptr->nonVirtualFunc(); // "Base::nonVirtualFunc" - 컴파일타임에 결정
    
    delete ptr;
}
```

### 3. override와 final 키워드
```cpp
class Base {
public:
    virtual void func1() {}
    virtual void func2() {}
    virtual void func3() final {}  // 더 이상 오버라이드 불가
};

class Derived : public Base {
public:
    void func1() override {}  // 올바른 오버라이드
    void func2() override {}  // 올바른 오버라이드
    // void func3() override {} // 컴파일 에러! final 함수는 오버라이드 불가
};

class FinalClass final : public Base {  // 더 이상 상속 불가
public:
    void func1() override {}
};

// class CannotInherit : public FinalClass {};  // 컴파일 에러!
```

---

## 🔎 심화 학습

### RTTI (Run-Time Type Information)
```cpp
#include <typeinfo>

void demonstrateRTTI() {
    Base* ptr = new Derived();
    
    // typeid 연산자
    cout << "Type: " << typeid(*ptr).name() << endl;
    
    // dynamic_cast
    Derived* derivedPtr = dynamic_cast<Derived*>(ptr);
    if (derivedPtr) {
        cout << "Successfully cast to Derived" << endl;
    }
    
    delete ptr;
}
```

### 가상 함수 성능 고려사항
```cpp
// 가상 함수는 약간의 성능 오버헤드가 있음
// - 간접 함수 호출 (vtable 룩업)
// - 인라인 최적화 어려움
// - 추가 메모리 사용 (vptr)

class PerformanceCritical {
public:
    // 성능이 중요한 경우 비가상 함수 고려
    void fastFunction() { /* 인라인 가능 */ }
    
    // 다형성이 필요한 경우에만 가상 함수 사용
    virtual void polymorphicFunction() = 0;
};
```

---

## 🌐 외부 링크
- [cppreference - Virtual Functions](https://en.cppreference.com/w/cpp/language/virtual)
- [Scott Meyers - Effective C++](https://www.aristeia.com/books.html)
- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/)

---

## 💡 실무 적용 팁

1. **상속 vs 컴포지션 선택 기준**:
   - "is-a" 관계: 상속 사용
   - "has-a" 관계: 컴포지션 사용
   - 코드 재사용만 목적이라면 컴포지션 고려

2. **가상 함수 사용 가이드라인**:
   - 다형성이 필요한 경우에만 virtual 사용
   - 기본 클래스의 소멸자는 항상 virtual로 선언
   - 성능이 중요한 부분에서는 신중하게 고려

3. **메모리 관리**:
   - 스마트 포인터 활용으로 자동 메모리 관리
   - RAII 패턴으로 자원 관리
   - 가상 소멸자로 올바른 소멸 보장

---

## 다음 학습 주제
- **고급 C++ 기능**: 템플릿 메타프로그래밍, SFINAE, Concepts
- **디자인 패턴**: 싱글톤, 팩토리, 옵저버, 전략 패턴
- **C++20 모던 기능**: 모듈, 코루틴, Ranges
- **메모리 관리**: 스마트 포인터, 커스텀 할당자, 메모리 풀

---

## 🪞 회고 질문
- 객체지향의 4가지 특성을 실무 예시와 함께 설명할 수 있는가?
- vtable의 동작 원리를 메모리 구조와 함께 설명할 수 있는가?
- 상속과 컴포지션 중 어떤 것을 선택할지 판단 기준을 가지고 있는가?
- 가상 함수 사용 시 성능 고려사항을 인지하고 적절히 판단할 수 있는가?