---
layout: post
title: "클래스(Class) - C++/C#/CS 기초"
date: 2025-09-22 15:00:00 +0900
categories: [Tech Interview, Study Plan]
tags: [c++, c#, computer-science, algorithm, data-structure, operating-system, network, database, design-pattern]
---

# 클래스(CLASS)

## 📌 개념 정리
- [객체지향]({{ site.baseurl }}/posts/whatis-oop/)의 기본 단위.
- 데이터(멤버 변수)와 동작(멤버 함수)을 묶은 사용자 정의 타입.
- 접근 제어자: `public`, `protected`, `private`.
- C++에서는 `struct`도 사용 가능하나 기본 접근 지정자가 다름 (`class`는 private, `struct`는 public).

## 💻 예제
```cpp
class Animal {
private:
    int age;
public:
    Animal(int age) : age(age) {}
    void Speak() { cout << "Generic sound\n"; }
};
```

## ⚡ 주의점
- 캡슐화를 위해 멤버 변수는 보통 `private`로 두고, 동작 메서드를 통해 조작.
- 생성자/소멸자 활용해 객체 수명 관리.

## 🔗 관련 페이지
- [객체지향]({{ site.baseurl }}/posts/whatis-oop/)
- [상속]({{ site.baseurl }}/posts/whatis-inheritance/)
- [추상클래스]({{ site.baseurl }}/posts/whatis-abstractclass/)
- [다형성]({{ site.baseurl }}/posts/whatis-polymorphism/)
- [컴포지션]({{ site.baseurl }}/posts/whatis-composition/)
