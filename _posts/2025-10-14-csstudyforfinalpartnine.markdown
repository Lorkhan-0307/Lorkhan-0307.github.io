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

- “자식 클래스가 자기 자신을 부모 클래스의 템플릿 인자로 넘겨주는 패턴”

```cpp
// 일반적인 상속 관계
class Base {};
class Child : public Base {};
```

```cpp
// CRTP
template <typename T>
class Base {};

class Child : public Base<Child> {};
```



**특징 및 상세설명**  
- 가상 호출 없이 파생 타입별 최적화/바인딩.  
- 정책 기반 설계(policy-based design)에 유용.

- virtual 없이 컴파일 타임에 호출 대상이 결정됨
- 함수 포인터(vtable) 없음 → 오버헤드 0
- 인라인 최적화 가능 → 매우 빠름

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

---

<details markdown="1">
<summary><strong>UE-1) 빌드 시스템과 쿠킹 프로세스 최적화</strong></summary>

**핵심 요약**  
Unreal의 빌드는 `Build.cs` 설정을 바탕으로 모듈 단위로 진행되며, 쿠킹은 **리소스를 플랫폼별 실행 가능 포맷으로 변환**하는 과정이다.  
쿠킹 최적화의 핵심은 **불필요한 Asset/Shader/Localization 변환 최소화**다.

**특징 및 상세설명**  
- 빌드 파이프라인: UBT(Unreal Build Tool) → UHT(Reflection Header) → 링커 → Pak 생성.  
- 쿠킹은 에디터→Cooked Data(압축, 압축 해제, 캐싱) → 패키징.  
- `-iterate` 옵션으로 변경된 Asset만 재쿠킹.  
- `Derived Data Cache(DDC)` 재활용으로 빌드 속도 향상.  
- CI/CD에서 Headless 빌드 시 `CookOnTheFly` 서버 사용 가능.

**면접식 답변**  
> UBT와 UHT는 각각 코드와 메타데이터를 처리하고, 쿠킹은 콘텐츠를 플랫폼별 실행 가능한 형태로 바꿉니다.  
> 쿠킹 속도는 DDC 재사용과 Incremental Cook 설정으로 크게 개선됩니다.

</details>

---

<details markdown="1">
<summary><strong>UE-2) Reflection 시스템의 구조와 사용 이유</strong></summary>

**핵심 요약**  
Reflection은 **런타임 타입 정보(RTTI)**와 **에디터/GC/리플리케이션 지원**을 위해 Unreal에서 별도 구현된 메타데이터 시스템이다.

**특징 및 상세설명**  
- `UCLASS`, `USTRUCT`, `UPROPERTY`, `UFUNCTION` 매크로로 메타데이터 등록.  
- UHT(Unreal Header Tool)가 파싱 후 `.generated.h` 생성.  
- GC, 리플리케이션, 블루프린트 노출 모두 Reflection 기반.  
- C++의 RTTI보다 더 풍부한 런타임 메타 정보 제공.

**면접식 답변**  
> Unreal은 C++의 RTTI 대신 자체 리플렉션을 사용해 에디터, GC, 리플리케이션을 통합적으로 지원합니다.  
> `UObject` 계열만 이 시스템을 활용할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>UE-3) UObject의 GC 구조와 수명 관리</strong></summary>

**핵심 요약**  
UObject는 Unreal의 **Garbage Collector(GC)**에서 추적 관리되며,  
참조가 없고 `RF_RootSet`에 속하지 않으면 수거 대상이 된다.

**특징 및 상세설명**  
- `AddToRoot()`로 수거 방지 가능, `RemoveFromRoot()`로 해제.  
- `UPROPERTY()`로 참조 등록 시 GC 추적됨.  
- `NewObject()`와 `ConstructObject()`의 차이는 컨텍스트/Outer 처리.  
- GC는 Mark-and-Sweep 방식이며, Tick 루프 내에서 주기적 수행.

**면접식 답변**  
> UObject는 GC 대상이므로 일반 포인터 관리보다 안전하지만,  
> UPROPERTY 누락 시 수거될 수 있어 항상 참조를 명시해야 합니다.

</details>

---

<details markdown="1">
<summary><strong>UE-4) Garbage Collection은 언제 동작하는가?</strong></summary>

**핵심 요약**  
엔진 루프 중 `UEngine::Tick()` 과정에서 **Mark-and-Sweep** 형태로 주기적으로 호출된다.

**특징 및 상세설명**  
- `CollectGarbage()` 직접 호출로 수동 실행 가능.  
- PIE(Play In Editor)나 Level 전환 시 강제 수집.  
- GC 주기는 `gc.TimeBetweenPurgingPendingKillObjects` 등으로 제어 가능.  
- Editor 환경은 더 자주 실행됨.

**면접식 답변**  
> 대부분의 상황에서 엔진이 자동으로 GC를 수행하지만,  
> 대용량 객체나 레벨 전환 직후엔 명시적 호출로 메모리 피크를 조절하기도 합니다.

</details>

---

<details markdown="1">
<summary><strong>UE-5) Subsystem 기반 UI 설계 이유</strong></summary>

