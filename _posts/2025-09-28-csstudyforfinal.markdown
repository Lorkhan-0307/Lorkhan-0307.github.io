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

## 🖥️ 컴퓨터 구조 Q/A
   
<details>
<summary><strong>1) 폰 노이만 구조와 하버드 구조의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>폰 노이만 구조</strong>는 프로그램 명령어와 데이터를 동일한 메모리에 저장하고, 하나의 버스를 통해 CPU와 주고받는 방식입니다. 이 구조는 설계가 단순하고 범용적으로 사용되지만, 명령어와 데이터가 같은 버스를 공유하기 때문에 병목 현상(폰 노이만 병목)이 발생할 수 있습니다.  
<strong>하버드 구조</strong>는 명령어와 데이터를 별도의 메모리와 버스를 통해 관리합니다. 따라서 CPU가 동시에 명령어를 읽고 데이터를 접근할 수 있어 병렬성이 향상됩니다. 다만 구조가 복잡하고 하드웨어 비용이 증가합니다.  
즉, <strong>공통점</strong>은 모두 프로그램 명령어와 데이터를 처리하기 위한 CPU-메모리 구조라는 점이고, <strong>차이점</strong>은 버스를 공유하느냐 분리하느냐에 따른 성능 및 설계 복잡도의 차이입니다.
</details>

---

<details>
<summary><strong>2) CPU의 주요 구성 요소는 무엇인가요?</strong></summary>
<strong>A.</strong> CPU는 크게 <strong>연산 장치(ALU)</strong>, <strong>제어 장치(CU)</strong>, 그리고 <strong>레지스터</strong>로 구성됩니다. ALU는 산술 및 논리 연산을 담당하고, CU는 메모리와 입출력 장치 간 제어 신호를 관리하며, 레지스터는 명령어 주소, 중간 계산 결과, 상태 플래그 등을 보관하는 초고속 임시 저장 공간입니다. 이 세 요소가 협력하여 명령어 사이클을 수행합니다.
</details>

---

<details>
<summary><strong>3) 파이프라이닝이란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>파이프라이닝</strong>은 명령어 실행을 여러 단계로 나누어 동시에 처리하는 기법입니다. 예를 들어, 명령어 가져오기(IF), 해석(ID), 실행(EX), 메모리 접근(MEM), 결과 기록(WB) 단계를 겹쳐 수행하여 처리량을 향상시킵니다.  
즉, 세탁 공장에서 빨래, 건조, 다림질을 동시에 진행하는 것처럼 CPU도 여러 명령어를 병렬적으로 처리합니다. 다만 데이터 종속성, 제어 종속성 같은 <strong>Hazard 문제</strong>가 발생할 수 있으며, 이를 해결하기 위해 <strong>포워딩</strong>이나 <strong>분기 예측</strong> 같은 기법을 사용합니다.
</details>

---

<details>
<summary><strong>4) 캐시 메모리의 지역성(Locality) 원리란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>캐시 메모리</strong>는 CPU와 메인 메모리 간 속도 차이를 줄이기 위해 사용되며, <strong>지역성(Locality)</strong> 원리에 기반합니다.  
- <strong>시간 지역성</strong>: 최근에 접근한 데이터가 가까운 미래에도 다시 접근될 가능성이 크다.  
- <strong>공간 지역성</strong>: 특정 메모리 주소를 참조하면 그 주변 주소도 곧 참조될 가능성이 크다.  
캐시는 이 원리를 활용하여 CPU가 필요할 것 같은 데이터를 미리 가져와 성능을 높입니다.
</details>

---

<details>
<summary><strong>5) 캐시 일관성(Cache Coherency) 문제는 무엇이고, 어떻게 해결하나요?</strong></summary>
<strong>A.</strong> 멀티코어 환경에서는 각 코어가 독립된 캐시를 갖기 때문에 동일한 메모리 주소에 대해 서로 다른 값을 가질 수 있습니다. 이를 <strong>캐시 일관성 문제</strong>라고 합니다.  
해결책으로는 <strong>MESI 프로토콜</strong> 같은 캐시 코히어런시 프로토콜을 사용합니다. MESI는 캐시 라인의 상태를 Modified, Exclusive, Shared, Invalid로 정의하여, 한 코어가 데이터를 수정하면 다른 코어의 캐시 라인을 무효화하거나 갱신하여 일관성을 보장합니다.
</details>

---

<details>
<summary><strong>6) 파이프라인 Hazard에는 어떤 종류가 있고, 어떻게 해결하나요?</strong></summary>
<strong>A.</strong> <strong>Hazard</strong>는 파이프라인의 성능을 방해하는 상황을 말하며 세 가지로 나눌 수 있습니다.  
- <strong>구조적 Hazard</strong>: 하드웨어 자원을 동시에 사용하려 할 때 발생 → 추가 하드웨어나 자원 분할로 해결  
- <strong>데이터 Hazard</strong>: 이전 명령어 결과가 아직 나오지 않았는데 다음 명령어가 그 데이터를 필요로 할 때 발생 → 포워딩(forwarding)이나 파이프라인 스톨로 해결  
- <strong>제어 Hazard</strong>: 분기 명령어로 인해 실행 경로가 불확실할 때 발생 → 분기 예측, 지연 분기 기법으로 해결  
이처럼 Hazard는 파이프라이닝의 효율성을 제한하는 주요 요인이지만, 현대 CPU는 다양한 기법으로 이를 최소화합니다.
</details>

---

<details>
<summary><strong>7) RISC와 CISC의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>RISC(Reduced Instruction Set Computer)</strong>는 단순하고 짧은 길이의 명령어를 사용하여 파이프라이닝과 최적화에 유리합니다. 반면, <strong>CISC(Complex Instruction Set Computer)</strong>는 복잡한 명령어를 제공해 한 줄의 코드로 많은 작업을 수행할 수 있습니다.  
<strong>공통점</strong>은 모두 범용 프로세서 구조라는 점이지만, RISC는 하드웨어 설계를 단순화하고 컴파일러 최적화에 의존하며, CISC는 하드웨어가 더 많은 기능을 제공해 코드 밀도를 높일 수 있습니다. 대표적으로 ARM은 RISC, x86은 CISC 구조입니다.
</details>

---

<details>
<summary><strong>8) 분기 예측이란 무엇이며 왜 필요한가요?</strong></summary>
<strong>A.</strong> <strong>분기 예측</strong>은 조건문 같은 분기 명령어가 있을 때, 실제 결과가 나오기 전에 분기 방향을 예측하여 파이프라인이 멈추지 않고 계속 실행되도록 하는 기법입니다. 예측이 맞으면 성능이 크게 향상되지만, 틀리면 파이프라인을 플러시하고 다시 실행해야 하므로 성능이 저하됩니다.  
현대 CPU는 <strong>1-bit, 2-bit 예측기</strong>, <strong>분기 목표 버퍼(BTB)</strong> 등을 사용해 높은 정확도의 분기 예측을 수행합니다.
</details>

---

<details>
<summary><strong>9) 멀티코어 환경에서 동기화가 필요한 이유는 무엇인가요?</strong></summary>
<strong>A.</strong> 멀티코어 시스템에서는 여러 코어가 동시에 동일한 메모리 자원에 접근할 수 있습니다. 이때 동기화가 없으면 데이터 불일치나 경합이 발생해 프로그램의 올바른 동작이 보장되지 않습니다. 따라서 <strong>뮤텍스</strong>, <strong>세마포어</strong>, <strong>메모리 배리어</strong> 같은 동기화 도구를 통해 순서를 보장해야 합니다.
</details>

---

<details>
<summary><strong>10) SIMD와 MIMD의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>SIMD(Single Instruction, Multiple Data)</strong>는 하나의 명령어로 여러 데이터를 동시에 처리하는 방식으로, 벡터 연산이나 멀티미디어 처리에 적합합니다. 반면 <strong>MIMD(Multiple Instruction, Multiple Data)</strong>는 여러 명령어가 여러 데이터를 독립적으로 처리하는 방식으로, 멀티코어 CPU나 분산 시스템에서 사용됩니다.  
즉, SIMD는 동일 연산을 병렬화하는 데 특화되어 있고, MIMD는 보다 일반적인 병렬 처리를 지원합니다.
</details>

---

## 🧪 운영체제 Q/A

<details>
<summary><strong>1) 프로세스와 스레드의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>프로세스</strong>는 실행 중인 프로그램의 인스턴스로, 독립적인 메모리 공간(코드, 데이터, 스택, 힙)을 갖습니다. <strong>스레드</strong>는 프로세스 내부의 실행 단위로, 코드와 데이터를 공유하면서 개별적인 스택만 유지합니다. 따라서 프로세스는 서로 격리되어 안정성이 높지만 컨텍스트 스위칭 비용이 크고, 스레드는 자원을 공유해 효율적이지만 동기화 문제가 발생할 수 있습니다.
</details>

---

<details>
<summary><strong>2) PCB(Process Control Block)란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>PCB</strong>는 운영체제가 각 프로세스를 관리하기 위해 유지하는 데이터 구조입니다. 여기에는 프로세스 ID, 레지스터 값, 프로그램 카운터, 메모리 맵, 스케줄링 우선순위, 열려 있는 파일 목록 등이 저장됩니다. 운영체제는 컨텍스트 스위칭 시 PCB에 상태를 저장하거나 복원하여 프로세스를 이어 실행합니다.
</details>

---

