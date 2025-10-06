---
layout: post
title: "기술면접 대비 CS 공부 - CPP"
date: 2025-10-04 16:00:00 +0900
categories: [Tech Interview, Study Plan]
tags: [c++, c-sharp, computer-science, algorithm, data-structure, operating-system, network, database, design-pattern, unity, unreal]
slug: csstudyforfinalpartthree
---

# 면접 대비 사전 QnA 정리 - CPP


# 🔷 C++ 면접 예상 질문 50선 – 모범답변 (확장본 1/3: 1–20)

---

## 언어 기초 & 메모리

<details markdown="1">
<summary><strong>1) 스택과 힙 메모리의 차이를 설명해주세요. (꼬리: 스택 오버플로우/힙 단편화)</strong></summary>


<strong>A.</strong> <strong>스택</strong>은 함수 호출 시 콜 프레임이 LIFO로 쌓였다가 함수가 끝나는 순간 자동으로 회수되는 메모리 영역입니다. 지역 변수와 반환 주소, 일부 레지스터 보관 등 아주 짧은 수명의 데이터에 최적화되어 있고, 증감이 단순한 포인터 이동만으로 끝나므로 <strong>매우 빠릅니다</strong>. 다만 OS가 할당한 <strong>스택 크기 한계</strong>(수 MB 수준)를 넘으면 <strong>스택 오버플로우</strong>가 발생합니다. 재귀 깊이가 비정상적으로 깊거나, 큰 배열을 지역에 잡을 때 자주 문제가 됩니다.

<strong>힙</strong>은 <strong>동적 할당</strong>에 쓰이며, 런타임에 필요 용량만큼 자유롭게 요청/반납할 수 있습니다. 유연하지만, 내부적으로는 <strong>프리 리스트 관리/병합/분할</strong> 같은 부가 작업이 필요해서 오버헤드가 있고, 다양한 크기의 블록이 들락날락하면 <strong>외부 단편화</strong>가 생겨 큰 연속 블록이 부족한 상황이 발생할 수 있습니다. 이를 완화하기 위해 <strong>풀 알로케이터/슬랩</strong> 같은 도메인 맞춤 할당기를 사용하거나, 큰 객체를 묶어 배치하고 재사용(Object Pool)하는 전략을 병행합니다. 결과적으로 <strong>짧고 예측 가능한 수명</strong>은 스택, <strong>가변 용량·가변 수명</strong>은 힙이 담당한다고 정리합니다.

</details>
<br>

<br>

<details markdown="1">
<summary><strong>2) RAII란 무엇인가요? (꼬리: 스마트 포인터와의 관계)</strong></summary>


<strong>A.</strong> <strong>RAII(Resource Acquisition Is Initialization)</strong>는 “자원 획득 = 객체 초기화”라는 철학으로, <strong>생성자에서 자원을 획득</strong>하고 <strong>소멸자에서 반드시 반납</strong>하도록 해 예외·조기 반환이 있어도 누수가 없게 만드는 C++의 핵심 관용구입니다. 파일 핸들, 소켓, 뮤텍스 락, 그래픽스 핸들, 메모리 등 모든 <strong>유한 자원</strong>에 적용할 수 있고, 스코프를 벗어나는 순간 자동 정리가 보장됩니다.  
스마트 포인터는 RAII의 대표 사례입니다. <strong>unique_ptr</strong>는 소유권을 단독 보유하며 스코프 종료 시 <strong>delete</strong>를 자동 호출해 누수를 차단하고, <strong>shared_ptr</strong>는 참조 카운트로 여러 개가 소유를 공유하되 마지막 포인터가 파괴될 때 자원을 해제합니다. <strong>weak_ptr</strong>는 순환 참조를 끊는 비소유 링크로, RAII가 자원 수명과 소유권을 <strong>타입 시스템</strong>으로 안전하게 모델링하게 해줍니다. 락도 <strong>lock_guard/scoped_lock</strong>로 감싸면 예외가 나도 자동 해제가 보장됩니다.
</details>
<br>

---

