```
---
layout: post
title: "C# 메모리 관리 (GC, IDisposable, async/await, 박싱과 언박싱 완벽 가이드)"
date: 2025-09-22 18:00:00 +0900
categories: [Tech Interview, Study Plan]
tags: [c#, memory, garbage-collector, IDisposable, async-await, boxing, unboxing]
slug: csharp-memory-management
---

# C# 메모리 관리 (GC, IDisposable, async/await, 박싱과 언박싱)

## 📌 학습 목표
- [Garbage Collector]({{ site.baseurl }}/posts/whatis-gc/) 동작 방식 이해  
- [IDisposable]({{ site.baseurl }}/posts/whatis-idisposable/) 패턴 활용  
- [async-await]({{ site.baseurl }}/posts/whatis-asyncawait/)의 작동 원리 이해  
- [박싱과 언박싱]({{ site.baseurl }}/posts/whatis-boxingunboxing/) 문제 확인  

---

## 📝 개념 정리

### 1. [Garbage Collector]({{ site.baseurl }}/posts/whatis-gc/)

**기본 동작 원리**  
- 세대별 관리(Gen 0, 1, 2)로 메모리 효율성 극대화  
- 불필요한 객체 탐색 후 해제  
- Stop-the-world 발생 가능 (성능 이슈)  

**세대별 GC 동작**
```csharp
// Gen 0: 새로 생성된 객체들 (가장 빈번한 수집)
var temp = new StringBuilder(); // Gen 0에 할당

// Gen 1: Gen 0 수집에서 살아남은 객체들
// Gen 2: 장기간 살아있는 객체들 (수집 빈도 낮음)
```

---

### 2. [IDisposable]({{ site.baseurl }}/posts/whatis-idisposable/)

**핵심 개념**  
- `Dispose()` 메서드를 통한 명시적 자원 해제  
- `using` 문에서 자동 호출 보장  
- 파일, DB 연결, 네트워크 소켓 등 자원 해제에 필수  

**구현 패턴**
```csharp
public class MyResource : IDisposable
{
    private bool disposed = false;
    private FileStream fileStream;

    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }

    protected virtual void Dispose(bool disposing)
    {
        if (!disposed)
        {
            if (disposing)
            {
                // 관리되는 자원 해제
                fileStream?.Dispose();
            }
            // 관리되지 않는 자원 해제
            disposed = true;
        }
    }

    ~MyResource()
    {
        Dispose(false);
    }
}
```

---

### 3. [async-await]({{ site.baseurl }}/posts/whatis-asyncawait/)

**동작 원리**  
- Task 기반 비동기 처리 모델  
- 컴파일러가 상태 머신으로 변환하여 실행  
- 스레드를 블록하지 않고 비동기적으로 작업 수행  

**실제 변환 과정**
```csharp
// 원본 코드
public async Task<string> GetDataAsync()
{
    await Task.Delay(1000);
    return "Data loaded";
}

// 컴파일러가 생성하는 상태 머신 (개념적)
public Task<string> GetDataAsync()
{
    var stateMachine = new GetDataAsyncStateMachine();
    stateMachine.builder = AsyncTaskMethodBuilder<string>.Create();
    stateMachine.state = -1;
    stateMachine.builder.Start(ref stateMachine);
    return stateMachine.builder.Task;
}
```

---

### 4. [박싱과 언박싱]({{ site.baseurl }}/posts/whatis-boxingunboxing/)

**성능 문제**  
- 값 타입을 object로 변환할 때 발생(박싱)  
- 다시 꺼낼 때 캐스팅 필요(언박싱)  
- 힙 할당과 GC 압박 증가로 성능 비용 발생  

**문제 코드와 해결책**
```csharp
// 문제: 박싱 발생
ArrayList list = new ArrayList();
list.Add(42); // int가 object로 박싱
int value = (int)list[0]; // 언박싱

