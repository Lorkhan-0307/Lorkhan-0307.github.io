---
layout: post
title:  "백준 2750번 python 풀이 - 수 정렬하기"
date:   2024-07-06 17:40:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 정렬]
---

## 문제 링크
[2750번](https://www.acmicpc.net/problem/2750)

## 해결책
```python
# 물론 sort()를 쓰면 쉽지만... 구현 해야겠지...?
def bubbleSort(myList):
    for j in range(len(myList) - 1):
        for i in range(len(myList) - 1 - j):
            if myList[i] > myList[i + 1]:
                temp = myList[i]
                myList[i] = myList[i + 1]
                myList[i + 1] = temp
    return myList


def selectionSort(myList):
    for j in range(len(myList)):
        _min = 1001
        idx = 1001
        for i in range(j, len(myList)):
            if (myList[i] < _min):
                _min = myList[i]
                idx = i
        temp = myList[idx]
        myList[idx] = myList[j]
        myList[j] = temp

    return myList


def insertionSort(myList):
    # ??? 아 이거 구현 어떻게 하더라
    return myList


def quickSort(myList):
    if (len(myList) <= 1):
        return myList

    pivot = len(myList) // 2
    listA = []
    listB = []
    listPivot = [myList[pivot]]
    for i in range(len(myList)):
        if myList[i] > myList[pivot]:
            listB.append(myList[i])
        elif myList[i] < myList[pivot]:
            listA.append(myList[i])

    return quickSort(listA) + listPivot + quickSort(listB)


N = int(input())

numList = []
for i in range(N):
    numList.append(int(input()))

numList = quickSort(numList)
for i in numList:
    print(i)

```

## 주석으로 달 설명


정렬! 아 정말 쉬워보이면서도 복잡한 그 정렬!

일단은 기억나던 정렬인 버블 정렬을 먼저 구현을 하고, 그 뒤에 비슷하면서도 반대인 삽입 정렬을 구현했다.
이후, 퀵 정렬을 구현하였다.

그 외에도 다양한 정렬이 있지만, 일단 세개만 구현하고, 나머지는 이후 진행하자.
먼저, 각 정렬들을 한번 정리해보자.

#### 버블 정렬(Bubble Sort)
버블 정렬은 인접한 두 요소를 비교하고, 순서가 잘못되어 있다면 교환하는 방식의 정렬으로, N번 정렬을 진행할 때 마다, 제일 큰 수가 제일 뒤로 가게 된다.

시간 복잡도는 O(N^2)이다.
단순하지만 비효율적이고, 작은 데이터셋에서만 실용적이다.
즉, 빠르게 구현해야 하고, 데이터셋이 작다면 버블 정렬을 사용하자.

#### 선택 정렬(Selection Sort)
선택 정렬은 전체 리스트 중에 가장 작은 요소를 제일 앞의 요소와 교환하는 과정을 반복한다.

시간 복잡도는 O(N^2)이다.
이해하기 쉽고, 구현은 버블정렬보다 조금 귀찮은 정도? 하지만 역시 비효율적이다.

#### 삽입 정렬(Insertion Sort)
삽입 정렬은 배열의 요소를 하나씩 적절한 위치로 이동시킨다.
이게 무슨 말인고 하니...

삽입 정렬의 경우, 먼저 첫번째 요소는 정렬된 것으로 간주한다.
이후, 두번째 요소부터 배열의 올바른 위치로 이동시킨다.
만약, (5, 2, 9, 1, 5, 6)의 배열이 있다면

(5) -> (2,5) -> (2,5,9) -> (1,2,5,9) -> (1,2,5,5,9) -> (1,2,5,5,6,9)의 순서로 진행된다.

시간 복잡도는 최악의 경우(거꾸로 정렬된 배열) O(N^2), 최선의 경우 O(N), 평균적으로 O(N^2)이다.

삽입 정렬의 경우, 어느정도 정렬된 배열에 대해 높은 효율성을 지닌다.


#### 퀵 정렬(Quick Sort)
퀵 정렬은 피벗(pivot)을 설정하고, 피벗보다 큰 요소와 작은 요소를 따로 나눈 다음에, 각 정렬을 재귀적으로 실행한다.

시간 복잡도는 평균적으로 O(N log N), 최악의 경우 O(N^2)을 가진다.
평균적으로 매우 빠른 정렬 알고리즘이나, 피벗을 잘 선택하는 것이 중요하다.

#### 병합 정렬(Merge Sort)
병합 정렬은 전체 리스트를 반으로 나눈 뒤, 각각을 재귀적으로 정렬한 후에 이를 병합하는 방식으로 진행된다.
퀵 정렬과 매우 유사하다.

시간 복잡도는 평균적으로 O(N log N)을 지니며, 안정적이고 대용량 데이터를 다룰 때 효율적이다.


#### 힙 정렬(Heap Sort)
힙 정렬은 힙 자료구조를 이용하여 정렬하는 방식이다. 최대 힙이나 최소 힙을 구축한 뒤에, 요소를 하나씩 꺼내어 정렬하는 방식이다.

시간 복잡도는 O(N log N)이며, 항상 O(N log N)의 시간 복잡도를 보장하고, 제자리 정렬이다.


일단 이정도로 마무리하도록 하겠다.
