---
layout: post
title:  "백준 10828번 python 풀이 - 스택"
date:   2024-07-10 15:40:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 스택]
---

## 문제 링크
[10828번](https://www.acmicpc.net/problem/10828)

## 해결책
```python
from typing import Any
import sys

class Stack:

    '''class Empty(Exception):
        pass

    class Full(Exception):
        pass
    '''
    def __init__(self, capacity: int = 99999) -> None:
        self.stk = [None] * capacity
        self.capacity = capacity
        self.ptr = 0

    def __len__(self) -> int:
        return self.ptr

    def is_empty(self) -> bool:
        return self.ptr <= 0

    def is_full(self) -> bool:
        return self.ptr >= self.capacity

    def push(self, value: Any) -> None:
        if self.is_full():
            raise Stack.Full
        self.stk[self.ptr] = value
        self.ptr += 1
        return

    def pop(self) -> Any:
        if self.is_empty():
            return -1
            #raise Stack.Empty
        self.ptr -= 1
        return self.stk[self.ptr]

    # 백준을 위한 동일한 함수 중복
    def size(self) -> int:
        return self.ptr

    def empty(self) -> int:
        if self.ptr <= 0:
            return 1
        else:
            return 0

    def top(self) -> Any:
        if self.is_empty():
            return -1
            #raise Stack.Empty
        return self.stk[self.ptr - 1]



N = int(sys.stdin.readline().rstrip())

stk = Stack()
ans = []

for i in range(N):

    code = sys.stdin.readline().rstrip()
    if " " in code:
        order, num = code.split()
        num = int(num)
    else:
        order = code

    if order == "push":
        stk.push(num)
    elif order == "pop":
        ans.append(stk.pop())
    elif order == "top":
        ans.append(stk.top())
    elif order == "size":
        ans.append(stk.size())
    elif order == "empty":
        ans.append(stk.empty())

for i in ans:
    print(i)
```

## 주석으로 달 설명

STACK!
자료구조형의 시작이다.
스택의 대한 기본적인 설명을 먼저 하고 시작해보겠다.


#### Stack이란?


자료구조에서 스택이란 후입 선출(LIFO: Last In First Out)의 특성을 가지는 자료구조형이다. 조금 더 쉽게 이야기하자면, 스택은 가장 마지막에 삽입된 요소가 호출시에 가장 먼저 반환되는 형태를 가지고 있다.

이는 배열의 형태와 연결 리스트의 형태 두가지로 구현이 가능하다.

배열의 형태에서는 단순히 idx값을 확인하며 삽입하면 가능하고, 연결 리스트에서는 각 노드의 포인터를 연결해주기만 하면 된다.
물리 공간에서의 차이가 조금 있는데, 배열의 형태의 스택 구조에서는 물리적 메모리에서 연속된 공간을 가지게 되겠지만, 연결 리스트의 형태에서는 ADT(Abstract Data Type)로는 연속될 수 있지만, 실제 물리 메모리 구조에서는 서로 불연속적으로 배치되고, 서로가 가지고 있는 포인터를 통해 서로 연결되어 있을 것이다.

---

스택은 이 뿐만 아니라, 메모리에서도 중요한 요소를 가진다.
메모리에는 코드, 데이터, 스택, 힙 네가지 영역이 존재한다.
이 때, 코드와 데이터 영역은 각각 실행할 프로그램의 코드와 전역변수 및 정적변수가 저장되어 있고, 가변적으로 변하는 부분은 스택 영역과 힙 영역이다.

스택 영역에는 함수의 호출과 관계되는 지역 변수와 매개변수가 저장되는 영역이다. 이러한 스택 영역은 함수의 호출과 함께 할당되며, 함수의 호출이 완료되면 소멸한다.
스택 영역에 저장되는 함수의 호출 정보를 스택 프레임(Stack Frame)이라고 한다.

그렇기에, 재귀 호출의 량이 많아지면 이러한 스택 프레임이 점점 증가하여 힙 영역을 침범하게 되고, 힙 영역을 침범함으로써 힙 영역이 오염되면 스택 오버플로우가 발생하여, 오류가 발생하게 된다.

힙 영역은... 굳이 여기서 지금 설명하지는 않고, 아래쪽에 새로운 페이지의 링크를 달아두겠다.

하여튼 이런식으로 메모리에서도 스택이 사용된다는 것이다!

