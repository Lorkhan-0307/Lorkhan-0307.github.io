---
layout: post
title: "기술면접 대비 CS 공부 - CPP"
date: 2025-10-04 16:00:00 +0900
categories: [Tech Interview, Study Plan]
tags: [c++, c-sharp, computer-science, algorithm, data-structure, operating-system, network, database, design-pattern, unity, unreal]
slug: csstudyforfinalpartthree
---

# 면접 대비 사전 QnA 정리 - CPP

## 언어 기초 & 메모리

<details markdown="1">
<summary><strong>1) 스택과 힙 메모리의 차이를 설명해주세요. (꼬리: 스택 오버플로우/힙 단편화)</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- <strong>스택</strong>: 자동 관리, 빠름, 크기 제한 존재  
- <strong>힙</strong>: 동적 관리, 유연함, 단편화 가능  

---

<strong>🔹 특징 및 상세설명</strong>  
- 스택은 함수 호출 시 자동으로 공간이 할당되고 반환되며, 지역 변수와 매개변수, 반환 주소 등이 저장된다.  
- 과도한 재귀나 대형 지역 배열로 인해 스택 한계를 넘으면 스택 오버플로우가 발생한다.  
- 힙은 런타임에 동적으로 할당되며, 다양한 크기의 블록이 섞이면 외부 단편화가 생긴다.  
- 단편화를 줄이기 위해 <strong>메모리 풀, 아레나 할당기, 오브젝트 풀</strong> 등을 사용한다.

---

<strong>💬 면접식 답변</strong>  
> 스택은 자동으로 관리되어 빠르지만 크기 제한이 있고, 힙은 동적 할당으로 유연하지만 단편화가 발생할 수 있습니다.  
> 즉, 짧은 수명엔 스택, 가변적인 수명엔 힙이 적합합니다.

</details>

---

<details markdown="1">
<summary><strong>2) RAII란 무엇인가요? (꼬리: 스마트 포인터와의 관계)</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- “자원 획득 = 객체 초기화”  
- 생성자에서 획득, 소멸자에서 자동 해제  

---

<strong>🔹 특징 및 상세설명</strong>  
- RAII는 C++의 핵심 철학으로, 객체 수명에 자원 관리(파일, 소켓, 메모리 등)를 결합한다.  
- 예외가 발생하더라도 소멸자가 호출되어 자원이 자동 해제된다.  
- 대표 구현: <strong>unique_ptr</strong> (단독 소유), <strong>shared_ptr</strong> (참조 카운팅 공유), <strong>lock_guard</strong> (뮤텍스 자동 해제).  

---

<strong>💬 면접식 답변</strong>  
> RAII는 객체의 수명과 자원 관리를 연결하는 C++의 핵심 개념입니다.  
> 생성자에서 자원을 획득하고 소멸자에서 해제되므로, 예외가 발생해도 안전하게 관리됩니다.  
> 스마트 포인터는 RAII의 대표적인 예시입니다.

</details>

---

<details markdown="1">
<summary><strong>3) 포인터와 참조의 차이는? (꼬리: nullptr vs NULL)</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 포인터: 주소 저장, 재지정 가능, nullptr 가능  
- 참조: 별칭, 재지정 불가, null 불가  

---

<strong>🔹 특징 및 상세설명</strong>  
- 포인터는 메모리 주소를 직접 다루며 연산 가능하지만, 안전하지 않다.  
- 참조는 유효한 객체를 반드시 가리켜야 하고 null 상태가 될 수 없다.  
- C++11의 <strong>nullptr</strong>은 타입 안전한 null 리터럴이며, <strong>NULL</strong>보다 명확하다.  

---

<strong>💬 면접식 답변</strong>  
> 포인터는 메모리 주소를 저장하는 변수이고 null이 가능하지만,  
> 참조는 단순히 객체의 별칭이며 null이 될 수 없습니다.  
> C++11 이후에는 nullptr을 사용하여 타입 안전성을 확보합니다.

</details>

---

<details markdown="1">
<summary><strong>4) 얕은 복사와 깊은 복사의 차이는 무엇인가요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 얕은 복사: 포인터 주소만 복제 (리소스 공유)  
- 깊은 복사: 리소스 자체를 새로 복사 (독립 소유)  

---

<strong>🔹 특징 및 상세설명</strong>  
- 얕은 복사는 두 객체가 동일한 리소스를 가리켜 하나가 해제되면 다른 쪽은 댕글링 포인터 위험이 있다.  
- 깊은 복사는 메모리를 새로 할당해 데이터를 복제하므로 안전하다.  
- 리소스를 관리하는 클래스는 <strong>Rule of 5</strong>(복사/이동 생성자, 복사/이동 대입, 소멸자)를 반드시 정의해야 한다.  

---

<strong>💬 면접식 답변</strong>  
> 얕은 복사는 주소만 복제하여 리소스를 공유하지만, 깊은 복사는 리소스 자체를 새로 할당해 독립성을 보장합니다.  
> 리소스 소유 타입에서는 Rule of 5 구현이 필수입니다.

</details>

---

<details markdown="1">
<summary><strong>5) const의 역할과 mutable의 의미는?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- const: 불변성 보장  
- mutable: 예외적으로 수정 허용  

---

<strong>🔹 특징 및 상세설명</strong>  
- const 객체는 멤버를 변경할 수 없으며, const 멤버 함수는 외부에서 관찰 가능한 상태를 변경하지 않아야 한다.  
- mutable은 캐시나 통계값 등 논리적 불변성을 깨지 않는 멤버에 사용된다.  
- const 포인터에서는 ‘포인터 자체’와 ‘대상이 가리키는 값’의 불변성을 구분해야 한다.  

---

<strong>💬 면접식 답변</strong>  
> const는 “이 값은 변경되지 않는다”는 불변성 계약을 의미하며,  
> mutable은 논리적 불변성을 깨지 않는 한도 내에서 내부 상태를 바꿀 수 있게 해줍니다.

</details>

---

<details markdown="1">
<summary><strong>6) static 키워드의 의미는? (꼬리: 초기화 시점)</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- static: 정적 수명, 프로그램 전체 공유  
- 함수 내부 static은 지연 초기화 가능  

---

<strong>🔹 특징 및 상세설명</strong>  
- 전역 static: 다른 번역 단위에서 접근 불가 (내부 링크).  
- 함수 내부 static: 호출 간 값 유지, C++11부터 스레드 안전 보장.  
- 클래스 static 멤버: 모든 인스턴스가 공유하는 단일 변수.  
- 정적 초기화 순서 문제를 피하려면 함수 내부 static 사용 권장.  

---