**핵심 요약**  
UIController를 `UGameInstanceSubsystem`에 두면 **Persistent Level 간 UI 유지**와  
**전역적 초기화 순서 제어**가 가능하다.

**특징 및 상세설명**  
- `GameInstanceSubsystem`: 월드 전환 시에도 유지.  
- `UIController`가 레이어(Canvas)별로 Widget을 관리.  
- GameMode/UI 간 의존성 분리 → 모듈화.  
- `InitializeSubsystems()`로 순차 초기화 보장.

**면접식 답변**  
> 서브시스템 기반 설계로 UI 생명주기를 월드와 분리해  
> Seamless Travel 중에도 UI를 유지하고, 전역 컨트롤을 통합할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>UE-6) SpawnActor의 내부 동작</strong></summary>

**핵심 요약**  
`SpawnActor`는 **CDO(Class Default Object)**를 복제(clone)해 새로운 인스턴스를 생성한다.

**특징 및 상세설명**  
- Actor 생성 순서: CDO 복제 → 생성자 → `PostInitProperties()` → `BeginPlay()`.  
- `SpawnParameters`로 Owner/CollisionHandling 등 제어.  
- 네트워크 환경에선 서버만 실제 스폰 → 클라이언트는 리플리케이션으로 반영.

**면접식 답변**  
> `SpawnActor`는 CDO 복제 기반이므로 초기값 설정은 CDO에 정의하는 게 깔끔합니다.  
> 네트워크 게임에선 반드시 서버에서 스폰해야 동기화가 이루어집니다.

</details>

---

<details markdown="1">
<summary><strong>UE-7) 컴포넌트와 상속의 차이</strong></summary>

**핵심 요약**  
상속은 **is-a**, 컴포넌트는 **has-a** 관계.  
컴포넌트는 기능을 조합할 때, 상속은 공통 인터페이스를 확장할 때 사용.

**특징 및 상세설명**  
- 상속은 계층 깊어질수록 유연성 감소.  
- 컴포넌트는 `UActorComponent` 또는 `USceneComponent` 기반.  
- 블루프린트 기반 클래스에서 재사용성 극대화.

**면접식 답변**  
> 상속은 기본 속성과 행위의 일관성을 유지할 때,  
> 컴포넌트는 여러 기능을 조립할 때 더 적합합니다.

</details>

---

<details markdown="1">
<summary><strong>UE-8) 컴포넌트 간 통신 방법</strong></summary>

**핵심 요약**  
컴포넌트 간 직접 참조보단 **Delegate/EventDispatcher/Interface**를 활용한다.

**특징 및 상세설명**  
- C++에서는 `DECLARE_DYNAMIC_MULTICAST_DELEGATE`로 브로드캐스트 가능.  
- BP에서는 Event Dispatcher로 유사 동작.  
- Interface 기반 접근 시 의존성 최소화.

**면접식 답변**  
> 상호 참조를 피하려면 인터페이스나 델리게이트를 이용해  
> 이벤트 기반 통신을 설계하는 것이 안전합니다.

</details>

---

<details markdown="1">
<summary><strong>UE-9) Draw Call의 개념과 성능 영향</strong></summary>

**핵심 요약**  
Draw Call은 GPU에 “이 오브젝트를 렌더링하라”는 **렌더 명령 단위**이다.  
CPU → GPU 전송 호출 횟수이며, 많을수록 오버헤드 증가.

**특징 및 상세설명**  
- Draw Call 병합 = Batching / Instancing.  
- 머티리얼/셰이더/텍스처 변경은 추가 Draw Call 발생.  
- UE에서 `r.MeshDrawCommands.*`로 모니터링 가능.

**면접식 답변**  
> Draw Call은 CPU-GPU 사이 병목의 핵심 원인입니다.  
> 동일 머티리얼/메시를 묶어 배칭하면 프레임타임을 줄일 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>UE-10) Anchor와 Pivot의 차이</strong></summary>

**핵심 요약**  
Anchor는 **위치 기준(부모 기준 비율)**,  
Pivot은 **회전/스케일 기준(자기 자신 내 비율)**을 의미한다.

**특징 및 상세설명**  
- Anchor는 레이아웃 조정 시 반응형 UI에 중요.  
- Pivot은 애니메이션 회전/스케일 기준점 제어.  
- UE 에디터에서 Anchor는 파란 사각, Pivot은 노란 점으로 표시.

**면접식 답변**  
> Anchor는 “어디에 붙을지”, Pivot은 “어디를 중심으로 회전할지”의 차이입니다.

</details>

---

<details markdown="1">
<summary><strong>UE-11) 나나이트와 루멘의 차이/관계</strong></summary>

**핵심 요약**  
Nanite는 **지오메트리 스트리밍**,  
Lumen은 **글로벌 일루미네이션(GI)** 시스템이다.

**특징 및 상세설명**  
- Nanite: 삼각형 단위 가시성 스트리밍(LOD 대체).  
- Lumen: 실시간 반사/간접광 계산.  
- 둘 다 GPU 연산 기반으로 CPU 부하 감소.  
- 상호 독립적이지만 렌더 파이프라인에서 통합적으로 작동.

**면접식 답변**  
> Nanite는 “디테일 유지”, Lumen은 “조명 품질 개선”으로  
> UE5의 두 핵심 렌더링 혁신 기술입니다.

