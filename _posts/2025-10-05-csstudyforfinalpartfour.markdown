---
layout: post
title: "기술면접 대비 CS 공부 - 03"
date: 2025-10-05 16:00:00 +0900
categories: [Tech Interview, Study Plan]
tags: [c++, c-sharp, computer-science, algorithm, data-structure, operating-system, network, database, design-pattern, unity, unreal]
slug: csstudyforfinalpartfour
---

# 면접 대비 사전 QnA 정리 - OS


# 🔷 OS 면접 예상 질문 50선 – 모범답변

---

<details markdown="1">
<summary><strong>1. 운영체제란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>

운영체제(Operating System, OS)는 사용자와 하드웨어 사이의 중재자로서,
시스템 자원을 효율적으로 관리하고 프로그램이 실행될 수 있도록 환경을 제공하는 시스템 소프트웨어입니다.

---

<strong>🔹 특징 및 상세설명</strong>

- **주요 역할**
  1. **프로세스 관리:** 프로그램 실행, 스케줄링, 동기화, 교착상태 관리
  2. **메모리 관리:** 가상 메모리, 페이징, 세그멘테이션
  3. **파일 시스템 관리:** 파일 생성, 삭제, 접근 제어
  4. **입출력(I/O) 관리:** 디바이스 제어, 버퍼링, 인터럽트 처리

- **핵심 구성요소**
  - **커널(Kernel):** 하드웨어 자원 제어 및 시스템 핵심 기능 담당
  - **시스템 콜(System Call):** 사용자 프로그램이 커널 기능을 요청하는 인터페이스
  - **쉘(Shell):** 사용자와 운영체제 간의 명령 인터페이스 (CLI 또는 GUI)

---

<strong>💬 면접식 답변</strong>

운영체제는 하드웨어와 사용자 사이의 다리 역할을 하는 시스템 소프트웨어입니다.
CPU, 메모리, 파일 시스템 등의 자원을 효율적으로 관리하면서,
프로그램이 안정적으로 실행될 수 있도록 지원합니다.
즉, 컴퓨터의 자원을 '관리'하고, 사용자가 프로그램을 '실행'할 수 있게 하는 기반 시스템입니다.

</details>

---

<details markdown="1">
<summary><strong>2. 커널(Kernel)이란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>

커널은 운영체제의 핵심으로, CPU, 메모리, 입출력 장치 등의 하드웨어 자원을 직접 제어하고 관리하는 소프트웨어 계층입니다.

---

<strong>🔹 특징 및 상세설명</strong>

- **커널의 역할**
  - **프로세스 관리:** 생성, 스케줄링, 종료
  - **메모리 관리:** 페이지 테이블, 주소 변환, 가상 메모리
  - **파일 시스템 관리:** 파일 접근 및 권한 관리
  - **I/O 제어:** 디바이스 드라이버를 통해 하드웨어를 제어

- **커널의 유형**
  - **모놀리식 커널:** 모든 기능이 하나의 큰 커널에 통합 (예: Linux)
  - **마이크로커널:** 최소 기능만 커널에 두고 나머지는 사용자 모드에서 실행 (예: macOS, QNX)

- **실행 권한**
  - Ring 0 (가장 높은 권한 레벨)에서 실행됨

---

<strong>💬 면접식 답변</strong>

커널은 운영체제의 핵심 부분으로, CPU, 메모리, 디스크와 같은 하드웨어를 직접 관리하는 역할을 합니다.
프로세스와 메모리를 스케줄링하고, I/O 요청을 처리하며, 시스템 자원을 효율적으로 분배하죠.
즉, 커널은 운영체제의 '심장'이라고 할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>3. 커널 스케줄링(Kernel Scheduling)이란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>

커널 스케줄링은 CPU를 여러 프로세스나 스레드에 효율적으로 분배하기 위한 정책과 메커니즘을 의미합니다.

---

<strong>🔹 특징 및 상세설명</strong>

- **목적**
  - CPU 효율 극대화
  - 응답시간 최소화
  - 공정한 자원 분배

- **스케줄링 단위**
  - **프로세스 단위** (커널 스레드 포함)
  - **스레드 단위** (멀티스레드 환경)

- **대표적인 스케줄링 방식**
  - Round Robin, Priority Scheduling, MLFQ

- **커널 스케줄러 종류**
  - Windows: Dispatcher
  - Linux: Completely Fair Scheduler (CFS)

---

<strong>💬 면접식 답변</strong>

커널 스케줄링은 CPU를 어떤 프로세스나 스레드에 언제, 얼마나 할당할지를 결정하는 메커니즘입니다.
운영체제는 스케줄러를 통해 프로세스를 관리하며, 공정성과 효율성을 동시에 달성하려 합니다.
예를 들어, Linux는 CFS 스케줄러를 통해 CPU 사용량을 균등하게 분배합니다.

</details>

---

<details markdown="1">
<summary><strong>4. 커널은 네트워크 계층 중 어디를 담당하나요?</strong></summary>

<strong>🧠 핵심 요약</strong>

커널은 OSI 7계층 중 **하위 4계층(1~4계층)**, 즉 물리계층부터 전송계층까지를 주로 담당합니다.

---

<strong>🔹 특징 및 상세설명</strong>

- **물리계층 (1계층)**
  - 디바이스 드라이버를 통해 하드웨어 제어

- **데이터링크계층 (2계층)**
  - 네트워크 인터페이스 카드(NIC) 제어

- **네트워크계층 (3계층)**
  - IP 라우팅, 패킷 처리 (커널의 네트워크 스택)

- **전송계층 (4계층)**
  - TCP/UDP 소켓 통신 처리

- **응용계층 (5~7계층)**
  - 사용자 모드에서 실행되는 애플리케이션이 담당 (예: HTTP, FTP 등)

---

<strong>💬 면접식 답변</strong>

커널은 네트워크 계층 중 하위 4계층, 즉 물리 ~ 전송 계층을 담당합니다.
하드웨어 드라이버를 제어하고, IP 라우팅과 TCP/UDP 소켓 통신을 처리하죠.
반면, HTTP 같은 상위 계층은 응용 프로그램에서 구현됩니다.

</details>

---

<details markdown="1">
<summary><strong>5. CPU 스케줄링 알고리즘에 대해 설명해주세요.</strong></summary>

<strong>🧠 핵심 요약</strong>

CPU 스케줄링 알고리즘은 여러 프로세스가 CPU를 효율적으로 공유하도록 우선순위와 실행 순서를 결정하는 정책입니다.

---

<strong>🔹 특징 및 상세설명</strong>

- **비선점형(Non-Preemptive)**
  - FCFS (First Come First Served)
  - SJF (Shortest Job First)

- **선점형(Preemptive)**
  - RR (Round Robin)
  - SRTF (Shortest Remaining Time First)
  - Priority Scheduling
  - MLFQ (Multi-Level Feedback Queue)

