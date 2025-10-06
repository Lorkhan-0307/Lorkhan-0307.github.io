---
layout: post
title: "기술면접 대비 CS 공부 - CSharp"
date: 2025-10-05 16:00:00 +0900
categories: [Tech Interview, Study Plan]
tags: [c++, c-sharp, computer-science, algorithm, data-structure, operating-system, network, database, design-pattern, unity, unreal]
slug: csstudyforfinalpartsix
---

# 면접 대비 사전 QnA 정리 - C#


# 🔷 C# 면접 예상 질문 50선 – 모범답변

---

<details markdown="1">
<summary><strong>1. C#의 주요 특징은 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- C#은 **객체지향 언어(OOP)** 기반으로, **안전성**, **생산성**, **자동 메모리 관리(GC)**, **.NET 통합성**이 강점입니다.

---

<strong>🔹 상세설명</strong>  
C#은 Microsoft의 .NET Framework 위에서 동작하는 언어로, C++의 성능과 Java의 안정성을 결합했습니다.  
주요 특징은 다음과 같습니다:  
1. **객체지향(OOP)** 지원 — 캡슐화, 상속, 다형성  
2. **자동 메모리 관리(GC)**  
3. **타입 안정성(Type Safety)**  
4. **LINQ, async/await, 람다** 등 고급 문법 제공  
5. **Cross-platform (.NET Core)** 지원  

---

<strong>💬 면접식 답변</strong>  
C#은 객체지향 언어로서 생산성과 안정성을 동시에 제공합니다.  
특히 자동 메모리 관리(GC)와 async/await 같은 고급 기능이 있어 현대적인 애플리케이션 개발에 적합합니다.
</details>

---

<details markdown="1">
<summary><strong>2. C#과 C++의 주요 차이는 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- C++은 **메모리 직접 제어**, C#은 **GC 자동 관리**  
- C#은 **.NET 런타임 환경에서 안전성 중심**,  
  C++은 **네이티브 퍼포먼스 중심**

---

<strong>🔹 상세설명</strong>  

| 항목 | C++ | C# |
|------|------|------|
| 메모리 | 수동 관리 (new/delete) | 자동 관리 (GC) |
| 실행 | Native (컴파일 → 기계어) | CLR 위에서 JIT 컴파일 |
| 포인터 | 직접 접근 가능 | 제한적 (unsafe 블록) |
| 플랫폼 | 독립적 빌드 필요 | .NET 런타임 종속 |
| 주요 용도 | 게임 엔진, 시스템 | 앱/서버, 클라우드 |

---

<strong>💬 면접식 답변</strong>  
C++은 메모리를 직접 다루기 때문에 성능이 높지만 위험합니다.  
C#은 .NET 런타임에서 GC로 메모리를 관리해 안정적이고 개발 속도가 빠릅니다.
</details>

---

<details markdown="1">
<summary><strong>3. CLR(Common Language Runtime)이란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- .NET 실행 환경으로, C#, VB.NET 등 다양한 언어를 **공통 중간 언어(IL)** 로 실행시켜줍니다.

---

<strong>🔹 상세설명</strong>  
CLR은 .NET의 핵심 런타임 엔진입니다.  
컴파일된 C# 코드는 **IL(Intermediate Language)** 로 변환되고,  
실행 시 **JIT(Just-In-Time) 컴파일러**가 IL을 기계어로 변환합니다.  
CLR은 GC, 예외 처리, 스레드 관리, 보안, 타입 검증 등을 제공합니다.

---

<strong>💬 면접식 답변</strong>  
CLR은 .NET의 런타임으로, IL 코드를 기계어로 변환해 실행하며  
GC, 예외 처리, 보안 등을 관리해 개발자가 안정적으로 프로그램을 작성할 수 있도록 돕습니다.
</details>

---

<details markdown="1">
<summary><strong>4. Boxing과 Unboxing이란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **Boxing**: 값 타입 → 참조 타입(object) 변환  
- **Unboxing**: 참조 타입(object) → 값 타입 변환  
- 둘 다 **성능 비용 발생**

---

<strong>🔹 상세설명</strong>  
C#에서 int, float 같은 값 타입을 object에 담으면 **Boxing**이 발생합니다.  
반대로 object에서 다시 값 타입으로 꺼낼 때는 **Unboxing**이 일어납니다.  
이 과정은 힙 메모리 할당 및 복사를 수반하므로 성능 저하의 원인이 됩니다.  

```csharp
int a = 10;
object b = a;     // Boxing
int c = (int)b;   // Unboxing
```

---

<strong>💬 면접식 답변</strong>  
Boxing은 값 타입을 참조형으로 변환하는 과정이고,  
Unboxing은 그 반대입니다.  
둘 다 힙 메모리 할당이 일어나므로 가능하면 제네릭을 사용해 피하는 것이 좋습니다.
</details>

---

<details markdown="1">
<summary><strong>5. Value Type과 Reference Type의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **Value Type**: 스택에 저장, 복사 시 값이 복제됨  
- **Reference Type**: 힙에 저장, 참조를 공유  

---

<strong>🔹 상세설명</strong>  

| 구분 | Value Type | Reference Type |
|------|-------------|----------------|
| 저장 위치 | Stack | Heap |
| 예시 | int, float, struct | class, string, array |
| 복사 방식 | 값 복사 | 참조 복사 |
| GC 영향 | 없음 | GC 관리 대상 |

---

<strong>💬 면접식 답변</strong>  
Value Type은 스택에 직접 값이 저장되고, Reference Type은 힙에 저장된 객체를 참조합니다.  
구조체(struct)는 값 타입이고, 클래스(class)는 참조 타입입니다.
</details>

