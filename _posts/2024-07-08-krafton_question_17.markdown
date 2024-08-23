---
layout: post
title:  "백준 10971번 python 풀이 - 외판원 순회2"
date:   2024-07-08 16:43:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 외판원, 완전탐색]
---

## 문제 링크
[10971번](https://www.acmicpc.net/problem/10971)

## 해결책
```python
import sys

N = int(sys.stdin.readline())
ans = 5000001

weight_graph = []
haveToList = []

for i in range(N):
    haveToList.append(i)


def thisIsTheWay(first, depth, weight, start, end, togoList):
    global ans

    if weight_graph[start][end] == 0 and depth != 0:
        return
    depth += 1
    weight += weight_graph[start][end]
    togoList.remove(end)
    if depth == N:
        if weight_graph[end][first] == 0:
            return
        weight += weight_graph[end][first]
        if ans > weight:
            ans = weight
            return

    for i in range(len(togoList)):
        if togoList[i] != end:
            copyTogoList = togoList.copy()
            thisIsTheWay(first, depth, weight, end, togoList[i], copyTogoList)
    return


for i in range(N):
    weight_graph.append(list(map(int, input().split())))

for i in range(N):
    temphavetoList = haveToList.copy()
    thisIsTheWay(i, 0, 0, i, i, temphavetoList)

print(ans)
```

## 주석으로 달 설명

외판원 문제에 도착했다.
일단은 완전 탐색과 비슷한 방식으로 구현했다.

이유는 단순하다. 한쪽 방향이 아닌, 양방향으로써 제작되어 있고, 직관적이기 때문이다.
1년전의 과거의 내가 SCPC였나... 대회를 준비할 때 외판원을 푼 것을 보니, 지금의 코드는 3864ms이지만 당시는 무려 144ms로 문제를 풀었다.
물론 C++로 풀긴 했었지만...

그래서! 당시의 C++ 코드를 python으로 재구성하여 실행시켰더니, 실행시간이 무려 5196ms가 나왔다.
당시보다 조금 나아진 것일까...
잘은 모르겠다.

일단 풀이 자체는 심플하다. 조건에 맞춰서 직관적으로 코드가 구성되어 있다.

각 weight는 2차원 배열의 형태로 weight_graph에 들어가 있다.
또한, 각 미방문 지점을 havetoList로 저장해서 넘겨주고 있다.
물론, 전역적으로 list를 만들어, TF를 이용해서 확인하는 방법도 있다.

그리고, 현재 및 앞으로의 지점을 넘겨주고, depth가 N에 도착하면 해당 지점 -> 처음 지점 으로 이동하는 weight를 합산에 추가한다. 이 때, 해당 weight가 0이면 그냥 return 해버린다.

결과적으로 전역적인 곳에 추가된 값을 출력한다.