<strong>💬 면접식 답변</strong>  
> static은 객체의 생명주기를 프로그램 전체로 확장시키는 키워드입니다.  
> 함수 내부 static은 스레드 안전하게 한 번만 초기화됩니다.

</details>

---

<details markdown="1">
<summary><strong>7) inline 함수란 무엇인가요? (꼬리: 컴파일러 최적화와의 관계)</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- inline: 중복 정의 허용 + 치환 힌트  
- 실제 인라인 여부는 컴파일러가 결정  

---

<strong>🔹 특징 및 상세설명</strong>  
- 호출 오버헤드를 줄일 수 있으나 코드 부피가 증가한다.  
- 헤더에 정의된 템플릿 함수나 래퍼 함수는 자동 inline 취급된다.  
- 최적화 결정은 컴파일러의 재량이며, LTO나 PGO 분석에 따라 다르다.  

---

<strong>💬 면접식 답변</strong>  
> inline은 컴파일러에게 “이 함수를 치환해달라”는 힌트입니다.  
> 하지만 실제 인라인 여부는 컴파일러가 결정하며, 호출 오버헤드를 줄이지만 코드 크기가 늘 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>8) 메모리 정렬(alignment)과 패딩(padding)은 왜 필요한가요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 정렬: CPU 접근 효율 향상  
- 패딩: 정렬 기준을 맞추기 위한 공간  

---

<strong>🔹 특징 및 상세설명</strong>  
- CPU는 정렬된 주소에서 데이터를 읽을 때 가장 빠르다.  
- 구조체는 각 멤버의 정렬 단위에 맞춰 패딩이 삽입된다.  
- 낭비를 줄이려면 큰 타입부터 배치하거나 `#pragma pack`으로 정렬 단위를 조정한다.  
- 캐시라인 경계도 중요하며, false sharing 방지를 위해 주의해야 한다.  

---

<strong>💬 면접식 답변</strong>  
> 구조체의 필드는 CPU 정렬 기준을 맞추기 위해 패딩을 포함합니다.  
> 큰 타입부터 배치하거나 패킹을 통해 공간 낭비를 줄일 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>9) volatile의 역할은 무엇이며 멀티스레드에서 사용 가능한가요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- volatile: 메모리에서 항상 읽게 함  
- 동기화 보장은 없음  

---

<strong>🔹 특징 및 상세설명</strong>  
- volatile은 “외부 요인으로 값이 바뀔 수 있음”을 의미하며, 최적화 방지를 위해 사용된다.  
- 하지만 atomic성, 가시성, 순서를 보장하지 않아 멀티스레드 동기화에는 부적절하다.  
- 스레드 간 통신에는 <strong>std::atomic</strong> 또는 뮤텍스를 사용해야 한다.  

---

<strong>💬 면접식 답변</strong>  
> volatile은 하드웨어 레지스터 같은 값을 매번 메모리에서 읽도록 강제하지만,  
> 멀티스레드 동기화에는 적합하지 않습니다. 스레드 간 통신은 atomic으로 처리해야 합니다.

</details>

---

<details markdown="1">
<summary><strong>10) new/delete와 malloc/free의 차이는? (꼬리: new가 malloc을 호출?)</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- new/delete: 객체 생성 + 생성자/소멸자 호출  
- malloc/free: 단순 메모리 블록 할당/반납  

---

<strong>🔹 특징 및 상세설명</strong>  
- new는 <strong>operator new</strong>를 통해 메모리를 확보하고 생성자를 호출한다.  
- malloc은 단순한 바이트 버퍼를 반환하며, 생성자 호출이 없다.  
- 실패 시 new는 예외(<strong>std::bad_alloc</strong>)를, malloc은 nullptr을 반환한다.  
- 내부 구현에서 new가 malloc을 호출할 수도 있지만 의미적으로는 다르다.  

---

<strong>💬 면접식 답변</strong>  
> new는 객체를 생성하고 생성자까지 호출하지만, malloc은 단순히 버퍼를 할당할 뿐입니다.  
> 내부적으로 malloc을 사용할 수는 있지만, new는 타입 안전성과 예외 처리가 포함된 상위 개념입니다.

</details>

---

## 객체지향 & 다형성

<details markdown="1">
<summary><strong>11) 객체지향(OOP)의 핵심 개념을 설명해주세요. (꼬리: 캡슐화 vs 추상화)</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- OOP의 4대 개념: <strong>캡슐화, 상속, 다형성, 추상화</strong>  
- 복잡한 시스템을 구조화하고 재사용성을 높임  

---

<strong>🔹 특징 및 상세설명</strong>  
- <strong>캡슐화</strong>: 데이터와 함수를 하나로 묶고 외부 접근을 제한함 (정보 은닉).  
- <strong>상속</strong>: 기존 클래스를 기반으로 새로운 클래스를 정의, 중복 제거.  
- <strong>다형성</strong>: 동일 인터페이스로 다양한 객체 동작 가능.  
- <strong>추상화</strong>: 불필요한 세부를 감추고 본질만 드러냄.  

예시: `IRenderer` 인터페이스를 통해 DirectX, Vulkan, Metal 렌더러를 교체 가능하게 설계.

---

<strong>💬 면접식 답변</strong>  
> 객체지향은 캡슐화, 상속, 다형성, 추상화로 구성됩니다.  
> 특히 캡슐화는 변경의 영향을 내부로 제한하고, 추상화는 복잡성을 숨겨 상위 계층을 단순하게 합니다.

</details>

---

<details markdown="1">
<summary><strong>12) 가상 함수는 어떻게 동작하나요? (꼬리: vtable은 어디에 존재하나요?)</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 가상 함수는 런타임에 호출 대상이 결정됨.  
- 각 클래스는 vtable을 하나 가지며, 객체는 vptr을 통해 접근.  

---

<strong>🔹 특징 및 상세설명</strong>  
- vtable(가상 함수 테이블): 각 클래스가 보유하는 함수 포인터 배열.  
- vptr(가상 함수 포인터): 객체가 자신이 속한 vtable을 가리키는 숨겨진 포인터.  
- 호출 시: <strong>객체 → vptr → vtable → 함수 주소</strong> 순으로 호출.  
- 오버헤드는 포인터 접근 1회 수준이며, 다형성 구현의 핵심이다.

---

<strong>💬 면접식 답변</strong>  
> 가상 함수는 객체가 가진 vptr이 클래스의 vtable을 가리켜 런타임에 호출 대상을 결정합니다.  
> vtable은 클래스 단위로 존재하며, 객체당 오버헤드는 포인터 하나 수준입니다.

</details>

---

