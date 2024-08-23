---
layout: post
title:  "백준 1181번 python 풀이 - 단어 정렬"
date:   2024-07-08 09:30:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 정렬]
---

## 문제 링크
[1181번](https://www.acmicpc.net/problem/1181)

## 해결책
```python
import sys

N = int(input())

strList = []
for i in range(N):
    strList.append(sys.stdin.readline().rstrip())


def comparison(strA, strB):
    if len(strA) > len(strB):
        return 1
    elif len(strA) < len(strB):
        return 0
    else:
        for i in range(len(strA)):
            if strA[i] > strB[i]:
                return 1
            elif strA[i] < strB[i]:
                return 0
            else:
                continue
        return 2


def sort3_forstr(a, b, c):
    if comparison(a, b):
        if comparison(a, c):
            if comparison(b, c):
                return [c, b, a]
            else:
                return [b, c, a]
        else:
            return [b, a, c]
    else:
        if comparison(a, c):
            return [c, a, b]
        else:
            if comparison(b, c):
                return [a, c, b]
            else:
                return [a, b, c]


def quickSort_forStr(strList):
    if len(strList) <= 1:
        return strList

    if len(strList) == 2:
        if comparison(strList[0], strList[1]) == 2:
            return [strList[0]]
        if comparison(strList[0], strList[1]):
            temp = strList[0]
            strList[0] = strList[1]
            strList[1] = temp
        return strList

    mid_index = len(strList) // 2

    if len(strList) == 3:
        return sort3_forstr(strList[0], strList[1], strList[2])

    firstThreeList = sort3_forstr(strList[0], strList[mid_index], strList[-1])

    pivot = firstThreeList[1]

    listA = [firstThreeList[0]]
    listB = [firstThreeList[1], firstThreeList[2]]

    strList[mid_index], strList[-2] = strList[-2], strList[mid_index]

    for i in range(1, len(strList) - 2):
        if comparison(strList[i], pivot):
            listB.append(strList[i])
        else:
            listA.append(strList[i])

    return quickSort_forStr(listA) + quickSort_forStr(listB)


noDupliStrList = []
for i in strList:
    if i not in noDupliStrList:
        noDupliStrList.append(i)

ans = quickSort_forStr(noDupliStrList)

for i in ans:
    print(i)
```

## 주석으로 달 설명

세번째 정렬 문제。

이번 정렬 문제의 경우 입력된 데이터는 string이고, 이 문자열들을 제시된 기준에 맞추어서 변경해야 한다.

이전에 제작한 quicksort를 조금 변형하여, 서로 대소비교를 하는 부분을 조건에 맞는 방식의 comparison 함수를 제작하여 비교하도록 제작하였다.
동시에, 처음 입력을 받고 나서, 이의 중복이 없는 방식을 진행하였는데, 이를 처음 입력받을 때 진행하면 더 좋지 않을까라는 생각과, 대소비교를 진행할 때 제외하는 방법을 사용하면 더 낫지 않을까라는 생각을 하게 되었다.
