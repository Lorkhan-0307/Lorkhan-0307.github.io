---
layout: post
title: "기술면접 대비 CS 공부 - 02"
date: 2025-10-04 16:00:00 +0900
categories: [Tech Interview, Study Plan]
tags: [c++, c-sharp, computer-science, algorithm, data-structure, operating-system, network, database, design-pattern, unity, unreal]
slug: csstudyforfinalparttwo
---

# 면접 대비 사전 QnA 정리 - CPP


# 🔷 C++ 면접 예상 질문 50선 – 모범답변 (확장본 1/3: 1–20)

---

## 언어 기초 & 메모리

<details>
<summary><strong>1) 스택과 힙 메모리의 차이를 설명해주세요. (꼬리: 스택 오버플로우/힙 단편화)</strong></summary>
<strong>A.</strong> <strong>스택</strong>은 함수 호출 시 콜 프레임이 LIFO로 쌓였다가 함수가 끝나는 순간 자동으로 회수되는 메모리 영역입니다. 지역 변수와 반환 주소, 일부 레지스터 보관 등 아주 짧은 수명의 데이터에 최적화되어 있고, 증감이 단순한 포인터 이동만으로 끝나므로 <strong>매우 빠릅니다</strong>. 다만 OS가 할당한 <strong>스택 크기 한계</strong>(수 MB 수준)를 넘으면 <strong>스택 오버플로우</strong>가 발생합니다. 재귀 깊이가 비정상적으로 깊거나, 큰 배열을 지역에 잡을 때 자주 문제가 됩니다.  
<strong>힙</strong>은 <strong>동적 할당</strong>에 쓰이며, 런타임에 필요 용량만큼 자유롭게 요청/반납할 수 있습니다. 유연하지만, 내부적으로는 <strong>프리 리스트 관리/병합/분할</strong> 같은 부가 작업이 필요해서 오버헤드가 있고, 다양한 크기의 블록이 들락날락하면 <strong>외부 단편화</strong>가 생겨 큰 연속 블록이 부족한 상황이 발생할 수 있습니다. 이를 완화하기 위해 <strong>풀 알로케이터/슬랩</strong> 같은 도메인 맞춤 할당기를 사용하거나, 큰 객체를 묶어 배치하고 재사용(Object Pool)하는 전략을 병행합니다. 결과적으로 <strong>짧고 예측 가능한 수명</strong>은 스택, <strong>가변 용량·가변 수명</strong>은 힙이 담당한다고 정리합니다.
</details>

---

<details>
<summary><strong>2) RAII란 무엇인가요? (꼬리: 스마트 포인터와의 관계)</strong></summary>
<strong>A.</strong> <strong>RAII(Resource Acquisition Is Initialization)</strong>는 “자원 획득 = 객체 초기화”라는 철학으로, <strong>생성자에서 자원을 획득</strong>하고 <strong>소멸자에서 반드시 반납</strong>하도록 해 예외·조기 반환이 있어도 누수가 없게 만드는 C++의 핵심 관용구입니다. 파일 핸들, 소켓, 뮤텍스 락, 그래픽스 핸들, 메모리 등 모든 <strong>유한 자원</strong>에 적용할 수 있고, 스코프를 벗어나는 순간 자동 정리가 보장됩니다.  
스마트 포인터는 RAII의 대표 사례입니다. <strong>unique_ptr</strong>는 소유권을 단독 보유하며 스코프 종료 시 <strong>delete</strong>를 자동 호출해 누수를 차단하고, <strong>shared_ptr</strong>는 참조 카운트로 여러 개가 소유를 공유하되 마지막 포인터가 파괴될 때 자원을 해제합니다. <strong>weak_ptr</strong>는 순환 참조를 끊는 비소유 링크로, RAII가 자원 수명과 소유권을 <strong>타입 시스템</strong>으로 안전하게 모델링하게 해줍니다. 락도 <strong>lock_guard/scoped_lock</strong>로 감싸면 예외가 나도 자동 해제가 보장됩니다.
</details>

---

<details>
<summary><strong>3) 포인터와 참조의 차이는? (꼬리: nullptr vs NULL)</strong></summary>
<strong>A.</strong> <strong>포인터</strong>는 메모리 주소를 담는 <strong>값 객체</strong>로 재지정이 가능하고 <strong>null</strong>이 될 수 있습니다. 산술 연산과 배열 인덱싱도 허용되어 강력하지만, 잘못 쓰면 댕글링/더블 딜리트 같은 치명적 버그를 초래할 수 있습니다. <strong>참조</strong>는 “다른 객체의 또 다른 이름(alias)”로 <strong>반드시 유효한 객체에 바인딩</strong>되어야 하고 일단 바인딩되면 다른 대상을 가리키도록 바꿀 수 없습니다(재바인딩 불가). 문법적으로 편하고 최적화에도 유리해, <strong>API 매개변수</strong>에서 “반드시 있어야 하는 인자” 표현에 적합합니다.  
<code>nullptr</code>는 C++11에 도입된 <strong>타입 안전한 널 리터럴</strong>입니다. 오버로드 해석에서 <code>0</code> 또는 <code>NULL</code>(대개 <code>0</code> 매크로)보다 모호성이 적고, <code>void*</code>로의 암시적 변환도 의도적으로 제한됩니다. 면접에선 “포인터는 선택/재지정/널 가능, 참조는 필수/재지정 불가/널 불가”로 간결히 대비하면 좋습니다.
</details>

---

<details>
<summary><strong>4) 얕은 복사 vs 깊은 복사 (꼬리: 복사/대입 구현 주의점)</strong></summary>
<strong>A.</strong> <strong>얕은 복사</strong>는 포인터 값만 복제해 <strong>리소스를 공유</strong>하는 형태(두 객체가 같은 버퍼를 가리킴)이고, <strong>깊은 복사</strong>는 버퍼 자체를 새로 할당하고 내용을 복제해 <strong>독립 소유</strong>를 보장합니다. 사용자 정의 리소스를 가진 타입은 <strong>Rule of 5</strong>(복사/이동 생성자, 복사/이동 대입, 소멸자)를 염두에 두고 일관되게 소유권 의미를 구현해야 합니다. 예외 안전성은 <strong>복사 후 스왑</strong> 관용구로 강화할 수 있고, 대입 연산에서는 <strong>자기 대입 방지</strong>, <strong>리소스 릭 없이 강한 보장</strong>(실패 시 원자성 유지)을 신경 써야 합니다. 큰 버퍼는 카피-온-라이트 또는 <strong>참조 카운팅</strong>으로 비용을 완화할 수 있지만 멀티스레딩과의 상호작용을 면밀히 설계해야 합니다.
</details>