<details markdown="1">
<summary><strong>13) 순수 가상 함수와 추상 클래스의 차이는?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 순수 가상 함수: 구현이 없는 인터페이스 함수  
- 추상 클래스: 순수 가상 함수를 하나 이상 포함한 클래스  

---

<strong>🔹 특징 및 상세설명</strong>  
- 순수 가상 함수는 `<code>= 0</code>` 형태로 선언한다.  
- 추상 클래스는 인스턴스화할 수 없고, 파생 클래스가 반드시 구현해야 한다.  
- C++에는 `interface` 키워드가 없으며, 순수 가상 함수만 가진 클래스를 인터페이스로 사용한다.  
- 다형성 확장을 쉽게 하고, 의존성 역전을 돕는다.  

---

<strong>💬 면접식 답변</strong>  
> 순수 가상 함수는 구현이 없는 함수 선언이고, 이를 포함한 클래스가 추상 클래스가 됩니다.  
> C++에서는 인터페이스를 이렇게 구현하며, 확장성과 테스트 용이성이 높아집니다.

</details>

---

<details markdown="1">
<summary><strong>14) 오버로딩과 오버라이딩의 차이는?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 오버로딩: 함수 이름은 같지만 시그니처가 다름 (컴파일 타임)  
- 오버라이딩: 상속 관계에서 가상 함수를 재정의 (런타임)  

---

<strong>🔹 특징 및 상세설명</strong>  
- 오버로딩은 정적 다형성으로, 컴파일 시점에 어떤 함수가 호출될지 결정된다.  
- 오버라이딩은 런타임 다형성으로, 실제 객체 타입에 따라 함수 호출이 달라진다.  
- `override` 키워드는 의도하지 않은 오타나 오버로딩 혼동을 방지한다.  

---

<strong>💬 면접식 답변</strong>  
> 오버로딩은 같은 이름의 함수를 인자나 타입에 따라 구분하는 정적 다형성이고,  
> 오버라이딩은 상속받은 가상 함수를 재정의하는 런타임 다형성입니다.

</details>

---

<details markdown="1">
<summary><strong>15) 템플릿은 왜 사용하나요? (꼬리: 인스턴스화 시점)</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 타입에 독립적인 코드 재사용  
- 인스턴스화 시점: 실제 사용 시  

---

<strong>🔹 특징 및 상세설명</strong>  
- 템플릿은 코드 중복 없이 여러 타입을 처리할 수 있다.  
- 인스턴스화는 컴파일 시 발생하며, 사용된 타입별로 별도 코드가 생성된다.  
- 과도한 인스턴스화는 코드 부풀림과 빌드 시간 증가를 유발할 수 있다.  
- C++20의 `Concepts`와 `if constexpr`로 제약 조건을 명확히 표현할 수 있다.  

---

<strong>💬 면접식 답변</strong>  
> 템플릿은 타입에 독립적인 코드를 작성하기 위한 문법으로,  
> 실제 사용 시 컴파일러가 타입에 맞춰 인스턴스화합니다.  
> 중복을 줄이지만 과도한 사용은 코드 크기를 늘릴 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>16) 다중 상속의 장단점은 무엇인가요? (꼬리: 다이아몬드 문제)</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 장점: 다양한 인터페이스 구현 가능  
- 단점: 모호성, 다이아몬드 상속 문제  

---

<strong>🔹 특징 및 상세설명</strong>  
- 다중 상속은 여러 베이스 클래스를 동시에 상속받을 수 있다.  
- 그러나 동일한 조상 클래스가 여러 경로로 중복 상속되면 다이아몬드 문제가 생긴다.  
- 이를 해결하기 위해 <strong>virtual 상속</strong>을 사용하여 중복 베이스를 하나로 공유한다.  
- 설계 복잡도가 높아지므로, 대체로 <strong>합성(Composition)</strong>이 더 선호된다.  

---

<strong>💬 면접식 답변</strong>  
> 다중 상속은 여러 인터페이스를 한 클래스에서 구현할 수 있지만,  
> 다이아몬드 구조로 인한 중복 문제가 발생할 수 있습니다.  
> 대부분의 경우 virtual 상속이나 합성으로 해결합니다.

</details>

---

<details markdown="1">
<summary><strong>17) 가상 소멸자가 필요한 이유는?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 다형적 베이스 클래스는 반드시 가상 소멸자 필요  
- 이유: delete 시 파생 소멸자 미호출 방지  

---

<strong>🔹 특징 및 상세설명</strong>  
- 베이스 포인터로 파생 객체를 delete할 때, 소멸자가 가상이 아니면 베이스의 소멸자만 호출된다.  
- 결과적으로 파생 클래스의 리소스가 해제되지 않아 누수가 발생한다.  
- 규칙: “가상 함수가 하나라도 있다면 소멸자도 반드시 virtual”  

---

<strong>💬 면접식 답변</strong>  
> 다형적으로 객체를 다룰 때 베이스 클래스의 소멸자가 virtual이 아니면 파생 소멸자가 호출되지 않습니다.  
> 따라서 가상 함수가 있다면 소멸자도 반드시 virtual로 선언해야 합니다.

</details>

---

<details markdown="1">
<summary><strong>18) friend 키워드는 언제 사용하나요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 클래스 외부에서 내부 멤버 접근 허용  
- 접근 제한을 예외적으로 완화  

---

<strong>🔹 특징 및 상세설명</strong>  
- friend는 특정 함수나 클래스가 비공개(private) 멤버에 접근할 수 있게 한다.  
- 연산자 오버로딩(`operator<<`, `operator==`) 구현 시 자주 사용된다.  
- 그러나 남용 시 캡슐화가 깨지고 결합도가 높아지므로 최소한으로 사용해야 한다.  

---

<strong>💬 면접식 답변</strong>  
> friend는 특정 함수나 클래스가 내부 멤버에 접근할 수 있도록 허용하는 키워드입니다.  
> 연산자 오버로딩이나 팩토리 메서드 등 제한적인 경우에만 사용합니다.

</details>

---

<details markdown="1">
<summary><strong>19) 연산자 오버로딩의 장단점은?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 장점: 코드 가독성 향상  
- 단점: 남용 시 의미 모호화  

---

<strong>🔹 특징 및 상세설명</strong>  
- 연산자 오버로딩은 타입에 맞는 자연스러운 연산 표현을 가능하게 한다.  
- 멤버 vs 비멤버 선택:  
  - 멤버 함수로 구현 → `operator[]`, `operator=`, `operator()`  
  - 비멤버 함수로 구현 → `operator+`, `operator==`, `operator<<`  
- 의미적 일관성을 유지해야 하며, 부수 효과(side effect)를 최소화해야 한다.  