<details markdown="1">
<summary><strong>3) 포인터와 참조의 차이는? (꼬리: nullptr vs NULL)</strong></summary>
<strong>A.</strong> <strong>포인터</strong>는 메모리 주소를 담는 <strong>값 객체</strong>로 재지정이 가능하고 <strong>null</strong>이 될 수 있습니다. 산술 연산과 배열 인덱싱도 허용되어 강력하지만, 잘못 쓰면 댕글링/더블 딜리트 같은 치명적 버그를 초래할 수 있습니다. <strong>참조</strong>는 “다른 객체의 또 다른 이름(alias)”로 <strong>반드시 유효한 객체에 바인딩</strong>되어야 하고 일단 바인딩되면 다른 대상을 가리키도록 바꿀 수 없습니다(재바인딩 불가). 문법적으로 편하고 최적화에도 유리해, <strong>API 매개변수</strong>에서 “반드시 있어야 하는 인자” 표현에 적합합니다.  
<code>nullptr</code>는 C++11에 도입된 <strong>타입 안전한 널 리터럴</strong>입니다. 오버로드 해석에서 <code>0</code> 또는 <code>NULL</code>(대개 <code>0</code> 매크로)보다 모호성이 적고, <code>void*</code>로의 암시적 변환도 의도적으로 제한됩니다. 면접에선 “포인터는 선택/재지정/널 가능, 참조는 필수/재지정 불가/널 불가”로 간결히 대비하면 좋습니다.
</details>
<br>

---

<details markdown="1">
<summary><strong>4) 얕은 복사 vs 깊은 복사 (꼬리: 복사/대입 구현 주의점)</strong></summary>
<strong>A.</strong> <strong>얕은 복사</strong>는 포인터 값만 복제해 <strong>리소스를 공유</strong>하는 형태(두 객체가 같은 버퍼를 가리킴)이고, <strong>깊은 복사</strong>는 버퍼 자체를 새로 할당하고 내용을 복제해 <strong>독립 소유</strong>를 보장합니다. 사용자 정의 리소스를 가진 타입은 <strong>Rule of 5</strong>(복사/이동 생성자, 복사/이동 대입, 소멸자)를 염두에 두고 일관되게 소유권 의미를 구현해야 합니다. 예외 안전성은 <strong>복사 후 스왑</strong> 관용구로 강화할 수 있고, 대입 연산에서는 <strong>자기 대입 방지</strong>, <strong>리소스 릭 없이 강한 보장</strong>(실패 시 원자성 유지)을 신경 써야 합니다. 큰 버퍼는 카피-온-라이트 또는 <strong>참조 카운팅</strong>으로 비용을 완화할 수 있지만 멀티스레딩과의 상호작용을 면밀히 설계해야 합니다.
</details>
<br>

---

<details markdown="1">
<summary><strong>5) const의 쓰임새 (꼬리: const 멤버함수 + mutable)</strong></summary>
<strong>A.</strong> <strong>const</strong>는 “불변 계약”을 타입에 새기기 위한 키워드입니다. <strong>객체 자체</strong>를 const로 만들면 모든 <strong>비-<code>mutable</code></strong> 멤버를 바꿀 수 없고, <strong>포인터</strong>에는 “포인터가 가리키는 대상이 불변인지, 포인터 자체가 불변인지”를 각각 표시할 수 있습니다. <strong>const 멤버 함수</strong>는 “논리적으로 상태를 바꾸지 않는다”라는 API 계약을 표현하고, 호출자는 해당 함수가 외부 관찰 가능한 상태를 변형하지 않음을 신뢰할 수 있습니다. <strong>mutable</strong>은 캐시/통계 카운터처럼 <strong>관찰 가능한 불변성</strong>을 깨지 않는 내부 상태를 예외적으로 변경 가능케 합니다. const는 최적화 관점에서도 유리해, 컴파일러가 <strong>알리아싱 가정</strong>을 안전히 할 수 있도록 돕습니다.
</details>
<br>

---

<details markdown="1">
<summary><strong>6) static 의미 (꼬리: 초기화 시점)</strong></summary>
<strong>A.</strong> <strong>static 지역 변수</strong>는 함수 호출 간 값을 유지하는 정적 수명이고, <strong>네임스페이스/전역의 static</strong>은 내부 연결(linkage)을 만들어 <strong>번역 단위</strong>에 심볼을 숨깁니다. <strong>클래스 정적 멤버</strong>는 모든 인스턴스가 공유하는 <strong>한 벌의 저장소</strong>를 의미합니다. 초기화는 <strong>정적 초기화</strong>(제로/상수) → <strong>동적 초기화</strong>(런타임 코드 필요)로 나뉘며, C++11 이후 <strong>함수 지역 정적</strong>은 <strong>스레드 안전한 지연 초기화</strong>가 보장됩니다(“마이어스 싱글톤” 구현 근거). 초기화 순서 의존성 문제를 피하려면 전역 정적 대신 함수 지역 정적·모듈 초기화 패턴을 권장합니다.
</details>
<br>

