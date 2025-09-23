---
layout: post
title: "가상 함수 vptr(virtual pointer) - C++/C#/CS 기초"
date: 2025-09-22 15:00:00 +0900
categories: [Tech Interview, Study Plan]
tags: [c++, c-sharp, computer-science, algorithm, data-structure, operating-system, network, database, design-pattern]
slug: whatis-vptr
---

# 가상 함수 vptr(virtual pointer)

## 📌 개념 정리
- [클래스]({{ site.baseurl }}/posts/whatis-class/)에 `virtual` 함수가 있으면, 객체에 **vptr(virtual pointer)** 생성.
- vptr은 [vtable]({{ site.baseurl }}/posts/whatis-vtable/)을 가리키며 런타임에 함수 호출을 결정.

## 💻 예제
```cpp
class Animal {
public:
    virtual void Speak() { cout << "Animal\n"; }
};
```
- 객체가 생성되면 `vptr`이 초기화되어 [vtable]({{ site.baseurl }}/posts/whatis-vtable/)을 참조.
- `a->Speak();` 호출 시 vptr → vtable → 실제 함수 주소 실행.

## ⚡ 주의점
- 모든 객체마다 vptr 1개 저장 (메모리 오버헤드).
- 다형적 동작 없으면 불필요한 비용.

## 🔗 관련 페이지
- [클래스]({{ site.baseurl }}/posts/whatis-class/)
- [다형성]({{ site.baseurl }}/posts/whatis-polymorphism/)
- [vtable]({{ site.baseurl }}/posts/whatis-vtable/)
- [순수가상함수]({{ site.baseurl }}/posts/whatis-purevirtualfunction/)
- [추상클래스]({{ site.baseurl }}/posts/whatis-abstractclass/)