<details>
<summary><strong>3) 컨텍스트 스위칭이란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>컨텍스트 스위칭</strong>은 CPU가 현재 실행 중인 프로세스의 상태를 PCB에 저장하고, 다른 프로세스의 상태를 불러와 실행을 전환하는 과정입니다. 이 과정은 CPU 시간을 공유하기 위해 필요하지만, 저장·복원 작업 때문에 오버헤드가 발생합니다. 스레드 간 전환은 자원을 공유하기 때문에 비교적 빠르지만, 프로세스 간 전환은 메모리 맵 교체까지 필요해 더 무겁습니다.
</details>

---

<details>
<summary><strong>4) 교착 상태(Deadlock)가 발생하는 조건은 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>교착 상태</strong>는 프로세스들이 서로의 자원을 기다리며 무한 대기하는 상황으로, 네 가지 조건이 동시에 충족될 때 발생합니다.  
1) <strong>상호 배제</strong>: 자원은 한 번에 한 프로세스만 사용할 수 있다.  
2) <strong>점유와 대기</strong>: 자원을 보유한 채 다른 자원을 기다린다.  
3) <strong>비선점</strong>: 자원을 강제로 뺏을 수 없다.  
4) <strong>환형 대기</strong>: 프로세스들이 원형으로 자원을 기다린다.  
이 중 하나라도 깨면 교착 상태를 예방할 수 있습니다.
</details>

---

<details>
<summary><strong>5) 세마포어와 뮤텍스의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>뮤텍스</strong>는 상호 배제를 위한 잠금 장치로, 자원을 하나의 스레드만 사용할 수 있도록 보장합니다. 소유권 개념이 있어 잠근 스레드만 잠금을 해제할 수 있습니다. 반면 <strong>세마포어</strong>는 정수 값을 사용하여 여러 자원을 동시에 관리할 수 있고, 소유권 개념이 없습니다. 따라서 세마포어는 자원 개수가 여러 개인 경우 유용하며, 뮤텍스는 단일 자원 보호에 적합합니다.
</details>

---

<details>
<summary><strong>6) 가상 메모리란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>가상 메모리</strong>는 실제 물리 메모리 크기와 관계없이 큰 주소 공간을 제공하는 메모리 관리 기법입니다. 프로세스는 자신만의 연속된 메모리 공간을 갖는 것처럼 보이지만, 실제로는 페이지 단위로 잘려 물리 메모리에 매핑됩니다. 필요한 페이지가 없으면 <strong>페이지 폴트</strong>가 발생해 디스크에서 데이터를 불러옵니다. 이를 통해 메모리 효율성과 보안성을 높일 수 있습니다.
</details>

---

<details>
<summary><strong>7) 페이징과 세그멘테이션의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>페이징</strong>은 메모리를 고정 크기의 페이지 단위로 나누는 방식으로, 외부 단편화를 방지하지만 내부 단편화가 발생할 수 있습니다. <strong>세그멘테이션</strong>은 코드, 데이터, 스택처럼 의미 단위로 메모리를 나누는 방식으로, 프로그램 구조를 반영할 수 있지만 외부 단편화가 발생할 수 있습니다. 현대 운영체제는 보통 두 방식을 혼합해 사용합니다.
</details>

---

<details>
<summary><strong>8) 페이지 교체 알고리즘에는 어떤 것들이 있나요?</strong></summary>
<strong>A.</strong> 대표적인 <strong>페이지 교체 알고리즘</strong>에는 FIFO(가장 먼저 들어온 페이지 교체), LRU(가장 오래 사용하지 않은 페이지 교체), Optimal(앞으로 가장 오래 사용하지 않을 페이지 교체)이 있습니다. 실제 운영체제에서는 LRU를 근사한 알고리즘(Clock 등)이 자주 사용됩니다. 각 알고리즘은 교체 빈도와 성능에 차이가 있습니다.
</details>

---

<details>
<summary><strong>9) 인터럽트와 폴링의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>인터럽트</strong>는 입출력 장치나 하드웨어가 CPU에 작업 완료를 알리는 방식으로, CPU 자원을 효율적으로 사용합니다. 반면 <strong>폴링</strong>은 CPU가 주기적으로 장치 상태를 확인하는 방식으로 구현은 단순하지만 CPU 사이클 낭비가 큽니다. 따라서 실시간성이 중요한 경우에는 인터럽트, 단순 반복적인 경우에는 폴링이 사용되기도 합니다.
</details>

---

<details>
<summary><strong>10) 시스템 콜(System Call)이란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>시스템 콜</strong>은 사용자 프로그램이 운영체제의 커널 기능에 접근하기 위한 인터페이스입니다. 예를 들어, 파일 열기, 프로세스 생성, 네트워크 소켓 통신 같은 작업은 직접 하드웨어에 접근하지 않고, 시스템 콜을 통해 커널에 요청하여 실행됩니다. 이를 통해 보안성과 안정성이 보장됩니다.
</details>

---

## ⚙️ C++ Q/A

<details>
<summary><strong>1) 포인터와 참조의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>포인터</strong>는 메모리 주소를 저장하는 변수로, null 값을 가질 수 있고 가리키는 대상을 변경할 수 있습니다. <strong>참조</strong>는 변수의 별칭(alias)으로 null을 가질 수 없으며 한 번 초기화되면 다른 대상을 가리킬 수 없습니다. 포인터는 유연하지만 실수 가능성이 많고, 참조는 안전성을 보장하는 대신 제약이 있습니다.
</details>

---

<details>
<summary><strong>2) 얕은 복사와 깊은 복사의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>얕은 복사</strong>는 포인터 값만 복사하여 두 객체가 같은 메모리를 가리키는 방식이라 원본과 복사본이 데이터를 공유합니다. 반면 <strong>깊은 복사</strong>는 포인터가 가리키는 대상까지 새로 할당하고 복제하여 두 객체가 독립적인 데이터를 가지게 합니다. 자원 관리가 필요한 클래스에서는 깊은 복사가 필수적입니다.
</details>

---

<details>
<summary><strong>3) RAII(Resource Acquisition Is Initialization)란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>RAII</strong>는 객체의 생성자에서 자원을 획득하고, 소멸자에서 자원을 해제하는 방식으로 자원을 관리하는 패턴입니다. 이를 통해 예외가 발생하더라도 소멸자가 자동 호출되어 자원이 안전하게 해제됩니다. C++에서 스마트 포인터, 파일 핸들 관리 등에 널리 사용됩니다.
</details>

---

<details>
<summary><strong>4) 스마트 포인터의 종류와 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>unique_ptr</strong>은 단독 소유를 보장하며 복사가 불가능합니다. <strong>shared_ptr</strong>은 참조 횟수를 통해 여러 포인터가 같은 자원을 공유할 수 있게 하고, <strong>weak_ptr</strong>은 shared_ptr의 순환 참조 문제를 방지하기 위해 소유권 없는 참조를 제공합니다. 이들을 적절히 활용하면 메모리 해제를 자동화할 수 있습니다.
</details>

---

<details>
<summary><strong>5) 가상 함수와 순수 가상 함수의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>가상 함수</strong>는 기본 클래스에서 선언되고 파생 클래스에서 재정의할 수 있는 함수입니다. <strong>순수 가상 함수</strong>는 구현이 없는 함수로, 파생 클래스가 반드시 재정의해야 합니다. 따라서 순수 가상 함수가 하나 이상 있으면 그 클래스는 추상 클래스가 됩니다.
</details>

---

<details>
<summary><strong>6) vtable이란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>vtable(virtual table)</strong>은 클래스의 가상 함수 포인터를 저장한 테이블입니다. 객체는 내부적으로 vptr(가상 함수 테이블 포인터)을 가지고 있으며, 이를 통해 런타임에 실제 호출할 함수를 결정합니다. 이렇게 함으로써 다형성을 구현할 수 있습니다.
</details>

---

<details>
<summary><strong>7) Rule of 3/5/0란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Rule of 3</strong>는 소멸자, 복사 생성자, 복사 대입 연산자를 함께 정의해야 한다는 원칙입니다. <strong>Rule of 5</strong>는 여기에 이동 생성자와 이동 대입 연산자까지 포함합니다. <strong>Rule of 0</strong>은 자원 관리를 다른 객체에 맡겨 별도의 정의가 필요 없는 경우를 말합니다. 이 규칙을 따르면 자원 누수를 방지할 수 있습니다.
</details>

---

<details>
<summary><strong>8) 이동 생성자와 복사 생성자의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>복사 생성자</strong>는 객체의 내용을 새로 복제하는 방식으로 동작합니다. 반면 <strong>이동 생성자</strong>는 기존 객체의 자원 소유권을 새로운 객체로 이전하여 불필요한 복사를 줄입니다. 특히 대용량 데이터를 다룰 때 이동语의미는 성능을 크게 개선합니다.
</details>

---

<details>
<summary><strong>9) RVO(Return Value Optimization)란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>RVO</strong>는 함수에서 객체를 반환할 때 불필요한 복사나 이동을 생략하는 컴파일러 최적화 기법입니다. 반환값을 호출자 스택 프레임에 직접 생성하여 임시 객체 생성을 피합니다. 이는 성능 최적화에 매우 유용합니다.
</details>

---