---

<details markdown="1">
<summary><strong>7) inline 함수 (꼬리: 컴파일러 최적화와의 관계)</strong></summary>
<strong>A.</strong> <strong>inline</strong>의 역사적 의미는 “중복 정의 허용(ODR 해결)” + “치환 힌트”입니다. 오늘날 치환 여부는 <strong>컴파일러/링커의 결정</strong>이며, PGO/LTO 같은 최적화가 더 큰 영향력을 가집니다. 인라인은 <strong>호출 오버헤드 제거</strong>·<strong>레지스터 할당 최적화</strong> 같은 이점이 있지만, <strong>코드 부피 증가</strong>로 아이캐시 압박이 생길 수 있습니다. 일반적으로 아주 작은 단순 접근자/래퍼, 빈번 호출 경로에만 신중히 적용하고, 헤더 정의가 필요한 템플릿/헤더 온리 유틸리티에서는 자연스럽게 inline 취급됩니다.
</details>
<br>

---

<details markdown="1">
<summary><strong>8) 메모리 alignment/padding (꼬리: struct 낭비 줄이기)</strong></summary>
<strong>A.</strong> CPU는 특정 경계 정렬에서 데이터를 읽을 때 가장 효율적입니다. 구조체는 필드의 정렬 요건을 만족하기 위해 <strong>패딩</strong>을 삽입하고, 마지막에도 구조체 전체 정렬에 맞게 패딩이 붙습니다. 메모리 낭비를 줄이려면 <strong>큰 타입부터 배치</strong>하고, 동일한 크기의 필드를 묶으며, 필요 시 <strong>패킹 지시자</strong>를 쓰되(예: <code>#pragma pack</code>) <strong>잘못 정렬된 접근</strong>이 성능 저하나 하드웨어 예외를 유발할 수 있음을 이해해야 합니다. 멀티스레딩에서는 <strong>캐시라인 정렬</strong>로 false sharing을 피하는 것도 중요합니다.
</details>
<br>

---

<details markdown="1">
<summary><strong>9) volatile 역할 (꼬리: 멀티스레드 동기화 가능?)</strong></summary>
<strong>A.</strong> <strong>volatile</strong>은 컴파일러 최적화에 “이 위치의 값은 외부 요인으로 바뀔 수 있으니 매번 메모리에서 읽어라”를 지시합니다(메모리 매핑 I/O 등). 하지만 <strong>재정렬 방지/가시성/원자성</strong>을 보장하지 않으므로 <strong>멀티스레드 동기화</strong>에는 적합하지 않습니다. 스레드 간 통신에는 <strong>std::atomic</strong>(필요 시 메모리 오더 지정)과 <strong>뮤텍스/조건변수</strong>를 사용해야 합니다. 가끔 임베디드에서 하드웨어 레지스터 접근에 한정해 의미가 있습니다.
</details>
<br>

---

<details markdown="1">
<summary><strong>10) new/delete vs malloc/free (꼬리: new가 malloc을 호출?)</strong></summary>
<strong>A.</strong> <strong>new</strong>는 <strong>operator new</strong>를 통해 메모리를 확보하고 <strong>생성자</strong>를 호출합니다. 실패 시 기본적으로 <strong>예외</strong>(<code>std::bad_alloc</code>)를 던집니다. <strong>malloc</strong>은 <strong>바이트 버퍼만</strong> 반환하며 생성자 호출이 없고, 실패 시 <strong>nullptr</strong>을 반환합니다. 구현에 따라 <strong>operator new</strong>가 내부적으로 <strong>malloc</strong>을 사용할 수 있지만, 의미론은 분명히 다릅니다. 커스텀 <strong>operator new/delete</strong>로 풀/아레나 할당기를 주입하는 패턴은 고성능 시스템에서 흔합니다.
</details>
<br>

---

## 객체지향 & 다형성

