---
layout: post
title:  "백준 2493번 python 풀이 - 탑"
date:   2024-07-10 15:40:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 스택]
---

## 문제 링크
[2493번](https://www.acmicpc.net/problem/2493)

## 해결책
```python
import sys

N = int(sys.stdin.readline().rstrip())

nums = list(map(int, sys.stdin.readline().rstrip().split()))

tempStack = []

for i in range(N):
    fin = False
    while len(tempStack) > 0:
        if tempStack[-1][0] > nums[i]:
            print(tempStack[-1][1] + 1, end=" ")
            fin = True
            break
        else:
            tempStack.pop()
    tempStack.append([nums[i], i])
    if fin is False:
        print("0", end=" ")
```

## 주석으로 달 설명

STACK 2...

사실 내 힘으로 해결하진 못했다.
내가 생각한 스택의 해결책은 정답은 맞았지만, O(N log N)의 시간 복잡도를 가져, 조건의 시간 부분을 맞추지 못했기 때문이다.

문제의 해결 방안은, 스택에 삽입할 때 문제를 해결하는 것에 있었다.
입력을 받자마자 스택을 확인하고, 스택의 크기가 0이면 바로 삽입하고, 그게 아닌 경우, 스택 peek을 통해 확인 후, 만약 크기가 자신보다 크면 해당의 idx + 1 을 출력하고 스택에 push하며, 아니면 pop 해버린다.
다 pop해버리면 자신을 push한다.

스택을 중간중간 본다 라는 아이디어를 챙겨가자.