---

<strong>💬 면접식 답변</strong>  
> 연산자 오버로딩은 클래스의 의미를 더 자연스럽게 표현할 수 있지만,  
> 남용하면 코드 가독성과 유지보수가 오히려 나빠질 수 있습니다.  
> 연산자 의미에 부합할 때만 신중히 사용해야 합니다.

</details>

---

<details markdown="1">
<summary><strong>20) 복사 생성자와 이동 생성자의 차이는 무엇인가요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 복사: 리소스 복제  
- 이동: 리소스 소유권 이전  

---

<strong>🔹 특징 및 상세설명</strong>  
- 복사 생성자는 새 객체를 만들고 원본의 데이터를 복제한다.  
- 이동 생성자는 원본의 자원을 새 객체로 이전하고 원본을 안전한 상태로 만든다.  
- 대용량 객체(버퍼, 컨테이너 등)에서는 이동이 복사보다 훨씬 효율적이다.  
- 이동 생성자는 noexcept로 선언하면 STL 컨테이너에서 최적화가 적용된다.  

---

<strong>💬 면접식 답변</strong>  
> 복사는 데이터를 새로 복제하는 반면, 이동은 기존 리소스의 소유권을 옮깁니다.  
> 큰 객체에서는 이동이 훨씬 효율적이며, noexcept 선언 시 성능 이점이 큽니다.

</details>

---

# 🔷 C++ 면접 예상 질문 50선 – 모범답변 (3/3: 21–30)

---

## 메모리, 컨테이너, STL

<details markdown="1">
<summary><strong>21) STL이란 무엇이며, 어떤 장점을 가지나요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- STL(Standard Template Library): C++ 표준 템플릿 기반의 컨테이너, 알고리즘, 반복자 라이브러리  
- 재사용성, 안정성, 제너릭 프로그래밍 기반  

---

<strong>🔹 특징 및 상세설명</strong>  
- STL은 <strong>컨테이너(Container)</strong>, <strong>알고리즘(Algorithm)</strong>, <strong>반복자(Iterator)</strong>로 구성되어 있음.  
- 컨테이너: vector, list, map, set 등 자료 저장 구조.  
- 알고리즘: sort, find, count, accumulate 등 범용 연산 함수.  
- 반복자: 컨테이너를 일관된 방식으로 순회하는 추상화 계층.  
- 코드 재사용성과 효율성이 높으며, 템플릿 기반으로 다양한 타입에서 동작.  

---

<strong>💬 면접식 답변</strong>  
> STL은 C++의 표준 템플릿 기반 라이브러리로, 컨테이너와 알고리즘, 반복자를 제공해  
> 코드 재사용성과 성능을 높입니다. 제너릭 프로그래밍의 핵심 도구입니다.

</details>

---

<details markdown="1">
<summary><strong>22) vector와 list의 차이점은 무엇인가요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- vector: 연속 메모리, 랜덤 접근 빠름  
- list: 비연속 메모리, 삽입/삭제 빠름  

---

<strong>🔹 특징 및 상세설명</strong>  
- vector는 <strong>동적 배열</strong>로, 인덱스 접근이 빠르지만 중간 삽입/삭제는 느리다.  
- list는 <strong>이중 연결 리스트</strong>로, 삽입/삭제는 O(1)이지만 랜덤 접근이 불가능하다.  
- 메모리 지역성(Locality)은 vector가 훨씬 우수하다.  
- 대부분의 경우 vector가 더 효율적이며, 대용량 데이터에서도 캐시 효율이 높다.  

---

<strong>💬 면접식 답변</strong>  
> vector는 연속된 메모리를 사용하는 동적 배열이고, list는 비연속 메모리를 사용하는 연결 리스트입니다.  
> 대부분의 경우 캐시 효율이 좋은 vector를 사용합니다.

</details>

---

<details markdown="1">
<summary><strong>23) vector의 capacity와 reserve의 차이는?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- capacity: 실제 할당된 메모리 크기  
- reserve: 미리 capacity를 확보하는 함수  

---

<strong>🔹 특징 및 상세설명</strong>  
- vector는 요소가 추가될 때 용량(capacity)이 가득 차면 2배씩 증가하며 새 메모리로 복사 이동된다.  
- reserve(n)은 미리 n개의 공간을 확보해 재할당 비용을 줄인다.  
- capacity는 할당된 공간의 크기이며, size는 실제 원소 개수다.  
- shrink_to_fit()으로 남는 capacity를 줄일 수 있다.  

---

<strong>💬 면접식 답변</strong>  
> reserve는 vector가 재할당을 반복하지 않도록 미리 공간을 확보하는 함수입니다.  
> capacity는 실제 메모리 크기를 의미하고, size는 현재 원소 수를 뜻합니다.

</details>

---

<details markdown="1">
<summary><strong>24) push_back과 emplace_back의 차이는 무엇인가요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- push_back: 객체 복사 또는 이동 삽입  
- emplace_back: 객체를 제자리에서 직접 생성  

---

<strong>🔹 특징 및 상세설명</strong>  
- push_back은 이미 생성된 객체를 복사하거나 이동하여 vector에 삽입한다.  
- emplace_back은 전달된 인자로 vector 내부에서 직접 생성(생성자 호출).  
- 복사나 이동이 생략되어 <strong>불필요한 임시 객체 생성이 없다.</strong>  
- 따라서 복사 비용이 큰 객체에서는 emplace_back이 효율적이다.  

---

<strong>💬 면접식 답변</strong>  
> push_back은 이미 만들어진 객체를 복사하거나 이동시키지만,  
> emplace_back은 생성자 인자를 바로 전달해 vector 내부에 직접 객체를 생성합니다.  
> 성능상 emplace_back이 더 효율적입니다.

</details>

---

<details markdown="1">
<summary><strong>25) lvalue와 rvalue의 차이는 무엇인가요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- lvalue: 메모리에 이름이 존재, 수정 가능  
- rvalue: 임시 값, 표현식 종료 시 소멸  

---

<strong>🔹 특징 및 상세설명</strong>  
- lvalue: 변수처럼 메모리 주소가 존재하고 재사용 가능.  
- rvalue: 임시 객체, 즉시 소멸(리터럴, 연산 결과 등).  
- 무브 시멘틱(`std::move`)은 lvalue를 rvalue로 캐스팅하여 무브 시멘틱을 유도.  
- rvalue 참조(`T&&`)는 성능 최적화(이동 생성자 등)에 핵심적 역할을 한다.  

---

<strong>💬 면접식 답변</strong>  
> lvalue는 이름이 있는 메모리 객체이고, rvalue는 일시적인 임시 값입니다.  
> 무브 시멘틱은 rvalue 참조를 이용해 복사를 생략하고 자원을 효율적으로 이전합니다.

