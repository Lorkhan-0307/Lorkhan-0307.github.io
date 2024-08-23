---
layout: post
title:  "크래프톤 정글 주제별 탐구 - 메모리의 동적 할당"
date:   2024-07-26 21:00:00 +0900
categories: computer_structures
tags: [C, 크래프톤정글용어정리, 메모리, 동적할당]
---

# 정의
가상 메모리에는 코드, 데이터, 힙, 스택 영역이 있었다.
이중, 데이터 영역과 스택 영역은 전에 컴파일 타임에 미리 결정된다고 했다.

하지만, 힙 영역의 경우 프로그램이 실행되는 도중인 런타임(Run time)에 사용자의 결정에 따라 할당된다!
이렇게 런타임에 할당받는 것을 우리는 메모리의 동적 할당(Dynamic Allocation)이라고 한다.


## Malloc 함수

기본적으로 우리가 공간을 할당할 때에는 malloc함수를 사용한다.
malloc 함수는 stdlib 헤더에 있다.
원형은 다음과 같다.

```c
#include <stdlib.h>
void *malloc(size_t size);
```

이와 같이, malloc 함수는 인수로 할당받고자 하는 메모리의 크기를 바이트 단위로 받으며, 이를 할당한 뒤에는 이의 첫번째 바이트를 가리키는 주소값을 반환한다.
힙 영역에 공간이 없으면 Null 포인터를 반환한다.

## Free 함수

공간을 할당하면 해제해야 한다!
Free 함수는 이 힙 영역에 할당받은 공간을 다시 OS에게 되돌려주는 역할을 한다.

이를 해제 안하면, 메모리 누수(memory leak)가 발생한다.

사용법은 아래와 같다.

```c
#include <stdlib.h>
void free(void* ptr);
```

## calloc 함수

calloc 함수는 malloc과 비슷하지만, 잘 쓰지 않는다.

calloc 함수는 할당하고자 하는 메모리의 크기를 두개의 인수로 나누어 받으며, 메모리를 할당받은 후에 해당 메모리의 모든 비트값을 0으로 초기화해준다!

```c
#include <stdlib.h>
void *calloc(size_t nmemb, size_t size);
```
첫번째 인수 nmemb는 메모리 블록의 갯수, 두번째 인수 size는 각 블록의 바이트 수를 나타낸다.
그래서, nmemb의 수만큼의 메모리 블록을 할당할 수 있게 한다.


## realloc 함수

realloc 함수는 이미 할당된 메모리의 크기를 바꾸어 재할당할 때 사용한다.

```c
#include <stdlib.h>
void *realloc(void *ptr, size_t size);
```

첫번째 인수는 크기를 바꾸고자 하는 메모리 공간을 가리키는 포인터를, 두번째 인수는 재할당할 크기를 전달한다.

첫번째 인수가 NULL이면 malloc과 동일한 동작을 한다.