</details>

---

<details markdown="1">
<summary><strong>UE-12) UE 렌더링 파이프라인(전통 vs UE5)</strong></summary>

**핵심 요약**  
전통 UE는 **Deferred Rendering** 기반,  
UE5는 Nanite+Lumen 중심의 **Virtualized Pipeline**으로 진화했다.

**특징 및 상세설명**  
- UE4: GBuffer 생성 → Lighting Pass → Post Processing.  
- UE5: Nanite Geometry Pass → Lumen GI → Virtual Shadow Map.  
- 하이엔드 환경에선 RTX Path Tracer 병행 가능.

**면접식 답변**  
> UE5는 GBuffer 중심의 구조에서 벗어나 지오메트리와 조명 모두 가상화된 파이프라인으로 옮겨왔습니다.

</details>

---

<details markdown="1">
<summary><strong>UE-13) Post Processing 단계의 역할</strong></summary>

**핵심 요약**  
렌더링 후 최종 화면에 **후처리 효과(Bloom, DoF, Tonemapping)**를 적용하는 과정이다.

**특징 및 상세설명**  
- UE에서 `PostProcessVolume` 또는 `PostProcessMaterial`로 제어.  
- 렌더 타깃에 적용되는 셰이더 기반 이미지 연산.  
- 후처리 순서: Tonemap → Exposure → FX.  

**면접식 답변**  
> 후처리는 최종 시각적 완성도를 높이지만,  
> 효과가 많을수록 GPU 비용이 증가하므로 품질과 성능의 균형이 중요합니다.

</details>

---

<details markdown="1">
<summary><strong>Unity-1) Input System과 Input Class의 차이 및 마이그레이션 포인트</strong></summary>

**핵심 요약**  
`Input Class`는 **구버전 단일 입력 API**,  
`Input System`은 **이벤트 기반, 멀티 디바이스 대응형** 입력 처리 시스템이다.

**특징 및 상세설명**  
- `Input.GetKey` / `GetAxis` → 즉시 폴링 방식.  
- `Input System`은 `InputAction` 기반으로 입력 이벤트를 바인딩.  
- 멀티플레이, 리바인딩, 디바이스 핫스왑 지원.  
- Player Settings → “Active Input Handling”을 New로 변경해야 충돌 없음.

**면접식 답변**  
> 새 Input System은 이벤트 기반이라 구조적이고, 리바인딩/패드 지원이 자연스럽습니다.  
> 단, 기존 `Input`과 병행 시 예외(`InvalidOperationException`)가 자주 발생하므로 완전 전환이 필요합니다.

</details>

---

<details markdown="1">
<summary><strong>Unity-2) Coroutine과 async/await의 차이</strong></summary>

**핵심 요약**  
Coroutine은 **프레임 단위 일시 정지**,  
`async/await`은 **비동기 태스크(Task 기반)** 로직 제어를 위한 C# 기능이다.

**특징 및 상세설명**  
- Coroutine: `yield return`으로 프레임 타이밍 제어.  
- async: 스레드 풀 활용, CPU 비동기 작업에 적합.  
- `UniTask`를 이용하면 둘의 장점을 통합 가능.  
- Coroutine은 Unity 메인 스레드에서만 동작.

**면접식 답변**  
> Coroutine은 “시간 기반 흐름 제어”, async는 “I/O 기반 병렬 처리”에 적합합니다.  
> Unity에서는 UI나 애니메이션은 Coroutine, 파일 I/O나 네트워크는 async를 씁니다.

</details>

---

<details markdown="1">
<summary><strong>Unity-3) FixedUpdate()가 프레임레이트에 영향을 받지 않는 이유</strong></summary>

**핵심 요약**  
`FixedUpdate()`는 **물리 시뮬레이션 주기(Time.fixedDeltaTime)**에 따라 일정하게 호출된다.

**특징 및 상세설명**  
- 렌더링 프레임과 분리된 물리 프레임.  
- deltaTime은 고정값(기본 0.02초).  
- `Time.fixedDeltaTime` 변경 시 물리 해상도 조절 가능.

**면접식 답변**  
> 물리 엔진의 안정성을 위해 일정 간격으로 동작하며, 프레임이 느려도 여러 번 호출되어 시뮬레이션이 일관됩니다.

</details>

---

<details markdown="1">
<summary><strong>Unity-4) Photon을 네트워크 솔루션으로 선택한 이유</strong></summary>

**핵심 요약**  
Photon은 **서버리스 구조, 빠른 프로토타이핑, 안정된 매칭/룸 시스템**을 제공한다.

**특징 및 상세설명**  
- UDP 기반, 자체 Relay Server 보유.  
- Custom Properties로 상태 동기화 가능.  
- 서버 구축 없이 PUN/Realtime SDK로 손쉽게 구현 가능.  
- 비용 구조가 단순(유저 수 기반).

**면접식 답변**  
> 빠른 구현과 상용 안정성을 원할 때 Photon이 적합합니다.  
> 반면 Dedicated Server 제어가 필요하면 Mirror나 Netcode for GameObjects가 낫습니다.

</details>

---

<details markdown="1">
<summary><strong>Unity-5) Background GC vs Concurrent GC</strong></summary>