</details>

---

<details markdown="1">
<summary><strong>26) map과 unordered_map의 차이는?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- map: 균형 이진 탐색 트리 기반 (정렬됨)  
- unordered_map: 해시 기반 (정렬되지 않음)  

---

<strong>🔹 특징 및 상세설명</strong>  
- map: Red-Black Tree 기반, 키 정렬 유지, 탐색 O(log N).  
- unordered_map: Hash Table 기반, 평균 탐색 O(1), 충돌 시 체이닝 사용.  
- 정렬된 순회가 필요하면 map, 빠른 조회가 필요하면 unordered_map을 사용한다.  

---

<strong>💬 면접식 답변</strong>  
> map은 정렬된 이진 탐색 트리 기반으로 O(log N) 탐색을,  
> unordered_map은 해시 기반으로 평균 O(1) 탐색을 제공합니다.  
> 정렬이 필요하지 않다면 unordered_map이 더 빠릅니다.

</details>

---

<details markdown="1">
<summary><strong>27) 해시 충돌이 발생하면 어떻게 처리되나요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 대표적 방식: 체이닝(Chaining), 개방 주소법(Open Addressing)  

---

<strong>🔹 특징 및 상세설명</strong>  
- 체이닝: 같은 해시 인덱스에 연결 리스트를 두어 충돌 원소들을 저장.  
- 개방 주소법: 비어 있는 다음 슬롯을 찾아 순차적으로 삽입(선형/이차/이중 해싱).  
- C++ STL의 unordered_map은 체이닝 방식을 사용한다.  
- 해시 품질이 나쁘면 충돌이 잦아지고 O(N) 탐색이 될 수 있으므로 해시 함수를 신중히 선택해야 한다.  

---

<strong>💬 면접식 답변</strong>  
> 해시 충돌은 체이닝(리스트 연결) 또는 개방 주소법으로 해결하며,  
> C++의 unordered_map은 체이닝을 기본으로 사용합니다.

</details>

---

<details markdown="1">
<summary><strong>28) std::sort는 어떤 알고리즘을 사용하나요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- C++ 표준 sort는 <strong>Introsort</strong> 알고리즘을 사용함.  
- 퀵정렬, 힙정렬, 삽입정렬을 혼합.  

---

<strong>🔹 특징 및 상세설명</strong>  
- Introsort는 기본적으로 <strong>퀵정렬</strong>을 사용하되,  
  재귀 깊이가 깊어지면 <strong>힙정렬</strong>로 전환해 최악의 O(N log N)을 보장.  
- 원소 개수가 작을 때는 <strong>삽입정렬</strong>로 전환 (캐시 효율 높음).  
- 이 하이브리드 구조 덕분에 평균/최악 성능이 모두 우수하다.  

---

<strong>💬 면접식 답변</strong>  
> std::sort는 퀵정렬, 힙정렬, 삽입정렬을 혼합한 Introsort 알고리즘을 사용합니다.  
> 재귀 깊이에 따라 자동으로 전환되어 항상 O(N log N) 성능을 보장합니다.

</details>

---

<details markdown="1">
<summary><strong>29) set과 unordered_set의 차이는?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- set: 정렬된 트리 기반 (O(log N))  
- unordered_set: 해시 기반 (O(1) 평균)  

---

<strong>🔹 특징 및 상세설명</strong>  
- set은 Red-Black Tree 기반으로 정렬 순서 유지, 중복 불가.  
- unordered_set은 해시 기반으로 빠른 탐색 가능하지만 순서 보장 X.  
- 메모리 사용량은 unordered_set이 다소 많다.  

---

<strong>💬 면접식 답변</strong>  
> set은 정렬된 순서로 원소를 저장하고, unordered_set은 순서가 없는 해시 기반 컨테이너입니다.  
> 순서가 필요 없다면 unordered_set이 빠릅니다.

</details>

---

<details markdown="1">
<summary><strong>30) unique_ptr과 shared_ptr의 차이는?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- unique_ptr: 단일 소유  
- shared_ptr: 참조 카운트 기반 공유  

---

<strong>🔹 특징 및 상세설명</strong>  
- unique_ptr은 복사 불가, 이동만 가능 → 명확한 소유권 표현.  
- shared_ptr은 참조 카운트를 통해 여러 포인터가 자원을 공유.  
- 순환 참조 방지를 위해 weak_ptr 사용.  
- unique_ptr이 기본 선택이며, shared_ptr은 필요할 때만 사용.  

---

<strong>💬 면접식 답변</strong>  
> unique_ptr은 단일 소유권을 나타내며, shared_ptr은 참조 카운트로 공유 소유를 관리합니다.  
> 순환 참조를 피하기 위해 weak_ptr을 함께 사용합니다.
</details>

---

## 메모리 관리 & 동기화

<details markdown="1">
<summary><strong>31) 스마트 포인터는 내부적으로 어떻게 동작하나요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 스마트 포인터는 RAII 기반 자원 자동 해제 도구  
- shared_ptr은 참조 카운팅, unique_ptr은 소유권 이전  

---

<strong>🔹 특징 및 상세설명</strong>  
- <strong>unique_ptr</strong>: 복사 불가, 이동만 가능.  
- <strong>shared_ptr</strong>: `use_count`로 참조 수를 관리, 마지막 참조 해제 시 자원 해제.  
- <strong>weak_ptr</strong>: 순환 참조 방지용 비소유 포인터.  
- 참조 카운팅은 힙에 별도의 제어 블록을 두어 thread-safe하게 관리됨.  

---

<strong>💬 면접식 답변</strong>  
> 스마트 포인터는 RAII를 기반으로 자원의 수명과 객체 생명을 묶어 자동으로 해제합니다.  
> shared_ptr은 참조 카운트를 사용하고, unique_ptr은 단일 소유권으로 관리합니다.

</details>

---

<details markdown="1">
<summary><strong>32) 메모리 누수가 발생하는 원인은 무엇인가요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 할당 후 해제 누락  
- 순환 참조(shared_ptr), 예외 누락, 포인터 관리 실패  

---

<strong>🔹 특징 및 상세설명</strong>  
- <strong>delete 누락</strong>: 동적 할당 후 해제하지 않음.  
- <strong>shared_ptr 순환 참조</strong>: 서로를 shared_ptr로 참조하면 use_count가 0이 되지 않음.  
- <strong>예외 처리 누락</strong>: 예외 발생 시 delete 문 미도달.  
- 해결책: 스마트 포인터 사용, weak_ptr로 순환 차단, RAII 설계.  

