---
layout: post
title:  "백준 9020번 python 풀이 - 골드바흐의 추측"
date:   2024-07-05 15:56:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 브루트포스]
---

## 문제 링크
[9020번](https://www.acmicpc.net/problem/9020)

## 해결책
```python
from math import sqrt

def isprime(n):
    if n == 1:
        return False
    elif n == 2:
        return True
    elif n % 2 == 0:
        return False
    else:
        for j in range(3, int(sqrt(n)) + 1, 2):
            if n % j == 0:
                return False
        return True

T = int(input())

for t in range(T):
    N = int(input())

    for i in range(int(N / 2), N):
        if isprime(i):
            if isprime(N - i):
                print(N - i, i)
                break

```

## 주석으로 달 설명

이번 결과는 예상과 달랐다.

먼저, 편한 방식으로 진행했다.
이전 문제인 9번에서 소수인지를 확인하는 함수를 가져왔고, 이를 기반으로
N/2 부터 N-1 중 소수인 것을 N에서 뺀 숫자 역시 소수인지 확인하는 방법으로 진행하였고, 시간은 조금 걸렸지만 해결되었다.

그 다음에 생각한 코드는 다음과 같다.

```python
from math import sqrt


def isprime(n):
    if n == 1:
        return False
    elif n == 2:
        return True
    elif n % 2 == 0:
        return False
    else:
        for j in range(3, int(sqrt(n)) + 1, 2):
            if n % j == 0:
                return False
        return True


T = int(input())

for t in range(T):
    N = int(input())
    primeList = []

    for i in range(2, N - 1):
        if isprime(i):
            primeList.append(i)

    for i in range(int(len(primeList) / 2), len(primeList)):
        if (N - primeList[i]) in primeList:
            print(N - primeList[i], primeList[i])
            break
```
이는 소수 리스트를 1~N까지 먼저 만들고, 이를 기반으로 확인하는 방법이다.
이는 재밌게도, Timeout Error가 등장했다.

하지만 두번째 풀이에서 len(primeList)의 중간값이 반드시 N의 중간 즈음이 아니기 때문에 생각이 잘못되었던 것 같다.
추후 수정을 추가해 해결해보겠다.