- **평가 기준**
  - 평균 대기 시간, 응답 시간, CPU 활용률, 처리율 등

---

<strong>💬 면접식 답변</strong>

CPU 스케줄링은 여러 프로세스가 CPU를 효율적으로 사용할 수 있도록 실행 순서를 결정하는 과정입니다.
예를 들어, 짧은 작업을 우선 실행하는 SJF나, 순환 시간을 나누는 Round Robin 방식이 대표적입니다.
최근 OS에서는 MLFQ처럼 동적 우선순위를 사용하는 방식이 주로 활용됩니다.

</details>

---

<details markdown="1">
<summary><strong>6. 우선순위 기반 Preemptive Scheduling이란?</strong></summary>

<strong>🧠 핵심 요약</strong>

우선순위 기반 선점 스케줄링은 **우선순위가 높은 프로세스가 낮은 프로세스보다 먼저 CPU를 차지하는 방식**입니다.

---

<strong>🔹 특징 및 상세설명</strong>

- **기본 원리**
  - 새로 도착한 프로세스가 기존 실행 중인 프로세스보다 우선순위가 높다면, CPU를 즉시 선점(Preempt)합니다

- **장점**
  - 빠른 응답성과 높은 중요도 프로세스 처리 가능

- **단점**
  - 낮은 우선순위 프로세스가 무한정 대기하는 **Starvation(기아 현상)** 발생 가능

- **해결법**
  - Aging (대기 시간이 길수록 우선순위를 점진적으로 상승시킴)

---

<strong>💬 면접식 답변</strong>

우선순위 기반 선점 스케줄링은 높은 우선순위의 프로세스가 CPU를 먼저 사용하는 방식입니다.
중요 작업을 빠르게 처리할 수 있지만, 낮은 우선순위 작업이 영원히 실행되지 않는 기아 현상이 발생할 수 있습니다.
이를 방지하기 위해 Aging 기법을 적용해 우선순위를 점진적으로 조정합니다.

</details>

---

<details markdown="1">
<summary><strong>7. MLFQ(Multi-Level Feedback Queue)란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>

MLFQ는 여러 개의 우선순위 큐를 두고, 프로세스의 행동에 따라 동적으로 우선순위를 조정하는 스케줄링 알고리즘입니다.

---

<strong>🔹 특징 및 상세설명</strong>

- **다단계 큐 구조**
  - 각 큐는 서로 다른 우선순위와 타임슬라이스(Time Quantum)를 가짐

- **피드백 메커니즘**
  - CPU를 오래 사용하면 하위 큐로 이동, 입출력 위주면 상위 큐로 승격

- **장점**
  - CPU-bound, I/O-bound 프로세스를 모두 공정하게 처리

- **단점**
  - 구현이 복잡하며, 매개변수 조정이 중요함

---

<strong>💬 면접식 답변</strong>

MLFQ는 여러 개의 큐를 두고 프로세스의 실행 패턴에 따라 우선순위를 동적으로 변경하는 방식입니다.
CPU를 오래 사용하는 작업은 하위 큐로, 입출력 중심의 작업은 상위 큐로 이동합니다.
즉, 시스템의 효율성과 응답성을 모두 확보할 수 있는 스케줄링 방식입니다.

</details>

---

<details markdown="1">
<summary><strong>8. User Mode란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>

User Mode는 애플리케이션이 실행되는 권한이 제한된 CPU 모드로, 직접 하드웨어에 접근할 수 없습니다.

---

<strong>🔹 특징 및 상세설명</strong>

- **접근 제한**
  - 시스템 자원(CPU, 메모리 등)에 직접 접근 불가

- **커널 호출**
  - 시스템 콜을 통해 커널 모드에서만 자원 접근 가능

- **예시**
  - 브라우저, 게임 클라이언트, IDE 등은 User Mode에서 실행됨

- **보호 메커니즘**
  - 잘못된 접근으로부터 시스템 안정성을 보장

---

<strong>💬 면접식 답변</strong>

User Mode는 일반 애플리케이션이 실행되는 제한된 권한 모드입니다.
프로그램이 직접 하드웨어를 제어하지 못하고, 필요한 경우 커널 모드로 전환하여 자원을 요청합니다.
이 방식 덕분에 시스템 안정성과 보안이 유지됩니다.

</details>

---

<details markdown="1">
<summary><strong>9. 32bit와 64bit의 차이는 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>

32bit와 64bit는 CPU가 한 번에 처리할 수 있는 데이터 크기(레지스터 폭)와 주소 공간의 크기를 의미합니다.

---

<strong>🔹 특징 및 상세설명</strong>

- **데이터 버스/레지스터 폭 차이**
  - 32bit CPU: 4바이트(=32비트) 단위 연산
  - 64bit CPU: 8바이트(=64비트) 단위 연산

- **주소 공간 차이**
  - 32bit: 최대 4GB (2³²)
  - 64bit: 이론상 16EB (2⁶⁴)

- **특징**
  - 64bit는 더 큰 메모리 공간 접근 가능
  - 64bit OS에서는 32bit 프로그램을 호환 모드에서 실행 가능

---

<strong>💬 면접식 답변</strong>

32bit와 64bit의 차이는 CPU가 한 번에 처리할 수 있는 데이터의 폭과 메모리 주소 공간의 차이입니다.
32bit 시스템은 최대 4GB 메모리만 인식하지만, 64bit는 훨씬 큰 주소 공간을 다룰 수 있습니다.
최근 대부분의 OS와 애플리케이션은 64bit 환경을 기본으로 지원합니다.

</details>

---

<details markdown="1">
<summary><strong>10. 주소 버스(Address Bus)란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>

주소 버스는 CPU가 접근하려는 메모리나 I/O 장치의 주소를 전달하는 통로입니다.

---

<strong>🔹 특징 및 상세설명</strong>

- **역할**
  - CPU → 메모리/디바이스로 주소 신호를 전달

- **폭(bit 수)**
  - CPU의 주소 버스 폭이 접근 가능한 메모리 크기를 결정
  - 예: 32bit 주소 버스 → 4GB 메모리 접근 가능

- **데이터 버스(Data Bus)**
  - 실제 데이터를 전달

- **제어 버스(Control Bus)**
  - 읽기/쓰기 제어 신호를 전달

---

<strong>💬 면접식 답변</strong>

주소 버스는 CPU가 메모리나 I/O 장치에 접근하기 위해 주소를 전달하는 통로입니다.
예를 들어 32비트 CPU는 32개의 주소선을 통해 2³², 즉 4GB의 메모리 공간에 접근할 수 있습니다.
이 버스 구조는 CPU, 메모리, 주변장치 간 통신의 핵심 요소입니다.

</details>

---

<details markdown="1">
<summary><strong>11. System Call이란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>

System Call은 사용자 프로그램이 운영체제 커널의 기능(CPU, 메모리, 파일 등)에 접근하기 위해 호출하는 인터페이스입니다.

