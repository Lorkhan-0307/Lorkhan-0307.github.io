---
layout: post
title: "SOLID 원칙이란? - 객체지향 설계의 5가지 황금 법칙"
date: 2025-09-22 17:00:00 +0900
categories: [Tech Interview, Design Principles]
tags: [solid, oop, design-principles, clean-code, software-architecture, game-development]
slug: whatis-solid
mermaid: true
---

# SOLID 원칙이란?

## 📌 학습 목표
- SOLID 5가지 원칙의 개념과 필요성 이해
- 각 원칙을 위반했을 때의 문제점 파악
- 게임 개발에서의 실제 적용 사례 학습
- 리팩토링을 통한 SOLID 원칙 적용 방법 습득

## 📌 정의
**SOLID**는 객체지향 프로그래밍에서 **유지보수가 쉽고 확장 가능한 소프트웨어**를 만들기 위한 5가지 설계 원칙입니다. 로버트 마틴(Uncle Bob)이 제시한 이 원칙들은 **코드의 응집도를 높이고 결합도를 낮춰** 변경에 유연한 시스템을 만드는 것을 목표로 합니다.

## 📝 SOLID 개요
- **S**RP: Single Responsibility Principle (단일 책임 원칙)
- **O**CP: Open-Closed Principle (개방-폐쇄 원칙)
- **L**SP: Liskov Substitution Principle (리스코프 치환 원칙)
- **I**SP: Interface Segregation Principle (인터페이스 분리 원칙)
- **D**IP: Dependency Inversion Principle (의존성 역전 원칙)

---

## 1️⃣ SRP: 단일 책임 원칙

### 📌 정의
**하나의 클래스는 하나의 책임만을 가져야 한다.** 클래스가 변경되는 이유는 단 하나여야 합니다.

### ❌ 위반 사례

- 여러 책임을 가진 `Player`가 **이동/전투/저장/UI/계산**까지 모두 담당 → 변경 파급 큼


```csharp
// 나쁜 예 - 여러 책임을 가진 클래스
public class Player
{
    public string Name { get; set; }
    public int Level { get; set; }
    public int Health { get; set; }
    public Vector3 Position { get; set; }

    // 1. 플레이어 이동 책임
    public void Move(Vector3 direction)
    {
        Position += direction;
    }

    // 2. 전투 로직 책임
    public void Attack(Enemy target)
    {
        int damage = CalculateDamage();
        target.TakeDamage(damage);
    }

    // 3. 데이터 저장 책임
    public void SaveToDatabase()
    {
        var sql = $"UPDATE Players SET Level={Level}, Health={Health} WHERE Name='{Name}'";
        DatabaseConnection.Execute(sql);
    }

    // 4. UI 표시 책임
    public void DisplayStatus()
    {
        Console.WriteLine($"Player: {Name}, Level: {Level}, Health: {Health}");
    }

    // 5. 데미지 계산 책임
    private int CalculateDamage()
    {
        return Level * 10;
    }
}

// 문제점:
// 1. UI 변경 시 Player 클래스 수정
// 2. 데이터베이스 스키마 변경 시 Player 클래스 수정
// 3. 전투 시스템 변경 시 Player 클래스 수정
// 4. 이동 로직 변경 시 Player 클래스 수정
```

