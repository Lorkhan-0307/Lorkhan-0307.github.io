---
layout: post
title:  "백준 1914번 python 풀이 - 하노이 탑"
date:   2024-07-05 21:30:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 재귀]
---

## 문제 링크
[1914번](https://www.acmicpc.net/problem/1914)

## 해결책
```python
towerNum = int(input())


def nHanoi(n):
    if n == 2:
        return 3
    else:
        return 2 * nHanoi(n - 1) + 1




SMEHanoi = [[[1, 3]], [[1, 2], [1, 3], [2, 3]], [[1, 3], [1, 2], [3, 2], [1, 3], [2, 1], [2, 3], [1, 3]]]

how = []


def hanoi_converter(way, current):
    ans = []
    if way == "SEM":
        for i in SMEHanoi[current]:
            temp_element = []
            for element in i:
                if element == 1:
                    temp_element.append(1)
                elif element == 2:
                    temp_element.append(3)
                else:
                    temp_element.append(2)
            ans.append(temp_element)

    elif way == "MSE":
        for i in SMEHanoi[current]:
            temp_element = []
            for element in i:
                if element == 1:
                    temp_element.append(2)
                elif element == 2:
                    temp_element.append(1)
                else:
                    temp_element.append(3)
            ans.append(temp_element)

    return ans


def wayHanoi(current_tower, top_floor):
    # 현재 층이 목표층이면 종료
    if current_tower == top_floor:
        return SMEHanoi[current_tower - 1]

    ans = hanoi_converter("SEM", current_tower-1) + [[1, 3]] + hanoi_converter("MSE", current_tower-1)
    SMEHanoi.append(ans)

    wayHanoi(current_tower+1, top_floor)


print(nHanoi(towerNum))

if towerNum == 1:
    print("1 3")

elif towerNum <= 20:
    wayHanoi(3, towerNum)
    for i in SMEHanoi[towerNum-1]:
        for ele in i:
            print(ele, end=" ")
        print()
```

## 주석으로 달 설명

알고리즘의 몬헌 안자냐프, 알고리즘 백준 골드티어의 수문장,
알고리즘의 엘든링 멀기트, 입문자의 최고의 벽
바로 그 유명한 '하노이 타워'.

이전에 풀어본 경험이 있었으나, 다시 스스로 접근해보았다.

#### 횟수 세기

사실 횟수는 수식으로도 너무 유명하다.
하지만, 그 이유를 생각해보자.

먼저, n 타워라고 하면, 1~n-1의 원판들을 2번 기둥으로 옮긴다.
이는 1~n-1의 원판들을 3번 기둥으로 옮기는 것과 동일하다.

이후, n원판을 3번 기둥으로 옮긴다.

마지막으로, 2번 기둥에 있는 1~n-1의 원판들을 3번 기둥으로 옮긴다.

그렇기에, 이동하는 횟수는
이전 횟수 + 1 + 이전 횟수
즉, `f(n) = 2f(n-1) + 1` 이라는 수식을 찾을 수 있다.


#### 방법 찾기

이후, 타워의 높이가 1, 2, 3일때를 쭈욱 글로 써 보았다.

---
타워가 1인경우
1 3

---
타워가 2인 경우
1 2
1 3
2 3

---
타워가 3인 경우
1 3
1 2
3 2
1 3
2 1
2 3
1 3

이를 기반으로 중복을 고민하였다.
시작시의 제일 좌측의 제일 하단의 원판(제일 넓은 원판)만 제일 우측으로 옮기고 나면, 나머지 타워는 이전 단계의 움직임과 유사할 것이다.

이를 기반으로, 재귀 + DP의 구조를 생각하여 제작하였다.
세 기둥을 각각 S(Start), M(Middle), E(End)로 생각하고, 1, 2, 3일때의 결과를 먼저 저장했다.

이후의 결과의 경우, f(n)은 SME를 SEM형태의 f(n-1)(1~n-1까지의 원판을 두번째 기둥으로 이동) + 1->3(n 원판을 첫번째에서 세번째 기둥으로 이동) + MSE형태의 f(n-1)(1~n-1까지의 원판을 두번째에서 세번째 기둥으로 이동)의 결과값을 가지게 된다.

이러한 이유로, 이전 단계를 지속적으로 SMEHanoi 3중 리스트(...)에 저장한다. 그러고, 이를 기반으로 다음 단계의 결과를 계속해서 뽑아준다.

이 때, 목표 타워 층에 도착하면 이 반복을 멈춘다.