// 해결: 제네릭 사용
List<int> genericList = new List<int>();
genericList.Add(42); // 박싱 없음
int value2 = genericList[0]; // 언박싱 없음
```

---

## 💻 실무 예제 코드

### IDisposable 활용
```csharp
// using 문을 통한 자동 자원 해제
using (var fs = new FileStream("data.txt", FileMode.Open))
{
    // 파일 작업 수행
    // 블록 종료 시 자동으로 Dispose() 호출
}

// C# 8.0 using 선언
using var connection = new SqlConnection(connectionString);
connection.Open();
// 메서드 종료 시 자동으로 Dispose() 호출
```

### async/await 패턴
```csharp
public async Task<List<User>> GetUsersAsync()
{
    try
    {
        // 비동기 HTTP 요청
        var response = await httpClient.GetAsync("/api/users");
        var json = await response.Content.ReadAsStringAsync();
        
        // JSON 파싱도 비동기로
        return await JsonSerializer.DeserializeAsync<List<User>>(
            new MemoryStream(Encoding.UTF8.GetBytes(json)));
    }
    catch (HttpRequestException ex)
    {
        // 예외 처리
        throw new ServiceException("사용자 데이터 로드 실패", ex);
    }
}
```

---

## 🎯 연습 문제
1. `IDisposable`을 구현하는 `NetworkClient` 클래스를 작성하고 적절한 Dispose 패턴을 구현하세요.  
2. [박싱과 언박싱]({{ site.baseurl }}/posts/whatis-boxingunboxing/)이 발생하는 부분을 찾아 제네릭으로 최적화하세요.  
3. `async Task` 메서드와 `ConfigureAwait(false)`의 의미와 사용 사례를 설명하세요.  

---

## 🔎 심화 학습

### GC 최적화 기법
- **Server GC vs Workstation GC**: 멀티코어 서버 환경에서의 차이점  
- **GC 모드 설정**: `<gcServer>`와 `<gcConcurrent>` 설정  
- **대용량 객체 힙(LOH)**: 85KB 이상 객체들의 특별한 관리  

### 최신 C# 비동기 패턴
```csharp
// IAsyncDisposable 활용 (C# 8.0+)
await using var stream = new FileStream("large-file.dat", FileMode.Open);
// 비동기 Dispose 호출

// IAsyncEnumerable 활용 (C# 8.0+)
await foreach (var item in GetDataStreamAsync())
{
    ProcessItem(item);
}
```

---

## 🌐 외부 링크
- [Microsoft Docs - Garbage Collection](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/)  
- [Async/Await Best Practices](https://docs.microsoft.com/en-us/archive/msdn-magazine/2013/march/async-await-best-practices-in-asynchronous-programming)  
- [IDisposable Pattern Guidelines](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/dispose-pattern)  

---

## 💡 실무 적용 팁
1. **GC 모니터링**: `GC.GetTotalMemory()`와 성능 카운터로 메모리 사용량 추적  
2. **IDisposable 일관성**: 팀 내에서 using 문 사용 가이드라인 수립  
3. **async/await 데드락 방지**: UI 애플리케이션에서 `ConfigureAwait(false)` 활용  
4. **박싱 방지**: 컬렉션 사용 시 제네릭 타입 우선 선택  

---

## 🪞 회고
- GC의 세대별 수집 방식이 성능에 미치는 영향을 설명할 수 있는가?  
- [IDisposable]({{ site.baseurl }}/posts/whatis-idisposable/) 패턴을 실무에서 어떻게 적용할 수 있을까?  
- async/await 사용 시 주의해야 할 데드락 상황은 무엇인가?  
- [박싱과 언박싱]({{ site.baseurl }}/posts/whatis-boxingunboxing/)으로 인한 성능 저하를 어떻게 측정하고 개선할 수 있을까?  

---

## 🔗 관련 페이지
- [Garbage Collector]({{ site.baseurl }}/posts/whatis-gc/)  
- [IDisposable]({{ site.baseurl }}/posts/whatis-idisposable/)  
- [async-await]({{ site.baseurl }}/posts/whatis-asyncawait/)  
- [박싱과 언박싱]({{ site.baseurl }}/posts/whatis-boxingunboxing/)  
- [RAII]({{ site.baseurl }}/posts/whatis-raii/)  
```