### ✅ 올바른 적용(책임 분리)
```csharp
// 좋은 예 - 책임을 분리한 클래스들
public class Player
{
    public string Name { get; set; }
    public int Level { get; set; }
    public int Health { get; set; }
    public Vector3 Position { get; set; }

    // 플레이어 기본 정보만 관리
    public PlayerData ToPlayerData()
    {
        return new PlayerData(Name, Level, Health, Position);
    }
}

// 이동 전용 클래스
public class PlayerMovement
{
    private readonly Player player;

    public PlayerMovement(Player player)
    {
        this.player = player;
    }

    public void Move(Vector3 direction, float speed, float deltaTime)
    {
        player.Position += direction * speed * deltaTime;
    }

    public void Teleport(Vector3 newPosition)
    {
        player.Position = newPosition;
    }
}

// 전투 전용 클래스
public class PlayerCombat
{
    private readonly Player player;
    private readonly IDamageCalculator damageCalculator;

    public PlayerCombat(Player player, IDamageCalculator damageCalculator)
    {
        this.player = player;
        this.damageCalculator = damageCalculator;
    }

    public void Attack(Enemy target)
    {
        int damage = damageCalculator.CalculateDamage(player.Level);
        target.TakeDamage(damage);
    }
}

// 데이터 저장 전용 클래스
public class PlayerRepository
{
    public void Save(Player player)
    {
        var data = player.ToPlayerData();
        DatabaseConnection.Save(data);
    }

    public Player Load(string playerName)
    {
        var data = DatabaseConnection.Load(playerName);
        return new Player
        {
            Name = data.Name,
            Level = data.Level,
            Health = data.Health,
            Position = data.Position
        };
    }
}

// UI 표시 전용 클래스
public class PlayerUI
{
    public void DisplayStatus(Player player)
    {
        Console.WriteLine($"Player: {player.Name}, Level: {player.Level}, Health: {player.Health}");
    }

    public void DisplayHealthBar(Player player)
    {
        // 체력바 UI 로직
    }
}
```

---

## 2️⃣ OCP: 개방-폐쇄 원칙

### 📌 정의
**확장에는 열려있고, 변경에는 닫혀있어야 한다.** 새로운 기능 추가 시 기존 코드를 수정하지 않고 확장만으로 가능해야 합니다.

### ❌ 위반 사례
열거형 + `switch` 분기 확대(무기 추가 때마다 기존 코드 수정)

```csharp
// 나쁜 예 - 새로운 무기 추가 시 기존 코드 수정 필요
public enum WeaponType
{
    Sword,
    Bow,
    Staff
    // 새 무기 추가 시 enum 수정 필요
}

public class WeaponSystem
{
    public int CalculateDamage(WeaponType weaponType, int playerLevel)
    {
        // 새로운 무기 추가 시 switch문 수정 필요
        switch (weaponType)
        {
            case WeaponType.Sword:
                return playerLevel * 15;
            case WeaponType.Bow:
                return playerLevel * 12;
            case WeaponType.Staff:
                return playerLevel * 20;
            default:
                throw new ArgumentException("Unknown weapon type");
        }
    }

    public void PlayAttackAnimation(WeaponType weaponType)
    {
        // 새로운 무기 추가 시 switch문 수정 필요
        switch (weaponType)
        {
            case WeaponType.Sword:
                PlaySwordAnimation();
                break;
            case WeaponType.Bow:
                PlayBowAnimation();
                break;
            case WeaponType.Staff:
                PlayStaffAnimation();
                break;
        }
    }
}
```

### ✅ 올바른 적용 (전략/다형성)
```csharp
// 좋은 예 - 확장 가능한 구조
public interface IWeapon
{
    string Name { get; }
    int CalculateDamage(int playerLevel);
    void PlayAttackAnimation();
    void PlayAttackSound();
}

// 기존 무기들
public class Sword : IWeapon
{
    public string Name => "Iron Sword";

    public int CalculateDamage(int playerLevel)
    {
        return playerLevel * 15;
    }

    public void PlayAttackAnimation()
    {
        AnimationManager.Play("sword_slash");
    }

    public void PlayAttackSound()
    {
        AudioManager.Play("sword_clang");
    }
}

public class Bow : IWeapon
{
    public string Name => "Elven Bow";

    public int CalculateDamage(int playerLevel)
    {
        return playerLevel * 12;
    }

    public void PlayAttackAnimation()
    {
        AnimationManager.Play("bow_shoot");
    }

    public void PlayAttackSound()
    {
        AudioManager.Play("arrow_release");
    }
}

// 새로운 무기 추가 - 기존 코드 수정 없이 확장만
public class MagicWand : IWeapon
{
    public string Name => "Fire Wand";

    public int CalculateDamage(int playerLevel)
    {
        return playerLevel * 25; // 마법 무기는 높은 데미지
    }

    public void PlayAttackAnimation()
    {
        AnimationManager.Play("wand_cast");
        EffectManager.Play("fire_ball");
    }

    public void PlayAttackSound()
    {
        AudioManager.Play("magic_cast");
    }
}

// 무기 시스템 - 변경 없이 모든 무기 지원
public class WeaponSystem
{
    public void Attack(IWeapon weapon, int playerLevel, Enemy target)
    {
        int damage = weapon.CalculateDamage(playerLevel);
        weapon.PlayAttackAnimation();
        weapon.PlayAttackSound();
        target.TakeDamage(damage);
    }
}
```

