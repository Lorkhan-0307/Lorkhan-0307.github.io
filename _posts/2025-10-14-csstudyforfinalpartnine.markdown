---
layout: post
title: "기술면접 대비 CS 공부 - Somethings"
date: 2025-10-14 10:00:00 +0900
categories: [Tech Interview, Study Plan]
tags: [c++, c-sharp, computer-science, algorithm, data-structure, operating-system, network, database, design-pattern, unity, unreal]
slug: csstudyforfinalpartnine
---

# 면접 대비 사전 QnA 정리 - 내가 잘 모르던 것

---

<details markdown="1">
<summary><strong>C++-1) static과 const의 차이, 그리고 static const의 의미는?</strong></summary>

**핵심 요약**  
`static`은 저장 영역/수명(정적 수명)과 연결되고, `const`는 변경 불가(불변성)를 뜻합니다.  
`static const`는 **프로그램 전체 수명**을 가지며 **수정 불가**인 정적 상수를 만들 때 사용합니다.

**특징 및 상세설명**  
- `static (전역/네임스페이스)` : 내부 링크(translation unit 한정)로 심벌 노출을 제한.  
- `static (함수 내부)` : 첫 호출 시 한 번 초기화, 이후 호출 간 값 유지.  
- `static (클래스 멤버)` : 인스턴스가 아닌 **클래스 차원 공유**. 별도 정의 필요.  
- `const` : 읽기 전용. 포인터/참조의 ‘무엇이 고정되는지’ 주의(`const int*` vs `int* const`).  
- `static const` : 컴파일타임 상수로 사용 가능(특히 정수형/열거형 대체), ODR 규칙에 유의.

**면접식 답변**  
> `static`은 수명과 링크에, `const`는 변경 가능성에 대한 키워드입니다.  
> 둘을 합친 `static const`는 프로그램 전반에서 공유되지만 수정할 수 없는 상수를 정의할 때 유용합니다.  
> 특히 클래스의 `static const int`는 헤더에 선언하고 소스에서 정의해 ODR 문제를 피합니다.

</details>

---

<details markdown="1">
<summary><strong>C++-2) new/delete와 malloc/free의 차이</strong></summary>

**핵심 요약**  
`new/delete`는 **생성자/소멸자 호출**과 **타입 안전성**을 보장하고,  
`malloc/free`는 **바이트 단위 메모리 할당/해제**만 수행합니다.

**특징 및 상세설명**  
- `new` : 타입 크기 계산 + 메모리 할당 + 생성자 호출, 실패 시 예외(`std::bad_alloc`).  
- `delete` : 소멸자 호출 + 메모리 해제. 배열은 `delete[]`.  
- `malloc/free` : 생성자/소멸자 호출 없음, 실패 시 `nullptr` 반환.  
- 혼용 금지 : `new` ↔ `free`, `malloc` ↔ `delete`는 UB.  
- 배치 new(placement new) : 이미 확보된 버퍼에서 객체 구성 가능.

**면접식 답변**  
> 객체 수명 관리가 필요한 C++에서는 `new/delete`가 맞고, C 스타일 버퍼가 필요하면 `malloc/free`를 씁니다.  
> 생성자/소멸자 호출이 필요한 타입에는 반드시 `new/delete`를 사용해야 합니다.

</details>

---

<details markdown="1">
<summary><strong>C++-3) RAII와 스마트 포인터의 동작 원리</strong></summary>

**핵심 요약**  
RAII는 **자원은 객체의 수명에 묶는다**는 규칙이고, 스마트 포인터는 이를 구현한 **자원 소유 래퍼**입니다.

**특징 및 상세설명**  
- 블록(스코프) 종료 시 소멸자가 호출되며 자원 해제.  
- `std::unique_ptr` : 단일 소유, 이동만 허용.  
- `std::shared_ptr` : 참조 카운팅 기반 공동 소유. `std::weak_ptr`은 순환참조 방지.  
- 예외 안전성 : 소멸자가 예외 없이 자원 정리 → 리소스 누수 방지.

