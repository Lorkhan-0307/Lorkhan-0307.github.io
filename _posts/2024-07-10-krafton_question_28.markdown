---
layout: post
title:  "백준 2309번 python 풀이 - 최대 힙"
date:   2024-07-10 22:03:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 우선순위큐, 힙]
---

## 문제 링크
[11279번](https://www.acmicpc.net/problem/11279)

## 해결책
```python
import sys

class PriorityQueue:
    def __init__(self, capacity: int = 9999999) -> None:
        self.arr = [None] * capacity
        self.capacity = capacity
        self.size = 0


    def insert(self, val):
        self.size += 1
        idx = self.size
        self.arr[idx] = val
        while(idx > 1 and self.arr[idx//2] < self.arr[idx]):
            self.arr[idx//2], self.arr[idx] = self.arr[idx], self.arr[idx//2]
            idx //= 2
        return

    def heapify(self, i):
        largest = i
        left = 2 * i
        right = 2 * i + 1

        if(left <= self.size and self.arr[left] > self.arr[i]):
            largest = left
        if(right<=self.size and self.arr[right] > self.arr[largest]):
            largest = right

        if(largest != i):
            self.arr[i], self.arr[largest] = self.arr[largest], self.arr[i]
            self.heapify(largest)
        return

    def deque(self):
        if self.size == 0:
            return 0

        val = self.arr[1]
        self.arr[1] = self.arr[self.size]
        self.size -= 1

        self.heapify(1)
        return val





'''
class PriorityQueue:
    def __init__(self, limit: int = 1000):
        self.start = None
        self.end = None
        self.len = 0

    def queue(self, val):
        node = Node(val)
        if self.len == 0:
            self.start = node
        elif self.len == 1:
            if self.start.val > val:
                self.start.ptr = node
            else:
                node.ptr = self.start
                self.start = node
        else:
            cur = self.start
            if cur.val < val:
                node.ptr = self.start
                self.start = node
            else:
                while cur.ptr is not None:
                    if cur.ptr.val > val:
                        cur = cur.ptr
                node.ptr = cur.ptr
                cur.ptr = node

        self.len += 1

    def dequeue(self):
        if self.len <= 0:
            return 0
        val = self.start.val
        self.start = self.start.ptr
        self.len -= 1
        if self.len <= 0:
            self.start = None
            self.end = None
        return val
'''

N = int(sys.stdin.readline().rstrip())
que = PriorityQueue()
ans = []

for i in range(N):

    code = int(sys.stdin.readline().rstrip())
    if code:
        que.insert(code)
    else:
        ans.append(que.deque())

for i in ans:
    print(i)
```

## 주석으로 달 설명

우선순위 큐.
예전부터 나에게 잘 정리되지 않던 자료구조형이다.
분명 큐인데 트리 형태의 형을 가진다니, 이게 무슨?

일단 작동 방식은 이렇다.

root에 가장 큰 값이 온다 or root에 가장 작은값이 온다(parent가 child보다 크다, child_left와 child_right의 대소는 크게 상관없다), 또한 제일 하단의 child는 좌측부터 채운다.
이 규칙을 지속적으로 확인하는 함수를 제작한다(`heapify()`)
이를 기반으로, `insert()`함수는 새로운 값을 제일 좌측 하단에 넣고(idx = size), 그 다음에 child부터 정렬을 하면 된다.
새로 들어온 노드는 arr\[size\]로 잡는다면, 이를 기반으로 idx = size를 한 다음, `while(size!=1 and arr[i] > arr[i//2])` 라면  arr\[i\]와 arr\[i//2\]를 교체한다.
`deque()`의 경우, 반대로 root를 뽑아버린 다음, 제일 아래에 있는 노드를 root로드에 넣고, root부터 아까 이야기한 `heapify()`함수를 사용한다.
이런 방식으로 우선순위 큐가 작성된다.

이를 처음에 배열로 구현하지 않고, 연결 리스트로 구현했는데, 이 때 시간 복잡도는 N^2과 1 이다.
하지만, heap의 경우에는 NlogN이 평균이라 더 빠르다더라.

추가 공부를 해서 더 기입하도록 하자.
