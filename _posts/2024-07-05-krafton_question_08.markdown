---
layout: post
title:  "백준 1978번 python 풀이 - 소수 찾기"
date:   2024-07-05 15:56:00 +0900
categories: algorithm
tags: [python, 크래프톤정글문제풀이, 브루트포스]
---

## 문제 링크
[1978번](https://www.acmicpc.net/problem/1978)

## 해결책
```python
from math import sqrt

N = int(input())

lst = list(map(int, input().split()))
ans = 0
notPrime = False

for n in lst:
    if n == 1:
        continue
    elif n == 2:
        ans += 1
    elif n % 2 == 0:
        continue

    else:
        notPrime = False
        for i in range(3, int(sqrt(n))+1, 2):
            if n % i == 0:
                notPrime = True
                break
        if not notPrime:
            ans += 1

print(ans)
```

## 주석으로 달 설명

몇가지 고민을 진행하며 문제를 해결했다.
소수를 구하는 공식은 없기 때문이다.
그렇다면, 소수를 구하기 위해서는 Brute Force를 이용해야 할 텐데, 그나마 조금이라도 수를 줄이고 싶었다.

그렇다면 \( N \)개의 수에 대해 \( (N-1) \)번의 계산을 진행해야 하므로, 시간 복잡도는 \( O(N^2) \)가 된다.
이를 조금이나마 줄여보고자, \( 2~sqrt(n) \)까지의 계산으로 전환시켰다.
\( n \)의 약수중에 가장 클 수 있는 수가 \( sqrt(n) \)이기 때문이다.
또한, 2의 배수인 경우를 전부 배제하여, 조금이나마 가볍게 바꿔보고자 했다.

#### 추가적인 생각

소수를 구하는 방법은 과거 에라토스테네스의 체를 통한 방법을 알고 있었고, 지금도 그것의 조금 변화한 방식에 불과하다.
하지만 큰 수의 경우, Miller-Rabin 확률적 소수 판별법도 있다고 한다.
이에 대해 잠시 작성해보자.

#### Miller-Rabin 소수 판별법

Miller-Rabin 소수 판별법은 큰 수에 대해 소수인지 여부를 판별하는 확률적 알고리즘입니다. 이 알고리즘은 수가 소수인지 확률적으로 판별할 수 있으며, 특정 조건 하에서는 결정론적으로 동작할 수 있습니다. 이 알고리즘은 주로 큰 수의 소수성을 판별할 때 사용되며, 특히 암호학에서 많이 사용됩니다.

### Miller-Rabin 소수 판별법의 기본 개념

Miller-Rabin 테스트는 수 \( n \)이 소수인지 판별하기 위해 다음 단계를 거칩니다:

1. **n - 1을 d와 r로 표현**:
   - \( n - 1 = d \times 2^r \) 형태로 나타냅니다. 여기서 \( d \)는 홀수입니다.
   - \( r \)과 \( d \)를 찾기 위해 \( n - 1 \)을 2로 나눌 수 있을 때까지 나누면 됩니다.

2. **임의의 a 선택**:
   - 2부터 \( n - 2 \) 사이의 임의의 숫자 \( a \)를 선택합니다.

3. **테스트 진행**:
   - \( x = a^d \mod n \)를 계산합니다. 만약 \( x = 1 \) 또는 \( x = n - 1 \)이면 통과입니다.
   - 그렇지 않다면, 다음을 \( r - 1 \)번 반복합니다:
     - \( x = x^2 \mod n \)를 계산합니다.
     - 만약 \( x = n - 1 \)이면 통과입니다.
     - 만약 \( x = 1 \)이면 \( n \)은 합성수입니다.
   - 반복이 끝날 때까지 \( x \)가 \( n - 1 \)이 되지 않으면 \( n \)은 합성수입니다.

4. **확률적 판별**:
   - 위 과정을 \( k \)번 반복하여 모두 통과하면, \( n \)이 소수일 확률이 매우 높습니다.
   - 한 번이라도 실패하면 \( n \)은 합성수입니다.

### Miller-Rabin 소수 판별법의 코드 구현

다음은 Miller-Rabin 소수 판별법을 Python으로 구현한 코드입니다:

```python
import random

def miller_rabin(n, k=5):
    """Miller-Rabin 소수 판별법. n이 소수이면 True, 합성수이면 False"""
    if n <= 1:
        return False
    if n == 2:
        return True
    if n % 2 == 0:
        return False

    # n - 1 = d * 2^r 형태로 표현
    r, d = 0, n - 1
    while d % 2 == 0:
        d //= 2
        r += 1

    # Miller-Rabin 테스트를 k번 반복
    for _ in range(k):
        a = random.randint(2, n - 2)
        x = pow(a, d, n)
        if x == 1 or x == n - 1:
            continue

        for _ in range(r - 1):
            x = pow(x, 2, n)
            if x == n - 1:
                break
        else:
            return False

    return True

# 예제 사용
print(miller_rabin(29))  # 출력: True
print(miller_rabin(30))  # 출력: False
```

### 주요 포인트

1. **임의의 a 선택**:
   - 임의의 \( a \)를 2부터 \( n - 2 \) 사이에서 선택합니다. 이는 여러 번 반복하여 테스트합니다.

2. **확률적 테스트**:
   - 알고리즘은 확률적입니다. 즉, 여러 번 테스트하여 신뢰성을 높입니다. 기본적으로 \( k \)번 반복하며, \( k \)를 높일수록 오답 확률이 줄어듭니다.

3. **결정론적 사용**:
   - 특정 작은 수들에 대해서는 결정론적으로 동작할 수 있습니다. 예를 들어, 32비트 이하의 수에 대해서는 몇 가지 특정 \( a \) 값만으로도 충분히 소수를 판별할 수 있습니다.

### Miller-Rabin 테스트의 중요성

Miller-Rabin 테스트는 소수 판별에서 매우 중요한 역할을 합니다. 특히 큰 수의 소수성을 빠르고 효율적으로 판별할 수 있으며, 이는 암호학적 응용에서 매우 중요합니다. 이 알고리즘은 확률적이지만 매우 높은 신뢰성을 제공하며, 필요한 경우 반복 횟수를 늘려 신뢰성을 더욱 높일 수 있습니다.

---
참고하도록 하자!

