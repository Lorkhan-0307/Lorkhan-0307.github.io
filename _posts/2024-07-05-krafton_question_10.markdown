---
layout: post
title:  "백준 10872번 python 풀이 - 팩토리얼"
date:   2024-07-05 15:56:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 브루트포스]
---

## 문제 링크
[10872번](https://www.acmicpc.net/problem/10872)

## 해결책
```python
N = int(input())


def factorial(n):
    if n == 0:
        return 1
    elif n == 1:
        return 1
    return n * factorial(n - 1)


print(factorial(N))
```

## 주석으로 달 설명

그리 어렵게 진행하지 않았다.
단순한 재귀함수로 구현 완료하였다.
