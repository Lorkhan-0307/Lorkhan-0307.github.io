---
layout: post
title:  "크래프톤 정글 주제별 탐구 -그래프-DFS-BFS-"
date:   2024-07-12 15:30:00 +0900
categories: algorithm
tags: [python, 크래프톤정글주제탐구, 그래프, 그래프순회, 탐색]
---


# 정의
DFS, BFS는 탐색에 주로 사용되는 알고리즘이며, 작동 방식에 따라 구분된다.
둘 다 완전탐색에서 사용할 수 있으며, DFS는 재귀 또는 Stack을, BFS는 재귀 또는 힙(큐)를 이용해서 구현할 수 있다.


## DFS란?
DFS(Depth First Search: 깊이 우선 탐색)는 그래프 순회 방식의 일종으로, 하단의 설명할 BFS와 자주 구별되어 사용된다.

트리나 그래프에서 한쪽으로 가장 깊숙이 들어가서 탐색 후, 확인한 뒤 다시 올라가 다른 루트를 들어가는 방식이다.
이에 따라 가장 깊은 곳을 먼저 찾기 때문에, 이에 따라 깊이 우선 탐색이라 한다.

갈림길이 나타나면, 그곳에 갈림길이 있다는 정보를 남기고 들어간 후, 끝까지 들어가게 되면 직전 갈림길이 있는 위치로 이동하여 방문하지 않은 길으로 들어간다. 방문한 곳들은 전부 방문했다는 확인을 남긴다.

#### 특징
DFS는 가장 깊은곳부터 탐색하기에, 실제로 무엇인가를 탐색할때에는 BFS가 더 빠를 때가 많다. 하지만, 순회(Traversal) 혹은 백트래킹, 완전 탐색을 해야 한다고 하면, DFS가 더 유리한 경우가 많다.

이는 모든 경우의 수를 확인해야 하는 경우, BFS의 큐에 수많은 노드들이 계속 들어오기 때문이다.

#### 주의사항

하지만, DFS의 경우 매우 Depth가 긴 트리이거나, 트리의 Depth가 무한대가 된다면, 혹은 미로찾기에서 Cycle이 만들어지는 경우에 DFS는 이를 벗어나지 못하고 계속 돈다.

이를 막기 위한 중복검사를 넣을 경우, 가능은! 하지만 차라리 BFS를 구현하는 방식이 좋지 않을까?

## 구현

```python
```

## BFS란?
BFS(Breadth First Search: 너비 우선 탐색)은 DFS와 다르게, 같은 depth에 있는 것들을 모두 돈 뒤에 다음 Depth로 들어가는 방식으로, 방문한 노드를 확인하고, 그 child를 전부 queue에 넣는 방식으로 돌아간다.
모든 갈림길을 한번씩 탐색한 이후, 다음 갈림길로 내려간다.

BFS는 그리하여, 모든 경로를 탐색할 수 있기에, 무한한 깊이를 가지고 있거나 cycle이 있는 그래프의 경우에는 이를 해결할 수 있다.

또한, BFS는 모든 길을 탐색하기 때문에, 가중치가 없는 그래프의 경우, 목표지점까지 가는 최소경로를 찾는 방식에 자주 활용된다.

#### 특징

#### 주의사항

## 구현

문제 : baekjoon_1260

```python
import sys


def dfs(start):
    stk = [start]
    while stk:
        cur = stk.pop()
        if is_Visited[cur] is False:
            print(cur, end=" ")
        is_Visited[cur] = True
        for i in range(len(way_list[cur]) - 1, -1, -1):
            if is_Visited[way_list[cur][i]] is False:
                stk.append(way_list[cur][i])


def bfs(start):
    que = [start]
    while que:
        cur = que.pop(0)
        if is_Visited[cur] is False:
            print(cur, end=" ")
        is_Visited[cur] = True
        for v in way_list[cur]:
            if is_Visited[v] is False:
                que.append(v)


N, M, V = map(int, sys.stdin.readline().rstrip().split())
is_Visited = [False for _ in range(N + 1)]
way_list = [[] for _ in range(N + 1)]
for i in range(M):
    _from, _to = map(int, sys.stdin.readline().rstrip().split())
    if _to not in way_list[_from]:
        way_list[_from].append(_to)
    if _from not in way_list[_to]:
        way_list[_to].append(_from)

for i in range(N+1):
    way_list[i].sort()

dfs(V)
is_Visited = [False for _ in range(N + 1)]
print()
bfs(V)
```