**면접식 답변**  
> 파일 핸들, 메모리, 뮤텍스 같은 자원을 객체 수명에 연결해 자동 정리하는 패턴이 RAII입니다.  
> 실무에선 원칙적으로 생 포인터 대신 `unique_ptr`/`shared_ptr`로 소유권을 명시합니다.

</details>

---

<details markdown="1">
<summary><strong>C++-4) vector의 capacity와 재할당</strong></summary>

**핵심 요약**  
`size`는 사용 중 원소 수, `capacity`는 재할당 없이 담을 수 있는 최대치입니다.  
용량 초과 삽입 시 **더 큰 버퍼로 이동**하며 **기존 포인터/참조가 무효화**됩니다.

**특징 및 상세설명**  
- 재할당 시 보통 1.5~2배 성장(구현 의존).  
- `reserve(n)`으로 재할당 횟수/무효화를 줄임.  
- `shrink_to_fit()`은 비구속적(non-binding) 힌트.

**면접식 답변**  
> 반복 삽입 전 예상 크기만큼 `reserve`하면 성능과 안정성이 좋아집니다.  
> 재할당 후 iterator/포인터가 무효화됨을 항상 염두에 둬야 합니다.

</details>

---

<details markdown="1">
<summary><strong>C++-5) push_back vs emplace_back</strong></summary>

**핵심 요약**  
`push_back`은 **이미 만들어진 객체**를 복사/이동해서 넣고,  
`emplace_back`은 **컨테이너 내부에서 직접 생성**합니다.

**특징 및 상세설명**  
- 복사/이동 비용 절감 가능(특히 비가벼운 타입).  
- 단, 모든 경우에 `emplace_back`이 빠른 건 아님(완벽 전달/생성자 오버로드 주의).  

**면접식 답변**  
> 생성 비용이 큰 타입은 `emplace_back(args...)`가 유리합니다.  
> 단순 POD나 이미 RVO가 잘 되는 경우 성능 차는 미미할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>C++-6) initializer_list의 의미</strong></summary>

**핵심 요약**  
중괄호 `{}` 초기화를 **타입 안전**하게 전달하기 위한 **가벼운 뷰 타입**입니다.

**특징 및 상세설명**  
- `std::initializer_list<T>`는 요소 복사본의 포인터+크기 보유(읽기 전용).  
- 오버로드 모호성: `initializer_list`가 있으면 그 오버로드가 우선될 수 있음.

**면접식 답변**  
> 컨테이너 생성 시 `{1,2,3}` 같은 문법을 지원하고, 함수 인자로 리스트 리터럴을 자연스럽게 받을 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>C++-7) if constexpr의 역할</strong></summary>

**핵심 요약**  
컴파일타임 조건 분기로, **거짓 분기 코드는 아예 인스턴스화되지 않음** → SFINAE 대체/간소화.

**특징 및 상세설명**  
- 템플릿 메타프로그래밍 가독성 향상.  
- 잘못된 분기 구문이라도 인스턴스화되지 않으면 오류 없음.

**면접식 답변**  
> 타입 특성에 따라 컴파일 시 코드를 선택해 성능과 가독성을 동시에 확보합니다.

</details>

---

<details markdown="1">
<summary><strong>C++-8) Structured Binding의 동작</strong></summary>

**핵심 요약**  
튜플/배열/구조체를 **분해(binding)** 해서 여러 변수로 동시에 초기화합니다.

**특징 및 상세설명**  
- `auto [x,y] = pair;`  
- 참조 여부는 좌변 선언으로 제어(`auto& [x,y]`).  
- 비공개 멤버 구조체는 분해 불가(분해 요구 사항 충족 필요).

**면접식 답변**  
> 반환값이 많은 함수에서 간결하게 다룰 수 있어 코드 가독성이 크게 좋아집니다.

</details>

---

<details markdown="1">
<summary><strong>C++-9) Factory/Singleton 패턴의 장단점</strong></summary>

**핵심 요약**  
Factory는 생성 책임을 캡슐화, Singleton은 전역적 유일 인스턴스 보장.

