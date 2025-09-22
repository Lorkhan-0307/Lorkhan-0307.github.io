---
layout: post
title: "vtable(virtual table) - C++/C#/CS 기초"
date: 2025-09-22 15:00:00 +0900
categories: [Tech Interview, Study Plan]
tags: [c++, c#, computer-science, algorithm, data-structure, operating-system, network, database, design-pattern]
slug: whatis-vtable
---

# vtable(virtual table)

## 📌 개념 정리
- [가상함수vptr]({{ site.baseurl }}/posts/whatis-vptr/)이 참조하는 테이블.
- [클래스]({{ site.baseurl }}/posts/whatis-class/)의 가상 함수 주소 목록을 저장.
- 실행 시 [다형성]({{ site.baseurl }}/posts/whatis-polymorphism/)을 보장.

## 💻 동작 흐름
1. 객체 안의 vptr이 [vtable]({{ site.baseurl }}/posts/whatis-vtable/) 주소를 가리킴.
2. 함수 호출 시 vtable을 참조해 실제 함수 실행.

## 예시
- `Dog` 클래스의 vtable에는 `Dog::Speak` 주소가 들어감.
- `Animal* a = new Dog(); a->Speak();` → vptr이 Dog vtable 가리킴.

## ⚡ 주의점
- 각 [클래스]({{ site.baseurl }}/posts/whatis-class/)마다 vtable 1개.
- RTTI(typeid, dynamic_cast)도 vtable을 활용.

## 🔗 관련 페이지
- [가상함수vptr]({{ site.baseurl }}/posts/whatis-vptr/)
- [다형성]({{ site.baseurl }}/posts/whatis-polymorphism/)
- [순수가상함수]({{ site.baseurl }}/posts/whatis-purevirtualfunction/)
- [추상클래스]({{ site.baseurl }}/posts/whatis-abstractclass/)
