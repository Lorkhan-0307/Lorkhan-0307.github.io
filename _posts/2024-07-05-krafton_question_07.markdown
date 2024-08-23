---
layout: post
title:  "백준 2675번 python 풀이 - 문자열 반복"
date:   2024-07-05 14:55:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 반복문]
---

## 문제 링크
[2675번](https://www.acmicpc.net/problem/2675)

## 해결책
```python
T = int(input())

for i in range(T):
    R, S = input().split()
    R = int(R)
    for s in S:
        for j in range(R):
            print(s, end="")

    print()
```

## 주석으로 달 설명

이중 반복문을 통해 문제를 해결했다.
