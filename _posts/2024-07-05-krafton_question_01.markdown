---
layout: post
title:  "백준 10869번 python 풀이 - 사칙연산"
date:   2024-07-05 10:50:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 입출력, 사칙연산]
---

## 문제 링크
[10869번](https://www.acmicpc.net/problem/10869)

## 해결책
```
a, b = map(int, input().split())

print(a + b)
print(a - b)
print(a * b)
print(a // b)
print(a % b)
```

## 주석으로 달 설명

#### input
python에서의 input()문은 동일하게 builtin.py 에 존재한다.
```
def input(*args, **kwargs)
```
인자는 2개를 가지고 있으며, *args, **kwargs가 있다.

1. *args는 위치 인수[^1]를 가변적으로 받을 때 사용된다. 하지만 일반적으로 하나의 위치 인수만 사용한다.

2. **kwargs는 키워드 인수를 가변적으로 받을 때 사용된다. 하지만 input 함수는 일반적으로 키워드 인수를 사용하지 않는다.


#### map을 이용한 두 변수를 한번에 받기

```
a, b = map(int, input().split())
```

해당 코드의 동작 순서는 다음과 같다.
1. input()을 통해 문자열을 받는다. (example: '3 7')
2. split()을 통해 문자열을 공백을 기준으로 나누고, 이를 리스트로 반환한다. (example: ['3', '7'])
3. map(int, [])가 각 요소를 정수형으로 변환하여 리스트 내부의 요소들을 정수로 변환하고, iterator를 반환한다.(example: [3, 7])
4. 각 요소가 a, b에 들어간다.

map은 다음과 같은 정의를 가지고 있다.

```
class map(object):
    """
    map(func, *iterables) --> map object

    Make an iterator that computes the function using arguments from
    each of the iterables.  Stops when the shortest iterable is exhausted.
    """
    def __getattribute__(self, *args, **kwargs): # real signature unknown
        """ Return getattr(self, name). """
        pass

    def __init__(self, func, *iterables): # real signature unknown; restored from __doc__
        pass

    def __iter__(self, *args, **kwargs): # real signature unknown
        """ Implement iter(self). """
        pass

    @staticmethod # known case of __new__
    def __new__(*args, **kwargs): # real signature unknown
        """ Create and return a new object.  See help(type) for accurate signature. """
        pass

    def __next__(self, *args, **kwargs): # real signature unknown
        """ Implement next(self). """
        pass

    def __reduce__(self, *args, **kwargs): # real signature unknown
        """ Return state information for pickling. """
        pass
```

맵은 이에 따라, 특정 func를 거쳐, 요소들을 iterator로 반환한다.
코드에서는 int() func를 이용하여, 입력된 각 문자열을 정수형으로 변환하였다.
하지만, lambda 함수와 함께 사용할 수도 있다.

람다식은 추후에 실제로 사용할 때 추가기입하도록 한다.

#### map 객체
map 함수의 경우에는 즉시 계산된 결과를 반환하지 않고, 필요한 경우에만 계산하는 지연 평가(lazy evaluation)를 수행하는 'map'객체를 반환한다.
이 객체는 iterator처럼 동작하고, for 루프 나 list 함수를 이용해서 요소를 추출할 수 있다.

#### 장점
1. 효율성 : 지연 평가를 통해 메모리 효율적으로 동작한다.
2. 가독성 : 코드가 읽기 쉽다.

#### 주의사항
1. 한번만 순회 가능 : 'map'객체는 'iterator'로써 동작하며, 한번 순회하면 재사용할 수 없다.
2. 즉시 결과가 필요한 경우 변환이 필요 : 사용한 코드에서 map 객체를 a, b에 넣은 것처럼, map은 재사용이 안되고 즉시 결과가 필요한 경우에는 이를 list나 Tuple로 변환하여 사용해야 한다.


## 헷갈리는 용어 설명

#### 위치 인수(positional argument)
위치 인수란 함수 호출 시 함수 정의에 따라 순서대로 전달되는 인수를 말한다.
즉, 함수의 매개변수 리스트에서 위치에 따른 인수가 할당된다.
또한, 가변 위치 인수라는 것이 있다.

#### 가변 위치 인수
가변 위치 인수는 함수가 전달받는 위치 인수의 갯수를 미리 정하지 않고, 여러 개의 위치 인수를 받을 수 있는 것을 의미한다.
보통은 *args로 표기된다. 인수들은 튜플의 형태로 args에 저장된다.
```
def print_all(*args):
for arg in args:
    print(arg)

print_all(1, 2, 3, 4)  # 출력: 1 2 3 4
print_all('a', 'b', 'c')  # 출력: a b c
```

#### 키워드 인수
키워드 인수는 인수 이름을 명시하여 전달되는 인수이다. 예시는 다음과 같다.
```
def greet(name, message):
    print(f"{message}, {name}!")

# 1. 위치 인수 사용
greet("Lorkhan", "Hello!")

# 2. 키워드 인수 사용
greet(message="Hello", name="Lorkhan")
```

#### 이터레이터(Iterator)
이터레이터는 Python에서 반복 가능한 객체를 나타내는 개념이다. 이터레이터는 컬렉션(List, Tuple, Dictionary 등)의 요소들을 하나씩 순차접근이 가능하도록 해준다.
이터레이터에는 두가지 주요 메서드가 존재한다. '__iter__()'와 '__next__()'가 그것이다.

#### 이터레이터의 주요 개념

1. 이터레이터는 반복 가능한 객체(iterable)이다.
- 반복 가능한 객체는 '__iter__()' 메서드를 가지고 있어야 한다. 이 메서드는 이터레이터 객체를 반환한다.
- List, Tuple, 문자열 등은 모두 반복 가능한 객체이다.

2. 순차적 접근
- 이터레이터는 '__next__()'메서드를 가지고, 이는 다음 요소를 반환한다. 요소가 더이상 존재하지 않으면 'StopIteration'예외를 발생시킨다.
- 이터레이터는 자체적으로 반복 가능한 객체로, 다시 말해 '__iter__()'메서드를 가지고 있으며, 이는 스스로를 반환한다.

#### 예시

```
# 반복 가능한 객체
my_list = [1, 2, 3, 4]

# 이터레이터 객체 생성
my_iterator = iter(my_list)

# 이터레이터를 사용하여 요소에 접근
print(next(my_iterator))  # 출력: 1
print(next(my_iterator))  # 출력: 2
print(next(my_iterator))  # 출력: 3
print(next(my_iterator))  # 출력: 4
# print(next(my_iterator))  # StopIteration 예외 발생
```

#### 구현

```
class MyIterator:
    def __init__(self, data):
        self.data = data
        self.index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.index < len(self.data):
            result = self.data[self.index]
            self.index += 1
            return result
        else:
            raise StopIteration

# 사용 예
my_iter = MyIterator([1, 2, 3])

for item in my_iter:
    print(item)  # 출력: 1, 2, 3
```

