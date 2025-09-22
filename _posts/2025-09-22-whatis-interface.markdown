---
layout: post
title: "인터페이스(Interface)란? - 계약 기반 프로그래밍의 핵심"
date: 2025-09-22 14:00:00 +0900
categories: [Tech Interview, CS Fundamentals]
tags: [interface, oop, abstraction, polymorphism, design-pattern, programming-concept]
slug: whatis-interface
---

# 인터페이스(Interface)란?


## 📌 학습 목표
- 인터페이스 이해 및 활용  
- [추상 클래스]({{ site.baseurl }}/posts/whatis-abstractclass/)와 비교  
- 다형성, 느슨한 결합 설계 적용  

## 📌 정의
**인터페이스(Interface)**는 클래스가 구현해야 할 **메서드와 속성의 명세(계약)**를 정의하는 추상적인 타입입니다. 구현부 없이 **"무엇을 해야 하는가"**만 정의하며, 구현 클래스가 **"어떻게 할 것인가"**를 결정합니다.

## 📝 개념 정리
- 메서드/속성 시그니처만 제공, 구현은 클래스/구조체가 담당  
- 다중 구현 가능 (C# 단일 상속 제약 보완)  
- C# 8.0 이후 `default implementation` 지원  

---

## 🔑 핵심 특징

### 1. 계약(Contract) 정의
```csharp
// 인터페이스 - 계약 정의
public interface IDrawable
{
    void Draw();                    // 반드시 구현해야 함
    void SetColor(string color);    // 반드시 구현해야 함
    string GetInfo();               // 반드시 구현해야 함
}

// 구현 클래스 - 계약 이행
public class Circle : IDrawable
{
    private string color = "Black";

    public void Draw()
    {
        Console.WriteLine($"{color} 원을 그립니다.");
    }

    public void SetColor(string color)
    {
        this.color = color;
    }

    public string GetInfo()
    {
        return $"Color: {color}, Shape: Circle";
    }
}
```

### 2. 다중 구현 지원
```csharp
public interface IMovable
{
    void Move(int x, int y);
}

public interface IResizable
{
    void Resize(int width, int height);
}

// 여러 인터페이스 동시 구현
public class Rectangle : IDrawable, IMovable, IResizable
{
    private int x, y, width, height;
    private string color;

    public void Draw()
    {
        Console.WriteLine($"{color} 사각형 ({x},{y}) 크기 {width}x{height}");
    }

    public void Move(int newX, int newY)
    {
        x = newX;
        y = newY;
    }

    public void Resize(int newWidth, int newHeight)
    {
        width = newWidth;
        height = newHeight;
    }

    // IDrawable의 나머지 메서드들도 구현...
}
```

---

## 💡 인터페이스 vs 추상 클래스

| 구분 | 인터페이스 | 추상 클래스 |
|------|------------|-------------|
| **다중 상속** | ✅ 다중 구현 가능 | ❌ 단일 상속만 |
| **구현부** | ❌ 없음 (C# 8.0+는 기본 구현 가능) | ✅ 일부 메서드 구현 가능 |
| **필드** | ❌ 인스턴스 필드 불가 | ✅ 인스턴스 필드 가능 |
| **생성자** | ❌ 불가 | ✅ 가능 |
| **접근 제한자** | ✅ public (기본) | ✅ public, protected, private |
| **용도** | **계약 정의** | **공통 구현 + 강제 구현** |

```csharp
// 추상 클래스 - 공통 구현 제공
public abstract class Animal
{
    protected string name;  // 공통 필드

    // 공통 구현
    public void Sleep()
    {
        Console.WriteLine($"{name}이(가) 잠을 잡니다.");
    }

    // 강제 구현
    public abstract void MakeSound();
}

// 인터페이스 - 순수한 계약
public interface IFeedable
{
    void Feed(string food);
    bool IsHungry { get; }
}

// 둘 다 활용
public class Dog : Animal, IFeedable
{
    public bool IsHungry { get; private set; } = true;

    public override void MakeSound()
    {
        Console.WriteLine("멍멍!");
    }

    public void Feed(string food)
    {
        Console.WriteLine($"{food}를 먹습니다.");
        IsHungry = false;
    }
}
```

---

## 🎯 실제 사용 예제

### 1. 의존성 역전 원칙 (DIP) 적용

상위 모듈(비즈니스 로직)은 하위 모듈(파일 로깅, DB 저장)의 구체 클래스에 묶이면 변경에 취약해진다. 인터페이스를 사이에 두면 상위 모듈은 “무엇을 한다”는 계약(ILogger, IDatabase) 에만 의존하고, 실제 구현(FileLogger, SqlDatabase)은 쉽게 교체 가능하다(파일→콘솔, SQL→NoSQL 등). 테스트에서도 목 객체(Mock Object)를 주입하기가 쉬워진다.

```csharp
// 인터페이스로 추상화
public interface ILogger
{
    void Log(string message);
    void LogError(string error);
}

public interface IDatabase
{
    void Save(object data);
    T Load<T>(int id);
}

// 고수준 모듈 - 추상화에 의존
public class UserService
{
    private readonly ILogger logger;
    private readonly IDatabase database;

    // 인터페이스에 의존 (구체 클래스가 아닌)
    public UserService(ILogger logger, IDatabase database)
    {
        this.logger = logger;
        this.database = database;
    }

    public void CreateUser(string name)
    {
        var user = new User { Name = name };

        try
        {
            database.Save(user);
            logger.Log($"사용자 생성: {name}");
        }
        catch (Exception ex)
        {
            logger.LogError($"사용자 생성 실패: {ex.Message}");
        }
    }
}

// 저수준 모듈 - 구체적인 구현
public class FileLogger : ILogger
{
    public void Log(string message)
    {
        File.AppendAllText("app.log", $"[INFO] {DateTime.Now}: {message}\n");
    }

    public void LogError(string error)
    {
        File.AppendAllText("error.log", $"[ERROR] {DateTime.Now}: {error}\n");
    }
}

public class SqlDatabase : IDatabase
{
    private readonly string connectionString;

    public void Save(object data)
    {
        // SQL 데이터베이스에 저장
    }

    public T Load<T>(int id)
    {
        // SQL 데이터베이스에서 로드
        return default(T);
    }
}

// 사용
var logger = new FileLogger();
var database = new SqlDatabase();
var userService = new UserService(logger, database);
```

### 2. 전략 패턴 (Strategy Pattern)

결제 같은 “행위”가 여러 방식으로 바뀌는 영역은 전략 인터페이스(IPaymentStrategy) 를 두고, 각 구현(신용카드/페이팔)을 런타임에 주입하면 된다. 조건문 분기 없이 확장(새 결제수단 추가) 에 열려 있고, 테스트도 쉬워진다.

```csharp
public interface IPaymentStrategy
{
    bool ProcessPayment(decimal amount);
    string GetPaymentMethod();
}

public class CreditCardPayment : IPaymentStrategy
{
    private readonly string cardNumber;

    public CreditCardPayment(string cardNumber)
    {
        this.cardNumber = cardNumber;
    }

    public bool ProcessPayment(decimal amount)
    {
        Console.WriteLine($"신용카드({cardNumber})로 {amount}원 결제");
        return true;
    }

    public string GetPaymentMethod() => "Credit Card";
}

public class PayPalPayment : IPaymentStrategy
{
    private readonly string email;

    public PayPalPayment(string email)
    {
        this.email = email;
    }

    public bool ProcessPayment(decimal amount)
    {
        Console.WriteLine($"PayPal({email})로 {amount}원 결제");
        return true;
    }

    public string GetPaymentMethod() => "PayPal";
}

public class PaymentProcessor
{
    public void ProcessOrder(decimal amount, IPaymentStrategy paymentStrategy)
    {
        Console.WriteLine($"결제 방법: {paymentStrategy.GetPaymentMethod()}");

        if (paymentStrategy.ProcessPayment(amount))
        {
            Console.WriteLine("결제 성공!");
        }
        else
        {
            Console.WriteLine("결제 실패!");
        }
    }
}

// 사용
var processor = new PaymentProcessor();
processor.ProcessOrder(100000, new CreditCardPayment("1234-****-****-5678"));
processor.ProcessOrder(50000, new PayPalPayment("user@example.com"));
```

---

## 🔧 고급 활용

### 1. 제네릭 인터페이스

CRUD 같은 공통 저장소 패턴은 제네릭 인터페이스(IRepository<T>) 로 일반화하면, 타입별 중복을 줄이면서 형식 안전성을 얻을 수 있다. 제약(where T : class)을 통해 의도하지 않은 사용을 방지할 수 있고, 메모리/DB/원격저장소 등 구현체도 쉽게 바꾼다.

```csharp
public interface IRepository<T> where T : class
{
    void Add(T entity);
    T GetById(int id);
    IEnumerable<T> GetAll();
    void Update(T entity);
    void Delete(int id);
}

public class UserRepository : IRepository<User>
{
    private readonly List<User> users = new List<User>();

    public void Add(User entity)
    {
        users.Add(entity);
    }

    public User GetById(int id)
    {
        return users.FirstOrDefault(u => u.Id == id);
    }

    public IEnumerable<User> GetAll()
    {
        return users.AsReadOnly();
    }

    public void Update(User entity)
    {
        var existing = GetById(entity.Id);
        if (existing != null)
        {
            // 업데이트 로직
        }
    }

    public void Delete(int id)
    {
        var user = GetById(id);
        if (user != null)
        {
            users.Remove(user);
        }
    }
}
```

### 2. 인터페이스 상속

인터페이스는 다른 인터페이스를 상속해 행위를 조합할 수 있다. 예를 들어 `IShape`(면적), `I3DShape`(부피), `IColoredShape`(색상)를 조합하면, 구현체는 필요한 능력만 선택적으로 구현할 수 있다. 클래스의 다중 상속은 안되지만, 인터페이스의 다중 상속으로 유연한 모델링이 가능하다.

```csharp
public interface IShape
{
    double CalculateArea();
}

public interface IColoredShape : IShape
{
    string Color { get; set; }
    void ChangeColor(string newColor);
}

public interface I3DShape : IShape
{
    double CalculateVolume();
}

// 다중 인터페이스 상속 구현
public class ColoredCube : IColoredShape, I3DShape
{
    public string Color { get; set; }
    public double SideLength { get; set; }

    public double CalculateArea()
    {
        return 6 * SideLength * SideLength;  // 정육면체 표면적
    }

    public double CalculateVolume()
    {
        return SideLength * SideLength * SideLength;
    }

    public void ChangeColor(string newColor)
    {
        Color = newColor;
        Console.WriteLine($"색상이 {newColor}로 변경되었습니다.");
    }
}
```

---

## ⚡ 성능 고려사항

### 1. 인터페이스 호출 비용

인터페이스를 통한 호출은 런타임 디스패치(가상 호출)가 일어나 직접 호출보다 약간 느릴 수 있다. 대부분의 비즈니스 로직에서는 미미하지만, 핫패스(초고빈도 경로) 에서는 구조를 재검토하거나 정적 다형성(제네릭, 인라인 가능 코드) 을 고려해야 한다.

```csharp
// 직접 호출 (빠름)
var circle = new Circle();
circle.Draw();

// 인터페이스를 통한 호출 (약간 느림 - 가상 메서드 호출)
IDrawable drawable = new Circle();
drawable.Draw();  // 런타임에 실제 구현체 결정
```

### 2. 인터페이스 캐스팅 최적화
```csharp
public void ProcessShapes(IEnumerable<IShape> shapes)
{
    foreach (var shape in shapes)
    {
        // 나쁜 예 - 매번 is/as 사용
        if (shape is IColoredShape coloredShape1)
        {
            coloredShape1.ChangeColor("Red");
        }

        // 좋은 예 - 한 번만 확인
        if (shape is IColoredShape coloredShape2)
        {
            coloredShape2.ChangeColor("Blue");
            // coloredShape2를 계속 사용
        }
    }
}
```

---

## 🎯 면접 질문 & 답변

**Q: 인터페이스를 사용하는 이유는?**
A:
1. **느슨한 결합** - 구체 클래스가 아닌 추상화에 의존
2. **다형성 구현** - 같은 인터페이스를 다르게 구현
3. **테스트 용이성** - Mock 객체 생성 쉬움
4. **확장성** - 새로운 구현체 추가가 용이

**Q: 언제 추상 클래스 대신 인터페이스를 사용하나?**
A:
- **다중 상속**이 필요할 때
- **순수한 계약**만 정의하고 싶을 때
- **구현체들이 완전히 다른 방식**으로 동작할 때
- **플러그인 아키텍처**나 **전략 패턴** 구현시

---

## 💡 실무 적용 팁

1. **ISP(Interface Segregation Principle) 준수**
   - 큰 인터페이스보다 작고 구체적인 인터페이스들로 분리

2. **명명 규칙**
   - C#: `I` 접두사 (IDrawable, ILogger)
   - Java: 접두사 없음 (Drawable, Logger)

3. **인터페이스 설계 원칙**
   - 클라이언트가 필요하지 않은 메서드에 의존하지 않게 설계
   - 안정적이고 변경이 적은 계약 정의

---

## 🔗 관련 개념
- [추상 클래스]({{ site.baseurl }}/posts/whatis-abstractclass/)
- [다형성]({{ site.baseurl }}/posts/whatis-polymorphism/)
- [의존성 주입]({{ site.baseurl }}/posts/whatis-dependency-injection/)
- [SOLID 원칙]({{ site.baseurl }}/posts/whatis-solid/)