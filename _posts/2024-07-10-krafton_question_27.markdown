---
layout: post
title:  "백준 18258번 python 풀이 - 큐2"
date:   2024-07-10 22:03:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 큐]
---

## 문제 링크
[18258번](https://www.acmicpc.net/problem/18258)

## 해결책
```python
import sys


class Node:
    def __init__(self, val: int):
        self.val = val
        self.ptr = None


class Queue:
    def __init__(self, limit: int = 1000):
        self.start = None
        self.end = None
        self.len = 0

    def empty(self):
        if self.len == 0:
            return 1
        else:
            return 0

    def Size(self):
        return self.len

    def Front(self):
        if(self.len <= 0):
            return -1
        return self.start.val

    def Back(self):
        if(self.len <= 0):
            return -1
        return self.end.val

    def queue(self, val):
        node = Node(val)
        if self.len == 0:
            self.start = node
        else:
            self.end.ptr = node
        self.len += 1
        self.end = node

    def dequeue(self):
        if self.len <= 0:
            return -1
        val = self.start.val
        self.start = self.start.ptr
        self.len -= 1
        if self.len <= 0:
            self.start = None
            self.end = None
        return val


N = int(sys.stdin.readline().rstrip())
que = Queue()
ans = []

for i in range(N):

    code = sys.stdin.readline().rstrip()
    if " " in code:
        order, num = code.split()
        num = int(num)
    else:
        order = code

    if order == "push":
        que.queue(num)
    elif order == "pop":
        ans.append(que.dequeue())
    elif order == "front":
        ans.append(que.Front())
    elif order == "back":
        ans.append(que.Back())
    elif order == "size":
        ans.append(que.Size())
    elif order == "empty":
        ans.append(que.empty())

for i in ans:
    print(i)
```

## 주석으로 달 설명

이전에 만든 연결 리스트로 구현한 큐를 기반으로 제작했다.
딱히 그 이상은 없다...
