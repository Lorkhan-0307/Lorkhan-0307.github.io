---
layout: post
title:  "백준 1920번 python 풀이 - 수 찾기"
date:   2024-07-08 16:43:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 이진탐색]
---

## 문제 링크
[1920번](https://www.acmicpc.net/problem/1920)

## 해결책
```python
import sys


def binarySearch(nums, objective):
    key = len(nums) // 2
    if key == 1:
        if objective not in nums:
            return False
        else:
            return True

    elif key == 0:
        if nums[0] != objective:
            return False
        else:
            return True

    elif objective == nums[0] or objective == nums[-1]:
        return True

    if nums[key] > objective:
        return binarySearch(nums[1:key], objective)
    elif nums[key] < objective:
        return binarySearch(nums[key:-1], objective)
    else:
        return True


def binarySearch_V2(nums, objective):
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == objective:
            return 1
        elif nums[mid] > objective:
            right = mid - 1
        else:
            left = mid + 1
    return 0


N = int(sys.stdin.readline())
numList = list(map(int, sys.stdin.readline().rstrip().split()))
numList.sort()
_min = numList[0]
_max = numList[-1]

N = int(sys.stdin.readline())
findList = list(map(int, sys.stdin.readline().rstrip().split()))
for i in findList:
    if binarySearch_V2(numList, i):
        print(1)
    else:
        print(0)
```

## 주석으로 달 설명

이번에는 이진탐색이다!

처음에는 재귀적으로 구현하였다. 하지만 재귀의 오버헤드 때문인지 시간초과가 나타났다.(혹은 재귀 호출 수가 많아서 느려진건가...?)

그래서, 재귀 없이 while문으로 작성하였다.
사실 차이가 없을꺼라 생각하고, 어느정도 반신반의하면서 좌, 우의 끝 부분을 좁혀가는 방식으로 구현하였다.
이는 잘 통했고, 정답으로 유도되었다.

while문을 통해, 좌 끝이 우 끝보다 작거나 같다는 조건 하에서, 정렬된 리스트의 중간 값보다 objective가 크면 좌 끝을 중간값 +1로, 반대의 경우에는 우 끝을 중간값 -1로, 중간값과 동일하면 True, 즉 1을 리턴하고, while문에서 벗어나면 False, 즉 0을 리턴하도록 구현했다.