<details>
<summary><strong>10) std::move와 std::forward의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>std::move</strong>는 무조건 객체를 rvalue 참조로 캐스팅하여 이동语의미를 적용합니다. <strong>std::forward</strong>는 전달받은 인자가 lvalue인지 rvalue인지 구분하여 원래 의미를 보존하면서 전달합니다. 이 차이는 템플릿 함수에서 특히 중요합니다.
</details>

---

<details>
<summary><strong>11) const와 constexpr의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>const</strong>는 변수의 값을 변경할 수 없음을 보장하지만, 값은 런타임에 결정될 수도 있습니다. <strong>constexpr</strong>은 반드시 컴파일 타임에 평가될 수 있는 상수 표현식을 요구합니다. 따라서 constexpr은 컴파일러 최적화에 더 적합합니다.
</details>

---

<details>
<summary><strong>12) 템플릿 메타프로그래밍이란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>템플릿 메타프로그래밍</strong>은 템플릿을 활용해 컴파일 타임에 계산을 수행하거나 조건에 따라 코드를 생성하는 기법입니다. 이를 통해 불필요한 런타임 연산을 줄이고 코드 재사용성을 높일 수 있습니다. 예를 들어, 팩토리얼을 컴파일 시점에 계산할 수 있습니다.
</details>

---

<details>
<summary><strong>13) 다이아몬드 상속 문제를 해결하는 방법은 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>다이아몬드 상속</strong>은 두 개의 클래스가 동일한 기본 클래스를 상속하고, 또 다른 클래스가 이 두 클래스를 상속할 때 발생하는 문제입니다. 이 경우 기본 클래스가 두 번 포함되어 모호성이 생깁니다. 이를 해결하기 위해 <strong>가상 상속</strong>을 사용하여 공통 기본 클래스의 인스턴스를 하나만 유지합니다.
</details>

---

<details>
<summary><strong>14) dynamic_cast는 어떻게 동작하나요?</strong></summary>
<strong>A.</strong> <strong>dynamic_cast</strong>는 RTTI(런타임 타입 정보)를 사용해 안전한 다운캐스팅을 수행합니다. 잘못된 타입으로 캐스팅하려 하면 포인터의 경우 nullptr를 반환하고, 참조의 경우 예외를 던집니다. 이를 통해 잘못된 형변환을 방지할 수 있습니다.
</details>

---

<details>
<summary><strong>15) vector와 list의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>vector</strong>는 연속된 메모리에 데이터를 저장해 탐색과 랜덤 액세스가 빠릅니다. 그러나 삽입과 삭제는 비용이 큽니다. <strong>list</strong>는 노드 기반 이중 연결 리스트로 삽입과 삭제가 빠르지만, 랜덤 액세스가 불가능하고 캐시 효율이 떨어집니다.
</details>

---

<details>
<summary><strong>16) unordered_map과 map의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>map</strong>은 Red-Black Tree 기반으로 키가 자동 정렬되며 탐색, 삽입, 삭제가 O(logN)입니다. <strong>unordered_map</strong>은 해시 테이블 기반으로 평균적으로 탐색, 삽입, 삭제가 O(1)입니다. 따라서 정렬이 필요한 경우 map, 빠른 탐색이 필요한 경우 unordered_map을 사용합니다.
</details>

---

<details>
<summary><strong>17) C++에서 예외 처리는 어떻게 동작하나요?</strong></summary>
<strong>A.</strong> <strong>예외 처리</strong>는 throw로 예외를 발생시키고, 스택 언와인딩을 통해 소멸자가 호출되면서 자원이 해제된 후, catch 블록에서 예외를 처리합니다. noexcept로 함수가 예외를 던지지 않음을 명시해 최적화할 수도 있습니다.
</details>

---

<details>
<summary><strong>18) volatile 키워드는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>volatile</strong>은 변수가 외부 요인(하드웨어, 다른 스레드)에 의해 값이 변경될 수 있음을 알리기 위해 사용됩니다. 컴파일러가 해당 변수에 대한 접근을 최적화하지 않도록 하여, 항상 메모리에서 직접 읽도록 보장합니다.
</details>

---

<details>
<summary><strong>19) mutable 키워드는 어떤 용도로 사용되나요?</strong></summary>
<strong>A.</strong> <strong>mutable</strong>은 const 멤버 함수 안에서도 특정 멤버 변수를 수정할 수 있도록 허용합니다. 주로 캐싱, 디버깅 플래그와 같이 논리적으로는 불변이지만 물리적으로 수정이 필요한 경우 사용됩니다.
</details>

---

<details>
<summary><strong>20) interface와 virtual의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>virtual 함수</strong>는 기본 클래스에서 구현을 제공할 수도 있고, 파생 클래스에서 재정의해 다형성을 구현하는 데 사용됩니다. 반면 <strong>interface</strong>는 순수 가상 함수만 모아둔 형태로, 모든 메서드가 구현 없이 선언만 되어 있습니다. 즉, virtual은 선택적 재정의가 가능하지만, interface는 반드시 구현을 강제합니다. 이 차이 덕분에 interface는 설계 계약(Contract) 역할을 하고, virtual은 기본 구현을 공유하면서도 확장을 허용합니다.
</details>

---

<details>
<summary><strong>21) C++20에서 추가된 주요 기능은 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>C++20</strong>에서는 Concepts를 통한 제약 기반 템플릿, Ranges 라이브러리, Coroutines, 삼항 비교 연산자(<=>), Module 시스템 등이 추가되었습니다. 이로 인해 코드의 가독성과 안전성이 크게 향상되었고, 비동기 프로그래밍과 모듈화를 더 쉽게 구현할 수 있게 되었습니다.
</details>

---

## ⚙️ C# Q/A

<details>
<summary><strong>1) struct와 class의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>struct</strong>는 값 타입이라 스택에 주로 저장되고 복사 시 값이 통째로 복사됩니다. <strong>class</strong>는 참조 타입이라 힙에 저장되며, 참조가 전달됩니다. struct는 가볍고 불변 데이터에 적합하지만, class는 상속, 다형성 등 더 풍부한 기능을 지원합니다.
</details>

---

<details>
<summary><strong>2) 값 타입과 참조 타입의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>값 타입</strong>은 데이터를 직접 저장하고 스택에 주로 할당됩니다. <strong>참조 타입</strong>은 힙에 데이터를 저장하고 스택에는 참조만 저장됩니다. 값 타입은 빠르지만 크기가 제한되고, 참조 타입은 유연하지만 GC 관리가 필요합니다.
</details>

---

<details>
<summary><strong>3) C#의 Garbage Collector는 어떻게 동작하나요?</strong></summary>
<strong>A.</strong> .NET의 <strong>GC</strong>는 세대별 수집 방식(Gen0, Gen1, Gen2)을 사용합니다. 짧게 살아남는 객체는 낮은 세대에서 자주 수집되고, 오래 살아남는 객체는 높은 세대로 승격됩니다. 이 과정에서 마크-스윕-컴팩션 단계를 거쳐 불필요한 메모리를 회수합니다.
</details>

---

<details>
<summary><strong>4) IDisposable과 using 키워드의 관계는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>IDisposable</strong>은 객체가 사용한 비관리 자원을 해제하기 위한 Dispose 메서드를 정의하는 인터페이스입니다. <strong>using</strong> 구문은 try-finally 블록으로 변환되어 블록을 벗어날 때 Dispose가 자동 호출되므로 자원 누수를 방지할 수 있습니다.
</details>

---

<details>
<summary><strong>5) 박싱(Boxing)과 언박싱(Unboxing)이란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>박싱</strong>은 값 타입을 object 같은 참조 타입으로 변환하는 과정이고, <strong>언박싱</strong>은 참조 타입을 다시 값 타입으로 변환하는 과정입니다. 박싱과 언박싱은 힙 할당과 캐스팅 비용이 발생하므로 빈번한 경우 성능 문제가 될 수 있습니다.
</details>

---

<details>
<summary><strong>6) delegate와 event의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>delegate</strong>는 함수를 참조할 수 있는 타입으로, 메서드를 변수처럼 다룰 수 있게 합니다. <strong>event</strong>는 delegate를 기반으로 한 특별한 멤버로, 외부에서 직접 호출할 수 없고 구독과 해지만 가능합니다. 이를 통해 이벤트 발행-구독 패턴을 안전하게 구현합니다.
</details>

---

<details>
<summary><strong>7) LINQ의 장점은 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>LINQ</strong>는 데이터를 질의하는 방식을 코드로 통합해 가독성과 유지보수성을 높입니다. SQL과 유사한 문법으로 컬렉션을 처리할 수 있으며, 지연 실행을 통해 성능도 개선할 수 있습니다. 데이터베이스, XML, 객체 컬렉션 등 다양한 소스에 일관된 방식으로 질의할 수 있는 것이 큰 장점입니다.
</details>

---

<details>
<summary><strong>8) async와 await 키워드는 어떻게 동작하나요?</strong></summary>
<strong>A.</strong> <strong>async</strong> 메서드는 비동기 작업을 반환하고, <strong>await</strong>는 해당 작업이 완료될 때까지 비동기적으로 대기합니다. 컴파일러는 상태 머신을 생성해 메서드를 여러 부분으로 나누어 실행합니다. 이를 통해 복잡한 콜백 코드를 단순하게 작성할 수 있습니다.
</details>

---