---

<strong>🔹 특징 및 상세설명</strong>

- **역할**
  - 사용자 모드 프로그램이 직접 하드웨어를 제어할 수 없기 때문에, 커널 기능을 요청할 때 System Call을 통해 간접적으로 접근합니다

- **예시**
  - 파일: `open()`, `read()`, `write()`
  - 프로세스: `fork()`, `exec()`, `exit()`
  - 메모리: `mmap()`, `brk()`
  - 네트워크: `socket()`, `connect()`

- **동작 과정**
  1. 애플리케이션이 System Call 호출
  2. CPU가 **Trap 명령어**를 통해 커널 모드로 전환
  3. 커널이 요청된 작업 수행 후 결과 반환
  4. 사용자 모드로 복귀

---

<strong>💬 면접식 답변</strong>

System Call은 사용자 프로그램이 커널 기능을 요청하는 인터페이스입니다.
예를 들어, 파일을 열거나 프로세스를 생성할 때 직접 하드웨어에 접근하지 않고 System Call을 통해 커널이 대신 수행합니다.
즉, 커널과 사용자 프로그램 간의 통신 창구 역할을 하는 중요한 메커니즘입니다.

</details>

---

<details markdown="1">
<summary><strong>12. Interrupt(인터럽트)란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
인터럽트는 CPU가 실행 중인 작업을 잠시 중단하고,  
우선적으로 처리해야 하는 외부 또는 내부 이벤트를 처리하도록 알려주는 신호입니다.

---

<strong>🔹 특징 및 상세설명</strong>  
- **종류:**
  - **하드웨어 인터럽트:** 키보드 입력, I/O 완료 등 외부 장치 신호  
  - **소프트웨어 인터럽트:** 예외(Exception), System Call 등 내부 이벤트  
- **동작 과정:**  
  1. 인터럽트 발생  
  2. CPU가 현재 명령어 완료 후 상태를 저장  
  3. 인터럽트 벡터 테이블(IVT)에서 해당 ISR 주소 탐색  
  4. ISR(Interrupt Service Routine) 실행  
  5. `IRET` 명령으로 원래 실행하던 코드 복귀  

---

<strong>💬 면접식 답변</strong>  
인터럽트는 CPU가 현재 작업을 잠시 멈추고,  
입출력 완료나 예외 상황 같은 더 중요한 이벤트를 우선 처리하도록 하는 신호입니다.  
예를 들어 키보드 입력이 들어오면 CPU가 즉시 ISR을 실행하고, 처리가 끝나면 원래 작업으로 돌아옵니다.

</details>

---

<details markdown="1">
<summary><strong>13. Interrupt Vector Table(IVT)이란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
IVT는 각 인터럽트 번호에 대응하는 ISR(Interrupt Service Routine)의 주소를 저장하는 테이블입니다.

---

<strong>🔹 특징 및 상세설명</strong>  
- **역할:**  
  인터럽트가 발생했을 때 CPU가 어떤 함수를 실행해야 할지를 결정하는 매핑 테이블 역할  
- **구조:**  
  각 엔트리(Entry)는 인터럽트 번호(Index)와 ISR 주소를 매핑  
- **예시:**  
  - 0번: Divide by Zero  
  - 14번: Page Fault  
  - 32번 이상: 하드웨어 인터럽트  

---

<strong>💬 면접식 답변</strong>  
인터럽트 벡터 테이블은 각 인터럽트 요청 번호에 대응하는 ISR 주소를 저장한 테이블입니다.  
CPU는 인터럽트가 발생하면 IVT에서 해당 ISR의 주소를 찾아 즉시 실행하게 됩니다.  
즉, 인터럽트 처리의 ‘주소록’ 역할을 하는 데이터 구조입니다.

</details>

---

<details markdown="1">
<summary><strong>14. ISR(Interrupt Service Routine)이란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
ISR은 인터럽트가 발생했을 때 실행되는 함수(핸들러)로,  
해당 이벤트를 처리하고 시스템을 정상 상태로 복귀시키는 역할을 합니다.

---

<strong>🔹 특징 및 상세설명</strong>  
- **역할:**  
  - 인터럽트 원인 분석 및 관련 작업 처리  
  - 예: 키보드 입력 처리, 네트워크 패킷 수신, 타이머 인터럽트  
- **실행 과정:**  
  1. 인터럽트 발생 → CPU 상태 저장  
  2. ISR 진입 → 해당 작업 수행  
  3. 완료 후 `IRET` 명령으로 복귀  
- **제약:**  
  - 실행 중 다른 인터럽트 발생 시 마스킹 필요  
  - 커널 모드에서 동작 (고권한 코드)

---

<strong>💬 면접식 답변</strong>  
ISR은 인터럽트가 발생했을 때 실행되는 처리 루틴입니다.  
예를 들어 키보드 입력 시 키 값을 버퍼에 저장하는 코드가 ISR로 작동합니다.  
CPU는 ISR 수행 후 원래 실행 중이던 프로세스로 복귀합니다.

</details>

---

<details markdown="1">
<summary><strong>15. IRET 명령어란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
IRET(Interrupt Return)은 인터럽트 처리 후,  
저장해두었던 이전 CPU 상태(레지스터, 플래그, 명령어 포인터)를 복원하고 원래 코드로 돌아가는 명령어입니다.

---

<strong>🔹 특징 및 상세설명</strong>  
- **작동 원리:**  
  - ISR 진입 시 CPU는 현재 실행 상태(PC, 플래그, 스택 등)를 스택에 저장  
  - IRET은 이 상태를 복원하여 원래 실행 흐름으로 복귀  
- **역할:**  
  인터럽트 처리 후 시스템을 안정적으로 원래 컨텍스트로 되돌림  
- **관련 명령:**  
  - `CALL` / `RET` (일반 함수 호출)  
  - `INT` / `IRET` (인터럽트 호출 및 복귀)

---

<strong>💬 면접식 답변</strong>  
IRET 명령은 인터럽트 처리 후 CPU가 이전 실행 상태로 돌아가게 하는 복귀 명령어입니다.  
ISR이 완료되면 IRET이 스택에 저장된 프로그램 카운터와 플래그를 복원하여 원래 명령으로 복귀합니다.  
이 과정을 통해 인터럽트 전후의 실행 흐름이 일관되게 유지됩니다.

</details>

---

<details markdown="1">
<summary><strong>16. Synchronous(동기)와 Asynchronous(비동기)의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
동기(Synchronous)는 요청한 작업이 끝날 때까지 기다리는 방식이고,  
비동기(Asynchronous)는 요청 후 바로 다음 작업을 수행하며 결과를 나중에 받는 방식입니다.

---

<strong>🔹 특징 및 상세설명</strong>  