**특징 및 상세설명**  
- Factory 장점: 결합도↓, 테스트 용이, 생성 로직 중앙화.  
- Factory 단점: 클래스 수 증가, 과설계 위험.  
- Singleton 장점: 유일성, 접근 용이.  
- Singleton 단점: 전역 상태/숨은 의존성, 테스트 어려움, 생명주기 관리 문제.

**면접식 답변**  
> 생성 복잡성은 Factory로 분리하고, Singleton은 정말 **전역적 유일성**이 필요할 때만 신중히 사용합니다.

</details>

---

<details markdown="1">
<summary><strong>C++-10) inline 함수와 컴파일러 최적화</strong></summary>

**핵심 요약**  
`inline` 키워드는 **ODR(한 정의 규칙) 보조** 의미가 커졌고,  
인라인 여부 결정은 **컴파일러 최적화**가 주도합니다.

**특징 및 상세설명**  
- 강제 인라인이 아님(힌트).  
- 작은/자주 호출/간단한 함수는 최적화 단계에서 자동 인라인 가능.  
- 헤더 정의 허용을 위한 `inline`(ODR) 사용 빈번.

**면접식 답변**  
> 성능 튜닝은 프로파일링으로 판단하고, 인라인 여부는 컴파일러에 맡기는 것이 현대 C++의 기본 접근입니다.

</details>

---

<details markdown="1">
<summary><strong>C++-11) 가상 소멸자의 필요성</strong></summary>

**핵심 요약**  
기반 클래스 포인터로 **다형적 삭제** 시 소멸자를 가상으로 해야 **리소스 누수를 방지**합니다.

**특징 및 상세설명**  
- `Base* p = new Derived; delete p;`에서 `~Base()`가 가상이 아니면 `~Derived()` 미호출.  
- 인터페이스 역할 클래스는 **반드시 가상 소멸자**.

**면접식 답변**  
> 다형성 계층의 루트는 가상 소멸자를 넣는 것이 안전한 규칙입니다.

</details>

---

<details markdown="1">
<summary><strong>C++-12) Iterator란 무엇이며 내부 동작</strong></summary>

**핵심 요약**  
이터레이터는 컨테이너 요소에 대한 **포인터 유사 추상화**입니다.

**특징 및 상세설명**  
- 카테고리: 입력/출력/전진/양방향/임의접근.  
- 컨테이너 변경 시 무효화 규칙 다름(vector 재할당 등).  
- `begin()/end()`로 범위 기반 for 지원.

**면접식 답변**  
> 포인터처럼 보이지만 컨테이너 구현에 독립적이어서 일반화 알고리즘을 가능하게 합니다.

</details>

---

<details markdown="1">
<summary><strong>C++-13) NULL vs nullptr</strong></summary>

**핵심 요약**  
`nullptr`는 **타입이 있는 null 포인터 상수**(std::nullptr_t), 오버로드 해석이 안전합니다.

**특징 및 상세설명**  
- `NULL`은 구현에 따라 `0` 정의 → 정수 오버로드로 잘못 분해 가능.  
- `nullptr` 사용이 현대 C++의 표준.

**면접식 답변**  
> 모호성 제거와 타입 안전성을 위해 항상 `nullptr`을 사용합니다.

</details>

---

<details markdown="1">
<summary><strong>C++-14) set/unordered_set, map/unordered_map의 차이(해시 충돌 포함)</strong></summary>

**핵심 요약**  
`set/map`은 **정렬 트리(RB-Tree)**, `unordered_*`는 **해시 버킷** 기반.

**특징 및 상세설명**  
- 시간복잡도: 트리 O(logN), 해시 평균 O(1) / 최악 O(N).  
- 해시 충돌 처리: 체이닝(버킷에 리스트/노드), 로드팩터 관리, 리해싱.  
- 순서 보장: 트리는 정렬 순서, 해시는 순서 없음.

**면접식 답변**  
> 탐색 빈도가 높고 정렬이 불필요하면 `unordered_*`, 정렬 순회/범위 질의가 필요하면 `set/map`이 유리합니다.

</details>

---

<details markdown="1">
<summary><strong>C++-15) Code bloat(코드 부풀림)</strong></summary>