<details>
<summary><strong>9) 인터페이스와 추상 클래스의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>인터페이스</strong>는 메서드 시그니처만 정의하고, 구현은 전혀 포함하지 않습니다. <strong>추상 클래스</strong>는 일부 구현을 가질 수 있으며, 공통 기능을 제공하면서 특정 메서드는 파생 클래스가 반드시 구현하도록 강제합니다. 다중 상속이 불가능한 C#에서 인터페이스는 다형성을 확장하는 주요 수단입니다.
</details>

---

<details>
<summary><strong>10) dynamic 키워드는 어떤 의미인가요?</strong></summary>
<strong>A.</strong> <strong>dynamic</strong>은 컴파일 타임에 타입 검사를 하지 않고, 런타임에 바인딩을 수행하는 타입입니다. 이는 유연성을 제공하지만, 성능 저하와 런타임 오류 가능성이 있으므로 필요한 경우에만 사용하는 것이 좋습니다.
</details>

---

<details>
<summary><strong>11) Property와 Field의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Field</strong>는 단순히 데이터를 저장하는 변수이고, <strong>Property</strong>는 get/set 접근자를 통해 데이터에 접근하는 방법을 캡슐화합니다. Property는 데이터 접근 제어, 검증, 이벤트 트리거 같은 추가 로직을 포함할 수 있습니다.
</details>

---

<details>
<summary><strong>12) C#에서 다형성은 어떻게 구현되나요?</strong></summary>
<strong>A.</strong> <strong>다형성</strong>은 virtual/override 키워드를 통해 상속 관계에서 동적 디스패치를 구현할 수 있습니다. 또한 인터페이스 구현을 통해 다양한 타입이 동일한 메서드 시그니처를 가질 수 있게 하여 코드의 유연성과 확장성을 높입니다.
</details>

---

<details>
<summary><strong>13) 공변성과 반공변성이란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>공변성</strong>은 제네릭이 반환값을 상위 타입으로 변환할 수 있는 성질이고(out 키워드), <strong>반공변성</strong>은 매개변수를 하위 타입으로 변환할 수 있는 성질입니다(in 키워드). 이를 통해 제네릭 인터페이스를 보다 유연하게 사용할 수 있습니다.
</details>

---

<details>
<summary><strong>14) Nullable 타입이란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Nullable</strong>은 값 타입에도 null 값을 가질 수 있도록 해주는 기능입니다. 예를 들어 int?는 값이 없음을 표현할 수 있습니다. 이를 통해 데이터베이스와의 연동이나 선택적 데이터 처리에 유용하게 활용할 수 있습니다.
</details>

---

<details>
<summary><strong>15) string과 StringBuilder의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>string</strong>은 불변(immutable) 객체라서 값을 변경할 때마다 새로운 객체가 생성됩니다. 반면 <strong>StringBuilder</strong>는 가변(mutable) 버퍼를 사용해 문자열을 수정할 수 있어 반복적인 문자열 연결 작업에서 성능이 우수합니다.
</details>

---

<details>
<summary><strong>16) Indexer란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Indexer</strong>는 클래스나 구조체에 배열처럼 인덱스로 접근할 수 있게 하는 멤버입니다. 예를 들어 myCollection[0] 같은 형태로 사용자 정의 객체에 접근할 수 있습니다. 이는 컬렉션 클래스 구현 시 자주 사용됩니다.
</details>

---

<details>
<summary><strong>17) 이벤트 버블링(Event Bubbling)이란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>이벤트 버블링</strong>은 UI 이벤트가 가장 하위 컨트롤에서 발생한 후 상위 컨트롤로 전파되는 동작을 말합니다. WPF나 UWP 같은 프레임워크에서 지원되며, 이벤트 처리 순서를 유연하게 관리할 수 있습니다.
</details>

---

<details>
<summary><strong>18) Task와 ValueTask의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Task</strong>는 비동기 작업을 나타내며 항상 힙 할당을 발생시킵니다. <strong>ValueTask</strong>는 값 타입 구조체로, 이미 완료된 작업이나 캐시된 결과를 반환할 때 힙 할당을 피할 수 있어 성능이 더 좋습니다. 하지만 남용 시 코드 복잡도가 증가할 수 있습니다.
</details>

---

<details>
<summary><strong>19) Span과 Memory 타입의 특징은 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Span&lt;T&gt;</strong>은 메모리의 연속된 영역을 안전하게 참조할 수 있게 해주며, 힙 할당을 피할 수 있어 성능이 우수합니다. <strong>Memory&lt;T&gt;</strong>는 Span을 힙에도 저장할 수 있도록 확장한 타입입니다. 두 타입 모두 박싱과 불필요한 복사를 줄여 성능 최적화에 기여합니다.
</details>

---

<details>
<summary><strong>20) sync와 async는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>동기(sync)</strong> 방식은 작업이 끝날 때까지 다음 코드 실행이 차단되는 방식입니다. 반면 <strong>비동기(async)</strong> 방식은 작업이 백그라운드에서 진행되고, 완료 시 콜백이나 await를 통해 결과를 받습니다. 예를 들어 파일 읽기를 동기적으로 하면 작업이 끝날 때까지 대기해야 하지만, 비동기로 하면 UI 스레드가 멈추지 않고 다른 작업을 이어갈 수 있습니다.
</details>

---

<details>
<summary><strong>21) C# 9 이후에 추가된 주요 기능은 무엇인가요?</strong></summary>
<strong>A.</strong> C# 9에서는 <strong>record</strong> 타입, <strong>init-only property</strong>, <strong>top-level statements</strong>, <strong>패턴 매칭 확장</strong> 등이 추가되었습니다. 이 기능들은 불변 객체 생성, 간결한 코드 작성, 데이터 중심 프로그래밍을 더 쉽게 만들어 줍니다.
</details>

---

# 🔷 Unity 기술 질문 – 초급 (주니어 1-2년차)

---

### ✅ 기본 개념

<details>
<summary><strong>1) MonoBehaviour의 주요 라이프사이클 메서드들을 실행 순서대로 나열해주세요.</strong></summary>
<strong>A.</strong> Unity의 MonoBehaviour는 게임 오브젝트의 동작 단위를 정의하는 핵심 클래스입니다. 실행 순서는 Awake → OnEnable → Start → Update → LateUpdate → OnDisable → OnDestroy 순서로 진행됩니다. Awake는 객체가 생성될 때, Start는 첫 프레임 직전에, Update는 매 프레임마다, LateUpdate는 Update 이후에 호출되며, OnDisable과 OnDestroy는 오브젝트가 비활성화되거나 파괴될 때 실행됩니다.
</details>

---

<details>
<summary><strong>2) GameObject와 Component의 관계를 설명하고, 왜 이런 구조를 사용하는지 말씀해주세요.</strong></summary>
<strong>A.</strong> Unity에서 GameObject는 단순한 컨테이너이고, 실제 동작은 Component가 담당합니다. 예를 들어 Transform, Renderer, Collider 같은 기능은 모두 Component입니다. 이 구조는 컴포지션 기반 아키텍처를 제공해, 상속보다 유연하게 기능을 조합할 수 있는 장점이 있습니다.
</details>

---

<details>
<summary><strong>3) Prefab과 Instance의 차이점과 각각의 활용 사례를 설명해주세요.</strong></summary>
<strong>A.</strong> Prefab은 재사용 가능한 오브젝트 원형이고, Instance는 그 원형으로부터 생성된 복제본입니다. Prefab을 활용하면 UI 버튼, 몬스터, 아이템 등 반복 사용되는 자산을 관리하기 편리하며, Instance를 씬에서 배치해 수정할 수 있습니다. Prefab 변경은 모든 Instance에 반영되어 생산성이 높아집니다.
</details>

---

<details>
<summary><strong>4) Transform 컴포넌트의 position, localPosition, rotation의 차이점은 무엇인가요?</strong></summary>
<strong>A.</strong> position은 월드 좌표계에서의 절대 위치를 의미하고, localPosition은 부모를 기준으로 한 상대 위치입니다. rotation은 오브젝트의 월드 기준 회전값이고, localRotation은 부모 기준의 회전값입니다. 계층 구조가 깊을수록 local 단위가 유용합니다.
</details>

---

<details>
<summary><strong>5) Update, FixedUpdate, LateUpdate의 차이점과 각각 언제 사용해야 하나요?</strong></summary>
<strong>A.</strong> Update는 매 프레임 호출되며 일반적인 게임 로직에 적합합니다. FixedUpdate는 물리 엔진과 동기화된 고정 시간 간격으로 실행되므로 Rigidbody 기반 물리 연산에 사용합니다. LateUpdate는 Update 이후 실행되어 카메라 추적이나 애니메이션 후처리처럼 다른 업데이트 결과를 참조할 때 활용됩니다.
</details>

---

### ✅ 기본 시스템

<details>
<summary><strong>1) Unity의 씬 관리 시스템과 씬 전환 방법에 대해 설명해주세요.</strong></summary>
<strong>A.</strong> Unity는 Scene을 통해 게임 환경을 분리 관리합니다. 전환은 SceneManager.LoadScene() 또는 LoadSceneAsync()로 수행하며, Additive 모드를 사용하면 여러 씬을 동시에 로드해 멀티 씬 구조를 만들 수 있습니다. 이를 통해 로딩 화면, UI, 맵을 분리 관리할 수 있습니다.
</details>

---

