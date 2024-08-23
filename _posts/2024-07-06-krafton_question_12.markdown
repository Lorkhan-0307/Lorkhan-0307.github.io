---
layout: post
title:  "백준 9663 python 풀이 - N-Queen"
date:   2024-07-06 16:00:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 재귀]
---

## 문제 링크
[9663번](https://www.acmicpc.net/problem/9663)

## 해결책
```python
N = int(input())

_ans = 0


def solve(row, cols, diag1, diag2):
    global _ans

    # 마지막에 도착했을 시에는, 종료
    if row == N:
        _ans += 1
        return

    # 먼저 추가한 것을 기준으로 배치 불가한 것들을 제거.
    for column in range(N):
        if column not in cols and row - column not in diag1 and row + column not in diag2:
            cols.append(column)
            diag1.append(row - column)
            diag2.append(row + column)
            solve(row + 1, cols, diag1, diag2)
            cols.remove(column)
            diag1.remove(row - column)
            diag2.remove(row + column)


for col in range(N // 2):
    solve(1, [col], [0 - col], [0 + col])
_ans *= 2

if N % 2 == 1:
    solve(1, [N // 2], [0 - (N // 2)], [0 + (N // 2)])

print(_ans)
```

## 주석으로 달 설명

코딩/알고리즘 뉴비 절단기 2호, 쉬지도 않고 나오는 N-Queen 이다.
해결 방법을 생각하긴 쉽지만, 효율적으로 전환하는 과정이 이전과 다르며, 직관적인 방식으로 그대로 쭈욱 코드를 짜다 보면 비효율, O(N^3)에 빠지기 쉽다.

대략 처음부터 다시 시작하는 데에 꽤 오랜 시간을 사용했다.

기본적으로 처음에는 직관대로 작성을 하였다.

```python
N = int(input())

_ans = 0

# NXN의 2차원 배열을 생성

board = []
for i in range(N):
    temp_list = []
    for j in range(N):
        temp_list.append(0)
    board.append(temp_list)


def showboard(temp_board):
    for i in range(N):
        for j in range(N):
            print(temp_board[i][j], end="")
        print()

    print("\n\n\n")


def n_queen(x, y, depth, current_board):
    global _ans

    current_board[x][y] = 2

    dx = [1, 0, -1, 1, 1]
    dy = [0, 1, 1, 1, -1]

    for cross in range(5):
        tempx = x
        tempy = y
        cnt = 0
        while N > (tempx + (cnt * dx[cross])) > -1 and N > (tempy + (cnt * dy[cross])) > -1:
            if cnt == 0:
                current_board[tempx + (cnt * dx[cross])][tempy + (cnt * dy[cross])] = 2
            else:
                current_board[tempx + (cnt * dx[cross])][tempy + (cnt * dy[cross])] = 1
            cnt += 1

    depth += 1
    if depth == N:
        _ans += 1
        return

    for k in range(x + 1, N):

        if (k == N - 1 and depth == N - 1):
            for l in range(N):
                if current_board[k][l] == 0:
                    _ans += 1
            return

        else:
            for l in range(N):
                if current_board[k][l] == 0:
                    temp_board = [row[:] for row in current_board]
                    n_queen(k, l, depth, temp_board)


for i in range(N // 2):
    c_board = [row[:] for row in board]
    n_queen(0, i, 0, c_board)

_ans *= 2

if N % 2 == 1:
    c_board = [row[:] for row in board]
    n_queen(0, N // 2, 0, c_board)

print(_ans)
```

2차원 배열을 이용해서 해당 체스보드를 구현하고, 실제로 Queen이 갈 수 있는 위치들을 제거하고 그 외의 위치들을 방문하며 확인하고, 아닌 경우 위로 올라오는 방식으로 구현하였다.
만약 이 방식에서 방향성을 추가로 구현하여 예외처리를 진행하였다면, DFS방식을 통한 구현이 가능했을 거라 생각한다.

하지만 조금 다른 재귀적 방식을 만들고자 하였고, 해당 코드가 정상적으로(하지만 백준에서 Timeout이 발생할 정도의 긴 시간 요소) 답을 출력하지만, 동작 시간을 감소시키고자 하였다.

이를 위해서, 먼저 2차원배열을 1차원배열로 전환하고, 배열의 각 value를 column의 값으로 전환하였다.

또한, 이는 기존 코드에서도 진행하였지만, 처음 넣는 퀸의 위치를 1~N//2-1 까지만 진행하고, 이를 좌우 대칭을 진행한다 가정하여 result * 2 를 진행함으로써 값을 구하고, 홀수인 경우에는 이에 N // 2 일때의 상황을 추가적으로 가정하여 계산을 진행하였다.

이를 통해 문제를 해결할 수 있었다.