<details markdown="1">
<summary><strong>11) OOP 핵심 개념 (꼬리: 캡슐화 vs 추상화)</strong></summary>
<strong>A.</strong> OOP의 핵심은 <strong>캡슐화</strong>(데이터 은닉과 경계 설정), <strong>상속</strong>(행동 재사용), <strong>다형성</strong>(공통 인터페이스로 다른 구현 호출), <strong>추상화</strong>(본질만 노출)입니다. <strong>캡슐화</strong>는 “변경의 파급을 경계 내부로 가두는 일”이고, <strong>추상화</strong>는 “불필요한 세부를 가려 사용자를 단순화”합니다. 예를 들어 렌더링 파이프라인에서 상위 계층은 <strong>IRenderer</strong> 인터페이스만 의존하고, DirectX/Metal/Vulkan 구현은 <strong>교체 가능</strong>하도록 숨겨집니다. 이는 테스트가 쉬워지고, 교체/확장에 강한 구조를 만듭니다.
</details>
<br>

---

<details markdown="1">
<summary><strong>12) 가상 함수 동작 (꼬리: vtable은 객체/클래스 어디?)</strong></summary>
<strong>A.</strong> 가상 함수가 하나라도 있는 클래스의 각 객체는 <strong>vptr</strong>을 가집니다. vptr은 <strong>클래스 단위</strong>로 존재하는 <strong>vtable</strong>을 가리키고, vtable 안에는 가상 함수들의 <strong>실제 구현 주소</strong>가 순서대로 들어 있습니다. 호출 시 “객체 → vptr → vtable → 함수 포인터” 순으로 <strong>런타임 디스패치</strong>가 일어나 다형성이 실현됩니다. vtable은 클래스별로 1개(템플릿/가상 상속 등으로 변형 가능)이고, 객체마다 생기는 추가 부담은 <strong>포인터 하나</strong> 수준입니다. 이 오버헤드 대비 얻는 설계 유연성은 대개 매우 큽니다.
</details>
<br>

---

<details markdown="1">
<summary><strong>13) 순수 가상함수/추상 클래스 (꼬리: C++ 인터페이스 구현)</strong></summary>
<strong>A.</strong> <strong>순수 가상 함수</strong>(<code>=0</code>)는 구현이 없는 계약 메서드입니다. 이를 하나라도 가진 클래스는 <strong>추상 클래스</strong>가 되어 인스턴스화할 수 없고, 파생 클래스가 반드시 구현해야 합니다. C++에는 <strong>interface</strong> 키워드는 없지만, “<strong>모든 멤버가 순수 가상</strong>이고 <strong>가상 소멸자</strong>만 가진 베이스”를 관례적으로 인터페이스로 사용합니다. 이렇게 하면 상위는 <strong>무엇을</strong>만 의존하고, 하위는 <strong>어떻게</strong>를 자유롭게 바꿀 수 있어 개방-폐쇄 원칙을 실천하기 쉽습니다.
</details>
<br>

---

<details markdown="1">
<summary><strong>14) 다형성 (꼬리: 오버로딩 vs 오버라이딩)</strong></summary>
<strong>A.</strong> 다형성은 <strong>같은 호출</strong>이 <strong>다른 동작</strong>을 수행하게 하는 능력입니다. C++에서 정적 다형성은 템플릿/오버로딩으로, 동적 다형성은 가상 함수 <strong>오버라이딩</strong>으로 구현합니다. <strong>오버로딩</strong>은 같은 이름의 함수를 <strong>서명</strong>으로 구분하며 <strong>컴파일 타임</strong>에 결정됩니다. <strong>오버라이딩</strong>은 베이스의 가상 함수를 파생이 재정의하고 <strong>런타임</strong>에 vtable을 통해 디스패치됩니다. 유지보수와 테스트 관점에서는 동적 다형성이 <strong>확장성</strong>을 크게 높여주며, 정적 다형성은 <strong>제로 오버헤드</strong>로 고성능 제네릭을 가능케 합니다.
</details>
<br>

---

<details markdown="1">
<summary><strong>15) 템플릿 목적 (꼬리: 인스턴스화 시점)</strong></summary>
<strong>A.</strong> 템플릿은 <strong>타입/값을 매개변수화</strong>해 코드 중복 없이 범용 알고리즘/컨테이너를 제공합니다. 구체 타입이 실제로 사용되는 시점에 <strong>인스턴스화</strong>가 일어나며, 컴파일러는 각 조합에 대해 코드를 생성합니다. 과도한 조합은 <strong>코드 부풀림</strong>과 빌드 시간 증가를 유발하므로, 인터페이스를 간결히 하고 필요시 <strong>개념(Concepts)</strong>과 <strong>if constexpr</strong>로 제약을 명확히 하는 것이 좋습니다.
</details>
<br>

---

