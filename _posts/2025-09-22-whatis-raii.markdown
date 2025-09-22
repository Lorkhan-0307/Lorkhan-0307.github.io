---
layout: post
title: "RAII(Resource Acquisition Is Initialization) - C++/C#/CS 기초"
date: 2025-09-22 15:00:00 +0900
categories: [Tech Interview, Study Plan]
tags: [c++, c#, computer-science, algorithm, data-structure, operating-system, network, database, design-pattern]
slug: whatis-raii
---

# RAII(Resource Acquisition Is Initialization)

## 📌 개념 정리
- 자원 획득이 초기화라는 의미의 C++ 프로그래밍 기법
- 객체의 생성자에서 자원을 획득하고 소멸자에서 해제
- 스마트 포인터, using 문 등에서 활용되는 패턴