---

<details markdown="1">
<summary><strong>6. C#에서 class와 struct의 차이는 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **class**: 참조 타입(Heap), 상속 가능  
- **struct**: 값 타입(Stack), 상속 불가  

---

<strong>🔹 상세설명</strong>  
- struct는 **값 타입**이라 스택에 저장되고, 복사 시 값 자체가 복제됩니다.  
- class는 **참조 타입**으로 힙에 저장되고, GC의 관리 대상입니다.  
- struct는 **상속이 불가능**, 하지만 인터페이스 구현은 가능.  

---

<strong>💬 면접식 답변</strong>  
class는 참조 타입으로 힙에 저장되고 상속이 가능하지만,  
struct는 값 타입으로 가볍고 빠르며 주로 작은 데이터 묶음을 표현할 때 사용합니다.
</details>

---

<details markdown="1">
<summary><strong>7. interface와 abstract class의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **interface**: 규약 정의 (모든 메서드 추상)  
- **abstract class**: 일부 구현 포함 가능  

---

<strong>🔹 상세설명</strong>  

| 항목 | interface | abstract class |
|------|------------|----------------|
| 목적 | 계약 정의 | 공통 로직 + 추상 정의 |
| 구현 여부 | 불가능 (C# 8.0 이후 default 가능) | 가능 |
| 다중 상속 | 가능 | 불가능 |
| 필드 보유 | 불가 | 가능 |

---

<strong>💬 면접식 답변</strong>  
interface는 규약을 정의하고, abstract class는 공통 로직을 포함할 수 있습니다.  
보통 클래스 간 공통 기능을 나누고 싶다면 abstract class,  
역할 정의 위주라면 interface를 사용합니다.
</details>

---

<details markdown="1">
<summary><strong>8. override, overload의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **override**: 부모의 virtual 메서드 재정의  
- **overload**: 같은 이름, 다른 매개변수로 다형성 구현  

---

<strong>🔹 상세설명</strong>  
```csharp
class Base {
    public virtual void Print() {}
}

class Derived : Base {
    public override void Print() {}  // override
}

void Print(int a) {}  
void Print(string s) {}  // overload
```
- 오버로딩은 **컴파일 타임 다형성**,  
  오버라이드는 **런타임 다형성**.  

---

<strong>💬 면접식 답변</strong>  
Overload는 같은 이름의 메서드를 인자만 다르게 정의하는 것이고,  
Override는 부모 클래스의 virtual 메서드를 재정의하는 것입니다.
</details>

---

<details markdown="1">
<summary><strong>9. delegate란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- 메서드를 **변수처럼 참조**할 수 있는 형식  
- 콜백(callback) 구현에 사용  

---

<strong>🔹 상세설명</strong>  
```csharp
delegate void MyDelegate(string msg);
MyDelegate d = Print;
d("Hello!");
```
- 메서드를 참조하는 타입으로, 이벤트 시스템과 콜백 함수에 사용됩니다.  
- C#에서는 **람다, 익명 메서드**와 결합되어 강력한 표현 가능.  

---

<strong>💬 면접식 답변</strong>  
Delegate는 메서드를 변수처럼 저장하고 실행할 수 있는 참조 타입입니다.  
주로 콜백 함수나 이벤트 처리에서 사용됩니다.
</details>

---

<details markdown="1">
<summary><strong>10. event와 delegate의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **delegate**: 메서드 참조  
- **event**: delegate를 기반으로 한 **이벤트 모델**

---

<strong>🔹 상세설명</strong>  
- event는 delegate를 캡슐화해 외부에서 직접 호출 불가.  
- 이벤트 구독(`+=`), 해제(`-=`)만 허용.  

```csharp
public event Action OnClicked;
```

---

<strong>💬 면접식 답변</strong>  
delegate는 단순히 메서드를 참조하는 타입이고,  
event는 delegate를 기반으로 한 안전한 이벤트 시스템입니다.  
event는 외부에서 직접 실행할 수 없다는 점이 다릅니다.
</details>

---

<details markdown="1">
<summary><strong>11. LINQ(Language Integrated Query)란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- LINQ는 C#에서 **데이터를 질의(Query)** 하는 기능.  
- SQL 스타일 문법을 코드에 직접 통합할 수 있음.

---

<strong>🔹 상세설명</strong>  
- LINQ는 컬렉션, XML, DB 등 다양한 데이터 소스를 일관된 방식으로 탐색할 수 있게 해줍니다.  
- `IEnumerable<T>` 또는 `IQueryable<T>`를 기반으로 동작합니다.  
- **지연 실행(Lazy Evaluation)** 을 지원하여, 실제로 결과가 필요할 때만 쿼리가 실행됩니다.

```csharp
var result = from n in numbers
             where n % 2 == 0
             select n;
```

---

<strong>💬 면접식 답변</strong>  
LINQ는 C#에 내장된 데이터 질의 언어로, SQL처럼 데이터를 직접 필터링하고 가공할 수 있습니다.  
코드 가독성과 유지보수성이 높아지는 장점이 있습니다.
</details>

---

<details markdown="1">
<summary><strong>12. IEnumerable와 IQueryable의 차이는 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- `IEnumerable`: **메모리 내 컬렉션**에서 순차적으로 처리.  
- `IQueryable`: **원격 데이터 소스(DB)** 에 질의문으로 변환되어 실행.

---

<strong>🔹 상세설명</strong>  
- `IEnumerable`은 데이터를 **모두 가져온 뒤** 필터링. (메모리 내 연산)  
- `IQueryable`은 **지연 실행 쿼리**를 SQL로 변환해 DB에서 처리.  
- `IQueryable`은 LINQ-to-SQL, Entity Framework에서 주로 사용됩니다.

---

<strong>💬 면접식 답변</strong>  
IEnumerable은 메모리 내 컬렉션용, IQueryable은 DB 질의용입니다.  
즉, IQueryable은 SQL로 변환되어 서버에서 실행되므로 성능 면에서 효율적입니다.
</details>

---

<details markdown="1">
<summary><strong>13. async / await 키워드의 역할은?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **async**: 비동기 메서드 정의  
- **await**: 비동기 작업을 기다림 (스레드 블로킹 없이)

---

<strong>🔹 상세설명</strong>  
- `async` 메서드는 내부에 `await`를 포함해 비동기 실행을 정의.  
- `await`는 **비동기 작업(Task)** 이 완료될 때까지 제어권을 반환.  
- 스레드를 블로킹하지 않아 UI 응답성이 유지됩니다.

```csharp
async Task<int> LoadDataAsync() {
    await Task.Delay(1000);
    return 42;
}
```

---

<strong>💬 면접식 답변</strong>  
async/await는 비동기 작업을 동기식처럼 작성할 수 있게 해주는 구문입니다.  
스레드를 블로킹하지 않아 UI나 서버 응답성을 유지할 수 있습니다.
</details>

---

<details markdown="1">
<summary><strong>14. Task와 Thread의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **Thread**: 실제 OS 레벨의 실행 단위  
- **Task**: Thread 위에서 동작하는 **작업 단위(추상화)**  

---

<strong>🔹 상세설명</strong>  
- Thread는 OS가 직접 관리하며, 생성 비용이 높습니다.  
- Task는 ThreadPool 위에서 실행되며, **스케줄링과 예외 처리**가 내장되어 있습니다.  
- 비동기 병렬 처리 시 `Task`를 권장합니다.

```csharp
Task.Run(() => Console.WriteLine("Async Task"));
```

---

<strong>💬 면접식 답변</strong>  
Thread는 실제 실행 단위이고, Task는 이를 추상화한 고수준 작업 단위입니다.  
Task는 예외 처리와 스케줄링을 자동으로 관리하기 때문에 더 안전합니다.
</details>

---

<details markdown="1">
<summary><strong>15. C#의 Garbage Collector(GC)는 어떻게 동작하나요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **세대별(Generational) GC** 구조  
- 불필요한 객체를 자동으로 회수하며, 힙을 관리함.

---

<strong>🔹 상세설명</strong>  
- C# GC는 **세대(Generation 0, 1, 2)** 로 구분합니다.  
- 최근 생성된 객체(Gen 0)는 빠르게 수집되고, 오래된 객체는 상위 세대로 승격됩니다.  
- GC는 Mark → Sweep → Compact 과정을 거칩니다.  
- 백그라운드 수집, Low Latency 모드 등도 제공됩니다.

---

<strong>💬 면접식 답변</strong>  
C#의 GC는 세대별 수집 방식을 사용해 성능을 최적화합니다.  
짧게 사는 객체는 빠르게 회수하고, 오래 사는 객체는 덜 자주 검사해 효율을 높입니다.
</details>

---

<details markdown="1">
<summary><strong>16. IDisposable 인터페이스의 역할은?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **비관리 리소스**(파일, DB, 소켓 등)를 명시적으로 해제하는 패턴 제공.

---

<strong>🔹 상세설명</strong>  
- GC는 관리 힙만 회수하므로, **파일 핸들·소켓·DB 연결** 같은 비관리 자원은 직접 해제해야 합니다.  
- `IDisposable.Dispose()` 메서드로 리소스 해제를 정의하고,  
  `using` 문으로 자동 호출할 수 있습니다.

```csharp
using (var fs = new FileStream("data.txt", FileMode.Open)) {
    // 파일 사용
}
```

---

<strong>💬 면접식 답변</strong>  
IDisposable은 비관리 리소스를 안전하게 해제하기 위한 인터페이스입니다.  
using 문을 사용하면 Dispose가 자동 호출되어 누수를 방지합니다.
</details>

---

<details markdown="1">
<summary><strong>17. using과 await using의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **using**: 동기 리소스 해제  
- **await using**: 비동기 리소스 해제 (`IAsyncDisposable`)

---

<strong>🔹 상세설명</strong>  
C# 8.0부터 `IAsyncDisposable` 인터페이스가 도입되어  
비동기 리소스를 안전하게 해제할 수 있습니다.  

```csharp
await using var conn = new SqlConnection(...);
```

- `DisposeAsync()` 메서드가 호출되어 비동기적으로 리소스 정리 수행.

---

<strong>💬 면접식 답변</strong>  
await using은 비동기 리소스를 해제하기 위한 구문으로,  
네트워크 스트림이나 DB 연결처럼 비동기 처리가 필요한 자원에서 사용됩니다.
</details>

---

<details markdown="1">
<summary><strong>18. Exception 처리 방식에 대해 설명해주세요.</strong></summary>

<strong>🧠 핵심 요약</strong>  
- C#은 **try-catch-finally** 구문으로 예외를 처리.  
- 예외는 **객체(클래스)** 형태로 전달됩니다.

---

<strong>🔹 상세설명</strong>  
- 모든 예외는 `System.Exception`을 상속.  
- 예외 발생 시 스택 언와인딩(Stack Unwinding)이 일어나며,  
  가장 가까운 catch 블록이 실행됩니다.  
- finally는 예외 여부와 관계없이 항상 실행됩니다.  

---

<strong>💬 면접식 답변</strong>  
C#의 예외는 객체 기반으로 전달되며, try-catch-finally로 안전하게 처리합니다.  
자원 해제가 필요할 땐 finally 또는 using을 함께 사용하는 것이 좋습니다.
</details>

---

<details markdown="1">
<summary><strong>19. Reflection이란 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- 실행 중에 **형식 정보(Type Metadata)** 를 조사·조작하는 기능.  

---

<strong>🔹 상세설명</strong>  
Reflection은 런타임에 객체의 타입, 메서드, 프로퍼티, 어트리뷰트를 조회하고 호출할 수 있게 합니다.  

```csharp
Type t = typeof(MyClass);
MethodInfo m = t.GetMethod("Run");
m.Invoke(obj, null);
```

- 주로 **플러그인 로딩**, **DI 컨테이너**, **직렬화** 등에 사용됩니다.  
- 단점: 성능이 느리고, 런타임 오류 가능성이 있음.  

---

<strong>💬 면접식 답변</strong>  
Reflection은 런타임에 타입 정보를 읽거나 수정하는 기능입니다.  
DI 컨테이너나 JSON 직렬화 라이브러리 내부에서 많이 사용됩니다.
</details>

---

<details markdown="1">
<summary><strong>20. 람다(Lambda Expression)란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- 익명 메서드를 간결하게 표현하는 문법.  
- delegate와 함께 사용됨.

---

<strong>🔹 상세설명</strong>  
```csharp
Func<int, int> square = x => x * x;
```
- 람다는 익명 함수로, 인라인에서 정의 가능.  
- **LINQ**, **이벤트**, **Task**, **Delegate** 등에서 자주 사용됩니다.  
- `=>` 연산자는 입력과 출력의 관계를 나타냅니다.  

---

<strong>💬 면접식 답변</strong>  
람다는 익명 메서드를 간단히 표현하는 문법으로, 코드 가독성을 높이고  
LINQ나 콜백 함수에서 자주 활용됩니다.
</details>

---

<details markdown="1">
<summary><strong>21. Property와 Field의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **Field**: 실제 데이터를 저장하는 변수  
- **Property**: Field를 캡슐화한 접근자  

---

<strong>🔹 상세설명</strong>  
```csharp
private int _hp;
public int HP {
    get => _hp;
    set => _hp = Math.Max(0, value);
}
```
- Property는 Field에 접근하기 전 **유효성 검사, 로직 삽입**이 가능.  
- **자동 구현 프로퍼티(auto-property)**: `public int HP { get; set; }`  

---

<strong>💬 면접식 답변</strong>  
Field는 단순한 데이터 저장소이고, Property는 이를 제어하는 인터페이스 역할을 합니다.  
캡슐화를 유지하기 위해 Field 대신 Property를 사용하는 것이 일반적입니다.
</details>

---

<details markdown="1">
<summary><strong>22. 접근 제한자(access modifier)의 종류와 역할은?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **public**: 모든 곳에서 접근 가능  
- **private**: 클래스 내부 전용  
- **protected**, **internal**, **protected internal**, **private protected** 지원

---

<strong>🔹 상세설명</strong>  

| 제한자 | 설명 |
|---------|------|
| public | 모든 코드에서 접근 가능 |
| private | 클래스 내부 전용 |
| protected | 상속 클래스에서 접근 가능 |
| internal | 같은 어셈블리 내에서 접근 가능 |
| protected internal | 상속 + 같은 어셈블리 |
| private protected | 상속 + 같은 클래스 내 |

---

<strong>💬 면접식 답변</strong>  
접근 제한자는 클래스의 캡슐화를 강화하는 역할을 합니다.  
보통 필드는 private, Property는 public으로 두어 내부 상태를 보호합니다.
</details>

---

<details markdown="1">
<summary><strong>23. static 키워드의 역할은?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- 클래스의 **인스턴스와 무관하게** 공통으로 사용하는 멤버를 정의.  

---

<strong>🔹 상세설명</strong>  
- static 멤버는 **클래스 단위로 공유**되며, 인스턴스 생성 없이 접근 가능합니다.  
- 프로그램 시작 시 메모리에 한 번만 로드됩니다.  
- 정적 생성자(static constructor)는 한 번만 실행됩니다.

---

<strong>💬 면접식 답변</strong>  
static은 모든 인스턴스가 공유하는 멤버를 정의할 때 사용합니다.  
전역 유틸리티 클래스나 Singleton 구현 시 자주 사용됩니다.
</details>

---

<details markdown="1">
<summary><strong>24. sealed 키워드는 언제 사용하나요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- 클래스의 **상속을 금지**하거나  
  메서드의 **재정의를 방지**할 때 사용.

---

<strong>🔹 상세설명</strong>  
- `sealed class` → 더 이상 상속 불가  
- `sealed override` → 상속받은 메서드 재정의 불가  

```csharp
sealed class Player {}
```

---

<strong>💬 면접식 답변</strong>  
sealed는 클래스나 메서드의 상속·재정의를 막아 안정성과 의도된 동작을 보장합니다.
</details>

---

<details markdown="1">
<summary><strong>25. const와 readonly의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **const**: 컴파일 타임 상수  
- **readonly**: 런타임에 한 번만 할당 가능  

---

<strong>🔹 상세설명</strong>  
- const는 컴파일 시 값이 결정됨. (컴파일된 곳에 상수값 삽입)  
- readonly는 생성자에서 한 번만 할당 가능.  

```csharp
const float PI = 3.14f;
readonly string Name = "Lorkhan";
```

---

<strong>💬 면접식 답변</strong>  
const는 컴파일 시 고정되는 상수이고, readonly는 런타임에 한 번만 설정 가능한 상수입니다.  
주로 객체별 상수엔 readonly를, 전역 상수엔 const를 사용합니다.
</details>

---

<details markdown="1">
<summary><strong>26. Thread.Sleep()과 Task.Delay()의 차이는 무엇인가요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- `Thread.Sleep()`은 **현재 스레드를 차단(blocking)**  
- `Task.Delay()`는 **비동기로 대기(non-blocking)**

---

<strong>🔹 상세설명</strong>  
- `Thread.Sleep(ms)`는 지정된 시간 동안 **스레드를 정지**시켜 CPU를 점유하지 않게 합니다.  
- 하지만, 스레드 자체가 멈추므로 UI나 다른 비동기 작업이 함께 정지할 수 있습니다.  
- `Task.Delay(ms)`는 Task 기반 비동기 대기 함수로, 스레드를 점유하지 않고 **비동기적으로 기다립니다.**

```csharp
await Task.Delay(1000); // 스레드 점유 X
Thread.Sleep(1000);     // 스레드 차단 O
```

---

<strong>💬 면접식 답변</strong>  
Thread.Sleep은 실제 스레드를 멈추지만, Task.Delay는 스레드를 점유하지 않고 기다립니다.  
UI나 서버 환경에서는 Task.Delay를 사용해야 효율적입니다.
</details>

---

<details markdown="1">
<summary><strong>27. ThreadPool이란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **재사용 가능한 스레드 집합**으로, Task나 비동기 처리의 기반 구조.  

---

<strong>🔹 상세설명</strong>  
- .NET 런타임은 미리 일정 수의 스레드를 생성해 두고, 요청이 오면 재활용합니다.  
- 스레드 생성·소멸 비용이 크기 때문에, 재사용을 통해 성능을 높입니다.  
- `Task.Run()` 역시 내부적으로 ThreadPool을 사용합니다.

---

<strong>💬 면접식 답변</strong>  
ThreadPool은 재사용 가능한 스레드 모음으로, 매번 새 스레드를 만들지 않고 효율적으로 작업을 처리합니다.  
대부분의 비동기 작업은 ThreadPool 기반으로 동작합니다.
</details>

---

<details markdown="1">
<summary><strong>28. lock 키워드는 내부적으로 어떻게 작동하나요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- Monitor.Enter/Exit 구조를 래핑한 문법.  
- 하나의 스레드만 해당 블록에 진입하도록 보장.

---

<strong>🔹 상세설명</strong>  
- `lock(obj)`는 Monitor 클래스를 이용해 스레드 간 자원 접근을 제어합니다.  
- 동시에 두 스레드가 같은 객체를 잠그려 하면, 하나는 대기 상태로 들어갑니다.  
- deadlock 방지를 위해 항상 lock 순서를 일관성 있게 유지해야 합니다.

```csharp
lock (syncObj) {
    // 임계 구역
}
```

---

<strong>💬 면접식 답변</strong>  
lock은 Monitor 기반으로 동작하며, 한 번에 한 스레드만 임계 구역에 들어갈 수 있게 합니다.  
단, lock 순서를 지키지 않으면 데드락이 발생할 수 있습니다.
</details>

---

<details markdown="1">
<summary><strong>29. volatile 키워드는 언제 사용하나요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- CPU 캐시가 아닌 **메인 메모리**에서 항상 값을 읽게 함.  
- 멀티스레드 환경에서 변수 일관성을 보장.

---

<strong>🔹 상세설명</strong>  
- 컴파일러나 CPU가 변수 접근을 최적화하면서 캐시를 사용할 수 있습니다.  
- volatile을 붙이면 해당 변수는 매번 메모리에서 읽고, 캐시에 저장되지 않습니다.  
- 단, 복합 연산(++, += 등)은 여전히 원자적이지 않으므로 lock이 필요할 수 있습니다.

---

<strong>💬 면접식 답변</strong>  
volatile은 멀티스레드 환경에서 변수의 최신 값을 보장하기 위한 키워드입니다.  
하지만 원자성을 보장하지 않으므로, 필요 시 lock과 함께 사용합니다.
</details>

---

<details markdown="1">
<summary><strong>30. Immutable 객체란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- 한 번 생성되면 **상태가 변경되지 않는 객체**  
- 스레드 안전(Thread-safe)함.

---

<strong>🔹 상세설명</strong>  
- 대표 예: `string`, `System.Uri`, `DateTime`  
- 내부 값이 바뀌면 새 객체를 반환합니다.  
- 데이터의 일관성과 스레드 안전성을 유지할 수 있습니다.  

```csharp
string a = "Hello";
string b = a.Replace("H", "Y"); // 새로운 문자열 생성
```

---

<strong>💬 면접식 답변</strong>  
Immutable 객체는 한 번 만들어지면 변경할 수 없기 때문에, 스레드 간 공유 시 안전합니다.  
string이나 record가 대표적인 예입니다.
</details>

---

<details markdown="1">
<summary><strong>31. Record와 Class의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- Record: **값 기반(Value-based)** 비교  
- Class: **참조 기반(Reference-based)** 비교

---

<strong>🔹 상세설명</strong>  
- record는 값의 동등성(Equals, ==)을 자동으로 비교합니다.  
- class는 참조 비교(같은 객체인지)만 수행합니다.  
- record는 `with` 키워드로 불변 데이터 복사가 가능합니다.  

```csharp
public record Player(string Name, int Level);
```

---

<strong>💬 면접식 답변</strong>  
record는 값 자체로 동일성을 판단하고, class는 참조로 판단합니다.  
즉, record는 DTO나 immutable 데이터 전달용으로 적합합니다.
</details>

---

<details markdown="1">
<summary><strong>32. 구조체(struct)와 클래스(class)의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **struct**: 값 타입 (스택에 저장)  
- **class**: 참조 타입 (힙에 저장)

---

<strong>🔹 상세설명</strong>  
- struct는 복사가 이루어지고, class는 참조가 전달됩니다.  
- struct는 상속이 불가능하지만 인터페이스는 구현할 수 있습니다.  
- 소규모 데이터 집합(Point, Vector 등)에 유리합니다.

---

<strong>💬 면접식 답변</strong>  
struct는 값 타입이라 가볍고 빠르지만 상속이 불가능합니다.  
class는 참조 타입으로, 더 큰 객체나 다형성이 필요한 경우 적합합니다.
</details>

---

<details markdown="1">
<summary><strong>33. Boxing과 Unboxing이란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **Boxing**: 값 타입을 참조 타입(object)으로 변환  
- **Unboxing**: 참조 타입을 다시 값 타입으로 변환

---

<strong>🔹 상세설명</strong>  
- 박싱 시 힙에 새 객체가 생성되어 값이 복사됩니다.  
- 언박싱 시 다시 스택으로 값을 꺼내와 캐스팅됩니다.  
- 성능 오버헤드가 크기 때문에 자주 수행되면 피해야 합니다.  

```csharp
int a = 10;
object b = a;   // Boxing
int c = (int)b; // Unboxing
```

---

<strong>💬 면접식 답변</strong>  
Boxing은 값 타입을 object로 감싸는 과정이고, Unboxing은 반대로 되돌리는 과정입니다.  
힙 메모리를 사용하므로 성능에 주의해야 합니다.
</details>

---

<details markdown="1">
<summary><strong>34. Nullable 타입이란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- 값 타입에 null을 허용하기 위한 타입(`T?`)  

---

<strong>🔹 상세설명</strong>  
- 일반 값 타입(int, float)은 null 불가.  
- `int?`, `float?` 등으로 선언하면 null 값을 가질 수 있습니다.  
- `HasValue`와 `Value` 속성으로 접근합니다.  

---

<strong>💬 면접식 답변</strong>  
Nullable은 값 타입에서도 null을 다룰 수 있게 해주는 기능입니다.  
DB나 입력 데이터에서 값이 비어 있을 수 있는 경우 유용합니다.
</details>

---

<details markdown="1">
<summary><strong>35. pattern matching이란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **조건 분기문을 더 간결하게 표현**하는 기능.  
- C# 7 이상부터 switch 표현식 강화.

---

<strong>🔹 상세설명</strong>  
- 타입 검사, 값 비교, null 체크를 간결하게 수행.  
- `switch`, `is`, `when` 패턴을 조합해 사용.  

```csharp
object obj = 42;
if (obj is int n && n > 0)
    Console.WriteLine("Positive int");
```

---

<strong>💬 면접식 답변</strong>  
pattern matching은 타입과 조건을 동시에 검사하는 문법으로,  
switch와 is 구문을 간결하게 만들어줍니다.
</details>

---

<details markdown="1">
<summary><strong>36. Delegate와 Event의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **Delegate**: 함수 포인터  
- **Event**: Delegate에 접근 제어자를 추가한 것

---

<strong>🔹 상세설명</strong>  
- Delegate는 콜백을 위한 타입으로, 여러 메서드를 동시에 등록할 수 있습니다.  
- Event는 외부에서 **+=, -=** 만 가능하고, 직접 호출은 불가능합니다.  

---

<strong>💬 면접식 답변</strong>  
Delegate는 함수를 직접 참조하는 타입이고, Event는 이를 안전하게 감싸 외부 호출을 막은 구조입니다.
</details>

---

<details markdown="1">
<summary><strong>37. GC.Collect()를 직접 호출하면 안 되는 이유는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- GC는 자동으로 최적화된 시점에 동작  
- 강제 호출 시 **성능 저하와 일시 정지** 발생

---

<strong>🔹 상세설명</strong>  
- GC는 세대별로 메모리를 효율적으로 관리합니다.  
- 개발자가 임의로 `GC.Collect()`를 호출하면, 모든 세대를 강제로 검사하여 프로그램이 멈출 수 있습니다.  
- 일반적으로 명시 호출은 권장되지 않습니다.  

---

<strong>💬 면접식 답변</strong>  
GC.Collect()는 런타임의 최적화된 타이밍을 깨뜨리므로, 직접 호출은 피해야 합니다.  
특수한 상황(대규모 자원 해제 직후 등)에서만 예외적으로 사용됩니다.
</details>

---

<details markdown="1">
<summary><strong>38. Span&lt;T&gt;와 Memory&lt;T&gt;의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- Span: **스택 메모리 전용**(비관리 메모리 접근 불가)  
- Memory: **비동기·힙 메모리까지 지원**

---

<strong>🔹 상세설명</strong>  
- Span은 구조체 기반이며, **스택 영역에서 빠른 슬라이싱**을 지원.  
- Memory는 힙·비동기 시나리오에서도 안전하게 데이터를 관리.  
- Span은 `ref struct`이므로 async, lambda 등에서 사용 불가.

---

<strong>💬 면접식 답변</strong>  
Span은 빠른 메모리 접근용 스택 기반 구조체고, Memory는 비동기·힙까지 확장 가능한 버전입니다.
</details>

---

<details markdown="1">
<summary><strong>39. IDisposable 패턴을 직접 구현한다면?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- Dispose 호출 여부를 추적하고, 중복 해제 방지.  

---

<strong>🔹 상세설명</strong>  
```csharp
public class MyClass : IDisposable {
    private bool disposed;
    public void Dispose() {
        if (!disposed) {
            disposed = true;
            GC.SuppressFinalize(this);
        }
    }
}
```

---

<strong>💬 면접식 답변</strong>  
Dispose 패턴은 리소스가 한 번만 해제되도록 보호하고,  
GC가 중복으로 호출하지 않게 SuppressFinalize를 사용합니다.
</details>

---

<details markdown="1">
<summary><strong>40. ValueTask란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- Task보다 **가벼운 비동기 반환 타입**  
- 결과가 즉시 제공될 때 성능 이점이 있음.

---

<strong>🔹 상세설명</strong>  
- Task는 항상 힙 객체를 생성하지만, ValueTask는 구조체로 힙 할당을 줄입니다.  
- 단, 재사용이나 중복 Await은 금지되어 있습니다.  

---

<strong>💬 면접식 답변</strong>  
ValueTask는 경량 비동기 반환 타입으로, 자주 호출되는 함수에서 불필요한 Task 생성 비용을 줄여줍니다.
</details>

---

<details markdown="1">
<summary><strong>41. LINQ에서 지연 실행(Lazy Evaluation)이란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- 결과가 실제로 필요할 때만 쿼리가 실행되는 메커니즘.

---

<strong>🔹 상세설명</strong>  
- LINQ의 대부분의 연산자는 `IEnumerable`을 반환합니다.  
- `ToList()`, `Count()` 등을 호출해야 쿼리가 실행됩니다.  

---

<strong>💬 면접식 답변</strong>  
LINQ는 기본적으로 지연 실행을 사용해, 결과가 필요할 때만 실제 데이터를 가져옵니다.  
이를 통해 불필요한 연산을 줄일 수 있습니다.
</details>

---

<details markdown="1">
<summary><strong>42. async void는 왜 피해야 할까?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- 예외를 잡을 수 없고, 호출자가 제어할 수 없음.

---

<strong>🔹 상세설명</strong>  
- 일반적으로 `Task`나 `Task<T>`를 반환해야 예외를 await로 전달 가능.  
- async void는 이벤트 핸들러 외에는 사용 금지.  

---

<strong>💬 면접식 답변</strong>  
async void는 호출자가 완료 여부나 예외를 알 수 없기 때문에, Task를 반환해야 안전한 비동기 처리가 가능합니다.
</details>

---

<details markdown="1">
<summary><strong>43. Parallel.ForEach()의 장단점은?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- 장점: 자동 병렬화, 빠른 데이터 처리  
- 단점: 순서 보장 X, 부하 균형 이슈

---

<strong>🔹 상세설명</strong>  
- 내부적으로 ThreadPool을 사용하여 병렬 반복 수행.  
- 작업 순서가 중요하거나 부하가 불균일하면 속도 저하 가능.  

---

<strong>💬 면접식 답변</strong>  
Parallel.ForEach는 CPU 코어를 활용해 반복문을 병렬 실행하지만,  
순서나 부하 균형이 중요하다면 Task 기반 병렬화가 더 적합합니다.
</details>

---

<details markdown="1">
<summary><strong>44. LINQ의 Select와 SelectMany의 차이는?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- Select: 1:1 매핑  
- SelectMany: 1:N 평탄화(flatten)

---

<strong>🔹 상세설명</strong>  
- Select는 각 요소를 새 값으로 변환.  
- SelectMany는 내부 컬렉션을 하나로 평탄화.  

```csharp
list.SelectMany(x => x.Items);
```

---

<strong>💬 면접식 답변</strong>  
Select는 요소 변환, SelectMany는 내부 컬렉션을 풀어서 단일 시퀀스로 만듭니다.
</details>

---

<details markdown="1">
<summary><strong>45. yield return은 어떤 역할을 하나요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- 반복자(iterator)를 간결하게 구현하는 문법.  

---

<strong>🔹 상세설명</strong>  
- 상태를 저장하고, 다음 호출 시 이어서 실행됩니다.  
- 전체 데이터를 미리 만들지 않고 순차적으로 반환.  

```csharp
IEnumerable<int> GetNumbers() {
    for (int i = 0; i < 3; i++) yield return i;
}
```

---

<strong>💬 면접식 답변</strong>  
yield return은 데이터를 한 번에 모두 생성하지 않고, 필요할 때 하나씩 반환하는 반복자입니다.
</details>

---

<details markdown="1">
<summary><strong>46. expression-bodied member란?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- 간단한 속성·메서드를 `=>`로 표현하는 문법.  

---

<strong>🔹 상세설명</strong>  
```csharp
public int HP => _hp;
public void Print() => Console.WriteLine("Hello");
```
- 한 줄로 작성할 수 있어 가독성이 높습니다.

---

<strong>💬 면접식 답변</strong>  
expression-bodied member는 간단한 getter나 단일 구문을 깔끔하게 표현하기 위한 문법입니다.
</details>

---

<details markdown="1">
<summary><strong>47. async/await는 내부적으로 어떻게 동작하나요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- 컴파일러가 **상태 머신(state machine)** 으로 변환  
- `await` 지점에서 **비동기 작업 완료 콜백(continuation)** 을 등록  
- UI/ASP.NET에서는 기본적으로 **SynchronizationContext** 를 통해 컨텍스트 복귀

---

<strong>🔹 상세설명</strong>  
- `async` 메서드는 컴파일 타임에 상태 머신으로 변환되어, 각 `await` 지점이 **상태 전이** 포인트가 됩니다.  
- `await task`는 **작업이 완료되면 이어서 실행할 델리게이트(continuation)** 를 등록하고 현재 스택을 반환합니다(논블로킹).  
- **컨텍스트 캡처**: 기본적으로 UI/ASP.NET SynchronizationContext를 캡처하여 **await 이후 원래 컨텍스트**로 돌아갑니다.  
  - 라이브러리 코드에서는 **`ConfigureAwait(false)`** 로 컨텍스트 복귀를 비활성화해 데드락 위험과 오버헤드를 줄입니다.  
- 반환 타입: `Task`, `Task<T>`, `ValueTask`, `void(이벤트 한정)`.

---

<strong>💬 면접식 답변</strong>  
async/await는 컴파일러가 상태 머신으로 바꿔서 `await` 시점에 작업 완료 콜백을 등록합니다.  
UI나 ASP.NET 환경에서는 기본적으로 원래 컨텍스트로 복귀하고, 라이브러리 코드는 `ConfigureAwait(false)`로 복귀를 피하는 편이 안전합니다.
</details>

---

<details markdown="1">
<summary><strong>48. CancellationToken은 어떻게 사용하는 게 좋나요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **협력적 취소(cooperative cancellation)** 를 위한 토큰  
- `CancellationTokenSource` → `Token` 전달 → 작업 내에서 정기적으로 **`ThrowIfCancellationRequested()`** 또는 **IsCancellationRequested** 확인

---

<strong>🔹 상세설명</strong>  
- 취소 발신자: `CancellationTokenSource cts;` → `cts.Cancel()`  
- 취소 수신자: 메서드 시그니처에 `CancellationToken token` 전달  
- CPU 바운드 루프에서는 **주기적으로 토큰 확인**, I/O 바운드 작업은 **토큰 지원 API** 에 전달(예: `ReadAsync(..., token)`).  
- **권장 패턴**  
  - 라이브러리/공용 API는 무조건 `CancellationToken` 인자를 노출  
  - 취소 시 **정상 플로우** 로 처리(예외는 `OperationCanceledException`)  
  - 취소 후 **부분 결과/리소스 정리** 보장

```csharp
public async Task RunAsync(CancellationToken token) {
    while (true) {
        token.ThrowIfCancellationRequested();
        await Task.Delay(100, token);
    }
}
```

---

<strong>💬 면접식 답변</strong>  
CancellationToken은 호출자가 취소를 알릴 수 있도록 하는 협력적 메커니즘입니다.  
토큰을 API에 전파하고, 루프나 대기 지점에서 정기적으로 확인해 즉시 중단하도록 구현합니다.
</details>

---

<details markdown="1">
<summary><strong>49. ConfigureAwait(false)는 언제, 왜 쓰나요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **컨텍스트 복귀 방지**로 오버헤드/데드락 위험 감소  
- **라이브러리/백엔드 코드**에서 권장, **UI 스레드가 필요한 코드**에서는 사용 주의

---

<strong>🔹 상세설명</strong>  
- 기본 await는 SynchronizationContext를 캡처하고 완료 후 **원래 컨텍스트로 복귀**합니다.  
- 서버/라이브러리 환경에서는 컨텍스트 복귀가 불필요하므로 **`ConfigureAwait(false)`** 를 붙여 **스레드 전환 비용**과 **데드락 위험**을 줄입니다.  
- 단, **UI 업데이트가 필요한 코드**에서는 복귀가 필요하므로 `false`를 사용하면 안 됩니다.  

```csharp
await SomeIoAsync().ConfigureAwait(false); // 라이브러리/서버 코드 권장
```

---

<strong>💬 면접식 답변</strong>  
UI 의존성이 없는 라이브러리나 서버 사이드에서는 `ConfigureAwait(false)`로 컨텍스트 복귀를 막아 성능과 안정성을 높입니다.  
UI 스레드 접근이 필요한 구간에서는 기본 await를 사용해 안전하게 복귀합니다.
</details>

---

<details markdown="1">
<summary><strong>50. async/await에서 데드락은 왜 발생하고, 어떻게 피하나요?</strong></summary>

<strong>🧠 핵심 요약</strong>  
- **동기 대기(.Result/.Wait)** + **컨텍스트 캡처** 조합이 원인  
- 해결: **끝까지 async 전파**, **`ConfigureAwait(false)` 사용**, **동기 대기 금지**

---

<strong>🔹 상세설명</strong>  
- UI/ASP.NET SynchronizationContext가 있는 환경에서 `task.Result`로 동기 대기하면,  
  await 이후 **원래 컨텍스트로 복귀하려는 continuation** 이 **블록된 스레드**를 기다리면서 **교착 상태**가 발생합니다.  
- **해결책**  
  1) **async all the way**: 호출 스택 끝까지 비동기로 전파  
  2) 라이브러리/서버 코드에서 **`ConfigureAwait(false)`** 사용해 복귀 차단  
  3) 불가피한 경우 전용 백그라운드 스레드에서 동기 대기(권장 X)  

---

<strong>💬 면접식 답변</strong>  
데드락은 동기 대기와 컨텍스트 복귀가 맞물릴 때 발생합니다.  
그래서 비동기는 끝까지 async로 전파하고, 라이브러리 코드는 `ConfigureAwait(false)`를 사용해 컨텍스트 복귀를 막습니다.
</details>
