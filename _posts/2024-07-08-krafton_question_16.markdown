---
layout: post
title:  "백준 2309번 python 풀이 - 일곱 난쟁이"
date:   2024-07-08 15:54:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 정렬]
---

## 문제 링크
[2309번](https://www.acmicpc.net/problem/2309)

## 해결책
```python
import sys

dwarf_height_list = []
height_sum = 0

def printDwarf(dwarf_height_list):

    for i in range(8):
        for j in range(i + 1, 9):
            if height_sum - dwarf_height_list[i] - dwarf_height_list[j] == 100:
                for k in range(9):
                    if k != i and k != j:
                        print(dwarf_height_list[k])
                return


for i in range(9):
    dwarf_height_list.append(int(sys.stdin.readline()))

for i in range(9):
    height_sum += dwarf_height_list[i]

dwarf_height_list.sort()

printDwarf(dwarf_height_list)
```

## 주석으로 달 설명

탐색 문제.
크게 특별할 건 없고, 리스트의 요소 중 두개를 잡는 부분에서 조금 시간을 단축시켰다.
모든 요소를 도는 방식이 아닌, 마치 버블 정렬시에 사용하는 전체 중 두개를 선택하는 이중 for 문을 이용해 요소를 선별하여, 단순한 N^2에서 시간을 조금이나마 줄여봤다.