**핵심**: **인터페이스** 뒤로 변화 요소를 숨기고, **새 구현 추가**로 확장합니다.

---

## 3️⃣ LSP: 리스코프 치환 원칙

### 📌 정의
**상위 타입의 객체를 하위 타입의 객체로 치환해도 프로그램의 동작이 변하지 않아야 한다.** 서브클래스는 기반 클래스의 계약을 위반하지 않아야 합니다.

### ❌ 위반 사례
`Bird.Fly()` 계약을 `Penguin`이 지키지 못함 → 예외/버그

```csharp
// 나쁜 예 - LSP 위반
public abstract class Bird
{
    public abstract void Fly();
    public abstract void MakeSound();
}

public class Eagle : Bird
{
    public override void Fly()
    {
        Console.WriteLine("독수리가 하늘을 날아다닙니다.");
    }

    public override void MakeSound()
    {
        Console.WriteLine("끼익!");
    }
}

public class Penguin : Bird
{
    public override void Fly()
    {
        // 펭귄은 날 수 없음 - LSP 위반!
        throw new NotSupportedException("펭귄은 날 수 없습니다!");
    }

    public override void MakeSound()
    {
        Console.WriteLine("꽥꽥!");
    }
}

// 사용하는 코드
public class BirdManager
{
    public void MakeBirdsFly(List<Bird> birds)
    {
        foreach (Bird bird in birds)
        {
            bird.Fly(); // Penguin일 때 예외 발생!
        }
    }
}
```

### ✅ 올바른 적용 (계약 재정의/분해)
```csharp
// 좋은 예 - LSP 준수
public abstract class Bird
{
    public abstract void MakeSound();
    public abstract void Move(); // Fly 대신 일반적인 Move
}

// 날 수 있는 새들을 위한 인터페이스
public interface IFlyable
{
    void Fly();
    int FlightSpeed { get; }
}

// 수영할 수 있는 새들을 위한 인터페이스
public interface ISwimmable
{
    void Swim();
    int SwimSpeed { get; }
}

public class Eagle : Bird, IFlyable
{
    public int FlightSpeed => 80;

    public override void MakeSound()
    {
        Console.WriteLine("끼익!");
    }

    public override void Move()
    {
        Fly();
    }

    public void Fly()
    {
        Console.WriteLine("독수리가 하늘을 날아다닙니다.");
    }
}

public class Penguin : Bird, ISwimmable
{
    public int SwimSpeed => 15;

    public override void MakeSound()
    {
        Console.WriteLine("꽥꽥!");
    }

    public override void Move()
    {
        Console.WriteLine("펭귄이 뒤뚱뒤뚱 걸어갑니다.");
    }

    public void Swim()
    {
        Console.WriteLine("펭귄이 물 속에서 헤엄칩니다.");
    }
}

// 사용하는 코드
public class BirdManager
{
    public void MakeBirdsMove(List<Bird> birds)
    {
        foreach (Bird bird in birds)
        {
            bird.Move(); // 모든 새가 자신만의 방식으로 이동
        }
    }

    public void MakeFlyableBirdsFly(List<IFlyable> flyableBirds)
    {
        foreach (var bird in flyableBirds)
        {
            bird.Fly(); // 날 수 있는 새들만 날기
        }
    }
}
```

**핵심**: “상위 타입이 보장한 동작/제약을 하위가 **약화**시키지 말 것.”  
(전제조건 강화 금지, 사후조건 약화 금지, 불변식 깨짐 금지)