---

<details>
<summary><strong>5) const의 쓰임새 (꼬리: const 멤버함수 + mutable)</strong></summary>
<strong>A.</strong> <strong>const</strong>는 “불변 계약”을 타입에 새기기 위한 키워드입니다. <strong>객체 자체</strong>를 const로 만들면 모든 <strong>비-<code>mutable</code></strong> 멤버를 바꿀 수 없고, <strong>포인터</strong>에는 “포인터가 가리키는 대상이 불변인지, 포인터 자체가 불변인지”를 각각 표시할 수 있습니다. <strong>const 멤버 함수</strong>는 “논리적으로 상태를 바꾸지 않는다”라는 API 계약을 표현하고, 호출자는 해당 함수가 외부 관찰 가능한 상태를 변형하지 않음을 신뢰할 수 있습니다. <strong>mutable</strong>은 캐시/통계 카운터처럼 <strong>관찰 가능한 불변성</strong>을 깨지 않는 내부 상태를 예외적으로 변경 가능케 합니다. const는 최적화 관점에서도 유리해, 컴파일러가 <strong>알리아싱 가정</strong>을 안전히 할 수 있도록 돕습니다.
</details>

---

<details>
<summary><strong>6) static 의미 (꼬리: 초기화 시점)</strong></summary>
<strong>A.</strong> <strong>static 지역 변수</strong>는 함수 호출 간 값을 유지하는 정적 수명이고, <strong>네임스페이스/전역의 static</strong>은 내부 연결(linkage)을 만들어 <strong>번역 단위</strong>에 심볼을 숨깁니다. <strong>클래스 정적 멤버</strong>는 모든 인스턴스가 공유하는 <strong>한 벌의 저장소</strong>를 의미합니다. 초기화는 <strong>정적 초기화</strong>(제로/상수) → <strong>동적 초기화</strong>(런타임 코드 필요)로 나뉘며, C++11 이후 <strong>함수 지역 정적</strong>은 <strong>스레드 안전한 지연 초기화</strong>가 보장됩니다(“마이어스 싱글톤” 구현 근거). 초기화 순서 의존성 문제를 피하려면 전역 정적 대신 함수 지역 정적·모듈 초기화 패턴을 권장합니다.
</details>

---

<details>
<summary><strong>7) inline 함수 (꼬리: 컴파일러 최적화와의 관계)</strong></summary>
<strong>A.</strong> <strong>inline</strong>의 역사적 의미는 “중복 정의 허용(ODR 해결)” + “치환 힌트”입니다. 오늘날 치환 여부는 <strong>컴파일러/링커의 결정</strong>이며, PGO/LTO 같은 최적화가 더 큰 영향력을 가집니다. 인라인은 <strong>호출 오버헤드 제거</strong>·<strong>레지스터 할당 최적화</strong> 같은 이점이 있지만, <strong>코드 부피 증가</strong>로 아이캐시 압박이 생길 수 있습니다. 일반적으로 아주 작은 단순 접근자/래퍼, 빈번 호출 경로에만 신중히 적용하고, 헤더 정의가 필요한 템플릿/헤더 온리 유틸리티에서는 자연스럽게 inline 취급됩니다.
</details>

---

<details>
<summary><strong>8) 메모리 alignment/padding (꼬리: struct 낭비 줄이기)</strong></summary>
<strong>A.</strong> CPU는 특정 경계 정렬에서 데이터를 읽을 때 가장 효율적입니다. 구조체는 필드의 정렬 요건을 만족하기 위해 <strong>패딩</strong>을 삽입하고, 마지막에도 구조체 전체 정렬에 맞게 패딩이 붙습니다. 메모리 낭비를 줄이려면 <strong>큰 타입부터 배치</strong>하고, 동일한 크기의 필드를 묶으며, 필요 시 <strong>패킹 지시자</strong>를 쓰되(예: <code>#pragma pack</code>) <strong>잘못 정렬된 접근</strong>이 성능 저하나 하드웨어 예외를 유발할 수 있음을 이해해야 합니다. 멀티스레딩에서는 <strong>캐시라인 정렬</strong>로 false sharing을 피하는 것도 중요합니다.
</details>

---

<details>
<summary><strong>9) volatile 역할 (꼬리: 멀티스레드 동기화 가능?)</strong></summary>
<strong>A.</strong> <strong>volatile</strong>은 컴파일러 최적화에 “이 위치의 값은 외부 요인으로 바뀔 수 있으니 매번 메모리에서 읽어라”를 지시합니다(메모리 매핑 I/O 등). 하지만 <strong>재정렬 방지/가시성/원자성</strong>을 보장하지 않으므로 <strong>멀티스레드 동기화</strong>에는 적합하지 않습니다. 스레드 간 통신에는 <strong>std::atomic</strong>(필요 시 메모리 오더 지정)과 <strong>뮤텍스/조건변수</strong>를 사용해야 합니다. 가끔 임베디드에서 하드웨어 레지스터 접근에 한정해 의미가 있습니다.
</details>

---

<details>
<summary><strong>10) new/delete vs malloc/free (꼬리: new가 malloc을 호출?)</strong></summary>
<strong>A.</strong> <strong>new</strong>는 <strong>operator new</strong>를 통해 메모리를 확보하고 <strong>생성자</strong>를 호출합니다. 실패 시 기본적으로 <strong>예외</strong>(<code>std::bad_alloc</code>)를 던집니다. <strong>malloc</strong>은 <strong>바이트 버퍼만</strong> 반환하며 생성자 호출이 없고, 실패 시 <strong>nullptr</strong>을 반환합니다. 구현에 따라 <strong>operator new</strong>가 내부적으로 <strong>malloc</strong>을 사용할 수 있지만, 의미론은 분명히 다릅니다. 커스텀 <strong>operator new/delete</strong>로 풀/아레나 할당기를 주입하는 패턴은 고성능 시스템에서 흔합니다.
</details>

