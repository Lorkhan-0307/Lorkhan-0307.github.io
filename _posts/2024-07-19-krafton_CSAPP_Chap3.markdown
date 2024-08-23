---
layout: post
title:  "크래프톤 정글 CSAPP -3장-"
date:   2024-07-18 17:30:00 +0900
categories: computer_structures
tags: [python, 크래프톤정글주제탐구, 그래프, 위상정렬]
---

# 연습문제

%rax -> 0x100
0x104 -> 0xAB
$0x108 -> 0x108
(%rax) -> 0xFF
4(%rax) -> 0xAB
9(%rax, %rdx) -> 0x11
260(%rcx, %rdx) -> 0x13
0xFC(,%rcx,4) -> 0xFF
(%rax, %rdx,4) -> 0x11

3.2
movl %eax, (%rsp)
movw (%rax), %dx
movb %0xFF, %bl
movb (%rsp, %rdx, 4), %dl
movabsq (%rdx), %rax
movb %dx, (%rax)

3.4
char -> int
1byte를 4btyte로 변환하며, 동시에 char에 부호가 존재하므로 s 확장 방식을 사용하여야 한다.
movsbl (%rdi), %eax
movl %eax, (%rsi)

char -> unsigned
1byte를 4btyte로 변환하며, 동시에 char에 부호가 존재하므로 s 확장 방식을 사용하여야 한다.
movsbl (%rdi), %eax
movl %eax, (%rsi)
놀랍게도... 목적지의 부호는 신경을 안쓴다는 것 같다.

unsigned char -> long
1byte를 8byte로 바꾸는데 부호가 없다.
movzbq (%rdi), %rax -> movzbl (%rdi), %eax
movq %rax, (%rsi)

int -> char
4byte를 1byte로 변환을 해야하는데... 그러면 데이터를 자르고 가야하나?
movl (%rdi), %eax
movb %al, (%rsi)

부호는 대체 어쩌냐?

unsigned -> unsigned char
movl (%rdi), %eax
movb %al, (%rsi)

char -> short
1byte를 2byte로, 부호 있음

movsbw (%rdi), %ax
movw %ax, (%rsi)

3.5
```c
void decode1(long *xp, long *yp, long*zp)
{
    long x = *xp
    long y = *yp
    long z = *zp
    *yp = x
    *zp = y
    *xp = z
}
```

3.6
leaq 9(%rdx), %rax -> 9 + q
leaq(%rdx, %rbx), %rax -> p+q
leaq(%rdx, %rbx, 3), %rax -> 3p + q
leaq 2(%rbx, %rbx, 7), %rax ->8p + 2
leaq 0xE(, %rdx, 3), %rax -> 3q + 14
leaq 6(%rbx, %rdx, 7), %rax -> 7q + p + 6

3.7
x in %rdi, y in %rsi, z in %rdx
leaq (%rsi, %rsi,9) , %rbx -> 10y
leaq (%rbx, %rdx), %rbx -> 10y + z
leaq (%rbx, %rdi, %rsi), %rbx -> 10y + z + x*y

```c
short scale3(short x, short y, short z) {
    short t = (10 + x) * y + z;
    return t;
}
```



3.8

