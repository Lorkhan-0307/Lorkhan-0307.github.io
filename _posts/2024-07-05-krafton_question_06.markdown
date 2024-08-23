---
layout: post
title:  "백준 11654번 python 풀이 - 아스키 코드"
date:   2024-07-05 14:55:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 아스키코드]
---

## 문제 링크
[11654번](https://www.acmicpc.net/problem/11654)

## 해결책
```python
original_input = input()

print(ord(original_input))
```

## 주석으로 달 설명

#### ASKII 코드


#### ord, chr 함수

`ord()` 함수는 문자를 ASKII코드로 전환한다.
`chr()` 함수는 ASKII코드를 문자로 전환한다.
둘 다 (*args, **kwargs)를 인자로 받는다.
