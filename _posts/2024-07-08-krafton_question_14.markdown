---
layout: post
title:  "백준 2751번 python 풀이 - 수 정렬하기2"
date:   2024-07-08 09:30:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 정렬]
---

## 문제 링크
[2751번](https://www.acmicpc.net/problem/2751)

## 해결책
```python
import sys

N = int(input())

numList = []
for i in range(N):
    numList.append(int(sys.stdin.readline().rstrip()))


numList = quickSort(numList)
for i in numList:
    sys.stdout.write(str(i))
    sys.stdout.write('\n')

```

## 주석으로 달 설명

두번째 정렬 문제.
이번에는 데이터셋이 조금 크다.

처음에는 피벗을 선정하는 알고리즘을 추가한 피벗 정렬을 사용했다.

피벗의 선택 과정은 다음과 같다.

1. 데이터셋의 첫번째, 중간, 마지막의 값을 가져온다.
2. 이 셋을 정렬한다.
3. 중간값을 피벗값으로 정하고, 첫번째를 A List, 중간값 및 마지막 값을 B List에 저장한다.
4. 나머지 요소들에 대해 pivot과 대소비교를 진행한다.

하지만 지속적으로, 메모리 초과가 등장했다.
내부 구현이 조금 복잡하게 되었나 보다.

```python
import sys

def quickSort(myList):
    if (len(myList) <= 1):
        return myList

    if (len(myList) == 2):
        if myList[0] > myList[1]:
            temp = myList[0]
            myList[0] = myList[1]
            myList[1] = temp
        return myList

    # if(myList[0] > myList[len(myList)]):

    # pivot = pivotChoice(myList[0], myList[len(myList) // 2], myList[len(myList) - 1], 0, len(myList) // 2,len(myList) - 1)

    listTemp = [myList[0], myList[len(myList) // 2], myList[-1]]
    listTemp.sort()
    listA = [listTemp[0]]
    temp = listTemp[1]
    myList[len(myList) // 2] = myList[-2]
    listB = [temp, listTemp[2]]

    pivot = temp

    for i in range(1, len(myList) - 2):
        if myList[i] > pivot:
            listB.append(myList[i])
        elif myList[i] < pivot:
            listA.append(myList[i])

    return quickSort(listA) + quickSort(listB)

N = int(sys.stdin.readline())

num_list = []
for i in range(N):
    num_list.append(int(sys.stdin.readline()))

num_list = quickSort(num_list)

for i in num_list:
    print(i)
```


그래서 일단 sort를 사용해보기로 했다.
나머지 문제들을 해결하고, 그 뒤에 추가적으로 다시 구현해보자.