| 구분 | Synchronous | Asynchronous |
|------|--------------|--------------|
| **작동 방식** | 순차적 | 병렬 또는 비차단 |
| **예시** | 파일 읽기 시 완료까지 대기 | 콜백으로 결과 수신 |
| **장점** | 구현 간단, 예측 쉬움 | CPU 효율 높음 |
| **단점** | 대기 시간 발생 | 구현 복잡, 동기화 필요 |

---

<strong>💬 면접식 답변</strong>  
동기 방식은 작업이 끝날 때까지 다음 명령을 실행하지 않는 방식이고,  
비동기 방식은 요청 후 다른 일을 처리하다가 결과를 나중에 받는 방식입니다.  
예를 들어, Unity의 Coroutine이나 C#의 `async/await`가 대표적인 비동기 패턴입니다.

</details>

---

<details markdown="1">
<summary><strong>17. Coroutine과 비동기의 차이는 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
Coroutine은 함수 실행을 중단하고 나중에 재개할 수 있는 구조이고,  
비동기(Async)는 별도의 스레드나 이벤트 루프를 통해 병렬적으로 작업을 수행하는 개념입니다.

---

<strong>🔹 특징 및 상세설명</strong>  
- **Coroutine:**  
  - 싱글 스레드 내에서 실행 흐름을 일시 중단(`yield`) 후 재개  
  - Unity, Lua, Python 등에서 사용  
- **Async (비동기):**  
  - OS나 런타임 레벨에서 백그라운드 스레드/이벤트로 동작  
  - 예: `Task`, `Future`, `Promise`  
- **차이점:**  
  - Coroutine은 “동시성(Concurrency)”  
  - Async는 “병렬성(Parallelism)”

---

<strong>💬 면접식 답변</strong>  
Coroutine은 하나의 스레드에서 실행 흐름을 잠시 멈추고 나중에 이어가는 구조입니다.  
반면 비동기 작업은 OS 스케줄러나 별도 스레드에서 병렬로 실행되어 결과를 콜백이나 await로 받습니다.  
즉, Coroutine은 흐름 제어 중심이고, 비동기는 작업 분산 중심입니다.

</details>

---

<details markdown="1">
<summary><strong>18. Critical Section(임계구역)이란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
Critical Section은 여러 스레드가 동시에 접근하면 문제가 발생할 수 있는 코드 영역입니다.

---

<strong>🔹 특징 및 상세설명</strong>  
- **발생 원인:**  
  공유 자원에 대한 동시 접근 (예: 전역 변수, 버퍼)  
- **문제점:**  
  Race Condition, 데이터 불일치, 시스템 오류  
- **해결 방법:**  
  1. Mutex  
  2. Semaphore  
  3. SpinLock  
  4. Monitor / CriticalSection 객체 (Windows API)  

---

<strong>💬 면접식 답변</strong>  
임계구역은 여러 스레드가 동시에 접근하면 데이터 불일치가 발생할 수 있는 코드 부분입니다.  
이를 보호하기 위해 락(Mutex, Semaphore 등)을 사용하여 한 번에 하나의 스레드만 접근하도록 보장합니다.

</details>

---

<details markdown="1">
<summary><strong>19. Semaphore란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
Semaphore는 동시에 접근할 수 있는 스레드의 개수를 제어하는 동기화 도구입니다.

---

<strong>🔹 특징 및 상세설명</strong>  
- **구성요소:** 정수 값 `S` (허용 가능한 자원 수)  
- **작동 방식:**  
  - `wait()` → S 감소 (자원이 없으면 대기)  
  - `signal()` → S 증가 (대기 중인 스레드 깨움)  
- **유형:**  
  - **Binary Semaphore:** S = 0 or 1 (Mutex와 유사)  
  - **Counting Semaphore:** S > 1, 여러 자원 제어 가능  
- **특징:** 여러 스레드가 동시에 접근 가능하나, S값 제한 존재  

---

<strong>💬 면접식 답변</strong>  
Semaphore는 한 번에 접근할 수 있는 스레드 수를 제어하는 기법입니다.  
예를 들어, 데이터베이스 커넥션 풀의 최대 연결 개수를 제한할 때 사용됩니다.  
S값이 0이면 다른 스레드는 대기하고, 자원이 반환되면 다시 접근할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>20️⃣ Mutex란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
Mutex는 한 번에 하나의 스레드만 접근할 수 있도록 하는 상호 배제 동기화 객체입니다.

---

<strong>🔹 특징 및 상세설명</strong>  
- **핵심 원리:** 자원을 잠그고(`lock`), 사용이 끝나면 해제(`unlock`)  
- **특징:**  
  - 소유자(Owner) 개념 존재 → 락을 획득한 스레드만 해제 가능  
  - 교착 상태(Deadlock) 위험 존재  
- **사용 예시:**  
  - 파일 쓰기, 전역 변수 수정, UI 접근  
- **차이점:**  
  - Mutex: 한 스레드만 접근 가능  
  - Semaphore: 여러 스레드 제한적 접근 가능  

---

<strong>💬 면접식 답변</strong>  
Mutex는 한 번에 하나의 스레드만 공유 자원에 접근하도록 하는 동기화 메커니즘입니다.  
스레드가 자원을 잠근 후 반드시 해제해야 하며, 그렇지 않으면 교착 상태가 발생할 수 있습니다.  
즉, Mutex는 완전한 상호 배제를 보장하는 락입니다.

</details>

---

<details markdown="1">
<summary><strong>21. 프로세스(Process)란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
프로세스는 실행 중인 프로그램의 인스턴스로,  
코드, 데이터, 스택, 힙, 그리고 CPU 상태 등의 자원을 포함한 독립적인 실행 단위입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **구성요소:**  
  - Code Section (명령어)  
  - Data Section (전역/정적 변수)  
  - Stack (함수 호출, 지역 변수)  
  - Heap (동적 메모리)  
- **특징:**  
  - 고유한 **PID(Process ID)** 를 가짐  
  - 서로 독립된 메모리 공간 사용  
  - 운영체제가 **PCB(Process Control Block)** 으로 관리  
- **상태 전이:** Ready → Running → Waiting → Terminated  

---

<strong>💬 면접식 답변</strong>  
프로세스는 실행 중인 프로그램 하나를 의미하며,  
코드와 데이터를 포함해 독립된 메모리 공간을 가지고 있습니다.  
운영체제는 각 프로세스를 PCB로 관리하며, CPU 스케줄링을 통해 실행 순서를 조정합니다.

</details>

---

<details markdown="1">
<summary><strong>22. 스레드(Thread)란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
스레드는 프로세스 내부에서 실행되는 가장 작은 실행 단위이며,  
같은 프로세스 내에서 메모리 자원을 공유합니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **공유 자원:** Code, Data, Heap  
- **개별 자원:** Stack, Register, Program Counter  
- **장점:** 컨텍스트 스위치 비용 적음, 빠른 통신 가능  
- **단점:** 동기화 문제(Race Condition), 디버깅 어려움  
- **유형:**  
  - User-Level Thread (경량, 빠름)  
  - Kernel-Level Thread (시스템 관리 용이)