### 🎮 게임에서의 LSP 적용
```csharp
// 게임 유닛 시스템에서 LSP 적용
public abstract class GameUnit
{
    public int Health { get; protected set; }
    public Vector3 Position { get; set; }

    public abstract void TakeDamage(int damage);
    public abstract void Move(Vector3 direction);

    // 모든 유닛이 지켜야 할 계약
    protected virtual void OnDeath()
    {
        // 기본 사망 처리
        Health = 0;
    }
}

public class Player : GameUnit
{
    public override void TakeDamage(int damage)
    {
        Health -= damage;
        if (Health <= 0)
        {
            OnDeath();
        }
    }

    public override void Move(Vector3 direction)
    {
        Position += direction;
    }

    protected override void OnDeath()
    {
        base.OnDeath();
        // 플레이어만의 사망 처리
        ShowDeathUI();
        SaveGameProgress();
    }
}

public class Enemy : GameUnit
{
    public override void TakeDamage(int damage)
    {
        Health -= damage;
        if (Health <= 0)
        {
            OnDeath();
        }
    }

    public override void Move(Vector3 direction)
    {
        Position += direction;
    }

    protected override void OnDeath()
    {
        base.OnDeath();
        // 적만의 사망 처리
        DropLoot();
        GiveExperience();
    }
}

// 모든 GameUnit에 대해 동일하게 동작
public class CombatSystem
{
    public void ProcessAttack(GameUnit attacker, GameUnit target, int damage)
    {
        target.TakeDamage(damage); // Player든 Enemy든 동일하게 동작
    }
}
```

---

## 4️⃣ ISP: 인터페이스 분리 원칙

### 📌 정의
**클라이언트는 자신이 사용하지 않는 메서드에 의존하지 않아야 한다.** 큰 인터페이스를 작은 단위로 분리해야 합니다.

### ❌ 위반 사례

거대 `IGameUnit` → 단순 적도 불필요 기능 구현(예외/빈 구현 발생)

```csharp
// 나쁜 예 - 하나의 큰 인터페이스
public interface IGameUnit
{
    // 이동 관련
    void Move(Vector3 direction);
    void SetMoveSpeed(float speed);

    // 전투 관련
    void Attack(IGameUnit target);
    void TakeDamage(int damage);
    void Block();

    // 마법 관련
    void CastSpell(string spellName);
    void RegenerateMana();

    // 비행 관련
    void Fly(Vector3 direction);
    void Land();

    // 상호작용 관련
    void Interact(IGameUnit other);
    void Trade(IGameUnit trader);

    // 인벤토리 관련
    void PickupItem(Item item);
    void DropItem(Item item);
}

// 문제: 모든 유닛이 모든 기능을 구현해야 함
public class SimpleEnemy : IGameUnit
{
    public void Move(Vector3 direction) { /* 구현 */ }
    public void SetMoveSpeed(float speed) { /* 구현 */ }
    public void Attack(IGameUnit target) { /* 구현 */ }
    public void TakeDamage(int damage) { /* 구현 */ }

    // 사용하지 않는 기능들을 억지로 구현
    public void Block() { /* 빈 구현 또는 예외 */ }
    public void CastSpell(string spellName) { throw new NotSupportedException(); }
    public void RegenerateMana() { throw new NotSupportedException(); }
    public void Fly(Vector3 direction) { throw new NotSupportedException(); }
    public void Land() { throw new NotSupportedException(); }
    public void Interact(IGameUnit other) { throw new NotSupportedException(); }
    public void Trade(IGameUnit trader) { throw new NotSupportedException(); }
    public void PickupItem(Item item) { throw new NotSupportedException(); }
    public void DropItem(Item item) { throw new NotSupportedException(); }
}
```