---

<strong>💬 면접식 답변</strong>  
> 메모리 누수는 동적 할당 후 해제되지 않을 때 발생합니다.  
> 특히 shared_ptr 간 순환 참조가 대표적 원인으로, weak_ptr을 사용해 해결합니다.

</details>

---

<details markdown="1">
<summary><strong>33) RAII가 예외 안전성을 보장하는 이유는?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 생성자에서 자원 획득, 소멸자에서 자동 해제  
- 예외 발생 시에도 소멸자는 반드시 호출됨  

---

<strong>🔹 특징 및 상세설명</strong>  
- try 블록 내에서 예외가 발생해도, 스택에 있는 RAII 객체의 소멸자가 자동 호출된다.  
- 덕분에 자원 누수가 방지된다.  
- 예외 안전성 수준은 Basic, Strong, Nothrow 세 단계로 구분된다.  

---

<strong>💬 면접식 답변</strong>  
> RAII 객체는 소멸자에서 자원을 자동 해제하기 때문에 예외가 발생해도 누수가 없습니다.  
> 이는 C++의 스택 언와인딩(Stack Unwinding) 메커니즘 덕분입니다.

</details>

---

<details markdown="1">
<summary><strong>34) 멀티스레드 환경에서 race condition이란 무엇인가요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 여러 스레드가 동시에 공유 자원에 접근할 때 순서가 불확실한 상태  

---

<strong>🔹 특징 및 상세설명</strong>  
- 동시에 같은 변수를 읽거나 쓸 때 결과가 실행 순서에 따라 달라질 수 있다.  
- 해결책: <strong>뮤텍스, 스핀락, 세마포어</strong> 등 동기화 기법 사용.  
- atomic 연산으로도 해결 가능하나, 논리적 불변성이 깨지지 않도록 주의 필요.  

---

<strong>💬 면접식 답변</strong>  
> race condition은 여러 스레드가 동시에 데이터를 조작해 실행 순서에 따라 결과가 달라지는 문제입니다.  
> 뮤텍스나 atomic 연산으로 이를 방지합니다.

</details>

---

<details markdown="1">
<summary><strong>35) mutex와 spinlock의 차이는 무엇인가요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- mutex: 커널 개입, 스레드 블록  
- spinlock: 커널 개입 없음, 바쁜 대기  

---

<strong>🔹 특징 및 상세설명</strong>  
- mutex는 잠금 실패 시 스레드를 대기 상태로 전환 → context switch 발생.  
- spinlock은 잠금이 풀릴 때까지 반복 확인(루프) → 짧은 임계 구역에 유리.  
- 멀티코어 환경에서 spinlock은 빠를 수 있으나, 긴 대기에는 비효율적.  

---

<strong>💬 면접식 답변</strong>  
> mutex는 스레드를 블록해 CPU 낭비를 줄이고, spinlock은 커널 개입 없이 루프 대기합니다.  
> 임계 구역이 짧을 땐 spinlock이 더 효율적입니다.

</details>

---

<details markdown="1">
<summary><strong>36) deadlock이란 무엇이며, 발생 조건 4가지는?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 교착상태: 두 스레드가 서로 자원을 점유한 채 대기  
- 발생 조건 4가지: 상호배제, 점유대기, 비선점, 순환대기  

---

<strong>🔹 특징 및 상세설명</strong>  
1️⃣ <strong>상호배제(Mutual Exclusion)</strong> – 한 자원은 한 스레드만 사용 가능  
2️⃣ <strong>점유대기(Hold and Wait)</strong> – 하나 점유 후 다른 자원 대기  
3️⃣ <strong>비선점(No Preemption)</strong> – 자원 강제 회수 불가  
4️⃣ <strong>순환대기(Circular Wait)</strong> – 서로가 상대 자원을 대기  

예방: 락 획득 순서 통일, 타임아웃, Lock Hierarchy 사용 등  

---

<strong>💬 면접식 답변</strong>  
> 데드락은 여러 스레드가 서로의 자원을 기다리며 영원히 대기하는 상태입니다.  
> 상호배제, 점유대기, 비선점, 순환대기의 네 조건이 동시에 충족될 때 발생합니다.

</details>

---

<details markdown="1">
<summary><strong>37) condition_variable의 역할은?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 스레드 간 <strong>신호(wait/notify)</strong> 전달  
- 뮤텍스와 함께 사용  

---

<strong>🔹 특징 및 상세설명</strong>  
- 하나의 스레드가 특정 조건을 만족할 때 다른 스레드에 알림을 보냄.  
- `wait()`는 조건이 만족될 때까지 블록, `notify_one()` 또는 `notify_all()`로 깨움.  
- CPU 낭비 없이 효율적인 스레드 동기화 가능.  

---

<strong>💬 면접식 답변</strong>  
> condition_variable은 스레드 간 상태 변화를 알리는 신호 메커니즘입니다.  
> wait로 대기하고 notify로 깨워 효율적인 동기화를 제공합니다.

</details>

---

<details markdown="1">
<summary><strong>38) atomic은 어떤 상황에서 사용되나요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 단일 명령어로 실행되어 중단 불가능한 연산  
- lock-free 구현 가능  

---

<strong>🔹 특징 및 상세설명</strong>  
- atomic은 메모리 일관성을 보장하며, Lock-Free 자료구조 구현에 핵심적.  
- 예시: `std::atomic<int> counter; counter++;`  
- 내부적으로 CPU의 CAS(Compare-And-Swap) 명령을 사용.  
- 단, 복잡한 연산은 여전히 뮤텍스가 필요할 수 있다.  

---

<strong>💬 면접식 답변</strong>  
> atomic은 CPU의 하드웨어 명령을 이용해 원자적으로 수행되는 연산입니다.  
> Lock-Free 구조에서 동시성 안전성을 확보할 때 사용합니다.

</details>

---

<details markdown="1">
<summary><strong>39) Lock-Free 자료구조란 무엇인가요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 뮤텍스 없이 atomic 연산만으로 동시 접근을 제어하는 구조  

---

<strong>🔹 특징 및 상세설명</strong>  
- 예: Lock-Free Stack, Queue (CAS 기반).  
- 장점: 컨텍스트 스위치 없음 → 빠름.  
- 단점: 코드 복잡, ABA 문제 발생 가능.  
- 해결책: tag/version counter, hazard pointer 사용.  

---

<strong>💬 면접식 답변</strong>  
> Lock-Free 자료구조는 뮤텍스 없이 atomic 연산만으로 동시성 제어를 수행합니다.  
> 빠르지만 설계가 복잡하고 ABA 문제 등 주의가 필요합니다.