---

<strong>💬 면접식 답변</strong>  
스레드는 프로세스 내부에서 동작하는 실행 단위입니다.  
프로세스 자원을 공유하기 때문에 생성 속도가 빠르고 통신이 효율적이지만,  
동시에 접근 시 동기화 문제가 발생할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>23. PCB(Process Control Block)란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
PCB는 운영체제가 각 프로세스를 관리하기 위해 유지하는 정보 구조체입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **포함 정보:**  
  - 프로세스 상태 (Running, Ready 등)  
  - Program Counter  
  - Register 값  
  - 메모리 관리 정보 (Page Table)  
  - I/O 상태, 파일 핸들  
- **역할:**  
  프로세스 전환(Context Switch) 시, 상태 저장 및 복구에 사용  
- **저장 위치:**  
  커널 영역 (Kernel Space)

---

<strong>💬 면접식 답변</strong>  
PCB는 프로세스의 상태와 자원 정보를 저장한 운영체제 내부 구조체입니다.  
CPU 스위칭 시 PCB를 이용해 이전 상태를 저장하고, 다음 프로세스의 상태를 복원합니다.  
즉, 프로세스의 ‘신분증’ 역할을 하는 구조입니다.

</details>

---

<details markdown="1">
<summary><strong>24. 프로세스의 상태(State)는 어떻게 나뉘나요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
프로세스는 실행 상태에 따라 **New, Ready, Running, Waiting, Terminated** 로 구분됩니다.

---

<strong>🔹 특징 / 상세설명</strong>  
1. **New:** 프로세스 생성 중  
2. **Ready:** 실행 대기 중 (CPU 할당 대기)  
3. **Running:** CPU에서 실제 실행 중  
4. **Waiting (Blocked):** I/O나 이벤트 대기 중  
5. **Terminated:** 실행 완료  
- **상태 전이:**  
  - Dispatch: Ready → Running  
  - Timeout: Running → Ready  
  - Wait: Running → Waiting  
  - Wakeup: Waiting → Ready  

---

<strong>💬 면접식 답변</strong>  
프로세스는 생성(New)부터 실행(Running), 종료(Terminated)까지 여러 상태를 오갑니다.  
예를 들어, CPU 점유가 끝나면 Ready로 돌아가고, I/O 대기 시에는 Waiting으로 전환됩니다.  
이러한 상태 전이는 스케줄러가 관리합니다.

</details>

---

<details markdown="1">
<summary><strong>25. 프로세스 메모리 구조는 어떻게 되어있나요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
프로세스 메모리는 **Code, Data, Heap, Stack** 영역으로 나뉩니다.

---

<strong>🔹 특징 / 상세설명</strong>  

| 영역 | 내용 | 크기 변화 |
|------|------|-----------|
| Code | 프로그램 명령어 | 고정 |
| Data | 전역/정적 변수 | 고정 |
| Heap | 동적 메모리(new, malloc) | 증가/감소 |
| Stack | 함수 호출, 지역 변수 | 함수 호출 시 증가, 리턴 시 감소 |

- **특징:**  
  - Stack은 높은 주소 → 낮은 주소 방향으로 성장  
  - Heap은 낮은 주소 → 높은 주소 방향으로 성장  

---

<strong>💬 면접식 답변</strong>  
프로세스는 코드, 데이터, 힙, 스택 영역으로 나뉘며,  
코드는 실행 명령, 데이터는 전역 변수, 스택은 함수 호출 정보, 힙은 동적 메모리를 저장합니다.  
특히 스택과 힙은 반대 방향으로 확장되어 충돌을 방지합니다.

</details>

---

<details markdown="1">
<summary><strong>26. Stack 메모리가 높은 주소에서 낮은 주소로 확장되는 이유는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
스택과 힙이 서로 반대 방향으로 확장되도록 하여,  
서로 충돌하지 않고 효율적으로 메모리를 사용할 수 있기 때문입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **Heap:** 동적 메모리 → 낮은 주소부터 위로 증가  
- **Stack:** 함수 호출 → 높은 주소부터 아래로 감소  
- **이유:**  
  1. 메모리의 양끝에서 서로 향하도록 하여 공간을 유연하게 공유  
  2. 예측 가능한 오버플로 감지 용이  
  3. CPU 구조상 스택 포인터의 감소 연산이 효율적

---

<strong>💬 면접식 답변</strong>  
스택은 높은 주소에서 낮은 주소로, 힙은 반대로 확장됩니다.  
이 구조 덕분에 두 영역이 동적으로 메모리를 공유하며,  
프로그램이 사용하는 메모리 공간을 효율적으로 조절할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>27. Heap 메모리 관리에서 주의해야 할 점은 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
Heap은 동적 메모리 공간으로,  
메모리 누수나 단편화 등의 문제가 발생하지 않도록 관리가 필요합니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **주의사항:**  
  1. `new` / `malloc`으로 할당 후 반드시 `delete` / `free` 필요  
  2. 중복 해제(Double Free) 금지  
  3. Dangling Pointer(유효하지 않은 포인터 참조) 주의  
  4. 단편화(Heap Fragmentation) 발생 가능  