**핵심 요약**  
둘 다 **메인 스레드 정지 시간(STW, Stop The World)**을 줄이기 위한 GC 방식이다.

**특징 및 상세설명**  
- Background GC: 백그라운드 스레드에서 마크 단계 수행.  
- Concurrent GC: 메인 스레드와 병행 실행, 성능 부하 감소.  
- Unity 2021+는 Mono/.NET GC 개선으로 Concurrent 지원 확대.

**면접식 답변**  
> Background는 정지 시간 감소, Concurrent는 STW 최소화로 부드러운 프레임 유지에 도움됩니다.  
> GC 설정은 Editor 로그에서 “Incremental GC”로 확인할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>Unity-6) Unity의 GC(.NET 기반 여부)</strong></summary>

**핵심 요약**  
Unity의 GC는 **Mono/.NET 런타임의 Mark-Sweep 기반**이며, 일부 플랫폼에서 Incremental GC를 사용한다.

**특징 및 상세설명**  
- Unity 2019 LTS 이후 Incremental GC 옵션 추가.  
- IL2CPP 빌드 시에도 관리 객체는 별도 추적.  
- 세대별(Generation) 구조는 사용하지 않음.  
- `GC.Collect()`는 강제 호출이지만 비추천.

**면접식 답변**  
> Unity는 자체 GC를 쓰지 않고 Mono/.NET 기반을 커스터마이징했습니다.  
> 현재는 Incremental GC로 한 프레임당 작업을 분산합니다.

</details>

---

<details markdown="1">
<summary><strong>Unity-7) Dictionary vs SortedDictionary</strong></summary>

**핵심 요약**  
`Dictionary`는 **해시 기반**,  
`SortedDictionary`는 **이진 트리 기반(정렬된 Key 보장)**이다.

**특징 및 상세설명**  
- Dictionary: 평균 O(1), 순서 없음.  
- SortedDictionary: O(logN), Key 정렬 유지.  
- 내부 구현: `Dictionary`는 해시 테이블, `SortedDictionary`는 Red-Black Tree.

**면접식 답변**  
> 순서가 필요하면 SortedDictionary, 단순 조회면 Dictionary를 씁니다.

</details>

---

<details markdown="1">
<summary><strong>Unity-8) 해시 충돌 해결 방식</strong></summary>

**핵심 요약**  
Unity의 `Dictionary`는 **체이닝(Chaining)** 방식으로 해시 충돌을 해결한다.

**특징 및 상세설명**  
- 동일 버킷에 여러 Key 존재 가능.  
- 각 Entry는 연결 리스트 형태로 관리.  
- .NET 7 이후 Open Addressing 일부 적용 (Unity는 Mono 기반이라 이전 구조).

**면접식 답변**  
> 해시 충돌은 완전히 피할 수 없으므로, Key 분포를 고려한 커스텀 해시 함수를 쓰는 게 좋습니다.

</details>

---

<details markdown="1">
<summary><strong>Unity-9) Queue 내부 구현</strong></summary>

**핵심 요약**  
배열 기반 **순환 버퍼 구조**로 구현되어 있다.

**특징 및 상세설명**  
- head/tail 인덱스를 이용해 O(1) 입출력.  
- capacity 초과 시 2배 확장.  
- Enqueue/Dequeue 시 실제 데이터 이동 없음.

**면접식 답변**  
> 큐는 단순 연결 리스트보다 캐시 효율이 높고, 순환 버퍼 방식으로 빠른 연산이 가능합니다.

</details>

---

<details markdown="1">
<summary><strong>Unity-10) Batching의 개념과 성능 영향</strong></summary>

**핵심 요약**  
Batching은 여러 오브젝트를 **하나의 Draw Call로 묶는 최적화** 기법이다.

**특징 및 상세설명**  
- Static Batching: 움직이지 않는 오브젝트 병합.  
- Dynamic Batching: 작은 Mesh를 런타임에 병합.  
- GPU Instancing: 동일 Mesh를 여러 번 렌더링.  
- 오버헤드: Mesh 병합, Transform 업로드.

**면접식 답변**  
> Static은 메모리, Dynamic은 CPU 비용이 늘지만 DrawCall 감소 효과가 큽니다.  
> 가장 이상적인 방식은 Instancing 기반입니다.

</details>

---

<details markdown="1">
<summary><strong>Unity-11) 그래픽스 파이프라인에서 라이팅 적용 시점</strong></summary>

**핵심 요약**  
라이팅은 **Fragment(픽셀) 셰이더 단계**에서 주로 적용된다.

**특징 및 상세설명**  
- Vertex 단계에서 조명 계산 시 “버텍스 라이팅”.  
- Fragment 단계에서 조명 계산 시 “픽셀 라이팅”.  
- URP는 Forward+ 기반, HDRP는 Deferred 기반.

**면접식 답변**  
> 정밀한 라이팅이 필요한 장면은 픽셀 단위 조명이 필수이며, Forward vs Deferred 구조 선택에 따라 계산 위치가 달라집니다.

</details>

---

<details markdown="1">
<summary><strong>Unity-12) 지오메트리 단계에서의 라이팅 연산 가능성</strong></summary>

