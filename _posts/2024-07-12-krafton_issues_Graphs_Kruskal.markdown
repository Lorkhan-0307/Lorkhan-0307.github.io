---
layout: post
title:  "크래프톤 정글 주제별 탐구 -크루스칼 알고리즘-"
date:   2024-07-12 13:30:00 +0900
categories: algorithm
tags: [python, 크래프톤정글주제탐구, 트리, 신장 트리, 최소 신장 트리, MST]
---


# 정의
크루스칼 알고리즘이란, MST를 구하기 위해 사용되는 알고리즘이다.

# 동작 방식
간단히 크루스칼 알고리즘의 동작 순서에 대해 작성해보자.

1. 주어진 모든 간선 정보에 대해 간선 비용이 낮은 순서(오름차순)로 정렬을 수행
2. **정렬된 간선 정보를 하나씩 확인** 하면서 현재의 간선이 노드들 간의 **사이클을 발생시키는지 확인**
3. 만약 사이클이 발생하지 않은 경우, 최소 신장 트리에 포함시키고 사이클이 발생한 경우, 최소 신장 트리에 포함시키지 않음
4. 1번~3번 과정을 모든 간선 정보에 대해 반복 수행

---

이때 2번의 과정에서 **사이클 발생 여부** 의 코드를 짜기가 어려운데, 이는 **노드들의 부모 노드가 같다면** 사이클이 발생했다고 보고, 그렇지 않다면 사이클이 발생하지 않았다고 보는 방식을 통해 확인한다.

이 부모 노드가 같다 라는 부분은 Union 이라는 과정을 통해 구할 수 있는데, 크루스칼 알고리즘에서 이 과정은 다음과 같다.

1. 먼저, A와 B의 parent값을 비교하고, 둘이 다를 때 진행한다.
2. A의 parent, parent의 parent, ... 들 중 x == parent(x) 인 값을 찾는다.
3. B 역시 마찬가지로 y == parent(y)를 찾는다.
4. 이 x, y를 비교하여, 둘 중 작은 값으로 둘 다 통일시킨다.
5. 이를 통해 부모 값을 찾는다!

또한, 이 때 선택되는 간선의 총 수는 노드 수 -1(V-1)이 될 것이다.

# 구현

문제 1197번

```python
import sys


def get_Parent(val):
    if parent_list[val] == val:
        return val
    parent_list[val] = get_Parent(parent_list[val])
    return parent_list[val]


def union_parent(a, b):
    a = get_Parent(a)
    b = get_Parent(b)
    if a < b:
        parent_list[b] = a
    else:
        parent_list[a] = b


V, E = map(int, sys.stdin.readline().rstrip().split())

weight_list = []
parent_list = []

for i in range(V):
    parent_list.append(i)

for i in range(E):
    _from, _to, _weight = map(int, sys.stdin.readline().rstrip().split())
    _from -= 1
    _to -= 1
    weight_list.append([_weight, _from, _to])
weight_list.sort()

_sum = 0
for weight in weight_list:
    if get_Parent(weight[1]) != get_Parent(weight[2]):
        union_parent(weight[1], weight[2])
        _sum += weight[0]

print(_sum)
```