---

## 객체지향 & 다형성

<details>
<summary><strong>11) OOP 핵심 개념 (꼬리: 캡슐화 vs 추상화)</strong></summary>
<strong>A.</strong> OOP의 핵심은 <strong>캡슐화</strong>(데이터 은닉과 경계 설정), <strong>상속</strong>(행동 재사용), <strong>다형성</strong>(공통 인터페이스로 다른 구현 호출), <strong>추상화</strong>(본질만 노출)입니다. <strong>캡슐화</strong>는 “변경의 파급을 경계 내부로 가두는 일”이고, <strong>추상화</strong>는 “불필요한 세부를 가려 사용자를 단순화”합니다. 예를 들어 렌더링 파이프라인에서 상위 계층은 <strong>IRenderer</strong> 인터페이스만 의존하고, DirectX/Metal/Vulkan 구현은 <strong>교체 가능</strong>하도록 숨겨집니다. 이는 테스트가 쉬워지고, 교체/확장에 강한 구조를 만듭니다.
</details>

---

<details>
<summary><strong>12) 가상 함수 동작 (꼬리: vtable은 객체/클래스 어디?)</strong></summary>
<strong>A.</strong> 가상 함수가 하나라도 있는 클래스의 각 객체는 <strong>vptr</strong>을 가집니다. vptr은 <strong>클래스 단위</strong>로 존재하는 <strong>vtable</strong>을 가리키고, vtable 안에는 가상 함수들의 <strong>실제 구현 주소</strong>가 순서대로 들어 있습니다. 호출 시 “객체 → vptr → vtable → 함수 포인터” 순으로 <strong>런타임 디스패치</strong>가 일어나 다형성이 실현됩니다. vtable은 클래스별로 1개(템플릿/가상 상속 등으로 변형 가능)이고, 객체마다 생기는 추가 부담은 <strong>포인터 하나</strong> 수준입니다. 이 오버헤드 대비 얻는 설계 유연성은 대개 매우 큽니다.
</details>

---

<details>
<summary><strong>13) 순수 가상함수/추상 클래스 (꼬리: C++ 인터페이스 구현)</strong></summary>
<strong>A.</strong> <strong>순수 가상 함수</strong>(<code>=0</code>)는 구현이 없는 계약 메서드입니다. 이를 하나라도 가진 클래스는 <strong>추상 클래스</strong>가 되어 인스턴스화할 수 없고, 파생 클래스가 반드시 구현해야 합니다. C++에는 <strong>interface</strong> 키워드는 없지만, “<strong>모든 멤버가 순수 가상</strong>이고 <strong>가상 소멸자</strong>만 가진 베이스”를 관례적으로 인터페이스로 사용합니다. 이렇게 하면 상위는 <strong>무엇을</strong>만 의존하고, 하위는 <strong>어떻게</strong>를 자유롭게 바꿀 수 있어 개방-폐쇄 원칙을 실천하기 쉽습니다.
</details>

---

<details>
<summary><strong>14) 다형성 (꼬리: 오버로딩 vs 오버라이딩)</strong></summary>
<strong>A.</strong> 다형성은 <strong>같은 호출</strong>이 <strong>다른 동작</strong>을 수행하게 하는 능력입니다. C++에서 정적 다형성은 템플릿/오버로딩으로, 동적 다형성은 가상 함수 <strong>오버라이딩</strong>으로 구현합니다. <strong>오버로딩</strong>은 같은 이름의 함수를 <strong>서명</strong>으로 구분하며 <strong>컴파일 타임</strong>에 결정됩니다. <strong>오버라이딩</strong>은 베이스의 가상 함수를 파생이 재정의하고 <strong>런타임</strong>에 vtable을 통해 디스패치됩니다. 유지보수와 테스트 관점에서는 동적 다형성이 <strong>확장성</strong>을 크게 높여주며, 정적 다형성은 <strong>제로 오버헤드</strong>로 고성능 제네릭을 가능케 합니다.
</details>

---

<details>
<summary><strong>15) 템플릿 목적 (꼬리: 인스턴스화 시점)</strong></summary>
<strong>A.</strong> 템플릿은 <strong>타입/값을 매개변수화</strong>해 코드 중복 없이 범용 알고리즘/컨테이너를 제공합니다. 구체 타입이 실제로 사용되는 시점에 <strong>인스턴스화</strong>가 일어나며, 컴파일러는 각 조합에 대해 코드를 생성합니다. 과도한 조합은 <strong>코드 부풀림</strong>과 빌드 시간 증가를 유발하므로, 인터페이스를 간결히 하고 필요시 <strong>개념(Concepts)</strong>과 <strong>if constexpr</strong>로 제약을 명확히 하는 것이 좋습니다.
</details>

---

<details>
<summary><strong>16) 다중 상속 장단 (꼬리: 다이아몬드 문제 해결)</strong></summary>
<strong>A.</strong> 다중 상속은 여러 인터페이스를 동시에 구현할 수 있어 표현력이 높지만, <strong>모호성</strong>(동일 이름 충돌), <strong>다이아몬드 상속</strong>(중복 베이스) 같은 복잡성을 동반합니다. 다이아몬드 문제는 베이스를 <strong>virtual 상속</strong>으로 공유해 단 한 번만 존재하게 함으로써 해결합니다. 그래도 설계 복잡도와 디버깅 비용이 커지므로, 대부분의 경우 <strong>합성</strong>(has-a)과 인터페이스 상속의 조합이 더 단순하고 안전합니다.
</details>

---

<details>
<summary><strong>17) virtual 소멸자 필요성 (꼬리: 없으면?)</strong></summary>
<strong>A.</strong> 베이스 포인터/참조로 파생 객체를 <code>delete</code>할 때, 소멸자가 가상이 아니면 <strong>베이스 소멸자만 호출</strong>되어 파생 클래스가 소유한 자원(버퍼, 핸들)이 <strong>누수</strong>됩니다. 따라서 <strong>다형적 사용</strong>이 의도된 베이스에는 <strong>항상 가상 소멸자</strong>를 둡니다. 반대로 값-타입 베이스(다형성 미사용)에서는 불필요할 수 있습니다. 규칙: “<strong>가상 함수가 1개라도 있으면 소멸자도 virtual</strong>”.
</details>