### ✅ 올바른 적용 (용도별 인터페이스)
```csharp
// 좋은 예 - 기능별로 분리된 인터페이스
public interface IMovable
{
    void Move(Vector3 direction);
    void SetMoveSpeed(float speed);
    float MoveSpeed { get; }
}

public interface ICombatable
{
    void Attack(ICombatable target);
    void TakeDamage(int damage);
    int Health { get; }
}

public interface IDefendable
{
    void Block();
    bool IsBlocking { get; }
}

public interface IMagicUser
{
    void CastSpell(string spellName);
    void RegenerateMana();
    int Mana { get; }
}

public interface IFlyable
{
    void Fly(Vector3 direction);
    void Land();
    bool IsFlying { get; }
}

public interface IInteractable
{
    void Interact(IInteractable other);
}

public interface ITrader
{
    void Trade(ITrader trader);
    List<Item> TradeInventory { get; }
}

public interface IInventoryHolder
{
    void PickupItem(Item item);
    void DropItem(Item item);
    List<Item> Inventory { get; }
}

// 각 유닛은 필요한 인터페이스만 구현
public class SimpleEnemy : IMovable, ICombatable
{
    public float MoveSpeed { get; private set; } = 5.0f;
    public int Health { get; private set; } = 100;

    public void Move(Vector3 direction)
    {
        // 이동 구현
    }

    public void SetMoveSpeed(float speed)
    {
        MoveSpeed = speed;
    }

    public void Attack(ICombatable target)
    {
        target.TakeDamage(20);
    }

    public void TakeDamage(int damage)
    {
        Health -= damage;
    }
}

public class Player : IMovable, ICombatable, IDefendable, IInventoryHolder, IInteractable
{
    public float MoveSpeed { get; private set; } = 8.0f;
    public int Health { get; private set; } = 200;
    public bool IsBlocking { get; private set; }
    public List<Item> Inventory { get; private set; } = new List<Item>();

    // 모든 필요한 기능 구현
    public void Move(Vector3 direction) { /* 구현 */ }
    public void SetMoveSpeed(float speed) { MoveSpeed = speed; }
    public void Attack(ICombatable target) { /* 구현 */ }
    public void TakeDamage(int damage) { /* 구현 */ }
    public void Block() { IsBlocking = true; }
    public void PickupItem(Item item) { Inventory.Add(item); }
    public void DropItem(Item item) { Inventory.Remove(item); }
    public void Interact(IInteractable other) { /* 구현 */ }
}

public class FlyingMage : IMovable, ICombatable, IMagicUser, IFlyable
{
    // 마법사+비행 유닛의 필요한 기능만 구현
    public float MoveSpeed { get; private set; } = 6.0f;
    public int Health { get; private set; } = 150;
    public int Mana { get; private set; } = 100;
    public bool IsFlying { get; private set; }

    // 각 인터페이스의 메서드들 구현...
}
```

**핵심**: 인터페이스는 **작게**. “클라이언트 특정(사용자 맞춤)” 인터페이스를 만듭니다.

---

## 5️⃣ DIP: 의존성 역전 원칙

### 📌 정의
**상위 수준의 모듈은 하위 수준의 모듈에 의존해서는 안 된다. 둘 다 추상화에 의존해야 한다.** 구체적인 것이 추상적인 것에 의존해야 합니다.

### ❌ 위반 사례
상위 모듈이 파일/DB 구현에 직접 의존 (`new`)

```csharp
// 나쁜 예 - 상위 모듈이 하위 모듈에 직접 의존
public class GameSaveManager
{
    private FileSystemSaver fileSaver;      // 구체 클래스에 의존
    private DatabaseSaver dbSaver;          // 구체 클래스에 의존

    public GameSaveManager()
    {
        fileSaver = new FileSystemSaver();  // 직접 생성
        dbSaver = new DatabaseSaver();      // 직접 생성
    }

    public void SaveGame(GameData data, bool useDatabase)
    {
        if (useDatabase)
        {
            dbSaver.SaveToDatabase(data);
        }
        else
        {
            fileSaver.SaveToFile(data);
        }
    }
}

// 구체 클래스들
public class FileSystemSaver
{
    public void SaveToFile(GameData data)
    {
        File.WriteAllText("savegame.json", JsonSerializer.Serialize(data));
    }
}

public class DatabaseSaver
{
    public void SaveToDatabase(GameData data)
    {
        // 데이터베이스 저장 로직
    }
}

// 문제점:
// 1. 새로운 저장 방식 추가 시 GameSaveManager 수정 필요
// 2. 단위 테스트 어려움 (실제 파일/DB 필요)
// 3. 저장 방식 변경 시 상위 모듈까지 영향
```

