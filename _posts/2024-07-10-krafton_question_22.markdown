---
layout: post
title:  "백준 2630번 python 풀이 - 색종이 만들기"
date:   2024-07-09 10:00:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 분할정복]
---

## 문제 링크
[2630번](https://www.acmicpc.net/problem/2630)

## 해결책
```python
import sys

N = int(sys.stdin.readline().rstrip())

paperColor = []
for i in range(N):
    paperColor.append(list(map(int, sys.stdin.readline().rstrip().split())))

# 파란색 수(칸의 총합)에 따른 파란색 종이와 하얀색 종이
sheet = [[[0, 1]], [[0, 1], [1, 3], [2, 2], [3, 1], [1, 0]]]

blue = 0
white = 0


def dfs(n):
    global blue
    global white
    stack = [(0, n, 0, n)]

    while stack:
        minX, maxX, minY, maxY = stack.pop()

        _sum = 0
        for i in range(minX, maxX):
            for j in range(minY, maxY):
                _sum += paperColor[i][j]

        if (maxX - minX) == 2:
            blue += sheet[1][_sum][0]
            white += sheet[1][_sum][1]

        # 모두 파랄 경우
        elif _sum == (maxX-minX) ** 2:
            blue += 1
        # 모두 하얄 경우
        elif _sum == 0:
            white += 1

        else:
            stack.append((minX, (minX + maxX) // 2, minY, (minY+maxY) // 2))
            stack.append(((minX + maxX) // 2, maxX, minY, (minY+maxY) // 2))
            stack.append((minX, (minX + maxX) // 2, (minY+maxY) // 2, maxY))
            stack.append(((minX + maxX) // 2, maxX, (minY+maxY) // 2, maxY))


dfs(N)
print(white)
print(blue)
```

## 주석으로 달 설명

분할 정복...
Divide and Conquer. 커맨드 앤 컨커 같은 이 방식은 분할을 통해 문제를 계속 쉽게 만들어 해결하는 것이다.

이번 문제에서 이를 해결하기 위해, NXN의 value들의 총합을 구하고, 이를 N^2와 비교하여 0 이면 전부 하양, N^2이면 전부 파랑, 아니면 4칸으로 분할하여 이들을 스택에 넣고, 다 확인하여 전부 분할할 때까지 진행하였다.