**핵심 요약**  
이론상 가능하지만, **정확도가 낮고 제한적**이다.

**특징 및 상세설명**  
- Vertex 셰이더에서 근사 조명 계산 가능.  
- GPU 연산량 감소하지만 하이라이트 정확도 떨어짐.  
- 일반적으로 그림자/스페큘러 표현 불가.

**면접식 답변**  
> 저성능 환경에서는 버텍스 라이팅을 사용하지만,  
> 현대 파이프라인에서는 픽셀 단위 계산이 표준입니다.

</details>

---

<details markdown="1">
<summary><strong>Unity-13) ScriptableObject의 개념과 활용 이유</strong></summary>

**핵심 요약**  
`ScriptableObject`는 **에셋 형태의 데이터 컨테이너**로,  
메모리 절약과 데이터 재사용을 위한 구조다.

**특징 및 상세설명**  
- GameObject와 달리 씬에 존재하지 않음.  
- 여러 오브젝트가 동일 데이터 참조 가능.  
- 저장 포맷: `.asset`.  
- 설정/밸런스 데이터, 글로벌 설정, Pool 등에서 활용.

**면접식 답변**  
> ScriptableObject는 싱글톤처럼 공유되지만,  
> 에셋이므로 유지/관리 비용이 훨씬 적습니다.  
> “데이터 중심 설계(Data-Driven Design)”의 핵심 요소입니다.

</details>

---

<details markdown="1">
<summary><strong>Network-1) TCP vs UDP 차이 및 사용 시점</strong></summary>

**핵심 요약**  
TCP는 **신뢰성(순서, 재전송)** 보장,  
UDP는 **속도와 효율성**을 우선시한다.

**특징 및 상세설명**  
- TCP: 연결 지향형, 3-way handshake, 흐름 제어, 혼잡 제어.  
- UDP: 비연결형, 손실 가능성 있음, 브로드캐스트 지원.  
- 게임에서는 UDP 기반에 **신뢰성 계층(Reliable UDP)**을 추가하는 경우가 많다.

**면접식 답변**  
> TCP는 순서와 신뢰성이 필요한 채팅/로그인에,  
> UDP는 빠른 반응이 중요한 실시간 게임 패킷에 사용합니다.

</details>

---

<details markdown="1">
<summary><strong>Network-2) Reliable UDP와 TCP의 차이</strong></summary>

**핵심 요약**  
Reliable UDP는 **UDP 위에 직접 재전송 로직을 구현**한 구조로,  
TCP보다 **낮은 지연**과 **더 세밀한 제어**가 가능하다.

**특징 및 상세설명**  
- TCP는 커널 레벨에서 재전송, 순서보장 자동 처리.  
- Reliable UDP는 사용자 레벨에서 Ack/Nack, 시퀀스 관리.  
- 헤더 오버헤드 작고, 게임에 최적화된 신뢰성 조절 가능.

**면접식 답변**  
> TCP는 완전 신뢰성을, Reliable UDP는 “필요한 만큼의 신뢰성”을 제공합니다.  
> 예: 총알 피격 판정은 UDP, 인벤토리 저장은 TCP.

</details>

---

<details markdown="1">
<summary><strong>Network-3) 3-way handshake 과정</strong></summary>

**핵심 요약**  
TCP 연결을 성립하기 위한 **세 단계의 신호 교환**이다.

**특징 및 상세설명**  
1️⃣ **SYN**: 클라이언트 → 서버 연결 요청.  
2️⃣ **SYN-ACK**: 서버 → 클라이언트 수락 응답.  
3️⃣ **ACK**: 클라이언트 → 서버 확인 응답.  
이후 연결이 확립되고 데이터 전송 시작.

**면접식 답변**  
> TCP는 3단계 핸드셰이크로 신뢰 가능한 세션을 확보합니다.  
> 중간에 하나라도 손실되면 연결이 재시도됩니다.

</details>

---

<details markdown="1">
<summary><strong>Network-4) 데드락(Deadlock)의 발생 조건 4가지</strong></summary>

**핵심 요약**  
Deadlock은 **상호 배제, 점유 대기, 비선점, 순환 대기** 네 가지 조건이 모두 만족될 때 발생한다.

**특징 및 상세설명**  
- **상호 배제**: 한 자원은 한 프로세스만 접근 가능.  
- **점유 대기**: 자원 점유한 채 다른 자원 요청.  
- **비선점**: 자원을 강제로 회수 불가.  
- **순환 대기**: 자원 요청이 원형으로 연결.

**면접식 답변**  
> 4가지 중 하나라도 깨면 Deadlock은 방지됩니다.  
> 예를 들어 자원 순서를 고정하거나, 타임아웃을 두는 식입니다.

</details>

---

<details markdown="1">
<summary><strong>Network-5) 스와핑(Swapping)</strong></summary>

**핵심 요약**  
Swapping은 **프로세스를 메모리↔디스크 간 교체**하여 메모리 공간을 확보하는 기법이다.

**특징 및 상세설명**  
- 실행 중 프로세스를 백업(스왑 아웃), 다시 로드(스왑 인).  
- 메모리 부족 시 사용, 하지만 디스크 I/O 부담 큼.  
- 가상 메모리 페이징과는 개념상 유사하나 단위가 “프로세스 전체”.