### ✅ 올바른 적용 (추상화 + DI)
```csharp
// 좋은 예 - 추상화에 의존
public interface ISaveStrategy
{
    void Save(GameData data);
    GameData Load();
    bool IsAvailable();
}

// 상위 모듈 - 추상화에만 의존
public class GameSaveManager
{
    private readonly ISaveStrategy saveStrategy;

    // 의존성 주입으로 추상화 받음
    public GameSaveManager(ISaveStrategy saveStrategy)
    {
        this.saveStrategy = saveStrategy ?? throw new ArgumentNullException();
    }

    public void SaveGame(GameData data)
    {
        if (saveStrategy.IsAvailable())
        {
            saveStrategy.Save(data);
            Console.WriteLine("게임이 저장되었습니다.");
        }
        else
        {
            throw new InvalidOperationException("저장 서비스를 사용할 수 없습니다.");
        }
    }

    public GameData LoadGame()
    {
        return saveStrategy.Load();
    }
}

// 하위 모듈들 - 추상화를 구현
public class FileSystemSaveStrategy : ISaveStrategy
{
    private readonly string filePath;

    public FileSystemSaveStrategy(string filePath = "savegame.json")
    {
        this.filePath = filePath;
    }

    public void Save(GameData data)
    {
        File.WriteAllText(filePath, JsonSerializer.Serialize(data));
    }

    public GameData Load()
    {
        if (File.Exists(filePath))
        {
            var json = File.ReadAllText(filePath);
            return JsonSerializer.Deserialize<GameData>(json);
        }
        return new GameData();
    }

    public bool IsAvailable()
    {
        return Directory.Exists(Path.GetDirectoryName(filePath));
    }
}

public class CloudSaveStrategy : ISaveStrategy
{
    private readonly ICloudService cloudService;

    public CloudSaveStrategy(ICloudService cloudService)
    {
        this.cloudService = cloudService;
    }

    public void Save(GameData data)
    {
        var json = JsonSerializer.Serialize(data);
        cloudService.UploadFile("savegame.json", json);
    }

    public GameData Load()
    {
        var json = cloudService.DownloadFile("savegame.json");
        return JsonSerializer.Deserialize<GameData>(json);
    }

    public bool IsAvailable()
    {
        return cloudService.IsConnected();
    }
}

// 새로운 저장 방식 추가 - 기존 코드 수정 없음
public class DatabaseSaveStrategy : ISaveStrategy
{
    private readonly IDatabase database;

    public DatabaseSaveStrategy(IDatabase database)
    {
        this.database = database;
    }

    public void Save(GameData data)
    {
        database.Insert("GameSaves", data);
    }

    public GameData Load()
    {
        return database.Query<GameData>("SELECT * FROM GameSaves ORDER BY SaveTime DESC LIMIT 1").FirstOrDefault();
    }

    public bool IsAvailable()
    {
        return database.IsConnected();
    }
}

// 사용 예시
public class Game
{
    public void InitializeSaveSystem()
    {
        // 플랫폼에 따라 다른 저장 전략 선택
        ISaveStrategy saveStrategy;

        if (Platform.IsPC)
        {
            saveStrategy = new FileSystemSaveStrategy();
        }
        else if (Platform.HasCloudService)
        {
            saveStrategy = new CloudSaveStrategy(new CloudService());
        }
        else
        {
            saveStrategy = new DatabaseSaveStrategy(new LocalDatabase());
        }

        var saveManager = new GameSaveManager(saveStrategy);
    }
}
```

**핵심**: 코드가 **구체 클래스**가 아닌 **추상화**에 의존.  
구현체 선택은 **조립 루트/컨테이너**에서.

---

## 🎮 게임 개발에서의 SOLID 종합 적용

