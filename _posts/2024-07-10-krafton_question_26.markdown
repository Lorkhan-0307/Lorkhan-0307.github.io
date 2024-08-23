---
layout: post
title:  "백준 11866번 python 풀이 - 요세푸스 문제0"
date:   2024-07-10 21:33:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 큐]
---

## 문제 링크
[11866번](https://www.acmicpc.net/problem/11866)

## 해결책
```python
import sys


class Node:
    def __init__(self, val: int):
        self.val = val
        self.ptr = None


class Circle_queue:
    def __init__(self,  limit: int = 1000):
        self.que = [None] * limit
        self.start = None
        self.end = None
        self.len = 0

    def queue(self, val):
        node = Node(val)
        if self.len == 0:
            self.start = node
        else:
            self.end.ptr = node
            self.end.ptr = node
        self.len += 1
        self.end = node
        node.ptr = self.start

    def cut(self, jump):
        cur_node = self.start
        if (jump == 1):
            self.end.ptr = cur_node.ptr
            self.start = cur_node.ptr
            return cur_node.val
        for i in range(jump - 2):
            cur_node = cur_node.ptr

        temp = cur_node.ptr.val

        self.start = cur_node.ptr.ptr

        cur_node.ptr = cur_node.ptr.ptr

        return temp



N, K = map(int, sys.stdin.readline().rstrip().split())
my_queue = Circle_queue()
for i in range(1,N+1):
    my_queue.queue(i)

print("<", end="")
for i in range(N-1):
    print(my_queue.cut(K), end=", ")
print(my_queue.cut(K), end=">")


```

## 주석으로 달 설명

큐 문제 라고는 써있지만...
큐 + 연결 리스트로 풀었다.
큐보다 오히려 연결 리스트의 포인터로 해결한 기분?
그냥 조건대로 직관대로 연결 리스트로 구현했다.
다만, 그 위의 큐를 두어서, 연결 리스트를 관리하는 형태로 구현했는데... 이게 좋은 방법인지는 조금 고려할 필요가 있는것 같다.
