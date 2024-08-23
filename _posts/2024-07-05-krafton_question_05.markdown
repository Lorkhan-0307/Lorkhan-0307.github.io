---
layout: post
title:  "백준 15596번 python 풀이 - 정수 N개의 합"
date:   2024-07-05 13:40:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 함수]
---

## 문제 링크
[15596번](https://www.acmicpc.net/problem/15596)

## 해결책
```python
def solve(a: list):
    _sum = 0
    for val in a:
        _sum += val
    return _sum
```

## 주석으로 달 설명

#### def 를 이용한 python 함수 만들기

함수란, 재사용 가능한 코드 블럭을 말한다.
코드의 가독성, 유지보수 향상을 위해 사용된다.

#### 기본적인 함수 정의

```python
def function_name(parameters):
    ''' Some things to be done '''
    return result
```

parameters -> 인자들
result -> 반환되는 결과값

parameter는 가변 매개변수를 받을 수도 있다.
