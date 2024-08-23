---
layout: post
title:  "백준 8983번 python 풀이 - 사냥꾼"
date:   2024-07-10 09:00:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 이진탐색]
---

## 문제 링크
[8983번](https://www.acmicpc.net/problem/8983)

## 해결책
```python
import sys

M, N, L = map(int, sys.stdin.readline().rstrip().split())

xList = list(map(int, sys.stdin.readline().rstrip().split()))

animalList = []


ableList = []

xList.sort()


maxA = 0

for i in range(N):
    a, b = map(int, sys.stdin.readline().rstrip().split())
    # if a > maxA: maxA = a
    if b <= L:
        animalList.append(list((a, b)))

animalList.sort()


'''
for x in xList:
    for j in animalList:
        if j not in ableList:
            if j[1] - L <= x - j[0] <= L - j[1]:
                ableList.append(j)
    for i in ableList:
        if(i in animalList):
            animalList.remove(i)

print(ableList)
'''

ans = 0
for animal in animalList:
    left, right = 0, len(xList) - 1
    while left <= right:
        mid = (left + right)//2
        if xList[mid] - animal[0] > L - animal[1]:
            right = mid - 1
        elif xList[mid] - animal[0] < animal[1] - L:
            left = mid + 1
        else:
            ans += 1
            break
print(ans)


```

## 주석으로 달 설명

이진탐색 3...!
풀이가 이진탐색인 것만 알면 금방 풀 수 있다.
물론 그걸 눈치채는 것이 어려운 부분이기는 하지만...
조건은 `|xi-aj| + bj`, 다시말해 `j[1] - L <= x - j[0] <= L - j[1]` 일 때 가능하다.
이를 오히려 역으로 뒤집어서, animal의 좌표를 기준으로 x, 즉 포수의 위치를 이진탐색으로 찾아내서, 이가 존재하면 ans+=1, 없으면 break를 하고 다른것을 찾는 방식으로 진행하는 것이다.

