---
layout: post
title:  "백준 1629번 python 풀이 - 곱셈"
date:   2024-07-09 11:00:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 분할정복]
---

## 문제 링크
[1629번](https://www.acmicpc.net/problem/1629)

## 해결책
```python
import sys

A, B, C = map(int, sys.stdin.readline().rstrip().split())

remainArr = []
pattern = []


def findRemain(a, b, c):
    # b -> 2 / 2 / 2 / 2
    # 이런식이라고 생각해보자.
    # 그러면....
    if b == 1:
        return a % c
    if b %2 == 0:
        return (findRemain(a, b//2, c)**2)%c
    else:
        return ((findRemain(a, b // 2, c) ** 2)*a)%c


print(findRemain(A, B, C))

```

## 주석으로 달 설명

분할 정복..
이번은 좀 어려웠다.
사실 코드나 방법보다, % 기호가 분배공식이 적용된다는 줄 몰랐다.
다시 말해, (a % c)*(b % c)%c = (a*b)%c 의 방식으로 작성할 수 있다는 것이다.
