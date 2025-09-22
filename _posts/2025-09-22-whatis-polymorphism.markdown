---
layout: post
title: "다형성(Polymorphism) - C++/C#/CS 기초"
date: 2025-09-22 15:00:00 +0900
categories: [Tech Interview, Study Plan]
tags: [c++, c#, computer-science, algorithm, data-structure, operating-system, network, database, design-pattern]
---

# 다형성(Polymorphism)

## 📌 개념 정리
- 동일한 인터페이스로 다양한 동작을 수행하는 [객체지향]({{ site.baseurl }}/posts/whatis-oop/) 특성.
- 정적 다형성: 함수 오버로딩, 템플릿.
- 동적 다형성: [가상함수vptr]({{ site.baseurl }}/posts/whatis-vptr/), [vtable]({{ site.baseurl }}/posts/whatis-vtable/) 기반.

## 💻 예제
```cpp
class Shape {
public:
    virtual void Draw() { cout << "Generic Shape\n"; }
};
class Circle : public Shape {
public:
    void Draw() override { cout << "Circle\n"; }
};
```

## ⚡ 주의점
- 다형적 삭제를 위해 소멸자는 virtual 권장.
- 성능 요구가 크면 정적 다형성(템플릿) 고려.

## 🔗 관련 페이지
- [객체지향]({{ site.baseurl }}/posts/whatis-oop/)
- [상속]({{ site.baseurl }}/posts/whatis-inheritance/)
- [가상함수vptr]({{ site.baseurl }}/posts/whatis-vptr/)
- [vtable]({{ site.baseurl }}/posts/whatis-vtable/)
- [추상클래스]({{ site.baseurl }}/posts/whatis-abstractclass/)
- [순수가상함수]({{ site.baseurl }}/posts/whatis-purevirtualfunction/)