---

<details>
<summary><strong>18) friend 역할 (꼬리: 남용 단점)</strong></summary>
<strong>A.</strong> <strong>friend</strong>는 특정 함수/클래스에 <strong>캡슐화 경계</strong>를 넘는 접근 권한을 부여합니다. 연산자 오버로딩(특히 대칭 연산), 테스트 도우미, 팩토리에서 내부 생성 경로 노출 없이 객체를 만들 때 유용합니다. 그러나 남용하면 <strong>결합도</strong>가 증가하고 캡슐화가 무너져 변경 파급이 커집니다. 경험적으로 <strong>지역적·한정된 범위</strong>에서만 사용하고, 필요하면 <strong>공개 API/친구 클래스 최소화</strong>로 대체합니다.
</details>

---

<details>
<summary><strong>19) 연산자 오버로딩 (꼬리: 멤버 vs 비멤버)</strong></summary>
<strong>A.</strong> 오버로딩은 “그 타입이 수학/컨테이너적으로 그렇게 동작하는 것이 자연스러울 때”만 도입해야 합니다. <strong>멤버</strong>로 구현하는 것이 적절한 경우는 <strong>할당(<code>=</code>)</strong>, 인덱싱(<code>operator[]</code>), 호출(<code>()</code>)처럼 <strong>좌변 객체</strong>에 본질적으로 결합된 연산입니다. 반대로 <strong>대칭성</strong>이 중요한 이항 연산(<code>+</code>, <code>==</code>)은 <strong>비멤버</strong>가 적합합니다(암시적 변환 여지도 넓음). <strong>I/O 연산자</strong>(<code>&lt;&lt;, &gt;&gt;</code>)도 보통 비멤버로 작성합니다. 의미 보존·부수효과 최소화·예외 안전을 지키는 것이 핵심입니다.
</details>

---

<details>
<summary><strong>20) 복사/이동 생성자 차이 (꼬리: 왜 move?)</strong></summary>
<strong>A.</strong> <strong>복사</strong>는 새로운 리소스를 할당해 내용을 <strong>복제</strong>하고, <strong>이동</strong>은 소유권을 <strong>“옮기고 비우는”</strong> 연산으로 원본을 안전한 비활성 상태로 둡니다(예: 포인터 null). 이동은 큰 버퍼/핸들을 다룰 때 할당·복사 비용을 획기적으로 줄여 <strong>컨테이너 재할당</strong>, <strong>함수 반환</strong>, <strong>임시 객체 연산</strong>에서 큰 성능 차이를 만듭니다. 이동 가능 타입에 <strong>noexcept 이동</strong>을 제공하면 <code>std::vector</code>가 재할당 시 복사 대신 이동을 선택해 <strong>더 적은 강제 복사/예외 안전</strong>을 확보합니다. 따라서 “이 타입은 옮길 수 있다”를 타입에 명시하는 것이 현대 C++ 성능 최적화의 기본입니다.
</details>

---

---
layout: post
title: "기술면접 대비 CS 공부 - C++ 50문항 모범답변"
date: 2025-10-02 15:00:00 +0900
categories: [Tech Interview, C++]
tags: [c++, interview, data-structure, algorithm, memory, oop, concurrency]
slug: cs-study-cpp-50-answers
---

# 🔷 C++ 면접 예상 질문 50선 – 모범답변

---

## 언어 기초 & 메모리

<details>
<summary><strong>1) 스택과 힙 메모리의 차이를 설명해주세요. (꼬리: 스택 오버플로우/힙 단편화)</strong></summary>
<strong>A.</strong> 스택은 함수 호출 시 자동으로 프레임이 생성·제거되는 LIFO 영역이며, 할당/해제가 매우 빠르고 크기가 한정적입니다. 힙은 런타임에 동적 크기로 할당되는 영역으로 유연하지만, 관리 비용과 단편화 가능성이 있습니다. 스택 오버플로우는 재귀 과다·큰 지역 배열 등으로 스택 한도를 초과할 때 발생하고, 힙의 외부 단편화는 가변 크기 블록의 잦은 할당/해제로 연속 큰 블록이 부족해지는 상황에서 나타납니다.
</details>

---

<details>
<summary><strong>2) RAII란 무엇인가요? (꼬리: 스마트 포인터와의 관계)</strong></summary>
<strong>A.</strong> RAII는 객체의 수명에 자원(메모리, 파일, 락 등)의 소유권을 귀속시켜 생성자에서 획득하고 소멸자에서 해제하는 기법입니다. 예외가 발생해도 소멸자가 보장되어 누수를 원천 차단합니다. <code>unique_ptr</code>, <code>shared_ptr</code>, <code>lock_guard</code> 같은 스마트 포인터/락이 RAII의 대표적인 적용 예입니다.
</details>

---

<details>
<summary><strong>3) 포인터와 참조의 차이는? (꼬리: nullptr vs NULL)</strong></summary>
<strong>A.</strong> 포인터는 재지정이 가능하고 null일 수 있으며 주소 연산이 가능합니다. 참조는 생성 시 반드시 바인딩되고 null이 될 수 없으며 재바인딩이 불가합니다. <code>nullptr</code>는 타입이 있는 빈 포인터 리터럴(C++11)로 오버로드 해석이 안전하고, <code>NULL</code>은 보통 0 매크로라 모호성을 유발할 수 있습니다.
</details>

---

<details>
<summary><strong>4) 얕은 복사 vs 깊은 복사 (꼬리: 복사/대입 구현 주의점)</strong></summary>
<strong>A.</strong> 얕은 복사는 포인터 값만 복제해 자원을 공유하고, 깊은 복사는 자원을 새로 할당해 독립 복제합니다. 사용자 정의 자원을 가진 타입은 “복사 생성자, 복사 대입, 소멸자”를 함께 설계해야 합니다. 예외 안전성을 위해 복사-후-스왑(idiom)과 강한 예외 보장을 고려합니다.
</details>

