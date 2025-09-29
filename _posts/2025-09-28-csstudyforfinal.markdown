---
layout: post
title: "기술면접 대비 CS 공부 - 00"
date: 2025-09-28 15:00:00 +0900
categories: [Tech Interview, Study Plan]
tags: [c++, c-sharp, computer-science, algorithm, data-structure, operating-system, network, database, design-pattern, unity, unreal]
slug: csstudyforfinal
---

# 면접 대비 사전 QnA 정리

---

## 1. 🖥️ 컴퓨터 구조 Q/A
   
<details>
<summary><strong>1) 폰 노이만 구조와 하버드 구조의 차이는 무엇인가요?</strong></summary>
**A.** 폰 노이만 구조는 명령과 데이터가 같은 버스를 공유해 설계가 단순하지만 병목이 생길 수 있고, 하버드 구조는 명령과 데이터 버스를 분리해 병렬성이 높습니다.
</details>

<details>
<summary><strong>2) CPU의 주요 구성 요소는?</strong></summary>
**A.** ALU는 연산을 담당하고, CU는 제어 신호를 관리하며, 레지스터는 초고속 임시 저장소 역할을 합니다.
</details>

<details>
<summary><strong>3) 파이프라이닝이란?</strong></summary>
**A.** 명령어 실행 단계를 겹쳐 처리량을 높이는 방식으로, CPU 성능을 크게 개선할 수 있습니다.
</details>

<details>
<summary><strong>4) 캐시 메모리의 지역성 원리란?</strong></summary>
**A.** 최근 접근한 데이터를 다시 사용할 확률이 높다는 시간 지역성과, 인접 데이터를 함께 접근할 확률이 높은 공간 지역성이 있습니다.
</details>

<details>
<summary><strong>5) 캐시 일관성 문제 해결 방법은?</strong></summary>
**A.** MESI 같은 캐시 코히어런시 프로토콜을 사용하여 멀티코어 간 캐시 상태를 동기화합니다.
</details>

<details>
<summary><strong>6) 파이프라인 Hazard란?</strong></summary>
**A.** 구조적, 데이터, 제어 Hazard가 있으며, 포워딩이나 스톨 삽입, 분기 예측 등으로 해결합니다.
</details>

<details>
<summary><strong>7) RISC와 CISC의 차이는?</strong></summary>
**A.** RISC는 단순하고 고정 길이 명령어를 사용해 파이프라이닝에 유리하고, CISC는 복잡한 명령어로 코드 밀도를 높일 수 있습니다.
</details>

<details>
<summary><strong>8) 분기 예측이란?</strong></summary>
**A.** 분기 결과를 미리 예측해 파이프라인의 성능 저하를 줄이는 기법이며, 실패 시 파이프라인을 플러시해야 합니다.
</details>

<details>
<summary><strong>9) 멀티코어에서 동기화 문제가 발생하는 이유는?</strong></summary>
**A.** 여러 코어가 동일한 메모리를 동시에 접근할 때 데이터 불일치나 경합이 발생하기 때문입니다.
</details>

<details>
<summary><strong>10) SIMD와 MIMD의 차이는?</strong></summary>
**A.** SIMD는 동일한 명령으로 여러 데이터를 동시에 처리하고, MIMD는 서로 다른 명령으로 데이터를 병렬 처리합니다.
</details>

---

## 🧪 운영체제 Q/A

<details>
<summary><strong>1) 프로세스와 스레드의 차이는?</strong></summary>
**A.** 프로세스는 독립적인 실행 단위이고, 스레드는 프로세스 내에서 실행되는 가벼운 실행 흐름으로 메모리를 공유합니다.
</details>