- **해결책:**  
  - 스마트 포인터 (C++)  
  - 풀 할당(Memory Pool)  
  - Garbage Collection (C#, Java)

---

<strong>💬 면접식 답변</strong>  
Heap은 동적으로 메모리를 관리하기 때문에 누수나 단편화가 쉽게 발생합니다.  
그래서 C++에서는 스마트 포인터를, C#에서는 GC를 사용해 안전하게 관리하며,  
사용이 끝난 메모리는 반드시 해제해야 합니다.

</details>

---

<details markdown="1">
<summary><strong>28. 내부 단편화(Internal Fragmentation)란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
내부 단편화는 고정된 블록을 할당할 때 실제 사용보다 더 큰 공간이 낭비되는 현상입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **예시:**  
  4KB 블록 단위 할당 시, 3.6KB만 사용하면 0.4KB 낭비  
- **원인:**  
  고정 크기 메모리 블록 또는 페이지 기반 관리  
- **해결방법:**  
  - 동적 크기 할당 (Buddy System)  
  - 페이지 크기 조정  
  - 세그멘테이션과 결합

---

<strong>💬 면접식 답변</strong>  
내부 단편화는 프로세스가 필요한 양보다 큰 블록을 할당받아 낭비되는 공간입니다.  
예를 들어, 4KB 페이지에 3KB만 사용하면 남는 1KB가 낭비됩니다.  
이 문제는 가변 크기 블록이나 세그멘테이션 기법으로 완화할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>29. 외부 단편화(External Fragmentation)란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
외부 단편화는 메모리 중간중간에 작은 빈 공간이 생겨,  
총합은 충분하지만 연속된 공간을 할당할 수 없는 현상입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **예시:**  
  10MB 중 2MB + 3MB + 5MB로 나뉘어 존재 시,  
  6MB 프로세스를 연속적으로 배치 불가능  
- **해결방법:**  
  - **Compaction(압축)**: 메모리 공간 재배치  
  - **Paging**: 연속되지 않은 페이지 단위 관리  
  - **Segmentation + Paging 결합**  

---

<strong>💬 면접식 답변</strong>  
외부 단편화는 메모리 중간중간이 잘게 쪼개져 연속된 큰 공간을 할당할 수 없는 문제입니다.  
이 문제는 페이지 단위로 메모리를 분할하거나, 압축(Compaction)을 통해 완화할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>30️⃣ Memory Pool(메모리 풀)이란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
Memory Pool은 자주 할당/해제되는 작은 객체들을 미리 확보해두고 재사용하는 메모리 관리 기법입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **장점:**  
  - `malloc` / `free` 호출 감소 → 성능 향상  
  - 단편화 방지  
  - 예측 가능한 메모리 사용량  
- **구조:**  
  - 고정 크기 블록의 리스트를 유지  
  - 필요 시 즉시 제공 후 반환 시 재사용  
- **활용 예시:**  
  - 게임 오브젝트 관리 (Unity Object Pool)  
  - 네트워크 버퍼 관리  

---

<strong>💬 면접식 답변</strong>  
Memory Pool은 작은 객체를 반복적으로 할당할 때,  
미리 확보해둔 블록을 재사용하는 방식입니다.  
이를 통해 메모리 단편화를 줄이고, 할당/해제 속도를 크게 향상시킬 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>31. Virtual Memory(가상 메모리)란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
가상 메모리는 실제 물리 메모리보다 큰 공간을 사용하는 것처럼 보이게 하는 메모리 관리 기법으로,  
프로세스마다 독립된 주소 공간을 제공하여 안정성과 효율성을 높입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **핵심 아이디어:**  
  프로세스는 가상의 주소 공간(Virtual Address Space)을 사용하고,  
  MMU가 이를 실제 물리 주소로 변환합니다.  
- **장점:**  
  - 프로세스 간 메모리 보호  
  - 메모리 효율적 사용 (Demand Paging)  
  - 큰 프로그램 실행 가능  
- **구성:**  
  - Virtual Address ↔ Physical Address 매핑  
  - Page Table, TLB, MMU가 변환 담당  

---

<strong>💬 면접식 답변</strong>  
가상 메모리는 프로세스가 실제 물리 메모리보다 큰 공간을 사용할 수 있도록 하는 시스템입니다.  
이를 통해 메모리 보호, 프로세스 간 독립성, 효율적인 자원 분배가 가능해집니다.  
MMU가 주소 변환을 담당하고, 필요할 때만 페이지를 로드합니다.

</details>

---

<details markdown="1">
<summary><strong>32. Paging(페이징)이란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
페이징은 가상 메모리를 일정한 크기의 블록(Page)으로 나누고,  
이를 물리 메모리의 프레임(Frame)과 매핑하는 메모리 관리 기법입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **Page:** 가상 메모리의 고정 단위 (보통 4KB)  
- **Frame:** 물리 메모리의 고정 단위  
- **Page Table:** Page ↔ Frame 매핑 정보 저장  
- **장점:** 외부 단편화 해결  
- **단점:** 내부 단편화 발생 가능  

---

<strong>💬 면접식 답변</strong>  
페이징은 메모리를 일정한 단위로 나누어 관리함으로써 외부 단편화를 제거한 기법입니다.  
가상 페이지를 실제 프레임에 매핑하는 구조로, 각 프로세스마다 페이지 테이블을 가집니다.

</details>

---

<details markdown="1">
<summary><strong>33. Page Table이란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
페이지 테이블은 가상 주소와 물리 주소의 대응 관계를 저장하는 자료구조입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **구조:**  
  - Page Number → Frame Number 매핑  
  - Valid Bit: 존재 여부  
  - Dirty Bit: 수정 여부  
  - Access Bit: 접근 기록  
- **종류:**  
  - 단일 페이지 테이블  
  - 다단계 페이지 테이블 (2-Level, 3-Level)  
  - Inverted Page Table  
- **문제점:**  
  - 메모리 낭비 → 다단계 페이지 테이블로 개선  

---

<strong>💬 면접식 답변</strong>  
페이지 테이블은 가상 주소의 페이지 번호를 물리 메모리의 프레임 번호로 변환하는 자료구조입니다.  
이 정보를 통해 CPU가 주소 변환을 수행하며, 다단계 구조로 효율을 높입니다.

</details>

---

<details markdown="1">
<summary><strong>34. Page Fault란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
페이지 폴트는 CPU가 접근하려는 페이지가 물리 메모리에 없는 경우 발생하는 예외(Interrupt)입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **발생 과정:**  
  1. CPU가 페이지 접근  
  2. 페이지 테이블에서 Valid Bit = 0 확인  
  3. OS가 디스크에서 해당 페이지를 로드  
  4. 페이지 테이블 갱신 후 재시도  
- **원인:**  
  - Demand Paging  
  - Swapping  
- **성능 영향:**  
  - 디스크 접근이 느리므로 성능 저하  

---

<strong>💬 면접식 답변</strong>  
페이지 폴트는 CPU가 메모리에 없는 페이지를 접근할 때 발생하는 인터럽트입니다.  
운영체제는 이를 감지하고 필요한 페이지를 디스크에서 로드해 페이지 테이블을 갱신합니다.

</details>

---

<details markdown="1">
<summary><strong>35. Page Replacement Algorithm이란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
페이지 교체 알고리즘은 새로운 페이지를 메모리에 올리기 위해  
어떤 페이지를 제거할지를 결정하는 정책입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **주요 알고리즘:**  
  1. FIFO (First In First Out): 오래된 페이지 제거  
  2. LRU (Least Recently Used): 최근에 사용되지 않은 페이지 제거  
  3. LFU (Least Frequently Used): 사용 빈도 낮은 페이지 제거  
  4. Optimal: 앞으로 가장 오래 사용되지 않을 페이지 제거 (이론적)  
- **성능 지표:**  
  Page Fault Rate  

---

<strong>💬 면접식 답변</strong>  
페이지 교체 알고리즘은 메모리가 가득 찼을 때 어떤 페이지를 제거할지를 결정하는 방식입니다.  
가장 널리 쓰이는 알고리즘은 LRU이며, 실제 시스템에서는 효율을 위해 하이브리드 형태로 구현됩니다.

</details>

---

<details markdown="1">
<summary><strong>36. TLB(Translation Lookaside Buffer)란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
TLB는 최근 사용된 페이지 매핑 정보를 캐싱하는 고속 메모리로,  
주소 변환 속도를 향상시키는 하드웨어 장치입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **기능:** Virtual Page → Physical Frame 매핑 캐시  
- **구조:**  
  - Associative Memory로 구성 (병렬 검색)  
  - Miss 시 페이지 테이블 접근  
- **장점:** 주소 변환 시간 대폭 단축  
- **관련 용어:** TLB Hit / Miss  

---

<strong>💬 면접식 답변</strong>  
TLB는 페이지 테이블의 일부를 캐싱하여 주소 변환 속도를 높이는 장치입니다.  
최근 접근한 페이지 정보를 저장하고, TLB Miss 시에만 페이지 테이블을 참조합니다.

</details>

---

<details markdown="1">
<summary><strong>37. Swapping이란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
Swapping은 프로세스 전체를 디스크로 내보내거나 다시 메모리로 불러오는 기법으로,  
메모리 공간을 효율적으로 활용하기 위한 방법입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **방식:**  
  - 메모리 부족 시 비활성 프로세스를 디스크로 내보냄  
  - 필요 시 다시 로드  
- **장점:** 많은 프로세스 동시 실행 가능  
- **단점:** 디스크 I/O 오버헤드 (느림)  
- **현대 OS:** 부분 스왑 (페이징 기반)  

---

<strong>💬 면접식 답변</strong>  
Swapping은 실행 중인 프로세스를 디스크로 내보내거나 다시 불러오는 기술로,  
메모리를 절약하고 동시에 많은 프로세스를 처리할 수 있게 해줍니다.  
하지만 디스크 접근이 느려 자주 발생하면 성능이 떨어집니다.

</details>

---

<details markdown="1">
<summary><strong>38. Thrashing(스래싱)이란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
Thrashing은 페이지 교체가 너무 자주 발생해 CPU가 실제 작업보다  
페이지 교체에 더 많은 시간을 소비하는 현상입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **원인:**  
  - 메모리 부족  
  - 과도한 멀티프로세싱  
  - Working Set보다 작은 메모리  
- **결과:**  
  - CPU 이용률 급감  
  - 디스크 I/O 폭증  
- **해결책:**  
  - 프로세스 수 조절  
  - Working Set 관리  
  - Page Fault Rate 모니터링  

---

<strong>💬 면접식 답변</strong>  
스래싱은 메모리가 부족해 페이지 교체가 반복되면서 CPU가 거의 일하지 못하는 상태입니다.  
운영체제는 프로세스의 작업 집합 크기를 추적해 이를 방지합니다.

</details>

---

<details markdown="1">
<summary><strong>39. MMU(Memory Management Unit)이란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
MMU는 가상 주소를 물리 주소로 변환하고, 접근 권한을 검사하는 하드웨어 장치입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **주요 기능:**  
  - 주소 변환 (Virtual → Physical)  
  - 접근 보호 (Read/Write 권한 확인)  
  - 캐시 관리 및 페이지 테이블 참조  
- **구성 요소:**  
  - TLB  
  - Page Table Base Register  
- **장점:**  
  - CPU와 메모리 간 분리  
  - 보안성 강화  

---

<strong>💬 면접식 답변</strong>  
MMU는 CPU가 사용하는 가상 주소를 실제 물리 주소로 변환하는 하드웨어입니다.  
또한 접근 권한을 관리해, 프로세스 간 메모리 보호를 담당합니다.

</details>

---

<details markdown="1">
<summary><strong>40️⃣ Working Set이란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
Working Set은 특정 시간 동안 프로세스가 자주 사용하는 페이지들의 집합으로,  
프로세스의 “활성 메모리 영역”을 의미합니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **목적:**  
  Thrashing 방지, 메모리 효율 향상  
- **정의:**  
  최근 일정 시간 동안 참조된 페이지의 집합  
- **특징:**  
  - Working Set이 너무 작으면 Thrashing  
  - 너무 크면 메모리 낭비  
- **운영체제 역할:**  
  동적 조정 (Working Set Model)

---

<strong>💬 면접식 답변</strong>  
Working Set은 프로세스가 일정 시간 동안 실제로 사용하는 페이지의 집합입니다.  
운영체제는 이를 기준으로 메모리 할당을 조정해 Thrashing을 방지합니다.

</details>

---

<details markdown="1">
<summary><strong>41. I/O System(입출력 시스템)이란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
입출력 시스템은 CPU, 메모리, 주변 장치 간 데이터 교환을 관리하는 운영체제의 핵심 부분입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **I/O 구성 요소:**  
  - Device Controller (하드웨어 제어기)  
  - Device Driver (소프트웨어 제어기)  
  - Buffering / Spooling / DMA 관리  
- **주요 역할:**  
  - CPU와 I/O 동시 병행 수행  
  - 장치 독립성 보장 (추상화)  
  - 에러 처리 및 자원 스케줄링  
- **I/O 구조:**  
  Application → System Call → Kernel I/O Subsystem → Device Driver → Hardware  

---

<strong>💬 면접식 답변</strong>  
입출력 시스템은 CPU와 주변 장치 간의 데이터 교환을 효율적으로 관리하는 운영체제의 핵심 기능입니다.  
디바이스 드라이버와 버퍼링, DMA 등을 통해 CPU 부하를 줄이고, 입출력 효율을 높입니다.

</details>

---

<details markdown="1">
<summary><strong>42. Interrupt(인터럽트)란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
인터럽트는 CPU가 작업 중이더라도 예외적인 이벤트나 외부 신호에 즉시 반응하도록 하는 메커니즘입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **종류:**  
  - 하드웨어 인터럽트: I/O 장치, 타이머 등  
  - 소프트웨어 인터럽트: 시스템 콜, 예외(Exception)  
- **처리 절차:**  
  1. 현재 명령 중단 및 상태 저장  
  2. 인터럽트 벡터 테이블 참조  
  3. ISR(Interrupt Service Routine) 실행  
  4. 상태 복원 후 재개  
- **장점:**  
  CPU 자원 낭비 감소 (Polling 대비)  

---

<strong>💬 면접식 답변</strong>  
인터럽트는 CPU가 외부 장치나 시스템 이벤트에 즉시 반응하도록 하는 기능입니다.  
현재 작업을 일시 중단하고 ISR을 실행한 뒤, 다시 원래 작업으로 복귀합니다.

</details>

---

<details markdown="1">
<summary><strong>43. DMA(Direct Memory Access)란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
DMA는 CPU의 개입 없이 I/O 장치가 메모리에 직접 접근하여 데이터를 전송하는 기술입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **구조:** DMA Controller가 CPU 대신 데이터 전송을 수행  
- **장점:**  
  - CPU 부하 감소  
  - I/O 처리 속도 향상  
- **작동 과정:**  
  1. CPU가 DMA에 전송 명령  
  2. DMA가 직접 메모리 접근  
  3. 완료 후 CPU에 인터럽트 전달  

---

<strong>💬 면접식 답변</strong>  
DMA는 CPU 대신 I/O 장치가 직접 메모리에 데이터를 전송하도록 하는 방식입니다.  
CPU는 전송 명령만 내리고, 나머지는 DMA 컨트롤러가 처리하여 효율을 높입니다.

</details>

---

<details markdown="1">
<summary><strong>44. Buffering(버퍼링)과 Spooling(스풀링)의 차이는 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
Buffering은 데이터 전송 속도 차이를 완화하기 위한 임시 저장이고,  
Spooling은 여러 작업의 출력을 디스크에 임시 저장해 순차적으로 처리하는 기법입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **Buffering:**  
  - 메모리(RAM) 기반 임시 저장  
  - 생산자-소비자 속도 불일치 해결  
- **Spooling:**  
  - 디스크 기반 큐잉 시스템  
  - 프린터, 배치 작업 등에서 사용  
  - 병렬성 향상 (다중 사용자 환경)  

---

<strong>💬 면접식 답변</strong>  
버퍼링은 속도 차이를 줄이기 위해 데이터를 임시 저장하는 방식이고,  
스풀링은 작업을 디스크에 저장해 순서대로 처리하는 시스템입니다.  
프린터 출력 대기열이 스풀링의 대표적인 예시입니다.

</details>

---

<details markdown="1">
<summary><strong>45. Synchronous(동기)와 Asynchronous(비동기)의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
동기는 작업이 끝날 때까지 대기하는 방식이고,  
비동기는 작업 완료 여부와 상관없이 다음 작업을 수행하는 방식입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **Synchronous:**  
  - 함수 호출 시 반환 전까지 블록  
  - 직렬 처리  
- **Asynchronous:**  
  - 콜백, 이벤트 기반 처리  
  - 병렬성 활용 가능  
- **비교:**  
  - 동기는 예측 가능성 높음  
  - 비동기는 효율성과 응답성 높음  

---

<strong>💬 면접식 답변</strong>  
동기는 호출한 작업이 끝날 때까지 기다리는 구조이고,  
비동기는 결과와 상관없이 다음 명령을 수행합니다.  
Unity의 Coroutine이나 C#의 async/await는 비동기 처리를 대표하는 예입니다.

</details>

---

<details markdown="1">
<summary><strong>46. Deadlock(교착 상태)이란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
교착 상태는 두 개 이상의 프로세스가 서로의 자원을 기다리며  
영원히 대기하는 상태를 말합니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **발생 조건 (Coffman 조건 4가지):**  
  1. 상호 배제 (Mutual Exclusion)  
  2. 점유와 대기 (Hold and Wait)  
  3. 비선점 (No Preemption)  
  4. 순환 대기 (Circular Wait)  
- **해결 방법:**  
  - 예방 (Prevention)  
  - 회피 (Avoidance, Banker's Algorithm)  
  - 탐지 및 복구 (Detection & Recovery)  

---

<strong>💬 면접식 답변</strong>  
데드락은 여러 프로세스가 자원을 점유한 채 서로의 자원을 기다리는 상황입니다.  
이를 방지하기 위해 자원 할당 순서를 지정하거나, Banker's Algorithm 같은 회피 기법을 사용합니다.

</details>

---

<details markdown="1">
<summary><strong>47. Concurrency(동시성)과 Parallelism(병렬성)의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
동시성은 여러 작업이 번갈아가며 실행되는 것이고,  
병렬성은 여러 작업이 실제로 동시에 수행되는 것입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **Concurrency:**  
  - 논리적 병행 (싱글 코어도 가능)  
  - 스케줄러에 의해 번갈아 실행  
- **Parallelism:**  
  - 물리적 병행 (멀티 코어 필요)  
  - 진짜 동시 실행  
- **비교:**  
  - 동시성: 구조적 복잡성  
  - 병렬성: 성능 향상 중심  

---

<strong>💬 면접식 답변</strong>  
동시성은 여러 작업이 한정된 자원을 번갈아 사용하는 구조이고,  
병렬성은 여러 코어에서 동시에 작업이 수행되는 구조입니다.  
멀티스레딩은 동시성과 병렬성을 모두 활용할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>48. CPU-bound와 I/O-bound 작업의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
CPU-bound는 연산 중심의 작업, I/O-bound는 입출력 중심의 작업입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **CPU-bound:**  
  - 연산 위주 (예: 수학 연산, 암호화, AI 계산)  
  - CPU 성능이 병목  
- **I/O-bound:**  
  - 디스크/네트워크 중심  
  - I/O 대기 시간이 병목  
- **운영체제 스케줄링:**  
  - CPU-bound → Long Quantum  
  - I/O-bound → Short Quantum  

---

<strong>💬 면접식 답변</strong>  
CPU-bound는 연산이 많아 CPU 속도가 중요하고,  
I/O-bound는 입출력 대기 시간이 많아 디스크나 네트워크 성능이 중요합니다.  
운영체제는 이를 고려해 각기 다른 스케줄링 정책을 적용합니다.

</details>

---

<details markdown="1">
<summary><strong>49. Disk Scheduling Algorithm에는 어떤 것들이 있나요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
디스크 스케줄링 알고리즘은 헤드 이동 거리를 최소화하여 접근 시간을 줄이는 알고리즘입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **FCFS:** 요청 순서대로 처리  
- **SSTF:** 가장 가까운 트랙 우선 (Shortest Seek Time First)  
- **SCAN:** 엘리베이터 알고리즘 (왕복 이동)  
- **C-SCAN:** 한 방향으로만 이동  
- **LOOK / C-LOOK:** 실제 요청 위치까지만 이동  
- **평가 기준:** 평균 탐색 시간, 응답 시간  

---

<strong>💬 면접식 답변</strong>  
디스크 스케줄링은 요청된 I/O 작업의 순서를 결정해 성능을 최적화하는 알고리즘입니다.  
SSTF와 SCAN이 가장 자주 사용되며, C-LOOK은 고성능 디스크 환경에서 효율적입니다.

</details>

---

<details markdown="1">
<summary><strong>50️⃣ RAID 구조란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
RAID는 여러 개의 디스크를 하나처럼 묶어 성능 향상과 데이터 안정성을 동시에 얻는 기술입니다.

---

<strong>🔹 특징 / 상세설명</strong>  
- **RAID 0:** Striping (성능 향상, 안정성 없음)  
- **RAID 1:** Mirroring (복제, 안정성 높음)  
- **RAID 5:** Parity 기반 (속도 + 안정성 균형)  
- **RAID 10:** 1과 0의 혼합 (고속 + 고안정성)  
- **장점:**  
  - 읽기/쓰기 속도 향상  
  - 데이터 손실 대비  

---

<strong>💬 면접식 답변</strong>  
RAID는 여러 하드디스크를 하나의 논리적 장치로 묶어 성능과 안정성을 높이는 기술입니다.  
RAID 0은 속도, RAID 1은 복구 안정성, RAID 5/10은 균형형으로 자주 사용됩니다.

</details>