<details>
<summary><strong>2) 태그(Tag)와 레이어(Layer)의 차이점과 활용 방법을 설명해주세요.</strong></summary>
<strong>A.</strong> Tag는 오브젝트 식별을 위해 사용되며 주로 GameObject.FindWithTag 등 검색에 쓰입니다. Layer는 충돌 감지, 카메라 Culling Mask 등 렌더링/물리 처리 최적화에 활용됩니다. 즉, Tag는 논리적 분류, Layer는 물리적/렌더링 제어 용도라고 구분할 수 있습니다.
</details>

---

<details>
<summary><strong>3) Rigidbody와 Collider를 이용한 물리 시뮬레이션의 기본 원리를 설명해주세요.</strong></summary>
<strong>A.</strong> Rigidbody는 질량, 힘, 속도 같은 물리 속성을 적용해 오브젝트를 Unity 물리 엔진에서 시뮬레이션하도록 합니다. Collider는 충돌 영역을 정의해 다른 오브젝트와 상호작용을 가능하게 합니다. Rigidbody와 Collider가 함께 있어야 물리적으로 충돌이 감지되고 반응할 수 있습니다.
</details>

---

<details>
<summary><strong>4) Unity의 좌표계(월드, 로컬, 스크린, 뷰포트)에 대해 설명해주세요.</strong></summary>
<strong>A.</strong> 월드 좌표는 전역 공간 기준이고, 로컬 좌표는 부모 오브젝트 기준입니다. 스크린 좌표는 모니터 픽셀 좌표로 (0,0)은 좌하단입니다. 뷰포트 좌표는 카메라 시야를 (0~1) 범위로 정규화한 좌표로, 해상도와 무관하게 상대적 위치를 표현합니다.
</details>

---

<details>
<summary><strong>5) Inspector에서 public 변수와 [SerializeField] 속성의 차이점은 무엇인가요?</strong></summary>
<strong>A.</strong> public 변수는 코드와 Inspector 양쪽에서 접근 가능하지만, 캡슐화가 약해집니다. [SerializeField]는 private 변수도 Inspector에 노출할 수 있어, 데이터 드리븐 설계는 가능하면서 외부 접근은 막아 안정성을 유지할 수 있습니다.
</details>

---

## 🎮 Unity Q/A

<details>
<summary><strong>1) MonoBehaviour의 주요 라이프사이클 메서드 실행 순서를 설명해주세요.</strong></summary>
<strong>A.</strong> Unity의 라이프사이클은 <strong>Awake → OnEnable → Start → Update → LateUpdate → OnDisable → OnDestroy</strong> 순서로 진행됩니다. Awake는 스크립트 인스턴스가 로드될 때, Start는 첫 프레임 전에, Update는 매 프레임마다, LateUpdate는 Update 이후에 호출됩니다. OnDisable과 OnDestroy는 비활성화 및 파괴 시점에 각각 실행됩니다.
</details>

---

<details>
<summary><strong>2) GameObject와 Component의 관계는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>GameObject</strong>는 컨테이너이고, <strong>Component</strong>는 실제 기능을 담당하는 모듈입니다. Transform, Renderer, Collider 같은 컴포넌트를 붙여 기능을 확장합니다. Unity의 조립식 아키텍처는 이러한 구성 덕분에 재사용성과 확장성이 뛰어납니다.
</details>

---

<details>
<summary><strong>3) Prefab과 Instance의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Prefab</strong>은 재사용 가능한 오브젝트의 원본 데이터이고, <strong>Instance</strong>는 게임 씬에 배치된 복제본입니다. Prefab을 수정하면 모든 인스턴스에 변경 사항을 일괄 적용할 수 있습니다. 이를 통해 대규모 씬 관리와 반복 제작이 편리해집니다.
</details>

---

<details>
<summary><strong>4) Update, FixedUpdate, LateUpdate의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Update</strong>는 매 프레임 호출되며 주로 입력 처리에 사용됩니다. <strong>FixedUpdate</strong>는 물리 연산 주기에 맞춰 호출되어 Rigidbody 관련 연산을 안정적으로 처리합니다. <strong>LateUpdate</strong>는 Update가 끝난 뒤 호출되어 카메라 추적 같은 후처리에 적합합니다.
</details>

---

<details>
<summary><strong>5) Rigidbody와 Collider의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Rigidbody</strong>는 물리 연산을 적용받는 컴포넌트이고, <strong>Collider</strong>는 충돌 영역을 정의하는 컴포넌트입니다. Rigidbody 없이 Collider만 있으면 충돌 판정은 가능하지만 물리 연산은 적용되지 않습니다. 두 컴포넌트를 함께 사용하면 물리 기반 움직임과 충돌 감지가 가능합니다.
</details>

---

<details>
<summary><strong>6) Unity의 좌표계는 어떻게 구성되나요?</strong></summary>
<strong>A.</strong> Unity는 <strong>왼손 좌표계</strong>를 사용하며, 기본 단위는 미터입니다. <strong>World 좌표계</strong>는 전역 기준, <strong>Local 좌표계</strong>는 부모를 기준, <strong>Screen 좌표계</strong>는 픽셀 단위, <strong>Viewport 좌표계</strong>는 0~1 정규화된 화면 공간으로 표현됩니다.
</details>

---

<details>
<summary><strong>7) Inspector에서 public 변수와 [SerializeField] 변수의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>public 변수</strong>는 코드와 Inspector 모두에서 접근 가능합니다. <strong>[SerializeField]</strong>는 변수는 private이지만 Inspector에 노출됩니다. 즉, 캡슐화는 유지하면서도 디자이너가 값을 조정할 수 있습니다.
</details>

---

<details>
<summary><strong>8) Unity에서 GC(Garbage Collection)는 언제 발생하나요?</strong></summary>
<strong>A.</strong> Unity의 <strong>GC</strong>는 관리 힙에서 더 이상 참조되지 않는 객체가 쌓일 때 발생합니다. 주로 문자열 연결, 박싱, new 연산 등에서 힙 할당이 많아지면 GC가 자주 호출됩니다. GC는 모든 실행을 멈추기 때문에 성능 저하가 발생할 수 있어, 풀링 기법이나 구조체 활용으로 할당을 줄이는 것이 중요합니다.
</details>

---

<details>
<summary><strong>9) Object Pooling이 필요한 이유는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Object Pooling</strong>은 오브젝트를 매번 생성·파괴하지 않고 재사용하는 방식입니다. 이를 통해 메모리 할당/해제 비용과 GC 빈도를 줄이고, 특히 총알, 적 스폰 같은 반복적 생성/삭제가 많은 게임에서 성능을 크게 향상시킵니다.
</details>

---

<details>
<summary><strong>10) Draw Call이란 무엇이며 줄이는 방법은 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Draw Call</strong>은 CPU가 GPU에게 렌더링 명령을 내리는 호출입니다. Draw Call이 많아지면 CPU 병목이 발생합니다. 줄이는 방법으로는 <strong>메시 합치기</strong>, <strong>텍스처 아틀라스</strong>, <strong>GPU Instancing</strong>, <strong>UI Batching</strong> 등이 있습니다.
</details>

---

<details>
<summary><strong>11) Coroutine과 async/await의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Coroutine</strong>은 Unity 엔진 프레임워크 안에서 실행되는 비동기 흐름 제어로, 주로 시간 지연이나 프레임 단위 작업에 적합합니다. 반면 <strong>async/await</strong>는 C# 언어 레벨의 비동기 프로그래밍으로, IO 작업이나 외부 API 호출 같은 진짜 비동기 처리에 적합합니다.
</details>

---

<details>
<summary><strong>12) Animator Controller와 Blend Tree의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Animator Controller</strong>는 애니메이션 상태를 전환하는 상태 기계이고, <strong>Blend Tree</strong>는 파라미터 값에 따라 여러 애니메이션을 부드럽게 혼합하는 도구입니다. 예를 들어, 이동 속도 값에 따라 걷기와 달리기를 자연스럽게 전환할 수 있습니다.
</details>

---

<details>
<summary><strong>13) Addressable Asset System의 장점은 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Addressable</strong>은 에셋을 주소 기반으로 관리하여 메모리 로딩과 언로드를 효율적으로 제어할 수 있습니다. 특히 큰 프로젝트에서 빌드 크기를 줄이고, 필요할 때만 에셋을 다운로드하거나 메모리에 올릴 수 있어 메모리 효율이 좋아집니다.
</details>

---

<details>
<summary><strong>14) ScriptableObject는 언제 사용하나요?</strong></summary>
<strong>A.</strong> <strong>ScriptableObject</strong>는 데이터 중심 설계에 유용한 객체로, 인스턴스 없이도 에셋 형태로 저장됩니다. 게임 밸런스 데이터, 설정 값, 공유 리소스를 관리할 때 자주 사용됩니다. 이를 활용하면 메모리 효율이 좋아지고 인스펙터 기반 워크플로우도 편리해집니다.
</details>

---

<details>
<summary><strong>15) Unity Profiler로 어떤 것을 확인하나요?</strong></summary>
<strong>A.</strong> <strong>Unity Profiler</strong>는 CPU/GPU 사용량, 메모리, 렌더링, 오디오, GC 활동 등을 시각화해줍니다. 이를 통해 성능 병목 지점을 찾아 최적화 전략을 세울 수 있습니다. 예를 들어, Update가 과도하게 호출되는 스크립트를 찾거나 Draw Call을 줄일 대상을 확인할 수 있습니다.
</details>

---