---

<details>
<summary><strong>5) const의 쓰임새 (꼬리: const 멤버함수 + mutable)</strong></summary>
<strong>A.</strong> <code>const</code>는 변수 불변, 포인터의 대상/자체 불변, 멤버 함수의 논리적 불변(객체 상태 변경 금지)을 표현합니다. <code>mutable</code> 멤버는 <code>const</code> 함수에서도 수정 가능하여 캐시/통계 같은 비본질 상태에 유용합니다. API 계약을 명확히 하고 최적화 기회를 제공합니다.
</details>

---

<details>
<summary><strong>6) static 의미 (꼬리: 초기화 시점)</strong></summary>
<strong>A.</strong> <code>static</code> 지역은 함수 호출 간 상태 유지(정적 수명), 전역/네임스페이스 영역에서는 내부 연결(linkage) 제어, 클래스 정적 멤버는 모든 인스턴스가 공유합니다. 정적 저장 수명 객체는 프로그램 시작 전/첫 사용 시 초기화가 보장되며(C++11 이후 정적 초기화 순서 안전성을 위해 함수 지역 정적 권장) 파괴는 종료 시점입니다.
</details>

---

<details>
<summary><strong>7) inline 함수 (꼬리: 컴파일러 최적화와의 관계)</strong></summary>
<strong>A.</strong> <code>inline</code>은 본래 ODR(중복 정의) 해결과 치환 최적화를 암시하나, 실제 치환 여부는 컴파일러가 판단합니다. 작은 함수·템플릿·접근자에 유리하지만 과도하면 코드 부풀림으로 아이캐시 압박을 유발합니다. 현대 컴파일러는 프로파일 기반으로 더 똑똑하게 결정하므로 무분별한 수동 지정은 지양합니다.
</details>

---

<details>
<summary><strong>8) 메모리 alignment/padding (꼬리: struct 낭비 줄이기)</strong></summary>
<strong>A.</strong> 하드웨어 정렬 제약을 맞추기 위해 필드 사이에 패딩이 들어가며 잘못 배치하면 메모리 낭비가 큽니다. 큰 타입부터 배치·동종 타입 묶기·패킹 지시자 사용(부작용 주의)로 낭비를 줄일 수 있습니다. 캐시라인 정렬은 false sharing을 줄이는 데도 중요합니다.
</details>

---

<details>
<summary><strong>9) volatile 역할 (꼬리: 멀티스레드 동기화 가능?)</strong></summary>
<strong>A.</strong> <code>volatile</code>은 메모리 매핑 IO 등 “컴파일러 최적화 금지” 신호이지 동기화 보장을 하지 않습니다. 멀티스레딩에는 <code>std::atomic</code>과 적절한 메모리 오더/뮤텍스를 사용해야 합니다. <code>volatile</code>만으로 재정렬/가시성 문제를 해결할 수 없습니다.
</details>

---

<details>
<summary><strong>10) new/delete vs malloc/free (꼬리: new가 malloc을 호출?)</strong></summary>
<strong>A.</strong> <code>new</code>는 타입 크기만큼 메모리를 할당하고 생성자를 호출하며, 실패 시 예외를 던집니다. <code>malloc</code>은 바이트 단위 할당만 하고 초기화/생성자를 호출하지 않으며 실패 시 <code>nullptr</code>을 반환합니다. 구현에 따라 <code>operator new</code>가 내부적으로 malloc 계열을 쓸 수 있으나 의미적 차이는 명확합니다.
</details>

---

## 객체지향 & 다형성

<details>
<summary><strong>11) OOP 핵심 개념 (꼬리: 캡슐화 vs 추상화)</strong></summary>
<strong>A.</strong> 캡슐화(데이터 은닉), 상속(재사용), 다형성(동적 디스패치), 추상화(본질만 노출)가 핵심입니다. 캡슐화는 내부 변경 영향 차단, 추상화는 복잡도 축소라는 점에서 목적이 다릅니다. 인터페이스로 “무엇”을 고정하고 구현으로 “어떻게”를 교체합니다.
</details>

---

<details>
<summary><strong>12) 가상 함수 동작 (꼬리: vtable은 객체/클래스 어디?)</strong></summary>
<strong>A.</strong> 각 객체는 vptr을 가지고, vptr은 클래스 단위로 존재하는 vtable을 가리킵니다. 호출 시 vtable에서 올바른 함수 포인터를 찾아 실행하는 런타임 바인딩이 일어납니다. vtable은 클래스별 1개, 객체마다 생기는 건 vptr입니다.
</details>

---

<details>
<summary><strong>13) 순수 가상함수/추상 클래스 (꼬리: C++ 인터페이스 구현)</strong></summary>
<strong>A.</strong> <code>virtual void f() = 0;</code>처럼 구현 없는 순수 가상으로 인터페이스 계약을 정의합니다. 해당 클래스는 인스턴스화 불가하고 파생 클래스가 구현해야 합니다. C++ 인터페이스는 “모든 멤버가 순수 가상인 클래스”로 관례적으로 표현합니다.
</details>

---

<details>
<summary><strong>14) 다형성 (꼬리: 오버로딩 vs 오버라이딩)</strong></summary>
<strong>A.</strong> 다형성은 동일 인터페이스로 다양한 실제 타입의 동작을 수행하는 능력입니다. 오버로딩은 같은 이름의 서로 다른 서명(컴파일타임 결정), 오버라이딩은 가상 함수의 런타임 재정의입니다. 유지보수성과 확장성 측면에서 오버라이딩이 구조적 이점을 줍니다.
</details>

---

<details>
<summary><strong>15) 템플릿 목적 (꼬리: 인스턴스화 시점)</strong></summary>
<strong>A.</strong> 템플릿은 타입/값을 매개변수화하여 제네릭 코드를 작성하게 합니다. 구체적 타입이 사용될 때(ODR 범위 내) 인스턴스화가 발생하며, 링크 단계에서 중복 제거가 이뤄집니다. 과도한 인스턴스화는 코드 부풀림을 초래할 수 있습니다.
</details>