</details>

---

<details markdown="1">
<summary><strong>40) Memory Order(메모리 순서)란 무엇인가요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 멀티코어 환경에서의 메모리 접근 순서 제어  
- atomic 연산의 가시성·순서 보장 방식  

---

<strong>🔹 특징 및 상세설명</strong>  
- CPU와 컴파일러는 명령어를 재정렬할 수 있음 → 동기화 문제 발생.  
- C++11의 atomic은 memory_order를 명시적으로 지정 가능:  
  - <strong>relaxed</strong> (순서 보장 없음)  
  - <strong>acquire/release</strong> (잠금 해제 시점 제어)  
  - <strong>seq_cst</strong> (가장 강력한 순서 보장)  
- 올바른 선택이 Lock-Free 구조의 성능과 안전성 모두에 영향.  

---

<strong>💬 면접식 답변</strong>  
> Memory Order는 멀티코어에서 명령 실행 순서를 제어하는 개념으로,  
> atomic 연산이 다른 스레드에 어떻게 보이는지를 정의합니다.  
> acquire/release, seq_cst 등으로 제어합니다.

</details>

---

# 🔷 C++ 면접 예상 질문 50선 – 모범답변 (5/5: 41–50)

---

## 컴파일, 최적화, 설계 원칙, 예외, RTTI

<details markdown="1">
<summary><strong>41) C++의 컴파일 과정은 어떻게 이루어지나요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
1️⃣ 전처리 → 2️⃣ 컴파일 → 3️⃣ 어셈블 → 4️⃣ 링크  
각 단계는 독립적이며 오류 발생 시점이 다름  

---

<strong>🔹 특징 및 상세설명</strong>  
- **전처리**: `#include`, `#define` 처리 및 매크로 치환  
- **컴파일**: C++ 소스 → 어셈블리 코드 생성 (.obj, .o 파일)  
- **어셈블**: 어셈블리 → 기계어로 변환  
- **링크**: 여러 개의 오브젝트 파일을 결합하여 실행 파일 생성  
- 링커는 심볼 테이블을 통해 함수/변수 참조를 해결함  

---

<strong>💬 면접식 답변</strong>  
> C++은 전처리, 컴파일, 어셈블, 링크 네 단계를 거칩니다.  
> 전처리에서 매크로가 치환되고, 컴파일로 기계어 코드가 생성된 뒤,  
> 링커가 모든 참조를 해결해 최종 실행 파일을 만듭니다.

</details>

---

<details markdown="1">
<summary><strong>42) inline 함수는 언제 사용되며, 주의할 점은?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 작은 함수의 호출 오버헤드 제거용  
- 컴파일러가 강제하지 않음  

---

<strong>🔹 특징 및 상세설명</strong>  
- inline은 함수 본문을 호출 지점에 삽입하여 호출 오버헤드를 줄인다.  
- 단, 코드 크기 증가(인스트럭션 캐시 부담) 위험이 있음.  
- 가상 함수나 루프 내부에서 사용 시 성능 저하 가능.  
- modern compiler는 자동 인라인 최적화를 수행하므로 남용 금지.  

---

<strong>💬 면접식 답변</strong>  
> inline은 짧고 자주 호출되는 함수의 호출 오버헤드를 줄이지만,  
> 코드 부풀림으로 인한 캐시 비효율이 생길 수 있어 신중히 사용해야 합니다.

</details>

---

<details markdown="1">
<summary><strong>43) const와 constexpr의 차이는?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- const: 런타임 상수  
- constexpr: 컴파일타임 상수  

---

<strong>🔹 특징 및 상세설명</strong>  
- const는 초기화 후 변경 불가지만, <strong>컴파일 시간</strong>에 값이 확정될 필요는 없음.  
- constexpr은 반드시 컴파일 시 계산 가능한 값이어야 함.  
- constexpr은 함수, 생성자에도 적용 가능(C++11 이후).  
- constexpr은 상수 표현식 최적화를 통해 실행 속도를 향상시킴.  

---

<strong>💬 면접식 답변</strong>  
> const는 런타임 상수이고, constexpr은 컴파일 타임 상수입니다.  
> constexpr은 상수 계산을 컴파일 단계에서 수행하여 실행 속도를 높입니다.

</details>

---

<details markdown="1">
<summary><strong>44) RTTI(Run-Time Type Information)란 무엇인가요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 런타임에 객체의 실제 타입 정보를 제공하는 시스템  
- dynamic_cast, typeid에서 사용됨  

---

<strong>🔹 특징 및 상세설명</strong>  
- RTTI는 다형성 타입의 런타임 식별에 사용된다.  
- **typeid(obj)**: 객체의 실제 타입 반환  
- **dynamic_cast&lt;T*&gt;()**: 안전한 다운캐스팅 수행  
- 내부적으로 클래스마다 type_info 테이블(vtable과 별개)을 가진다.  
- RTTI는 오버헤드가 있으므로 최소한으로 사용하는 것이 권장된다.  

---

<strong>💬 면접식 답변</strong>  
> RTTI는 실행 중 객체의 실제 타입을 식별하는 시스템입니다.  
> dynamic_cast나 typeid에서 사용되며, 다형성 클래스에만 적용됩니다.

</details>

---

<details markdown="1">
<summary><strong>45) 예외 처리(exception handling)는 내부적으로 어떻게 작동하나요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- try/catch 블록과 스택 언와인딩(Stack Unwinding) 기반  

---

<strong>🔹 특징 및 상세설명</strong>  
- 예외 발생 시 스택 프레임을 하나씩 해제하며 catch 블록을 탐색.  
- 이 과정에서 지역 객체의 소멸자가 자동 호출됨.  
- 표준 라이브러리의 예외는 `std::exception`을 상속받음.  
- 예외는 성능 오버헤드가 있으므로, 성능 민감 구간에서는 반환값 기반 처리 선호.  

---

<strong>💬 면접식 답변</strong>  
> 예외 발생 시 스택을 거꾸로 해제하면서 해당 타입의 catch 블록을 찾습니다.  
> 이때 지역 객체 소멸자가 자동 호출되어 자원 누수를 방지합니다.

</details>

---

<details markdown="1">
<summary><strong>46) noexcept 키워드는 어떤 역할을 하나요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 함수가 예외를 던지지 않음을 보장  
- 최적화 및 강제 종료 제어  

---

<strong>🔹 특징 및 상세설명</strong>  
- noexcept 지정 시 예외가 발생하면 프로그램이 즉시 종료(`std::terminate`).  
- move 생성자에 noexcept를 붙이면 컨테이너 이동 시 불필요한 복사 방지.  
- 런타임 비용은 없지만, 최적화 힌트로 컴파일러에 전달된다.  