<details>
<summary><strong>16) ECS와 DOTS의 장점은 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>ECS(Entity Component System)</strong>는 데이터 중심 설계로 캐시 효율을 높이고 대량 객체를 효율적으로 관리합니다. <strong>DOTS</strong>는 이를 지원하는 Unity 프레임워크로, Job System과 Burst Compiler를 활용해 멀티코어 성능을 극대화할 수 있습니다.
</details>

---

<details>
<summary><strong>17) Canvas의 Render Mode 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Screen Space - Overlay</strong>는 UI를 카메라와 무관하게 그립니다. <strong>Screen Space - Camera</strong>는 카메라 기반으로 UI를 그려서 카메라 효과를 받을 수 있습니다. <strong>World Space</strong>는 UI를 3D 오브젝트처럼 배치할 수 있습니다. 성능과 연출 목적에 따라 선택이 달라집니다.
</details>

---

<details>
<summary><strong>18) 멀티스레딩 시 Unity API를 호출하면 어떤 문제가 발생하나요?</strong></summary>
<strong>A.</strong> Unity의 대부분 API는 메인 스레드에서만 안전하게 호출할 수 있습니다. 멀티스레드에서 호출하면 예기치 못한 크래시가 발생합니다. 따라서 별도의 스레드에서는 데이터만 처리하고, 결과를 메인 스레드에서 반영해야 합니다.
</details>

---

<details>
<summary><strong>19) AssetBundle과 Addressable의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>AssetBundle</strong>은 직접 번들 빌드와 관리가 필요한 방식이고, <strong>Addressable</strong>은 AssetBundle을 추상화하여 더 편리한 API와 자동 메모리 관리, 레퍼런스 추적을 제공합니다. Addressable은 사실상 AssetBundle의 발전형이라 할 수 있습니다.
</details>

---

<details>
<summary><strong>20) 모바일 게임 최적화에서 특별히 고려해야 할 점은 무엇인가요?</strong></summary>
<strong>A.</strong> 모바일은 성능과 메모리 제한이 심하기 때문에 <strong>Draw Call 최소화</strong>, <strong>텍스처 압축</strong>, <strong>밧데리 소모 줄이는 최적화</strong>, <strong>네트워크 최적화</strong>가 중요합니다. 또한 다양한 기기 해상도와 노치 영역을 고려한 UI 설계도 필수적입니다.
</details>

---

<details>
<summary><strong>21) Unity와 Unreal에서 최적화를 위해 사용한 것은 무엇이 있나요?</strong></summary>
<strong>A.</strong> Unity에서는 <strong>Object Pooling, Batching, Profiler 분석, Addressable Asset System</strong>을 통해 메모리와 CPU 부하를 줄였습니다. Unreal에서는 <strong>LOD, Occlusion Culling, World Partition, GAS 네트워크 최적화</strong> 같은 기능을 활용했습니다. 공통적으로 <strong>Draw Call 최소화, GC 관리, 멀티스레딩 활용</strong>을 통해 성능을 확보했습니다.
</details>

---

<details>
<summary><strong>22) 언리얼 GC와 유니티 GC는 무엇이 다른가요?</strong></summary>
<strong>A.</strong> <strong>Unity GC</strong>는 .NET 기반의 관리 힙을 사용하는 세대별 GC(Generational GC + Mark-Sweep Algorithm)로, C# 객체를 추적해 더 이상 참조되지 않는 객체를 수집합니다. 반면 <strong>Unreal GC</strong>는 UPROPERTY 매크로로 표시된 UObject 참조만 추적하는 마크-스윕(마크스윕만 사용! Generational 개념은 없다) 방식입니다. Unity는 .NET 런타임 규칙을 따르고, Unreal은 자체 메모리 관리 시스템을 가지며, UObject 외의 자원은 스마트 포인터나 RAII로 관리해야 한다는 점이 큰 차이입니다.
Unity에 경우, 이를 확장하기 위해 점진적 가비지 컬렉션(Incremental GC)를 사용하기도 합니다.
Incremental GC는 2019년에 도입된 것으로, GC Spike(Stop the world 문제)를 해결하고자, GC 작업을 여러 프레임에 나눠서 조금씩 수행하도록 하여, GC 한번 돌때 100ms 멈춤 같은 현상을 여러 프레임에 걸쳐 1~2ms씩 처리로 분산된 방식입니다.
</details>

---

## 🎮 Unreal Engine Q/A

<details>
<summary><strong>1) Blueprint와 C++의 차이점은 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Blueprint</strong>는 시각적 스크립팅으로 접근성이 높고 프로토타이핑이 빠릅니다. 반면 <strong>C++</strong>은 성능과 세밀한 제어가 가능하며 대규모 프로젝트에서 유지보수성이 뛰어납니다. 보통 빠른 기능 구현은 Blueprint로, 성능 최적화나 핵심 로직은 C++로 작성하는 식으로 병행합니다.
</details>

---

<details>
<summary><strong>2) Actor, Pawn, Character 클래스의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Actor</strong>는 UE의 가장 기본 단위로 모든 오브젝트의 기반 클래스입니다. <strong>Pawn</strong>은 플레이어나 AI가 조종할 수 있는 Actor이고, <strong>Character</strong>는 Pawn에 이동 컴포넌트와 캡슐, 메시, 애니메이션 시스템이 붙은 특수화된 형태입니다. 즉, Actor → Pawn → Character 순으로 기능이 확장됩니다.
</details>

---

<details>
<summary><strong>3) GameMode, GameState, PlayerState의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>GameMode</strong>는 게임 규칙을 정의하고 서버에서만 존재합니다. <strong>GameState</strong>는 게임의 전반적인 상태를 저장하고 클라이언트와 공유됩니다. <strong>PlayerState</strong>는 각 플레이어의 점수, 팀, 상태를 관리하며 역시 클라이언트와 동기화됩니다. 이렇게 역할이 분리되어 있어 멀티플레이에서 일관성을 유지할 수 있습니다.
</details>

---

<details>
<summary><strong>4) UPROPERTY 매크로는 왜 중요한가요?</strong></summary>
<strong>A.</strong> <strong>UPROPERTY</strong>는 가비지 컬렉션, 직렬화, 네트워크 복제(Replication), 에디터 노출 등을 지원하기 위해 사용됩니다. 단순 포인터는 GC가 추적하지 않기 때문에 반드시 UPROPERTY로 관리해야 안전합니다. 이를 통해 메모리 누수나 크래시를 방지할 수 있습니다.
</details>

---

<details>
<summary><strong>5) UE의 Garbage Collection 방식은 무엇인가요?</strong></summary>
<strong>A.</strong> UE의 <strong>GC</strong>는 마크-스윕 방식으로 동작합니다. UPROPERTY로 참조되는 UObject는 루트에서부터 추적되어 마크되고, 참조가 끊긴 객체는 스윕 단계에서 해제됩니다. 따라서 GC 관리 하에 두려면 반드시 UPROPERTY로 선언해야 합니다.
</details>

---

<details>
<summary><strong>6) Smart Pointer(TSharedPtr, TWeakPtr, TUniquePtr)의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>TSharedPtr</strong>은 참조 카운팅 기반 공유 포인터이고, <strong>TWeakPtr</strong>은 순환 참조 방지를 위해 소유권 없는 참조를 제공합니다. <strong>TUniquePtr</strong>은 단독 소유권을 보장해 RAII 방식으로 자원을 안전하게 관리합니다. UE의 non-UObject 타입 관리에 주로 사용됩니다.
</details>

---

<details>
<summary><strong>7) Level Streaming과 World Partition의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Level Streaming</strong>은 수동으로 서브 레벨을 불러오거나 언로드하는 방식이고, <strong>World Partition</strong>은 UE5에서 도입된 시스템으로 월드를 자동으로 셀 단위로 나누어 스트리밍합니다. World Partition은 오픈 월드 제작에 최적화되어 있습니다.
</details>

---

<details>
<summary><strong>8) Replication이란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Replication</strong>은 서버에서 클라이언트로 속성이나 함수 호출을 동기화하는 시스템입니다. 변수에 UPROPERTY(Replicated)를 붙이면 서버 값이 자동으로 클라이언트에 전달됩니다. 또한 RPC를 통해 클라이언트-서버 간 메서드 호출을 수행할 수도 있습니다.
</details>

---

<details>
<summary><strong>9) RPC의 종류에는 어떤 것이 있나요?</strong></summary>
<strong>A.</strong> UE에는 <strong>Server RPC</strong>, <strong>Client RPC</strong>, <strong>Multicast RPC</strong>가 있습니다. Server는 클라이언트에서 서버로, Client는 서버에서 특정 클라이언트로, Multicast는 서버에서 모든 클라이언트로 호출됩니다. 올바른 RPC 사용은 멀티플레이에서 핵심입니다.
</details>

---

<details>
<summary><strong>10) Client Prediction이란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Client Prediction</strong>은 네트워크 지연을 보완하기 위해 클라이언트가 입력을 즉시 반영해 결과를 예측하는 기술입니다. 이후 서버의 authoritative 결과와 비교해 차이가 있으면 보정(Server Reconciliation)합니다. 이를 통해 멀티플레이에서도 즉각적인 반응성을 제공합니다.
</details>

---

<details>
<summary><strong>11) Network Relevancy란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Network Relevancy</strong>는 플레이어에게 필요한 정보만 전송하는 개념입니다. 예를 들어, 멀리 떨어진 액터의 상태는 네트워크로 전송하지 않아 대역폭을 절약할 수 있습니다. 이는 대규모 멀티플레이에서 필수적인 최적화 기법입니다.
</details>