---

<details>
<summary><strong>16) 다중 상속 장단 (꼬리: 다이아몬드 문제 해결)</strong></summary>
<strong>A.</strong> 장점은 여러 인터페이스 조합, 단점은 모호성과 복잡도 상승입니다. 다이아몬드 문제는 가상 상속(<code>virtual</code> 상속)으로 베이스를 1회만 공유해 해결합니다. 가능하면 합성으로 대체하는 것이 단순합니다.
</details>

---

<details>
<summary><strong>17) virtual 소멸자 필요성 (꼬리: 없으면?)</strong></summary>
<strong>A.</strong> 베이스 포인터로 파생 객체를 삭제할 때 소멸자가 가상이 아니면 베이스 소멸자만 호출되어 리소스 누수가 발생합니다. 인터페이스/폴리모픽 베이스는 반드시 가상 소멸자를 둡니다. 값 타입 베이스라면 불필요할 수 있습니다.
</details>

---

<details>
<summary><strong>18) friend 역할 (꼬리: 남용 단점)</strong></summary>
<strong>A.</strong> <code>friend</code>는 캡슐화 경계를 넘는 접근을 허용하여 특정 함수/클래스에 내부 접근권을 줍니다. 디버깅/연산자 구현에 유용하지만, 남용하면 결합도가 증가하고 변경 영향 범위가 커집니다. 테스트/디버그 전용으로 최소화합니다.
</details>

---

<details>
<summary><strong>19) 연산자 오버로딩 (꼬리: 멤버 vs 비멤버)</strong></summary>
<strong>A.</strong> 의미적 일관성을 지키고 부수효과를 최소화해야 합니다. 대칭성 요구가 있는 이항 연산(<code>operator==, +</code>)은 비멤버/프렌드가 일반적이며, 할당/인덱스 등은 멤버가 적합합니다. 스트림 연산자는 보통 비멤버로 구현합니다.
</details>

---

<details>
<summary><strong>20) 복사/이동 생성자 차이 (꼬리: 왜 move?)</strong></summary>
<strong>A.</strong> 복사는 자원을 새로 할당·복제하고, 이동은 소유권을 이전하며 원본을 안전한 비어있는 상태로 둡니다. 임시/대용량 리소스를 다룰 때 이동은 불필요한 할당/복사를 줄여 성능을 크게 개선합니다. 컨테이너 재할당, 반환 최적화에서도 이점이 큽니다.
</details>

---

## 자료구조 & 알고리즘

<details>
<summary><strong>21) vector 내부 동작 (꼬리: push_back vs emplace_back)</strong></summary>
<strong>A.</strong> <code>vector</code>는 연속 메모리에 요소를 보관하며, capacity가 차면 지수적으로 확장 후 기존 요소를 이동합니다. <code>push_back</code>은 완성된 객체를 복사/이동해 넣고, <code>emplace_back</code>은 인자에서 그 자리에서 직접 생성(퍼펙트 포워딩)해 임시·복사를 줄입니다. 이미 완성 객체가 있다면 차이는 적을 수 있지만, 복잡한 생성 비용을 아낄 때 emplace가 유리합니다.
</details>

---

<details>
<summary><strong>22) map vs unordered_map (꼬리: 충돌 처리)</strong></summary>
<strong>A.</strong> <code>map</code>은 균형 이진트리로 정렬/범위 탐색에 유리하고 O(log n) 보장, <code>unordered_map</code>은 해시 기반으로 평균 O(1) 탐색이 장점입니다. <code>unordered_map</code>은 보통 <strong>chaining</strong>으로 충돌을 해결하며 삭제/반복자 안정성이 좋아 표준 요구에 부합합니다. 부하율이 높으면 <code>rehash</code>/<code>reserve</code>로 조절합니다.
</details>

---

<details>
<summary><strong>23) set vs multiset (꼬리: unordered_set 구조)</strong></summary>
<strong>A.</strong> <code>set</code>은 유일 키, <code>multiset</code>은 중복 허용입니다. 둘 다 보통 균형트리(레드블랙)로 정렬 순서가 유지됩니다. <code>unordered_set</code>은 해시 테이블(보통 chaining)을 사용합니다.
</details>

---

<details>
<summary><strong>24) priority_queue 구현 (꼬리: 최소 힙으로 쓰고 싶다면?)</strong></summary>
<strong>A.</strong> 기본은 최대 힙이며 내부적으로 <code>vector</code> + 힙 알고리즘(<code>push_heap/pop_heap</code>)로 구성됩니다. 최소 힙이 필요하면 비교자를 <code>std::greater&lt;T&gt;</code>로 주거나 요소를 음수화하는 등 사용자 비교자를 지정합니다. 커스텀 타입은 <code>operator&lt;</code> 또는 비교자 제공이 필요합니다.
</details>

---

<details>
<summary><strong>25) std::sort 내부 (꼬리: 왜 작은 파티션은 삽입정렬?)</strong></summary>
<strong>A.</strong> <code>std::sort</code>는 Introsort: QuickSort로 시작해 깊이가 임계치를 넘으면 HeapSort로 전환, 작은 파티션은 Insertion Sort로 마무리합니다. 작은 구간에서는 분기/재귀 오버헤드보다 단순한 이동이 빠르고 캐시 적중률이 높기 때문에 삽입정렬이 실성능이 좋습니다. 이렇게 평균/최악/상수항을 모두 잡습니다.
</details>

---

<details>
<summary><strong>26) stack vs queue (꼬리: 스택 두 개로 큐 만들기)</strong></summary>
<strong>A.</strong> 스택은 LIFO, 큐는 FIFO입니다. 스택 두 개로 큐를 만들 때, 입력 스택에 push, 출력 스택에서 pop하며, 출력이 비었을 때 입력을 모두 옮깁니다. 각 원소는 최대 1회 이동되므로 분할상환 O(1)입니다.
</details>

---

