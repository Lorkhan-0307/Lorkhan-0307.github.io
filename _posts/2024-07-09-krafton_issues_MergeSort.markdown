---
layout: post
title:  "크래프톤 정글 주제별 탐구 -병합 정렬-"
date:   2024-07-09 00:41:00 +0900
categories: algorithm
tags: [python, 크래프톤정글개념정리, 병합 정렬]
---

# 정의
병합 정렬이란, Divide and Conquer(분할 정복) 알고리즘을 기반으로 하는 정렬 방법으로, 수열을 나누어 나눈 수열들끼리 비교함으로써 진행되는 정렬 방식이다.

많은 량의 데이터가 있을 때 유리하다.



## 구현
```python
import sys

N = int(sys.stdin.readline().rstrip())
arra = []

for i in range(N):
    arra.append(int(sys.stdin.readline().rstrip()))


def mergeSort(arr):
    if (len(arr) < 2):
        return arr

    mid = len(arr) // 2
    leftArr = mergeSort(arr[:mid])
    rightArr = mergeSort(arr[mid:])

    i = j = 0

    sortedArr = []
    while i < len(leftArr) and j < len(rightArr):
        if leftArr[i] < rightArr[j]:
            sortedArr.append(leftArr[i])
            i += 1
        else:
            sortedArr.append(rightArr[j])
            j += 1

    sortedArr += leftArr[i:]
    sortedArr += rightArr[j:]

    return sortedArr

for i in mergeSort(arra):
    print(i)
```



## 관련 문제 링크
[2751번](https://www.acmicpc.net/problem/2751)