---

<details>
<summary><strong>12) Dedicated Server와 Listen Server의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Dedicated Server</strong>는 서버 전용 프로세스가 실행되는 방식으로 공정성과 안정성이 뛰어납니다. <strong>Listen Server</strong>는 호스트 클라이언트가 서버 역할도 동시에 하는 방식으로 설정은 간단하지만 호스트가 나가면 게임이 종료됩니다. 상용 멀티플레이 게임에서는 대부분 Dedicated Server를 사용합니다.
</details>

---

<details>
<summary><strong>13) Animation Blueprint의 역할은 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Animation Blueprint</strong>는 캐릭터 애니메이션의 상태 전환(State Machine)과 블렌딩(Blend Space)을 정의하는 자산입니다. 이를 통해 이동 속도, 방향, 상태 값에 따라 자연스럽게 애니메이션을 전환할 수 있습니다.
</details>

---

<details>
<summary><strong>14) GAS(Gameplay Ability System)란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>GAS</strong>는 UE의 확장 가능한 게임플레이 프레임워크입니다. Ability, AttributeSet, GameplayEffect, GameplayTag 같은 요소로 구성되며, 상태이상, 버프, 쿨타임 같은 복잡한 게임플레이 로직을 네트워크 동기화까지 포함해 구조적으로 구현할 수 있습니다.
</details>

---

<details>
<summary><strong>15) GameplayTag는 언제 사용되나요?</strong></summary>
<strong>A.</strong> <strong>GameplayTag</strong>는 문자열 기반의 키워드 시스템으로, 조건 분기, 효과 적용, 필터링 등에 활용됩니다. 예를 들어 "State.Stunned" 같은 태그를 부여해 스턴 상태를 관리할 수 있습니다. Enum보다 유연하며 확장성도 뛰어납니다.
</details>

---

<details>
<summary><strong>16) Subsystem의 종류는 무엇인가요?</strong></summary>
<strong>A.</strong> UE에는 <strong>EngineSubsystem</strong>, <strong>EditorSubsystem</strong>, <strong>GameInstanceSubsystem</strong>, <strong>WorldSubsystem</strong> 등이 있습니다. Engine 전역, 에디터, 게임 인스턴스, 월드 단위로 각각의 라이프사이클에 맞게 전역 기능을 구현할 수 있습니다.
</details>

---

<details>
<summary><strong>17) Lumen과 Nanite의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Lumen</strong>은 UE5의 실시간 글로벌 일루미네이션 및 반사 시스템이고, <strong>Nanite</strong>는 가상화 지오메트리 기술로 수억 개의 폴리곤을 효율적으로 처리합니다. Lumen은 사실적인 조명, Nanite는 대규모 고품질 메시 렌더링에 강점이 있습니다.
</details>

---

<details>
<summary><strong>18) UE의 Build 과정과 Cook 과정은 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Build</strong>는 C++ 코드와 셰이더 등을 컴파일하는 과정이고, <strong>Cook</strong>은 게임 콘텐츠를 패키징하기 전에 플랫폼별 최적화된 포맷으로 변환하는 과정입니다. Cook 과정을 통해 리소스 로딩 속도를 개선할 수 있습니다.
</details>

---

<details>
<summary><strong>19) Slate와 UMG의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Slate</strong>는 C++로 작성된 저수준 UI 프레임워크이고, <strong>UMG</strong>는 Slate를 기반으로 한 시각적 UI 제작 툴입니다. Slate는 성능과 커스터마이징에 강점이 있고, UMG는 접근성이 뛰어나 디자이너와 협업하기 좋습니다.
</details>

---

<details>
<summary><strong>20) UE5의 멀티스레딩 지원 방식은 무엇인가요?</strong></summary>
<strong>A.</strong> UE는 <strong>TaskGraph</strong> 시스템으로 멀티스레딩을 관리합니다. 렌더링 스레드, 게임 스레드, RHI 스레드 등이 병렬로 동작하며, 비동기 로딩이나 경량 연산은 별도 Task로 분리할 수 있습니다. 이를 통해 멀티코어 CPU 성능을 극대화할 수 있습니다.
</details>

---

## 🌐 네트워크 Q/A

<details>
<summary><strong>1) TCP와 UDP의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>TCP</strong>는 연결 지향적이며, 신뢰성과 순서를 보장하는 프로토콜입니다. 패킷 손실 시 재전송을 통해 보장하지만 속도가 느릴 수 있습니다. <strong>UDP</strong>는 비연결형으로 빠르고 가벼우며 순서를 보장하지 않습니다. 게임에서는 중요한 상태 동기화에는 TCP, 빠른 움직임 업데이트에는 UDP를 혼합해 사용합니다.
</details>

---

<details>
<summary><strong>2) 3-way Handshake란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>3-way Handshake</strong>는 TCP 연결을 수립하는 과정입니다. 클라이언트가 SYN을 보내고, 서버가 SYN+ACK로 응답한 뒤, 클라이언트가 ACK를 보내면서 연결이 확립됩니다. 이를 통해 양측이 통신 준비가 되었음을 확인합니다.
</details>

---

<details>
<summary><strong>3) 네트워크 지연(Latency)의 원인은 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Latency</strong>는 주로 물리적 거리, 라우팅 지연, 네트워크 혼잡, 서버 처리 속도 때문에 발생합니다. 게임에서는 예측(Prediction), 보정(Reconciliation), 지연 보상(Lag Compensation) 같은 기법을 통해 체감 지연을 줄입니다.
</details>

---

<details>
<summary><strong>4) NAT(Network Address Translation)이란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>NAT</strong>는 내부 네트워크의 사설 IP를 공인 IP로 변환하는 기술입니다. IPv4 주소 부족 문제를 해결했지만, P2P 통신에서는 NAT Traversal 문제가 생기기도 합니다. 게임에서는 주로 STUN, TURN, Relay 서버를 이용해 이를 해결합니다.
</details>

---

<details>
<summary><strong>5) HTTP와 WebSocket의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>HTTP</strong>는 요청-응답 기반으로 클라이언트가 요청해야만 서버가 응답합니다. 반면 <strong>WebSocket</strong>은 양방향 통신을 지원하며 연결을 유지한 상태에서 실시간 데이터를 주고받을 수 있습니다. 채팅이나 실시간 멀티플레이에 WebSocket이 적합합니다.
</details>

---

<details>
<summary><strong>6) 패킷 구조는 어떻게 되어 있나요?</strong></summary>
<strong>A.</strong> 일반적으로 <strong>패킷</strong>은 헤더와 페이로드로 구성됩니다. 헤더에는 송수신 IP, 포트, 프로토콜 정보가 포함되고, 페이로드에는 실제 데이터가 담깁니다. 게임 네트워크에서는 패킷 크기를 줄이기 위해 헤더를 최소화하고 바이너리 직렬화를 자주 사용합니다.
</details>

---

<details>
<summary><strong>7) 클라이언트-서버 모델과 P2P 모델의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>클라이언트-서버 모델</strong>은 중앙 서버가 상태를 관리해 공정성과 보안이 높습니다. 반면 <strong>P2P 모델</strong>은 지연이 적고 서버 비용이 들지 않지만, 보안과 동기화 문제가 발생할 수 있습니다. 대규모 게임은 보통 클라이언트-서버 모델을 채택합니다.
</details>

---

<details>
<summary><strong>8) 로드 밸런싱이란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>로드 밸런싱</strong>은 트래픽을 여러 서버로 분산시켜 부하를 줄이는 기술입니다. L4 스위치는 네트워크 계층(IP, 포트), L7 스위치는 애플리케이션 계층(URL, 쿠키) 기준으로 분산합니다. 게임 서버는 보통 지역별, 세션별 로드 밸런싱을 적용합니다.
</details>

---

<details>
<summary><strong>9) QoS(Quality of Service)는 게임 네트워크에서 왜 중요한가요?</strong></summary>
<strong>A.</strong> <strong>QoS</strong>는 네트워크 자원의 우선순위를 지정해 지연과 손실을 최소화하는 기술입니다. 게임에서는 실시간성이 중요한 패킷에 높은 우선순위를 주어 끊김 없는 플레이를 보장합니다.
</details>

---

<details>
<summary><strong>10) 게임에서 보안이 중요한 이유는 무엇인가요?</strong></summary>
<strong>A.</strong> 게임은 클라이언트가 악의적으로 변조될 수 있기 때문에 <strong>보안</strong>이 매우 중요합니다. 서버 검증, 암호화, 안티치트 솔루션을 통해 치팅을 방지해야 합니다. 이를 소홀히 하면 공정성이 무너지고 서비스 신뢰도가 크게 하락합니다.
</details>

---

## 🗄️ 데이터베이스 Q/A

<details>
<summary><strong>1) RDBMS와 NoSQL의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>RDBMS</strong>는 스키마 기반 관계형 데이터베이스로, SQL을 사용해 정형 데이터를 관리합니다. <strong>NoSQL</strong>은 비정형 데이터나 대규모 분산 처리에 적합하며, 문서, 키-값, 그래프 등 다양한 모델을 지원합니다. 게임에서는 계정·거래 로그는 RDBMS, 실시간 세션 데이터는 NoSQL을 혼합해 사용합니다.
</details>

---

