---
layout: post
title:  "백준 9498번 python 풀이 - 시험 성적"
date:   2024-07-05 13:30:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 조건문]
---

## 문제 링크
[9498번](https://www.acmicpc.net/problem/9498)

## 해결책
```
score = int(input())

if score >= 90:
    print("A")
elif score >= 80:
    print("B")
elif score >= 70:
    print("C")
elif score >= 60:
    print("D")
else:
    print("F")
```

## 주석으로 달 설명

#### if 조건문

[python document](https://docs.python.org/3/reference/compound_stmts.html#if)

크게 뭔가 설명할 게 없다.. 일단은 지금처럼 잘 쓰자.
