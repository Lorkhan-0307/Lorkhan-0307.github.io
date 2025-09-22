---
layout: post
title: "디자인 패턴(Design Patterns)이란? - 객체지향 설계의 검증된 해법"
date: 2025-09-22 18:00:00 +0900
categories: [Tech Interview, Design Patterns]
tags: [design-patterns, gof, singleton, observer, strategy, factory, mvc, game-development]
slug: whatis-design-patterns
---

# 디자인 패턴(Design Patterns)이란?

## 📌 학습 목표
- 디자인 패턴의 개념과 GOF 23개 패턴 이해
- 게임 개발에서 자주 사용되는 패턴들 파악
- 각 패턴의 장단점과 적절한 사용 시점 습득
- 실제 게임 프로젝트에서의 패턴 적용 사례 학습

## 📌 정의
**디자인 패턴(Design Patterns)**은 소프트웨어 설계에서 **반복적으로 발생하는 문제들에 대한 검증된 해결책**입니다. 객체 간의 상호작용과 책임 분배를 통해 **재사용 가능하고 유지보수가 쉬운 코드**를 작성하기 위한 설계 템플릿입니다.

## 📝 개념 정리
- **GOF(Gang of Four)**: 에리히 감마, 리처드 헬름, 랄프 존슨, 존 블리시디스가 정의한 23개 패턴
- **패턴의 구성**: 이름, 문제, 해법, 결과로 구성
- **카테고리**: 생성(Creational), 구조(Structural), 행동(Behavioral) 패턴
- **안티패턴**: 잘못된 설계 관행들 (싱글톤 남용, God Object 등)

---

## 🔑 디자인 패턴의 분류

### 1. 생성 패턴 (Creational Patterns)
객체 생성과 관련된 패턴들
- **싱글톤(Singleton)**: 클래스의 인스턴스를 하나로 제한
- **팩토리(Factory)**: 객체 생성 로직을 캡슐화
- **빌더(Builder)**: 복잡한 객체를 단계별로 생성
- **프로토타입(Prototype)**: 기존 객체를 복제하여 새 객체 생성

### 2. 구조 패턴 (Structural Patterns)
클래스나 객체의 구성과 관련된 패턴들
- **어댑터(Adapter)**: 호환되지 않는 인터페이스를 연결
- **데코레이터(Decorator)**: 객체에 새로운 기능을 동적으로 추가
- **퍼사드(Facade)**: 복잡한 시스템에 단순한 인터페이스 제공
- **컴포지트(Composite)**: 트리 구조로 객체들을 구성

### 3. 행동 패턴 (Behavioral Patterns)
객체 간의 상호작용과 책임 분배와 관련된 패턴들
- **옵저버(Observer)**: 객체 상태 변화를 관련 객체들에게 알림
- **전략(Strategy)**: 알고리즘을 교체 가능하게 만듦
- **커맨드(Command)**: 요청을 객체로 캡슐화
- **상태(State)**: 객체의 상태에 따라 행동 변경

---

## 🎮 게임 개발에서 자주 사용되는 패턴

### 1. 싱글톤 패턴 (Singleton Pattern)
```csharp
// 게임 매니저 - 전역 접근이 필요한 시스템
public class GameManager
{
    private static GameManager instance;
    private static readonly object lockObject = new object();

    public static GameManager Instance
    {
        get
        {
            if (instance == null)
            {
                lock (lockObject)
                {
                    if (instance == null)
                        instance = new GameManager();
                }
            }
            return instance;
        }
    }

    private GameManager() { }

    public GameState CurrentState { get; private set; }
    public void ChangeState(GameState newState) { /* 구현 */ }
}

// 사용
GameManager.Instance.ChangeState(GameState.Playing);
```

### 2. 옵저버 패턴 (Observer Pattern)
```csharp
// 게임 이벤트 시스템
public interface IGameEventObserver
{
    void OnPlayerLevelUp(int newLevel);
    void OnEnemyDefeated(Enemy enemy);
    void OnItemCollected(Item item);
}

public class GameEvents
{
    private List<IGameEventObserver> observers = new List<IGameEventObserver>();

    public void Subscribe(IGameEventObserver observer)
    {
        observers.Add(observer);
    }

    public void NotifyPlayerLevelUp(int newLevel)
    {
        foreach (var observer in observers)
        {
            observer.OnPlayerLevelUp(newLevel);
        }
    }
}

// UI 시스템이 게임 이벤트를 구독
public class UIManager : IGameEventObserver
{
    public void OnPlayerLevelUp(int newLevel)
    {
        ShowLevelUpNotification(newLevel);
    }

    public void OnEnemyDefeated(Enemy enemy)
    {
        UpdateScore(enemy.Points);
    }

    public void OnItemCollected(Item item)
    {
        UpdateInventoryDisplay();
    }
}
```