**면접식 답변**  
> Swapping은 물리 메모리가 부족할 때 전체 프로세스를 교체하지만,  
> 현대 OS는 페이징 기반으로 더 세밀하게 관리합니다.

</details>

---

<details markdown="1">
<summary><strong>Network-6) 원자적 연산(Atomic Operation)</strong></summary>

**핵심 요약**  
Atomic Operation은 **더 이상 분해되지 않는 연산**,  
즉 중간 상태가 외부에 노출되지 않는 연산이다.

**특징 및 상세설명**  
- 하드웨어 수준에서 보장 (`LOCK XADD`, `CMPXCHG`).  
- CPU 캐시 일관성 프로토콜(MESI)과 함께 동작.  
- 멀티스레드 환경에서 동기화 비용 최소화.

**면접식 답변**  
> Atomic 연산은 락보다 가볍고, 동시성 제어의 기본 단위입니다.  
> 예: `InterlockedAdd`, `std::atomic<int>`.

</details>

---

<details markdown="1">
<summary><strong>Network-7) Main Thread vs Worker Thread</strong></summary>

**핵심 요약**  
Main Thread는 **렌더링/UI/게임 루프**를 담당,  
Worker Thread는 **비동기 연산**을 수행한다.

**특징 및 상세설명**  
- Unity/Unreal 모두 렌더링은 Main Thread 고정.  
- Worker Thread는 Pathfinding, Physics, Networking 등에서 사용.  
- 스레드 간 데이터 공유 시 Lock-Free 구조나 Queue를 사용.

**면접식 답변**  
> 렌더링은 반드시 메인 스레드에서 수행되고,  
> 부하 분산은 Worker Thread로 처리해야 프레임 드랍을 방지할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>Network-8) Task vs Thread</strong></summary>

**핵심 요약**  
`Thread`는 **실행 단위**,  
`Task`는 **작업 단위(스케줄링 추상화)**다.

**특징 및 상세설명**  
- Thread는 OS 수준의 실행 단위, 직접 생성/관리 비용 높음.  
- Task는 ThreadPool에서 스케줄링되어 효율적.  
- `await`와 결합 시 비동기 논리 구조화 가능.

**면접식 답변**  
> Task는 Thread 위의 논리 단위로,  
> 직접 스레드 관리 대신 고수준 비동기 로직을 작성할 수 있게 해줍니다.

</details>

---

<details markdown="1">
<summary><strong>Network-9) Coroutine vs Task</strong></summary>

**핵심 요약**  
Coroutine은 **Unity 엔진 루프 기반**,  
Task는 **C# 런타임 비동기 시스템** 기반이다.

**특징 및 상세설명**  
- Coroutine: `yield return`으로 프레임 대기, 메인 스레드 고정.  
- Task: 스레드풀 기반, I/O 작업 분산 가능.  
- UniTask로 Coroutine 스타일 async 지원 가능.

**면접식 답변**  
> Coroutine은 게임 로직용, Task는 외부 연산용입니다.  
> UniTask는 이 둘을 통합해 Unity 친화적 비동기를 제공합니다.

</details>

---

<details markdown="1">
<summary><strong>Network-10) UniTask의 의미와 사용 이유</strong></summary>

**핵심 요약**  
`UniTask`는 **Unity 친화적인 lightweight Task 대체재**로,  
`async/await` 오버헤드를 줄이기 위한 라이브러리다.

**특징 및 상세설명**  
- GC 할당 없는 구조, struct 기반.  
- Unity의 프레임, Update, WaitForSeconds와 자연스러운 연동.  
- CancellationToken, Timeout 등 지원.

**면접식 답변**  
> UniTask는 “Task보다 빠르고 Coroutine보다 유연한” 구조로,  
> 모바일 빌드에서 GC 부하를 줄이기에 가장 효율적입니다.

</details>

---

<details markdown="1">
<summary><strong>C#-1) class vs struct 차이 및 저장 위치</strong></summary>

**핵심 요약**  
`class`는 **참조 타입(힙)**, `struct`는 **값 타입(스택 또는 인라인)**이다.

**특징 및 상세설명**  
- class: 참조 전달, GC 관리, 상속 가능.  
- struct: 복사 전달, 상속 불가, 박싱 주의.  
- struct는 작고 불변(immutable)할수록 효율적.  
- 구조체는 스택에 “항상” 저장되는 것은 아니며, 다른 객체 내부에 인라인 저장될 수도 있다.

**면접식 답변**  
> class는 동적 수명과 유연성을, struct는 빠른 접근과 메모리 효율성을 제공합니다.  
> 단, struct를 남용하면 복사 비용이 커집니다.

</details>

---

<details markdown="1">
<summary><strong>C#-2) 박싱(Boxing)과 언박싱(Unboxing)</strong></summary>

**핵심 요약**  
값 타입을 **object나 interface로 변환 시 박싱**,  
반대로 꺼낼 때 **언박싱**이 일어난다.

**특징 및 상세설명**  
- 박싱 시 새 힙 객체가 생성됨 → GC 부담.  
- 언박싱은 값 복사이므로 또 한 번 오버헤드 발생.  
- 자주 반복되면 성능 저하 심각.