<details>
<summary><strong>2) 정규화와 비정규화의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>정규화</strong>는 데이터 중복을 최소화하고 무결성을 유지하기 위한 구조화 과정입니다. <strong>비정규화</strong>는 성능 향상을 위해 일부러 중복을 허용하는 것입니다. 게임에서는 읽기 성능을 위해 비정규화된 테이블을 캐싱용으로 두기도 합니다.
</details>

---

<details>
<summary><strong>3) 인덱스(Index)의 장점과 단점은 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>인덱스</strong>는 검색 속도를 빠르게 하지만, 추가·삭제·갱신 시 인덱스도 갱신해야 하므로 쓰기 성능이 저하됩니다. 따라서 읽기 비중이 높은 게임 로그 조회에는 유리하지만, 자주 변경되는 세션 테이블에는 주의가 필요합니다.
</details>

---

<details>
<summary><strong>4) 트랜잭션의 ACID 특성은 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>ACID</strong>는 원자성, 일관성, 격리성, 지속성을 의미합니다. 트랜잭션은 반드시 전부 수행되거나 전혀 수행되지 않아야 하고, 무결성을 유지하며, 동시에 실행되어도 독립적이어야 하고, 완료된 후에는 영구적으로 반영되어야 합니다.
</details>

---

<details>
<summary><strong>5) 데이터베이스 샤딩(Sharding)이란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>샤딩</strong>은 대규모 데이터를 여러 DB 인스턴스로 분할해 저장하는 방법입니다. 게임에서는 유저 ID 범위별 샤딩이나 지역별 샤딩을 자주 사용합니다. 이를 통해 성능과 확장성을 확보할 수 있습니다.
</details>

---

<details>
<summary><strong>6) 캐시(Cache)는 왜 필요한가요?</strong></summary>
<strong>A.</strong> <strong>캐시</strong>는 자주 사용하는 데이터를 메모리에 저장해 DB 부하를 줄이고 응답 속도를 높입니다. Redis나 Memcached 같은 인메모리 캐시를 많이 사용하며, 게임에서는 랭킹, 세션 데이터, 자주 읽는 설정값을 캐싱합니다.
</details>

---

<details>
<summary><strong>7) ORM(Object Relational Mapping)이란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>ORM</strong>은 객체와 데이터베이스의 매핑을 자동으로 관리해주는 기술입니다. C#의 Entity Framework, Java의 Hibernate 같은 도구가 대표적입니다. 이를 통해 SQL을 직접 작성하지 않고 객체 지향적으로 데이터에 접근할 수 있습니다.
</details>

---

<details>
<summary><strong>8) Deadlock은 무엇이고 어떻게 방지하나요?</strong></summary>
<strong>A.</strong> <strong>Deadlock</strong>은 두 개 이상의 트랜잭션이 서로 자원을 기다리면서 무한 대기 상태에 빠지는 상황입니다. 이를 방지하기 위해 락 획득 순서를 일관되게 유지하거나, 타임아웃을 설정하거나, 격리 수준을 조정하는 방법을 사용합니다.
</details>

---

<details>
<summary><strong>9) Partitioning과 Sharding의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Partitioning</strong>은 하나의 DB 내에서 테이블을 물리적으로 분할하는 것이고, <strong>Sharding</strong>은 여러 DB 인스턴스에 데이터를 분산하는 것입니다. Partitioning은 관리 편의성이, Sharding은 확장성이 장점입니다.
</details>

---

<details>
<summary><strong>10) 게임 로그 데이터는 어떻게 저장하고 분석하나요?</strong></summary>
<strong>A.</strong> 게임 로그 데이터는 보통 <strong>NoSQL</strong>이나 <strong>빅데이터 파이프라인</strong>을 통해 저장하고, Elasticsearch, Hadoop, Spark 같은 시스템으로 분석합니다. 이를 통해 유저 행동 분석, 밸런스 조정, 치팅 탐지를 수행할 수 있습니다.
</details>

---

## 🎨 그래픽스 Q/A

<details>
<summary><strong>1) 게임에서 그래픽스 과정은 어떻게 이루어지나요?</strong></summary>
<strong>A.</strong> 게임 그래픽스는 일반적으로 <strong>Application 단계 → Geometry 단계 → Rasterization 단계 → Pixel 단계</strong> 순으로 처리됩니다.  
애플리케이션 단계에서는 게임 로직과 카메라 좌표 계산이 이루어지고, Geometry 단계에서는 정점 변환과 조명 계산이 수행됩니다. 이후 Rasterization 단계에서 3D 공간의 삼각형이 2D 픽셀로 변환되며, Pixel 단계에서 텍스처, 라이팅, 쉐이딩이 적용됩니다. 최종적으로 프레임 버퍼에 그려진 이미지가 화면에 출력됩니다.
</details>

---

<details>
<summary><strong>2) GPU와 CPU의 역할 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>CPU</strong>는 게임 로직, 물리 연산, AI 같은 범용 작업을 처리하고, <strong>GPU</strong>는 정점 변환, 픽셀 연산처럼 병렬화 가능한 그래픽스 연산을 처리합니다. CPU는 적은 스레드에서 강력한 연산을 수행하고, GPU는 수천 개의 코어를 활용해 대량의 연산을 동시에 처리하는 것이 차이입니다.
</details>

---

<details>
<summary><strong>3) 렌더링 파이프라인이란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>렌더링 파이프라인</strong>은 그래픽스 데이터가 화면에 출력되기까지의 일련의 단계입니다. 대표적으로 입력 조립(Input Assembly) → 정점 셰이더(Vertex Shader) → 래스터라이저(Rasterizer) → 픽셀 셰이더(Pixel Shader) → 출력 병합(Output Merger) 단계가 있습니다. 이 과정에서 3D 모델이 최종적으로 2D 이미지로 변환됩니다.
</details>

---

<details>
<summary><strong>4) Forward Rendering과 Deferred Rendering의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Forward Rendering</strong>은 각 픽셀을 그릴 때마다 모든 조명을 계산하는 방식이라 단순하지만 많은 조명이 있을 경우 성능이 떨어집니다. <strong>Deferred Rendering</strong>은 지오메트리 패스에서 정보를 G-Buffer에 저장한 후, 라이트 패스에서 조명을 한 번에 처리해 많은 조명을 효율적으로 다룰 수 있습니다. 대신 메모리 사용량은 더 큽니다.
</details>

---

<details>
<summary><strong>5) 텍스처 매핑(Texture Mapping)이란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>텍스처 매핑</strong>은 2D 이미지를 3D 모델 표면에 입히는 기술입니다. UV 좌표계를 통해 각 정점에 텍스처 좌표를 할당하고, 이를 픽셀 단위로 보간하여 모델에 적용합니다. 게임에서는 Diffuse, Normal, Specular 맵 등 다양한 텍스처를 활용해 사실감을 높입니다.
</details>

---

<details>
<summary><strong>6) 그림자(Shadow) 렌더링은 어떻게 이루어지나요?</strong></summary>
<strong>A.</strong> 대표적인 방식은 <strong>Shadow Mapping</strong>입니다. 먼저 빛의 시점에서 깊이 맵을 렌더링한 후, 실제 렌더링 시 픽셀이 그림자 맵 안에서 가려져 있는지 확인합니다. 또 다른 방식으로 <strong>Shadow Volume</strong>이 있지만, 성능 문제로 게임에서는 보통 Shadow Mapping을 사용합니다.
</details>

---

<details>
<summary><strong>7) 안티 앨리어싱(Anti-Aliasing)의 종류와 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>MSAA</strong>는 멀티 샘플링 기반으로 에지 부분의 픽셀을 부드럽게 처리합니다. <strong>FXAA</strong>는 포스트 프로세싱 기반으로 빠르지만 디테일 손실이 있을 수 있습니다. <strong>TAA</strong>는 시간 축을 활용해 더 부드러운 결과를 제공하지만 잔상(ghosting)이 생길 수 있습니다. 게임에서는 성능과 품질의 균형에 따라 선택합니다.
</details>

---

<details>
<summary><strong>8) LOD(Level of Detail)란 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>LOD</strong>는 객체의 거리에 따라 다른 해상도의 메시를 사용하는 기술입니다. 가까이 있는 객체는 고해상도 메시를, 멀리 있는 객체는 저해상도 메시를 사용해 성능을 절약합니다. 이를 통해 프레임율을 유지하면서도 시각적 품질 저하를 최소화할 수 있습니다.
</details>

---

<details>
<summary><strong>9) V-Sync는 무엇이고 장단점은 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>V-Sync</strong>는 모니터 주사율과 GPU 프레임 출력을 동기화하여 화면 찢김(티어링)을 방지하는 기술입니다. 장점은 화면 일관성이지만, 단점은 입력 지연(input lag)과 프레임 드랍 시 부드러움이 크게 저하된다는 점입니다. 대안으로 Adaptive Sync, G-Sync, FreeSync 같은 기술이 있습니다.
</details>

---

<details>
<summary><strong>10) Ray Tracing과 Rasterization의 차이는 무엇인가요?</strong></summary>
<strong>A.</strong> <strong>Rasterization</strong>은 삼각형을 픽셀로 투영해 빠르게 화면을 그리는 전통적인 방식이고, <strong>Ray Tracing</strong>은 광선을 추적해 물리적으로 정확한 빛 반사와 그림자를 계산하는 방식입니다. Ray Tracing은 사실적이지만 무겁고, Rasterization은 빠르지만 단순화된 결과를 냅니다. 최신 게임은 두 기술을 혼합해 사용합니다.
</details>
