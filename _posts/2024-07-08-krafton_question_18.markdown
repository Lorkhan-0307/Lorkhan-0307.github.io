---
layout: post
title:  "백준 2468번 python 풀이 - 안전 영역"
date:   2024-07-08 16:43:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 완전탐색]
---

## 문제 링크
[2468번](https://www.acmicpc.net/problem/2468)

## 해결책
```python
import sys

N = int(sys.stdin.readline())

grid = []

isVisited = []
_max = 0
_min = 999

for i in range(N):
    tempList = list(map(int, input().split()))
    grid.append(tempList)
    _tempList = tempList.copy()
    if _tempList[N - 1] > _max: _max = _tempList[N - 1]
    if _tempList[0] < _min : _min = _tempList[0]


dx = [1, -1, 0, 0]
dy = [0, 0, 1, -1]

ans = 0

recurcheck = 0

def dfs(x, y, h):
    global recurcheck
    recurcheck += 1

    for way in range(4):
        if 0 <= x + dx[way] <= N - 1 and 0 <= y + dy[way] <= N - 1:
            if IsVisited[x + dx[way]][y + dy[way]] is False and grid[x + dx[way]][y + dy[way]] > h:
                IsVisited[x + dx[way]][y + dy[way]] = True
                dfs(x + dx[way], y + dy[way], h)
            else:
                IsVisited[x + dx[way]][y + dy[way]] = True
                continue
        else:
            continue
    return


def dfs_stack(x, y, h):
    stack = [(x, y)]
    while stack:
        x, y = stack.pop()

        for way in range(4):
            nx, ny = x + dx[way], y + dy[way]
            if 0 <= nx < N and 0 <= ny < N:
                if not IsVisited[nx][ny] and grid[nx][ny] > h:
                    IsVisited[nx][ny] = True
                    stack.append((nx, ny))


ansH = 0

for h in range(0, _max+1):
    currentMax = 0
    IsVisited = [[False] * N for i in range(N)]

    for i in range(N):
        for j in range(N):
            if not IsVisited[i][j] and grid[i][j] > h:
                IsVisited[i][j] = True
                currentMax += 1
                dfs_stack(i, j, h)

    if currentMax > ans:
        ans = currentMax
        ansH = h





print(ans)

```

## 주석으로 달 설명

완전탐색. 그 두번째이다.

이번에는 기존의 완전탐색을 할 때 재귀적으로 처음에 진행하였지만, 곧이어 최대 재귀 호출 도착 에러에 봉착해버렸다.

백준의 경우, python 재귀 최대 가능 depth 는 1,000이고, 현재 조건은 N이 100이 될 수 있으므로, 나의 기존 재귀적인 DFS 방식의 코드는 최악의 경우 10,000번의 재귀호출에 도착할 수 있다.

곧이어, 이에 따라 재귀적으로 구현한 코드를 stack을 이용한 DFS로 교체하였다.
물론, 완전탐색이기 때문에, BFS로 교체하는 것 역시 아무 상관 없다.

마지막으로, 비의 깊이가 어느정도인지의 대한 범위가 없다.
즉, 비가 0만큼 올 수 있다는 점이 추가적으로 수정한 사항이다.