<details>
<summary><strong>27) 해시 충돌 해결 (꼬리: C++이 chaining을 쓰는 이유)</strong></summary>
<strong>A.</strong> Chaining(버킷에 연결리스트/소형벡터)과 Open Addressing(선형/제곱/이중해시)이 대표적입니다. C++의 <code>unordered_*</code>가 chaining을 선호하는 이유는 삭제 용이, 반복자 안정성, 고부하에서도 성능 예측 가능, 할당자/노드 기반 구현과의 궁합 때문입니다. 충돌이 잦다면 테이블 크기 확장과 더 좋은 해시를 선택합니다.
</details>

---

<details>
<summary><strong>28) 링크드 리스트 뒤집기 (꼬리: 재귀/반복/스택 장단)</strong></summary>
<strong>A.</strong> 반복 포인터법이 O(n)/O(1)로 가장 실용적이고, 재귀는 코드가 간결하지만 스택 오버플로우 위험이 있습니다. 스택 사용은 직관적이지만 O(n) 추가 공간을 소모합니다. 상황과 제약에 따라 선택합니다.
</details>

---

<details>
<summary><strong>29) 힙 정렬 시간복잡도와 이유 (꼬리: 우선순위 큐와 차이)</strong></summary>
<strong>A.</strong> 힙 정렬은 힙 구성 O(n) + n회 추출×재힙화 O(log n)으로 항상 O(n log n)입니다. 우선순위 큐는 “정렬”이 아니라 “항상 최상위 원소 접근/갱신”이 목적이며, 스트리밍 상황에서 유리합니다. 힙정렬은 제자리 정렬/비안정, 우선순위 큐는 동적 작업에 최적화된 자료구조입니다.
</details>

---

<details>
<summary><strong>30) 좋은 해시 함수 조건 (꼬리: 충돌 과다 시 대처)</strong></summary>
<strong>A.</strong> 균등 분포, 빠른 계산, 입력 민감도를 갖추어야 합니다. 충돌이 과다하면 테이블 확장(로드 팩터 낮춤), 다른 해시 함수 채택, 키 정규화·프리해싱, 혹은 자료구조 자체를 트리 계열로 교체를 검토합니다.
</details>

---

## 고급 C++ & Modern C++

<details>
<summary><strong>31) 스마트 포인터 종류/차이 (꼬리: 순환 참조 해결)</strong></summary>
<strong>A.</strong> <code>unique_ptr</code> 단독 소유, 이동만 허용. <code>shared_ptr</code> 참조 카운트 기반 공유. <code>weak_ptr</code>은 비소유 참조로 순환을 끊습니다. <code>shared_ptr</code> 순환은 한 쪽을 <code>weak_ptr</code>로 바꿔 해결합니다.
</details>

---

<details>
<summary><strong>32) rvalue/lvalue (꼬리: std::move vs std::forward)</strong></summary>
<strong>A.</strong> lvalue는 재참조 가능한 이름 있는 객체, rvalue는 일시적·소모 가능한 값입니다. <code>std::move</code>는 lvalue를 rvalue로 캐스팅하는 신호(실제 이동 여부는 타입이 결정), <code>std::forward</code>는 전달 참조에서 원래 값 범주를 보존해 퍼펙트 포워딩을 가능케 합니다.
</details>

---

<details>
<summary><strong>33) 람다 vs 일반 함수 (꼬리: 캡처 방식 차이)</strong></summary>
<strong>A.</strong> 람다는 클로저 객체를 생성하며 지역 변수 캡처가 가능합니다. 값 캡처는 복사본을 보관해 안전하지만 비용이 있고, 참조 캡처는 원본에 접근해 비용이 적으나 수명에 주의해야 합니다. 무상태 람다는 함수 포인터로 변환될 수 있습니다.
</details>

---

<details>
<summary><strong>34) constexpr vs const (꼬리: constexpr 함수 실행 시점)</strong></summary>
<strong>A.</strong> <code>const</code>는 변경 불가를 의미하고, <code>constexpr</code>는 컴파일타임 계산 가능성을 의미합니다. <code>constexpr</code> 함수/변수는 조건이 맞으면 컴파일타임에 평가되고, 아니면 런타임에 평가될 수 있습니다. 상수식 요구 컨텍스트에서 성능·안전성을 제공합니다.
</details>

---

<details>
<summary><strong>35) decltype vs auto (꼬리: 템플릿 타입 추론 차이)</strong></summary>
<strong>A.</strong> <code>auto</code>는 초기화식으로부터 타입을 추론하고, <code>decltype</code>은 표현식의 타입(값 범주 포함)을 가져옵니다. 템플릿에서는 전달 규칙(참조 붕괴 등) 차이로 결과가 달라질 수 있어 의도를 명확히 해야 합니다. 반환 타입 지연(<code>auto f() -&gt; decltype(expr)</code>) 패턴이 자주 쓰입니다.
</details>

---

<details>
<summary><strong>36) noexcept 의미 (꼬리: 던지면?)</strong></summary>
<strong>A.</strong> <code>noexcept</code>는 함수가 예외를 던지지 않음을 약속하여 최적화와 강한 보장을 가능케 합니다. 만약 던지면 <code>std::terminate</code>가 호출됩니다. 이동 생성자/대입에 <code>noexcept</code>를 붙이면 컨테이너 최적화가 활성화됩니다.
</details>

---

<details>
<summary><strong>37) 스레드 실행 (꼬리: join vs detach)</strong></summary>
<strong>A.</strong> <code>std::thread</code>에 호출 대상 전달로 스레드를 시작합니다. <code>join()</code>은 종료까지 대기하여 리소스를 수거하고, <code>detach()</code>는 백그라운드로 분리하여 join할 수 없게 합니다(수명/리소스 누수 주의). 스레드 풀·태스크 기반을 선호합니다.
</details>

---

<details>
<summary><strong>38) future vs promise (꼬리: async 동작)</strong></summary>
<strong>A.</strong> <code>promise</code>는 값을 세팅하는 쪽, <code>future</code>는 결과를 받는 쪽입니다. <code>std::async</code>는 작업을 비동기 실행하여 <code>future</code>를 반환하며 정책에 따라 즉시/지연 실행될 수 있습니다. <code>get()</code>은 결과 준비까지 블록합니다.
</details>

---