**핵심 요약**  
템플릿과 인라인 남용 등으로 **바이너리 크기와 I-캐시 압박**이 커지는 현상입니다.

**특징 및 상세설명**  
- 원인: 과도한 템플릿 인스턴스화, 인라인, 중복 코드 생성.  
- 대책: Pimpl, 템플릿 분리/재사용, 링크 타임 최적화(LTO), 가상 호출로 중복 축소.

**면접식 답변**  
> 핫패스가 아니면 인라인을 아끼고, 공통 로직을 템플릿-매개변수화로 중복 없이 설계합니다.

</details>

---

<details markdown="1">
<summary><strong>C++-16) std::forward vs std::move</strong></summary>

**핵심 요약**  
`std::move`는 **무조건 rvalue 캐스팅**,  
`std::forward<T>`는 **전달받은 값 범주를 보존**하는 조건부 캐스팅입니다.

**특징 및 상세설명**  
- 완벽 전달(perfect forwarding) : 템플릿 매개변수 `T&&`와 `forward<T>`의 조합.  
- `move` 남용 시 유효성 상실 주의.

**면접식 답변**  
> “내가 받았던 그 값 범주 그대로” 넘기려면 `forward`, 강제로 이동 의미를 주려면 `move`를 씁니다.

</details>

---

<details markdown="1">
<summary><strong>C++-17) CRTP(Curiously Recurring Template Pattern)</strong></summary>

**핵심 요약**  
파생 클래스가 **자기 자신을 템플릿 인자로 기반 클래스에 전달**하는 패턴으로,  
정적 다형성과 믹스인 구현에 사용됩니다.

**특징 및 상세설명**  
- 가상 호출 없이 파생 타입별 최적화/바인딩.  
- 정책 기반 설계(policy-based design)에 유용.

**면접식 답변**  
> 런타임 오버헤드 없이 다형적 확장을 구현할 때 CRTP를 고려합니다.

</details>

---

<details markdown="1">
<summary><strong>C++-18) 메모리 정렬(alignment)과 패딩(padding)</strong></summary>

**핵심 요약**  
정렬은 타입이 요구하는 **주소 배치 제약**, 패딩은 이를 맞추기 위해 삽입되는 **채움 바이트**입니다.

**특징 및 상세설명**  
- 멤버 선언 순서에 따라 구조체 크기/패딩이 달라짐.  
- `alignas`, `alignof`로 제어 가능.  
- 잘못된 정렬 접근은 성능 저하/하드웨어 예외 가능.

**면접식 답변**  
> 큰 정렬 요구 멤버를 앞에 배치해 패딩을 최소화하고, SIMD 타입은 `alignas`로 정렬을 맞춥니다.

</details>

---

<details markdown="1">
<summary><strong>C++-19) 템플릿의 동작과 컴파일 과정</strong></summary>

**핵심 요약**  
템플릿은 **인스턴스화 시점**에 코드가 생성되며, ODR/가시성 규칙에 민감합니다.

**특징 및 상세설명**  
- 선언/정의는 보통 헤더에 둬야 함(가시성 필요).  
- 인스턴스화 시점의 종속 이름/오버로드 해결.  
- 링크 단계에서 동일 인스턴스 병합(LTO와 상호작용).

**면접식 답변**  
> 템플릿은 “코드 생성기”라서 헤더에 구현을 두고, 종속 이름 규칙과 인스턴스화 타이밍을 이해해야 빌드 오류를 줄일 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>C++-20) iterator/포인터 무효화 규칙 핵심</strong></summary>

**핵심 요약**  
컨테이너 변경(재할당/삭제) 시 **기존 참조가 무효**가 될 수 있습니다.

**특징 및 상세설명**  
- `vector` : 재할당/삽입/삭제로 광범위 무효화.  
- `list` : 노드 기반 → 다른 요소에 영향 적음.  
- `unordered_*` : 리해시 시 이터레이터 무효화, 참조는 유지(구현 의존 항목 확인).

**면접식 답변**  
> 성능 최적화보다 먼저 **무효화 규칙**을 안전하게 지키는 게 크래시/UB를 막는 최우선입니다.
</details>