<details markdown="1">
<summary><strong>16) 다중 상속 장단 (꼬리: 다이아몬드 문제 해결)</strong></summary>
<strong>A.</strong> 다중 상속은 여러 인터페이스를 동시에 구현할 수 있어 표현력이 높지만, <strong>모호성</strong>(동일 이름 충돌), <strong>다이아몬드 상속</strong>(중복 베이스) 같은 복잡성을 동반합니다. 다이아몬드 문제는 베이스를 <strong>virtual 상속</strong>으로 공유해 단 한 번만 존재하게 함으로써 해결합니다. 그래도 설계 복잡도와 디버깅 비용이 커지므로, 대부분의 경우 <strong>합성</strong>(has-a)과 인터페이스 상속의 조합이 더 단순하고 안전합니다.
</details>
<br>

---

<details markdown="1">
<summary><strong>17) virtual 소멸자 필요성 (꼬리: 없으면?)</strong></summary>
<strong>A.</strong> 베이스 포인터/참조로 파생 객체를 <code>delete</code>할 때, 소멸자가 가상이 아니면 <strong>베이스 소멸자만 호출</strong>되어 파생 클래스가 소유한 자원(버퍼, 핸들)이 <strong>누수</strong>됩니다. 따라서 <strong>다형적 사용</strong>이 의도된 베이스에는 <strong>항상 가상 소멸자</strong>를 둡니다. 반대로 값-타입 베이스(다형성 미사용)에서는 불필요할 수 있습니다. 규칙: “<strong>가상 함수가 1개라도 있으면 소멸자도 virtual</strong>”.
</details>
<br>

---

<details markdown="1">
<summary><strong>18) friend 역할 (꼬리: 남용 단점)</strong></summary>
<strong>A.</strong> <strong>friend</strong>는 특정 함수/클래스에 <strong>캡슐화 경계</strong>를 넘는 접근 권한을 부여합니다. 연산자 오버로딩(특히 대칭 연산), 테스트 도우미, 팩토리에서 내부 생성 경로 노출 없이 객체를 만들 때 유용합니다. 그러나 남용하면 <strong>결합도</strong>가 증가하고 캡슐화가 무너져 변경 파급이 커집니다. 경험적으로 <strong>지역적·한정된 범위</strong>에서만 사용하고, 필요하면 <strong>공개 API/친구 클래스 최소화</strong>로 대체합니다.
</details>
<br>

---

<details markdown="1">
<summary><strong>19) 연산자 오버로딩 (꼬리: 멤버 vs 비멤버)</strong></summary>
<strong>A.</strong> 오버로딩은 “그 타입이 수학/컨테이너적으로 그렇게 동작하는 것이 자연스러울 때”만 도입해야 합니다. <strong>멤버</strong>로 구현하는 것이 적절한 경우는 <strong>할당(<code>=</code>)</strong>, 인덱싱(<code>operator[]</code>), 호출(<code>()</code>)처럼 <strong>좌변 객체</strong>에 본질적으로 결합된 연산입니다. 반대로 <strong>대칭성</strong>이 중요한 이항 연산(<code>+</code>, <code>==</code>)은 <strong>비멤버</strong>가 적합합니다(암시적 변환 여지도 넓음). <strong>I/O 연산자</strong>(<code>&lt;&lt;, &gt;&gt;</code>)도 보통 비멤버로 작성합니다. 의미 보존·부수효과 최소화·예외 안전을 지키는 것이 핵심입니다.
</details>
<br>

---

<details markdown="1">
<summary><strong>20) 복사/이동 생성자 차이 (꼬리: 왜 move?)</strong></summary>
<strong>A.</strong> <strong>복사</strong>는 새로운 리소스를 할당해 내용을 <strong>복제</strong>하고, <strong>이동</strong>은 소유권을 <strong>“옮기고 비우는”</strong> 연산으로 원본을 안전한 비활성 상태로 둡니다(예: 포인터 null). 이동은 큰 버퍼/핸들을 다룰 때 할당·복사 비용을 획기적으로 줄여 <strong>컨테이너 재할당</strong>, <strong>함수 반환</strong>, <strong>임시 객체 연산</strong>에서 큰 성능 차이를 만듭니다. 이동 가능 타입에 <strong>noexcept 이동</strong>을 제공하면 <code>std::vector</code>가 재할당 시 복사 대신 이동을 선택해 <strong>더 적은 강제 복사/예외 안전</strong>을 확보합니다. 따라서 “이 타입은 옮길 수 있다”를 타입에 명시하는 것이 현대 C++ 성능 최적화의 기본입니다.
</details>
<br>

---
