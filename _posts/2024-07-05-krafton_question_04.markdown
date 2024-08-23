---
layout: post
title:  "백준 2562번 python 풀이 - 최댓값"
date:   2024-07-05 13:40:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 반복문, 조건문]
---

## 문제 링크
[2562번](https://www.acmicpc.net/problem/2562)

## 해결책
```python
_max = 0
idx = 0

for i in range(9):
    temp = int(input())
    if _max < temp:
        _max = temp
        idx = i+1

print(_max)
print(idx)
```

## 주석으로 달 설명

이건... 뭐... 질문이나 답을 추가할 게 잘 없으므로, 가끔 잊는 range 문을 적어보자.

#### range문

range문은 python에서 숫자 시퀀스를 생성할 수 있는 내장 함수이다.
다만, 찾은 내장 함수는 상당히 길었으므로, 일단 여기에 적지는 않겠다.

range 함수의 기본 문법은 다음과 같다.
```python
range(start, stop, step)
```

1. start는 시퀀스의 시작 값. 시퀸스에 포함되며, 기본은 0이다.
2. stop은 시퀸스의 끝 값. 시퀸스에 포함되지 않는다. 기본값은 없다. 반드시 추가해야한다.
3. step은 시퀸스의 증분 값이며, 기본은 1이다.

#### range 객체의 특징

- iterable: range 객체는 반복 가능한 객체로, list, tuple처럼 사용 가능하다.
- 메모리 효율성: range 객체는 메모리를 효율적으로 사용하며, 전체 시퀸스를 메모리에 저장하지 않고 필요할 때 마다 각 숫자를 생성한다.
- 인덱싱: range객체는 리스트처럼 인덱싱이 가능하다.

```python
a = range(5)
print(a[2]) # 2가 출력된다.

b = range(5)
lst = list(b)
print(lst)  # [0, 1, 2, 3, 4]가 출력된다.
```

#### 주의사항
- step이 0일 경우 'ValueError'가 발생한다.
- 범위 유효성: start>stop 일때는 반드시 step이 음수여야 하고, 그 반대일 때에는 양수여야 한다.