### 실제 게임 시스템 예제
```csharp
// 1. SRP: 각 클래스는 하나의 책임만
public class Player
{
    public string Name { get; set; }
    public int Level { get; set; }
    public Vector3 Position { get; set; }
    public int Health { get; set; }
}

public class PlayerMovement
{
    public void Move(Player player, Vector3 direction, float deltaTime) { }
}

public class PlayerCombat
{
    public void Attack(Player player, IEnemy target) { }
}

// 2. OCP: 새로운 스킬 추가 시 확장만
public interface ISkill
{
    string Name { get; }
    void Execute(Player caster, ITarget target);
    bool CanUse(Player caster);
}

public class FireballSkill : ISkill
{
    public string Name => "Fireball";
    public void Execute(Player caster, ITarget target) { /* 구현 */ }
    public bool CanUse(Player caster) => caster.Mana >= 50;
}

public class SkillSystem
{
    private readonly List<ISkill> skills = new List<ISkill>();

    public void RegisterSkill(ISkill skill)
    {
        skills.Add(skill); // 새 스킬 추가 시 기존 코드 수정 없음
    }

    public void UseSkill(string skillName, Player caster, ITarget target)
    {
        var skill = skills.FirstOrDefault(s => s.Name == skillName);
        if (skill?.CanUse(caster) == true)
        {
            skill.Execute(caster, target);
        }
    }
}

// 3. LSP: 모든 적은 동일하게 대체 가능
public abstract class Enemy
{
    public abstract void TakeDamage(int damage);
    public abstract void Move();
}

public class Goblin : Enemy
{
    public override void TakeDamage(int damage) { /* 고블린 특화 구현 */ }
    public override void Move() { /* 고블린 이동 */ }
}

public class Dragon : Enemy
{
    public override void TakeDamage(int damage) { /* 드래곤 특화 구현 */ }
    public override void Move() { /* 드래곤 이동 */ }
}

// 4. ISP: 기능별 인터페이스 분리
public interface IDamageable
{
    void TakeDamage(int damage);
}

public interface IMovable
{
    void Move(Vector3 direction);
}

public interface IInteractable
{
    void OnInteract(Player player);
}

// 5. DIP: 추상화에 의존
public class GameWorld
{
    private readonly IRenderer renderer;
    private readonly IAudioManager audioManager;
    private readonly IInputManager inputManager;

    public GameWorld(IRenderer renderer, IAudioManager audioManager, IInputManager inputManager)
    {
        this.renderer = renderer;
        this.audioManager = audioManager;
        this.inputManager = inputManager;
    }

    public void Update()
    {
        var input = inputManager.GetInput();
        // 게임 로직 처리
        renderer.Render();
    }
}
```

---

## 🧩 요약 표

| 원칙 | 슬로건 | 위반 시 증상 | 대표 해결책 |
|---|---|---|---|
| **SRP** | 책임은 하나 | 변경 파급, 테스트 곤란 | 책임 분리, 모듈화 |
| **OCP** | 확장에 열림 | switch/if 지옥, 개방·폐쇄 깨짐 | 전략 패턴, 다형성, 플러그인 |
| **LSP** | 치환 가능 | 예외/버그, 계약 위반 | 계약 명세화, 타입 계층 재설계 |
| **ISP** | 작은 인터페이스 | 빈 구현, 예외 남발 | 클라이언트별 인터페이스 분리 |
| **DIP** | 추상화 의존 | 교체 불가, 테스트 어려움 | 인터페이스화 + DI 컨테이너 |

---

## 💡 실무 적용 팁

### 1. SOLID 원칙 적용 우선순위
```csharp
// 1단계: SRP부터 시작 - 클래스 책임 분리
// 2단계: DIP 적용 - 의존성 주입으로 결합도 낮추기
// 3단계: ISP 적용 - 인터페이스 세분화
// 4단계: OCP 적용 - 확장 가능한 구조 만들기
// 5단계: LSP 검증 - 상속 구조 점검

// 실제 리팩토링 과정
public class GameManager // 초기 버전 - SOLID 위반
{
    public void StartGame()
    {
        // 파일 저장, 네트워크 통신, UI 업데이트, 게임 로직 등 모든 것
    }
}

// 1단계: SRP 적용
public class GameManager
{
    private readonly SaveManager saveManager;
    private readonly NetworkManager networkManager;
    private readonly UIManager uiManager;

    // 게임 상태 관리만 담당
    public void StartGame() { }
}

// 2단계: DIP 적용
public class GameManager
{
    private readonly ISaveManager saveManager;      // 인터페이스에 의존
    private readonly INetworkManager networkManager;
    private readonly IUIManager uiManager;

    public GameManager(ISaveManager saveManager, INetworkManager networkManager, IUIManager uiManager)
    {
        // 의존성 주입
    }
}
```