**면접식 답변**  
> 박싱/언박싱은 숨은 비용이라,  
> 제네릭이나 Span, struct 기반 설계로 회피하는 것이 중요합니다.

</details>

---

<details markdown="1">
<summary><strong>C#-3) 제네릭을 사용하면 박싱을 피할 수 있는 이유</strong></summary>

**핵심 요약**  
제네릭은 **컴파일 시 구체 타입으로 인스턴스화**되기 때문에  
`object` 변환이 필요 없어 박싱을 회피한다.

**특징 및 상세설명**  
- `List<int>`는 내부에서 `int` 그대로 저장.  
- 비제네릭 컬렉션(`ArrayList`)은 모든 요소를 `object`로 저장 → 박싱 발생.  
- 제네릭은 런타임 타입 안정성과 성능을 동시에 확보.

**면접식 답변**  
> 제네릭은 타입 정보가 컴파일 시 확정되므로  
> object 변환이 없어 박싱이 발생하지 않습니다.

</details>

---

<details markdown="1">
<summary><strong>C#-4) interface 기본 구현(C# 8.0+)과 재정의</strong></summary>

**핵심 요약**  
C# 8부터 인터페이스도 **기본 구현(Default Implementation)**을 가질 수 있다.

**특징 및 상세설명**  
- 기존 구현체 호환성 유지용.  
- `class`에서 동일 시그니처를 구현하면 **기본 구현은 무시됨**.  
- 명시적 구현(`IInterface.Method()`) 형태로 접근 가능.

**면접식 답변**  
> 인터페이스의 기본 구현은 ABI 호환성을 높이지만,  
> 복잡한 다중 상속 구조에서는 모호성 문제가 생길 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>C#-5) foreach 문 효율성</strong></summary>

**핵심 요약**  
`foreach`는 `IEnumerable`/`IEnumerator` 인터페이스 기반으로  
**구조적이지만 약간의 오버헤드**가 있다.

**특징 및 상세설명**  
- `List<T>`의 경우 JIT에서 인라인 최적화되어 오버헤드 거의 없음.  
- `Dictionary` 등에서는 박싱/인터페이스 호출 오버헤드 존재.  
- 구조체 Enumerator는 `foreach`가 더 빠르기도 함.

**면접식 답변**  
> 일반적인 컬렉션에서는 `foreach`와 `for`의 성능 차이가 거의 없습니다.  
> 단, 커스텀 Enumerator나 LINQ 체인에서는 반복자 비용이 커질 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>C#-6) ArrayList vs List&lt;T&gt;</strong></summary>

**핵심 요약**  
`ArrayList`는 **비제네릭**,  
`List<T>`는 **제네릭 기반 컬렉션**이다.

**특징 및 상세설명**  
- ArrayList: 모든 요소를 object로 저장 → 박싱 발생.  
- List<T>: 타입 안정, 박싱 없음, 컴파일 타임 체크 가능.  
- List<T>가 .NET 2.0 이후 사실상 표준.

**면접식 답변**  
> ArrayList는 유연하지만 느리고 타입 안전성이 없습니다.  
> 현대 C#에서는 List<T>를 기본으로 사용합니다.

</details>

---

<details markdown="1">
<summary><strong>C#-7) Span과 Memory로 박싱 회피</strong></summary>

**핵심 요약**  
`Span<T>`와 `Memory<T>`는 **메모리를 직접 참조하는 타입 안전한 뷰(View)**다.  
데이터 복사 없이 슬라이스/전달 가능하다.

**특징 및 상세설명**  
- `Span<T>`: 스택 전용, 참조 구조체(ref struct).  
- `Memory<T>`: 힙에서도 안전하게 사용 가능.  
- 박싱/언박싱 없는 고성능 데이터 접근 제공.

**면접식 답변**  
> Span은 박싱 없이 배열, 버퍼, 문자열을 빠르게 다루는 수단입니다.  
> 특히 GC 힙을 건드리지 않아 메모리 효율이 뛰어납니다.

</details>

---

<details markdown="1">
<summary><strong>C#-8) IDisposable, using, await using의 차이</strong></summary>

**핵심 요약**  
`IDisposable`은 **명시적 자원 해제 인터페이스**,  
`using`은 **자동 호출 구문**,  
`await using`은 **비동기 Dispose 지원**이다.

**특징 및 상세설명**  
- `using`은 스코프 종료 시 `Dispose()` 호출.  
- `await using`은 `IAsyncDisposable` 구현체의 `DisposeAsync()` 호출.  
- `using var` 구문으로 더 간결하게 작성 가능.

**면접식 답변**  
> `using`은 동기 자원 정리, `await using`은 비동기 자원 정리에 사용됩니다.  
> 특히 Stream 등 I/O 리소스에서는 비동기 Dispose가 중요합니다.

</details>

---

<details markdown="1">
<summary><strong>C#-9) 공변성과 반공변성(Covariance / Contravariance)</strong></summary>

**핵심 요약**  
공변성은 **파생 → 기반으로 대입 가능**,  
반공변성은 **기반 → 파생으로 대입 가능**이다.