---

<strong>💬 면접식 답변</strong>  
> noexcept는 함수가 예외를 던지지 않음을 선언하며,  
> move 연산이나 컨테이너 최적화에 큰 영향을 줍니다.

</details>

---

<details markdown="1">
<summary><strong>47) 컴파일 타임 다형성과 런타임 다형성의 차이는?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 컴파일 타임: 템플릿 기반(static)  
- 런타임: virtual 함수 기반(dynamic)  

---

<strong>🔹 특징 및 상세설명</strong>  
- **컴파일 타임 다형성**: 함수 오버로딩, 템플릿 인스턴스화 등.  
- **런타임 다형성**: 가상 함수, vtable을 통한 동적 바인딩.  
- 전자는 인라인화 가능해 빠르지만 코드 크기 증가 가능.  
- 후자는 유연하지만 약간의 런타임 오버헤드가 존재.  

---

<strong>💬 면접식 답변</strong>  
> 컴파일 타임 다형성은 템플릿을 통해 컴파일 시 결정되고,  
> 런타임 다형성은 가상 함수를 통해 실행 중 결정됩니다.  
> 속도는 전자가 빠르지만 유연성은 후자가 높습니다.

</details>

---

<details markdown="1">
<summary><strong>48) SOLID 원칙을 설명해주세요.</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
1️⃣ SRP – 단일 책임  
2️⃣ OCP – 개방 폐쇄  
3️⃣ LSP – 리스코프 치환  
4️⃣ ISP – 인터페이스 분리  
5️⃣ DIP – 의존성 역전  

---

<strong>🔹 특징 및 상세설명</strong>  
- **SRP**: 클래스는 하나의 책임만 가져야 함.  
- **OCP**: 확장에는 열려 있고 수정에는 닫혀 있어야 함.  
- **LSP**: 상위 타입의 객체를 하위 타입으로 치환 가능해야 함.  
- **ISP**: 사용하지 않는 인터페이스에 의존하지 않도록 분리.  
- **DIP**: 고수준 모듈은 저수준 구현이 아닌 추상에 의존해야 함.  

---

<strong>💬 면접식 답변</strong>  
> SOLID 원칙은 객체지향 설계의 5대 핵심 원칙으로,  
> 유지보수성과 확장성을 높이기 위한 설계 가이드라인입니다.  
> 저는 특히 DIP와 OCP를 중요하게 생각합니다.

</details>

---

<details markdown="1">
<summary><strong>49) static 키워드는 C++에서 어떤 용도로 쓰이나요?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 변수 수명과 범위 제어  
- 함수/클래스 단위에서도 활용  

---

<strong>🔹 특징 및 상세설명</strong>  
- **정적 변수**: 함수 내부에서 한 번만 초기화되고, 호출 간 값 유지.  
- **정적 함수**: 해당 번역 단위(.cpp) 내에서만 접근 가능.  
- **클래스 정적 멤버**: 모든 객체가 공유하는 공용 데이터.  
- 프로그램 시작 시 초기화, 종료 시 소멸.  

---

<strong>💬 면접식 답변</strong>  
> static은 변수의 수명과 접근 범위를 조절하는 키워드입니다.  
> 전역 접근을 줄이고, 클래스 간 데이터 공유에 자주 사용됩니다.

</details>

---

<details markdown="1">
<summary><strong>50) C++에서 메모리 최적화를 위해 고려할 점은?</strong></summary>

---

<strong>🧠 핵심 요약</strong>  
- 불필요한 복사 제거  
- 캐시 지역성 향상  
- 객체 수명 최소화  

---

<strong>🔹 특징 및 상세설명</strong>  
- **무브 시멘틱** 활용 (`std::move`, `emplace_back`)  
- **reserve()** / **shrink_to_fit()**로 vector 재할당 최소화  
- **메모리 풀, 커스텀 알로케이터**로 동적 할당 최적화  
- 구조체 정렬(padding) 최소화로 메모리 낭비 방지  
- 캐시 친화적 데이터 레이아웃 설계 (AoS → SoA 변환)  

---

<strong>💬 면접식 답변</strong>  
> 메모리 최적화는 복사 제거, 캐시 효율, 동적 할당 최소화가 핵심입니다.  
> 특히 무브 시멘틱(Move Semantics: 복사 대신 자원의 소유권을 이전(move) 하는 기능)과 메모리 풀 사용은 실무 성능에 큰 차이를 만듭니다.

</details>

---

<details markdown="1">  
<summary><strong>51) C++에서 enum class란 무엇인가요?</strong></summary>  

---

<strong>🧠 핵심 요약</strong>  
- C++11부터 도입된 **타입 안전한 열거형(enum)**  
- 기존 enum의 **스코프 충돌** 및 **암묵적 변환 문제** 해결  
- **명시적 범위 지정(Color::Red)** 과 **형변환 제한**이 특징  

---

<strong>🔹 특징 및 상세설명</strong>  
- 기존 `enum`은 전역 스코프에 노출되어 이름 충돌 위험이 높고, `int`로 암묵 변환되어 **타입 안정성이 부족**함  
- `enum class`는 자체 스코프를 가져서 **이름 충돌을 방지**하고, **암묵적 형변환이 불가능**함  
- 서로 다른 enum class 간 비교 불가능 → **타입 안전성 보장**  
- 필요 시 명시적 캐스팅 사용 가능  
  ```cpp
  int n = static_cast<int>(Color::Green);
  ```  
- 기본 자료형 지정으로 메모리 절약 가능  
  ```cpp
  enum class ErrorCode : uint8_t { OK = 0, NotFound = 1, Unknown = 255 };
  ```  

---

<strong>💬 코드 예시</strong>  
```cpp
#include <iostream>
using namespace std;

enum class Color { Red, Green, Blue };
enum class Fruit { Apple, Banana, Orange };

int main() {
    Color c = Color::Red;
    Fruit f = Fruit::Apple;

    // if (c == f) ❌ 오류: 타입이 다름
    // int x = c; ❌ 암묵적 변환 불가
    int x = static_cast<int>(Color::Green); // ✅ 명시적 변환

    cout << "x = " << x << endl;
}
```

---

<strong>💬 면접식 답변</strong>  
> `enum class`는 C++11부터 도입된 타입 안전한 열거형입니다.  
> 기존 enum은 전역 스코프와 암묵적 형변환 문제로 안전하지 않았지만,  
> enum class는 자체 스코프를 가져 이름 충돌을 막고 암묵 변환을 제한해  
> **명확하고 안전한 코드 작성이 가능합니다.**

</details>