### 2. 게임 개발에서 주의할 점
```csharp
// 성능 vs SOLID 균형 맞추기
public class HighFrequencySystem
{
    // 매 프레임 호출되는 시스템에서는 과도한 추상화 피하기
    public void Update(float deltaTime)
    {
        // 직접 호출이 더 효율적일 수 있음
        // 하지만 테스트 가능성과 유지보수성도 고려
    }
}

// 게임 특화 패턴과 SOLID 조화
public class ComponentSystem // Entity-Component-System과 SOLID
{
    private readonly IComponentManager componentManager;

    public void ProcessEntities()
    {
        // SOLID 원칙을 지키면서도 ECS 패턴 활용
    }
}
```

---

## 🎯 면접 질문 & 답변

**Q: SOLID 원칙을 간단히 설명하세요.**
A:
- **SRP**: 클래스는 하나의 책임만 (변경 이유가 하나)
- **OCP**: 확장에 열리고 수정에 닫힘 (새 기능 추가 시 기존 코드 수정 X)
- **LSP**: 하위 타입으로 상위 타입 대체 가능
- **ISP**: 사용하지 않는 메서드에 의존 X (인터페이스 분리)
- **DIP**: 구체적인 것이 추상적인 것에 의존

**Q: 게임에서 SOLID를 어떻게 적용했나요?**
A: "플레이어 시스템을 예로 들면, 이동/전투/인벤토리를 별도 클래스로 분리(SRP), 새로운 스킬 추가 시 인터페이스 구현만으로 확장(OCP), 모든 적 유형이 동일한 전투 시스템에서 처리 가능(LSP), 필요한 기능만 구현하도록 인터페이스 분리(ISP), 플랫폼별 저장 시스템을 추상화로 교체 가능(DIP)하게 설계했습니다."

**Q: SOLID 원칙을 모두 지키려고 하면 복잡해지지 않나요?**
A: "성능이 중요한 게임에서는 균형이 필요합니다. 매 프레임 호출되는 시스템은 직접 호출을 유지하고, 비즈니스 로직이나 시스템 간 통신 부분에서 SOLID를 적용해 유지보수성을 높였습니다. 특히 테스트 가능성과 플랫폼 이식성에서 큰 도움이 되었습니다."

**Q: DIP와 IoC 컨테이너의 차이는?**
A: "DIP는 설계 원칙이고, IoC 컨테이너는 이를 구현하는 도구입니다. DIP는 '추상화에 의존하라'는 원칙이고, IoC 컨테이너는 의존성을 자동으로 주입해주는 프레임워크입니다."

**Q: LSP 위반의 실제 사례는?**
A: "Rectangle/Square 문제가 대표적입니다. Square가 Rectangle을 상속받을 때, SetWidth()만 호출해도 Height가 변경되어 Rectangle의 기대 동작과 다릅니다. 게임에서는 FlyingUnit이 GroundUnit을 상속받으면서 Move() 메서드의 동작이 완전히 달라지는 경우입니다."

---

## 🔗 관련 개념
- [의존성 주입]({{ site.baseurl }}/posts/whatis-dependency-injection/)
- [인터페이스]({{ site.baseurl }}/posts/whatis-interface/)
- [디자인 패턴]({{ site.baseurl }}/posts/whatis-design-patterns/)
- [클린 코드]({{ site.baseurl }}/posts/whatis-clean-code/)

---

## 🗺️ 한눈 개관 (Mermaid)

```mermaid
graph LR
  S[SRP\n단일 책임] --> O[OCP\n확장/폐쇄]
  O --> L[LSP\n치환 가능]
  L --> I[ISP\n인터페이스 분리]
  I --> D[DIP\n추상화 의존]
  subgraph Goals
    G1[응집도 ↑]
    G2[결합도 ↓]
    G3[변경 내성 ↑]
    G4[테스트 용이성 ↑]
  end
  S -.-> G1
  O -.-> G3
  L -.-> G3
  I -.-> G2
  D -.-> G2
```

---