**특징 및 상세설명**  
- 공변: `IEnumerable<Derived>` → `IEnumerable<Base>` 허용(`out`).  
- 반공변: `Action<Base>` → `Action<Derived>` 허용(`in`).  
- 제네릭 타입 매개변수 선언 시 `in`, `out` 키워드로 지정.

**면접식 답변**  
> 공변성은 반환형, 반공변성은 매개변수형에 적용됩니다.  
> 덕분에 제네릭 인터페이스 간 타입 호환성이 높아집니다.

</details>

---

<details markdown="1">
<summary><strong>CS-1) DP(Dynamic Programming) vs Greedy의 차이 및 활용 이유</strong></summary>

**핵심 요약**  
DP는 **부분 문제의 최적해를 저장하며 전체 최적해를 도출**,  
Greedy는 **현재 최선의 선택을 반복**해 전체 해를 근사한다.

**특징 및 상세설명**  
- DP: Overlapping Subproblems + Optimal Substructure 필요.  
- Greedy: 지역적 최적 → 전역 최적이 항상 보장되진 않음.  
- DP는 메모리 사용이 크고, Greedy는 빠르지만 완전성 부족.

**면접식 답변**  
> Greedy는 빠르고 단순하지만 최적 보장이 안 되고,  
> DP는 느리지만 항상 최적 해를 보장합니다.  
> 대표 예시는 동전 거스름돈 문제입니다.

</details>

---

<details markdown="1">
<summary><strong>CS-2) Segment Tree 구조와 부분합 최적화 원리</strong></summary>

**핵심 요약**  
Segment Tree는 **배열 구간에 대한 연산(합, 최소, 최대 등)**을  
O(logN) 시간에 수행하기 위한 **트리 기반 자료구조**이다.

**특징 및 상세설명**  
- 각 노드는 구간 [l, r]의 연산 결과를 저장.  
- 부분합, 최소/최대, Lazy Propagation(지연 업데이트) 지원.  
- 공간 복잡도 O(4N).

**면접식 답변**  
> 배열이 자주 갱신되고 구간 합을 자주 질의할 때 Segment Tree가 최적입니다.  
> 매 질의마다 전체 탐색(O(N)) 대신 O(logN)으로 해결됩니다.

</details>

---

<details markdown="1">
<summary><strong>CS-3) 이진 탐색 트리(Binary Search Tree)의 시간 복잡도</strong></summary>

**핵심 요약**  
BST의 탐색, 삽입, 삭제는 평균 O(logN), 최악 O(N)이다.

**특징 및 상세설명**  
- 트리 균형 유지 시 O(logN) 보장 (AVL, Red-Black Tree).  
- 한쪽으로 치우치면 선형 구조가 되어 O(N).  
- 중위 순회(In-order Traversal)는 항상 정렬된 결과를 반환.

**면접식 답변**  
> BST는 데이터가 정렬된 상태로 들어오면 성능이 급락하기 때문에,  
> 실무에서는 항상 균형 트리를 사용합니다.

</details>

---

<details markdown="1">
<summary><strong>CS-4) 벡터의 내적(Dot Product)</strong></summary>

**핵심 요약**  
두 벡터의 내적은 **크기 × 크기 × cosθ**,  
즉 방향 유사도(Projection)을 나타낸다.

**특징 및 상세설명**  
- A·B > 0 → 같은 방향, < 0 → 반대 방향.  
- A·B = 0 → 수직(orthogonal).  
- 조명 계산(조명각도, Normal-Lighting)에서 광범위하게 사용.

**면접식 답변**  
> 내적은 방향 일치도를 측정하는 연산으로,  
> 게임에서는 조명 밝기 계산의 핵심입니다.

</details>

---

<details markdown="1">
<summary><strong>CS-5) Backface Culling의 원리</strong></summary>

**핵심 요약**  
Backface Culling은 **카메라에서 보이지 않는 뒷면을 렌더링하지 않는 최적화**이다.

**특징 및 상세설명**  
- 폴리곤의 노멀 벡터와 시선 벡터 내적값으로 판단.  
- 내적 < 0 → 뒷면 → 렌더링 스킵.  
- GPU에서 자동 처리되며, 불필요한 픽셀 셰이딩 방지.

**면접식 답변**  
> Backface Culling은 GPU가 계산량을 절반 가까이 줄일 수 있는 기본 최적화입니다.  
> 단, 양면 머티리얼은 예외적으로 꺼야 합니다.

</details>

---

<details markdown="1">
<summary><strong>CS-6) PBR (Physics Based Rendering)의 개념</strong></summary>

**핵심 요약**  
PBR은 **물리 법칙 기반으로 빛의 반사·산란을 시뮬레이션**하는 렌더링 방식이다.

**특징 및 상세설명**  
- 핵심 구성: Albedo, Metallic, Roughness, Normal, AO.  
- Fresnel, Microfacet BRDF, Energy Conservation 원칙.  
- 재질 간 일관성 확보 → 현실감 향상.  
- UE/Unity 모두 PBR 기반 머티리얼 사용.

**면접식 답변**  
> PBR은 조명 세기나 각도에 따라 자연스러운 반응을 보이게 하는 물리 기반 셰이딩입니다.  
> 덕분에 아티스트가 조명 환경을 바꿔도 결과가 일관됩니다.

</details>