<details>
<summary><strong>39) 메모리 풀을 구현하는 이유 (꼬리: 게임에서 오브젝트 풀링)</strong></summary>
<strong>A.</strong> 동형·소형 객체의 빈번한 new/delete를 줄여 단편화와 할당 비용을 낮춥니다. 고정 크기 블록 슬라브/프리리스트로 O(1) 할당을 제공합니다. 게임에서는 총알/이펙트 같은 오브젝트를 재사용해 스파이크 없는 프레임을 유지합니다.
</details>

---

<details>
<summary><strong>40) explicit 생성자 필요성 (꼬리: 없으면 문제?)</strong></summary>
<strong>A.</strong> 단일 인자 생성자를 <code>explicit</code>로 지정하면 암시적 변환을 막아 모호·실수 호출을 방지합니다. 없으면 의도치 않은 변환으로 오버로드 해석이 흐트러지고 성능·정확성 문제가 생길 수 있습니다. API는 명시성이 중요합니다.
</details>

---

## 디자인 원칙 & 실무 응용

<details>
<summary><strong>41) SOLID 원칙 (꼬리: 모두 중요하다면 적용 방안)</strong></summary>
<strong>A.</strong> <strong>S</strong> 단일 책임: 변경 이유 하나로 모듈 분리. <strong>O</strong> 개방-폐쇄: 인터페이스/전략으로 확장, 기존 수정 최소화. <strong>L</strong> 리스코프 치환: 계약 준수·공변 반환으로 대체 가능성 확보. <strong>I</strong> 인터페이스 분리: 비대한 인터페이스를 역할별로 분할. <strong>D</strong> 의존 역전: 고수준이 구현이 아니라 추상에 의존, DI/팩토리로 주입. 모두 중요하므로 아키텍처 가이드·리뷰 체크리스트·테스트 전략에 각 원칙을 명문화합니다.
</details>

---

<details>
<summary><strong>42) 싱글톤 구현 (꼬리: 멀티스레드 안전)</strong></summary>
<strong>A.</strong> 마이어스 싱글톤(함수 지역 정적)으로 간단히 구현합니다. C++11 이후 정적 초기화는 스레드 안전이 보장됩니다. 전역 상태·테스트 어려움·수명 관리 문제로 남용은 피하고 DI를 고려합니다.
</details>

---

<details>
<summary><strong>43) 팩토리 패턴 (꼬리: 추상 팩토리 유용성)</strong></summary>
<strong>A.</strong> 생성 로직을 캡슐화해 클라이언트가 구체 타입을 모르게 합니다. 추상 팩토리는 제품군(서로 연관된 객체 집합)을 일관되게 생성할 때 유용하며, 교체·테마링이 쉬워집니다. DI 컨테이너와 궁합이 좋습니다.
</details>

---

<details>
<summary><strong>44) Observer 패턴 (꼬리: 이벤트 디스패처와 차이)</strong></summary>
<strong>A.</strong> 주체가 상태 변화 시 구독자에게 알리는 비동기 알림 구조입니다. 이벤트 디스패처/버스는 더 범용적이며 큐잉/필터링/우선순위 같은 인프라 기능이 추가됩니다. 결합을 낮추지만 무분별한 브로드캐스트는 디버깅을 어렵게 합니다.
</details>

---

<details>
<summary><strong>45) Command 패턴 (꼬리: Undo/Redo 설계)</strong></summary>
<strong>A.</strong> 요청을 객체로 캡슐화하여 큐잉/로깅/취소를 가능케 합니다. Undo/Redo는 <em>do/undo</em> 연산을 보관하거나 Memento로 상태 스냅샷을 관리합니다. 명령 병합/기록 한도/영속화 정책을 함께 설계합니다.
</details>

---

<details>
<summary><strong>46) 템플릿 메타프로그래밍(TMP) (꼬리: 장단점)</strong></summary>
<strong>A.</strong> 컴파일타임 계산·분기·타입 조작으로 런타임 오버헤드를 제거합니다(컨셉/if constexpr로 가독성 개선). 장점은 제로오버헤드·강한 타입 안전성, 단점은 컴파일 시간 증가·에러 메시지 난해함·코드 복잡도 상승입니다.
</details>

---

<details>
<summary><strong>47) C++ 모듈 vs 헤더 (꼬리: 빌드 속도 개선)</strong></summary>
<strong>A.</strong> 모듈은 파싱 결과를 BMI로 캐시해 중복 파싱을 제거하고 ODR 문제/매크로 오염을 줄입니다. 대규모 코드에서 빌드 시간을 크게 감소시킵니다. 점진 도입 시 모듈 경계 설계와 서드파티 호환을 검토합니다.
</details>

---

<details>
<summary><strong>48) 빌드 최적화 O1/O2/O3/Ofast (꼬리: O3 위험성)</strong></summary>
<strong>A.</strong> O1은 기본, O2는 균형잡힌 최적화, O3/Ofast는 공격적인 벡터화/루프 변환·정확성 무시(UB 가정 확대)를 수행합니다. O3는 미세한 UB가 성능은↑ 버그도↑를 야기할 수 있어 검증이 중요합니다. 코어 루틴만 선택적으로 상향하는 전략이 현실적입니다.
</details>

---

<details>
<summary><strong>49) 디버그 vs 릴리즈 (꼬리: 디버그에서 inline?)</summary>
<strong>A.</strong> 디버그는 어서션·심볼·최적화 해제, 릴리즈는 최적화 활성·심볼 축소입니다. 디버그에서는 인라인 치환이 제한되거나 비활성화되어 호출 경로가 유지됩니다. 릴리즈에서만 드러나는 타이밍/최적화 이슈도 있으므로 양쪽 테스트가 필수입니다.
</details>

---

<details>
<summary><strong>50) 게임 개발에서 C++을 쓰는 이유 (꼬리: 엔진에 유리한 특성)</strong></summary>
<strong>A.</strong> 성능·메모리 제어·하드웨어 접근·언어 생태(툴/엔진)가 강력하고, 제로오버헤드 추상화로 고수준 인터페이스와 저수준 제어를 동시에 제공합니다. 이동语义/맞춤 할당자/지속 메모리/멀티스레딩 지원이 엔진 코어에 적합합니다. 타 언어 스크립팅과의 하이브리드도 용이합니다.
</details>

---
