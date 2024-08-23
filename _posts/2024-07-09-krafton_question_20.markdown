---
layout: post
title:  "백준 2805번 python 풀이 - 나무 자르기"
date:   2024-07-09 00:41:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 이진탐색]
---

## 문제 링크
[2805번](https://www.acmicpc.net/problem/2805)

## 해결책
```python
import sys


def result_CutTree(arr, cut):
    result = 0
    for i in arr:
        if (i > cut):
            result += (i - cut)
    return result


def treeCuttingSearch(_min, _max, arr, target):
    triedList = []
    left, right = _min, _max
    mid = (left + right) // 2
    while (_min <= _max and mid not in triedList):
        triedList.append(mid)
        result = result_CutTree(arr, mid)
        if target == result:
            return mid
        elif target > result:
            right = mid - 1

        else:
            left = mid + 1

        mid = (left + right) // 2
    return mid


N, M = map(int, sys.stdin.readline().rstrip().split())
treeList = list(map(int, sys.stdin.readline().rstrip().split()))

treeList.sort()

print(treeCuttingSearch(0, treeList[-1] + 1, treeList, M))
```

## 주석으로 달 설명

이진탐색 2...!

이번에는 조금 독특한데...
이진탐색의 방향을 바꾼다.

만약 target과 result(자르고 남은 나무의 량)이 동일하다 하면 해당 값을 return 하는것이 최선이지만, 그렇지 않은 경우가 문제이다.
만약 target이 result보다 크다면, 즉 다시 말해 얻고자 하는 나무의 량이, 임의의 값을 넣었을 때, 얻어야 하는 나무의 량보다 적다면, right를 mid -1로 교체한다. 그렇게 하면, mid의 값이 감소하여, cutTree 함수에 넣었을 때 얻게 되는 나무의 량이 증가할 것이다.
반대의 경우, target 보다 result가 작다면, 얻고자 하는 나무의 량을 감소시켜야 한다. 이 때의 경우, mid의 값을 증가시켜, cutTree 함수에 넣었을 때 얻게 되는 나무의 량을 감소시킨다.