### 3. 전략 패턴 (Strategy Pattern)
```csharp
// AI 행동 전략
public interface IAIBehavior
{
    void Execute(Enemy enemy, Player target);
}

public class AggressiveAI : IAIBehavior
{
    public void Execute(Enemy enemy, Player target)
    {
        // 적극적으로 플레이어 추적 및 공격
        enemy.MoveTowards(target.Position);
        if (enemy.IsInRange(target))
            enemy.Attack(target);
    }
}

public class DefensiveAI : IAIBehavior
{
    public void Execute(Enemy enemy, Player target)
    {
        // 체력이 낮으면 도망, 높으면 천천히 접근
        if (enemy.Health < enemy.MaxHealth * 0.3f)
            enemy.Retreat();
        else
            enemy.PatrolArea();
    }
}

public class Enemy
{
    private IAIBehavior behavior;

    public void SetBehavior(IAIBehavior newBehavior)
    {
        behavior = newBehavior;
    }

    public void Update(Player target)
    {
        behavior?.Execute(this, target);
    }
}
```

---

## 💡 게임별 패턴 활용 예시

### RPG 게임에서의 패턴 활용
```csharp
// 팩토리 패턴 - 아이템 생성
public class ItemFactory
{
    public static Item CreateItem(ItemType type, int level)
    {
        return type switch
        {
            ItemType.Weapon => new Weapon(level),
            ItemType.Armor => new Armor(level),
            ItemType.Potion => new Potion(level),
            _ => throw new ArgumentException("Unknown item type")
        };
    }
}

// 커맨드 패턴 - 스킬 시스템
public interface ISkillCommand
{
    void Execute(Character caster, ITarget target);
    bool CanExecute(Character caster);
    void Undo(); // 일부 스킬은 되돌리기 가능
}

public class FireballCommand : ISkillCommand
{
    public void Execute(Character caster, ITarget target)
    {
        var damage = caster.Intelligence * 2;
        target.TakeDamage(damage);
        caster.ConsumeMana(50);
    }

    public bool CanExecute(Character caster)
    {
        return caster.Mana >= 50;
    }

    public void Undo() { /* 힐링 스킬의 경우 되돌리기 */ }
}
```

### 액션 게임에서의 패턴 활용
```csharp
// 상태 패턴 - 플레이어 상태 관리
public abstract class PlayerState
{
    public abstract void HandleInput(Player player, InputData input);
    public abstract void Update(Player player, float deltaTime);
    public abstract void OnEnter(Player player);
    public abstract void OnExit(Player player);
}

public class IdleState : PlayerState
{
    public override void HandleInput(Player player, InputData input)
    {
        if (input.IsMoving)
            player.ChangeState(new RunningState());
        else if (input.IsJumping)
            player.ChangeState(new JumpingState());
    }

    public override void Update(Player player, float deltaTime)
    {
        // 대기 애니메이션 재생
    }

    public override void OnEnter(Player player)
    {
        player.PlayAnimation("idle");
    }

    public override void OnExit(Player player) { }
}

public class Player
{
    private PlayerState currentState;

    public void ChangeState(PlayerState newState)
    {
        currentState?.OnExit(this);
        currentState = newState;
        currentState.OnEnter(this);
    }

    public void Update(float deltaTime)
    {
        currentState.Update(this, deltaTime);
    }
}
```

---

## 🎯 실무에서 자주 묻는 면접 질문

**Q: 싱글톤 패턴의 장단점은?**
A:
- **장점**: 전역 접근, 메모리 절약, 인스턴스 제어
- **단점**: 테스트 어려움, 결합도 증가, 멀티스레딩 이슈
- **게임에서**: GameManager, AudioManager 등에 사용하지만 DI로 대체 권장

**Q: 옵저버 패턴과 이벤트 시스템의 차이는?**
A: 옵저버 패턴은 직접적인 객체 참조를 통한 알림이고, 이벤트 시스템은 중간 매개체(EventBus)를 통한 느슨한 결합 방식입니다.

**Q: 어떤 패턴을 가장 많이 사용해봤나요?**
A: "전략 패턴을 AI 시스템에서, 옵저버 패턴을 UI 업데이트에서, 팩토리 패턴을 오브젝트 풀링과 함께 사용했습니다. 특히 전략 패턴은 런타임에 AI 행동을 바꿀 수 있어 유용했습니다."

---

## 🔗 관련 개념
- [SOLID 원칙]({{ site.baseurl }}/posts/whatis-solid/)
- [의존성 주입]({{ site.baseurl }}/posts/whatis-dependency-injection/)
- [객체지향 프로그래밍]({{ site.baseurl }}/posts/whatis-oop/)

---

*이 문서는 디자인 패턴의 기본 개념과 게임 개발에서의 활용을 다룹니다. 각 패턴에 대한 상세한 구현과 더 많은 예제는 추후 별도 포스트에서 다룰 예정입니다.*