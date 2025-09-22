---
layout: post
title: "단위 테스트(Unit Testing)란? - 안정적인 코드를 위한 필수 기법"
date: 2025-09-22 19:00:00 +0900
categories: [Tech Interview, Testing]
tags: [unit-testing, tdd, testing, mock, nunit, xunit, game-development, quality-assurance]
slug: whatis-unit-testing
mermaid: true
---

# 단위 테스트(Unit Testing)란?

## 📌 학습 목표
- 단위 테스트의 개념과 필요성 이해
- 테스트 작성 방법론(AAA, TDD) 습득
- Mock과 Stub을 활용한 격리된 테스트 작성
- 게임 개발에서의 테스트 적용 방법 학습

## 📌 정의
**단위 테스트(Unit Testing)**는 소프트웨어의 **가장 작은 단위(메서드, 함수, 클래스)**를 격리된 환경에서 테스트하는 방법입니다. 외부 의존성을 Mock으로 대체하여 **빠르고 반복 가능한 테스트**를 통해 코드의 정확성을 검증합니다.

## 📝 개념 정리
- **단위(Unit)**: 테스트 가능한 가장 작은 코드 조각 (메서드, 클래스)
- **격리(Isolation)**: 외부 의존성 없이 독립적으로 실행
- **자동화**: 수동 개입 없이 자동으로 실행 가능
- **반복 가능**: 언제든 같은 결과를 보장
- **빠른 피드백**: 몇 초 내에 테스트 결과 확인

---

## 🔑 단위 테스트의 핵심 특징

### 1. F.I.R.S.T 원칙
- **Fast**: 빠르게 실행 (초 단위)
- **Independent**: 독립적 실행 (테스트 간 의존성 없음)
- **Repeatable**: 반복 가능 (환경에 관계없이 동일한 결과)
- **Self-Validating**: 자체 검증 (Pass/Fail 명확)
- **Timely**: 적시에 작성 (코드 작성과 동시에)

### 2. AAA 패턴 (Arrange-Act-Assert)
```csharp
[Test]
public void CalculateDamage_WithCriticalHit_ShouldDoubleDamage()
{
    // Arrange - 테스트 준비
    var weapon = new Sword { BaseDamage = 100 };
    var player = new Player { CriticalChance = 1.0f }; // 100% 크리티컬

    // Act - 실행
    var damage = player.CalculateDamage(weapon);

    // Assert - 검증
    Assert.AreEqual(200, damage);
}
```

### 3. Given-When-Then 패턴 (BDD 스타일)
```csharp
[Test]
public void Player_WhenTakingDamage_ShouldReduceHealth()
{
    // Given - 주어진 조건
    var player = new Player { Health = 100 };

    // When - 특정 행동
    player.TakeDamage(30);

    // Then - 예상 결과
    Assert.AreEqual(70, player.Health);
}
```

---

## 🎮 게임 개발에서의 단위 테스트

### 1. 플레이어 시스템 테스트
```csharp
public class Player
{
    public int Health { get; private set; }
    public int MaxHealth { get; private set; }
    public bool IsDead => Health <= 0;

    public Player(int maxHealth = 100)
    {
        MaxHealth = maxHealth;
        Health = maxHealth;
    }

    public void TakeDamage(int damage)
    {
        if (damage < 0) throw new ArgumentException("Damage cannot be negative");

        Health = Math.Max(0, Health - damage);
    }

    public void Heal(int amount)
    {
        if (amount < 0) throw new ArgumentException("Heal amount cannot be negative");

        Health = Math.Min(MaxHealth, Health + amount);
    }
}

// 테스트 클래스
[TestFixture]
public class PlayerTests
{
    [Test]
    public void TakeDamage_WithValidDamage_ShouldReduceHealth()
    {
        // Arrange
        var player = new Player(100);

        // Act
        player.TakeDamage(30);

        // Assert
        Assert.AreEqual(70, player.Health);
    }

    [Test]
    public void TakeDamage_WithDamageExceedingHealth_ShouldSetHealthToZero()
    {
        // Arrange
        var player = new Player(100);

        // Act
        player.TakeDamage(150);

        // Assert
        Assert.AreEqual(0, player.Health);
        Assert.IsTrue(player.IsDead);
    }

    [Test]
    public void TakeDamage_WithNegativeDamage_ShouldThrowException()
    {
        // Arrange
        var player = new Player(100);

        // Act & Assert
        Assert.Throws<ArgumentException>(() => player.TakeDamage(-10));
    }

    [Test]
    public void Heal_WithValidAmount_ShouldIncreaseHealth()
    {
        // Arrange
        var player = new Player(100);
        player.TakeDamage(50); // Health = 50

        // Act
        player.Heal(30);

        // Assert
        Assert.AreEqual(80, player.Health);
    }

    [Test]
    public void Heal_ExceedingMaxHealth_ShouldCapAtMaxHealth()
    {
        // Arrange
        var player = new Player(100);
        player.TakeDamage(20); // Health = 80

        // Act
        player.Heal(50); // 130이 되려 하지만 max가 100

        // Assert
        Assert.AreEqual(100, player.Health);
    }
}
```

### 2. Mock을 활용한 의존성 테스트
```csharp
// 인터페이스
public interface IAudioManager
{
    void PlaySound(string soundName);
}

public interface IAnimationController
{
    void PlayAnimation(string animationName);
}

// 테스트 대상 클래스
public class WeaponSystem
{
    private readonly IAudioManager audioManager;
    private readonly IAnimationController animationController;

    public WeaponSystem(IAudioManager audioManager, IAnimationController animationController)
    {
        this.audioManager = audioManager;
        this.animationController = animationController;
    }

    public void FireWeapon(string weaponType)
    {
        // 애니메이션 재생
        animationController.PlayAnimation($"{weaponType}_fire");

        // 사운드 재생
        audioManager.PlaySound($"{weaponType}_shot");
    }
}

// Mock을 사용한 테스트
[TestFixture]
public class WeaponSystemTests
{
    private Mock<IAudioManager> mockAudioManager;
    private Mock<IAnimationController> mockAnimationController;
    private WeaponSystem weaponSystem;

    [SetUp]
    public void Setup()
    {
        mockAudioManager = new Mock<IAudioManager>();
        mockAnimationController = new Mock<IAnimationController>();
        weaponSystem = new WeaponSystem(mockAudioManager.Object, mockAnimationController.Object);
    }

    [Test]
    public void FireWeapon_WithRifle_ShouldPlayCorrectAnimationAndSound()
    {
        // Act
        weaponSystem.FireWeapon("rifle");

        // Assert
        mockAnimationController.Verify(x => x.PlayAnimation("rifle_fire"), Times.Once);
        mockAudioManager.Verify(x => x.PlaySound("rifle_shot"), Times.Once);
    }

    [Test]
    public void FireWeapon_WithPistol_ShouldPlayCorrectAnimationAndSound()
    {
        // Act
        weaponSystem.FireWeapon("pistol");

        // Assert
        mockAnimationController.Verify(x => x.PlayAnimation("pistol_fire"), Times.Once);
        mockAudioManager.Verify(x => x.PlaySound("pistol_shot"), Times.Once);
    }
}
```

### 3. 게임 로직 테스트 (인벤토리 시스템)
```csharp
public class Inventory
{
    private readonly Dictionary<Item, int> items = new Dictionary<Item, int>();
    public int MaxSlots { get; }

    public Inventory(int maxSlots = 20)
    {
        MaxSlots = maxSlots;
    }

    public bool AddItem(Item item, int quantity = 1)
    {
        if (items.Count >= MaxSlots && !items.ContainsKey(item))
            return false;

        if (items.ContainsKey(item))
            items[item] += quantity;
        else
            items[item] = quantity;

        return true;
    }

    public bool RemoveItem(Item item, int quantity = 1)
    {
        if (!items.ContainsKey(item) || items[item] < quantity)
            return false;

        items[item] -= quantity;
        if (items[item] == 0)
            items.Remove(item);

        return true;
    }

    public int GetItemCount(Item item)
    {
        return items.ContainsKey(item) ? items[item] : 0;
    }
}

[TestFixture]
public class InventoryTests
{
    private Item testItem;
    private Inventory inventory;

    [SetUp]
    public void Setup()
    {
        testItem = new Item("Test Potion");
        inventory = new Inventory(5); // 5슬롯
    }

    [Test]
    public void AddItem_WithEmptyInventory_ShouldAddSuccessfully()
    {
        // Act
        var result = inventory.AddItem(testItem, 3);

        // Assert
        Assert.IsTrue(result);
        Assert.AreEqual(3, inventory.GetItemCount(testItem));
    }

    [Test]
    public void AddItem_ExceedingMaxSlots_ShouldReturnFalse()
    {
        // Arrange - 5슬롯을 모두 채움
        for (int i = 0; i < 5; i++)
        {
            inventory.AddItem(new Item($"Item{i}"));
        }

        // Act - 새로운 아이템 추가 시도
        var result = inventory.AddItem(new Item("NewItem"));

        // Assert
        Assert.IsFalse(result);
    }

    [Test]
    public void RemoveItem_WithSufficientQuantity_ShouldRemoveSuccessfully()
    {
        // Arrange
        inventory.AddItem(testItem, 5);

        // Act
        var result = inventory.RemoveItem(testItem, 3);

        // Assert
        Assert.IsTrue(result);
        Assert.AreEqual(2, inventory.GetItemCount(testItem));
    }

    [Test]
    public void RemoveItem_WithInsufficientQuantity_ShouldReturnFalse()
    {
        // Arrange
        inventory.AddItem(testItem, 2);

        // Act
        var result = inventory.RemoveItem(testItem, 5);

        // Assert
        Assert.IsFalse(result);
        Assert.AreEqual(2, inventory.GetItemCount(testItem)); // 변화 없음
    }
}
```

---

## 🧪 테스트 종류와 전략

### 1. 테스트 피라미드
```
    /\
   /  \ E2E Tests (적음)
  /____\
 /      \ Integration Tests (중간)
/__________\
Unit Tests (많음)
```

### 2. 게임에서의 테스트 전략
```csharp
// 단위 테스트 - 개별 컴포넌트
[Test]
public void HealthComponent_TakeDamage_ShouldReduceHealth() { }

// 통합 테스트 - 시스템 간 상호작용
[Test]
public void CombatSystem_PlayerAttacksEnemy_ShouldReduceEnemyHealth() { }

// 엔드투엔드 테스트 - 전체 게임플레이 플로우
[Test]
public void GameFlow_StartToLevelComplete_ShouldWork() { }
```

### 3. 테스트 더블(Test Doubles)
```csharp
// Mock - 호출 검증
var mockLogger = new Mock<ILogger>();
mockLogger.Verify(x => x.Log("Game started"), Times.Once);

// Stub - 미리 정의된 응답
var stubDatabase = new Mock<IDatabase>();
stubDatabase.Setup(x => x.GetPlayerData(It.IsAny<int>()))
           .Returns(new PlayerData { Level = 5 });

// Fake - 단순한 구현체
public class FakeFileSystem : IFileSystem
{
    private Dictionary<string, string> files = new();

    public void WriteFile(string path, string content)
    {
        files[path] = content;
    }

    public string ReadFile(string path)
    {
        return files.ContainsKey(path) ? files[path] : null;
    }
}
```

---

## 🎯 실무에서 자주 묻는 면접 질문

**Q: 단위 테스트를 작성하는 이유는?**
A:
1. **빠른 피드백** - 수정 즉시 오류 확인
2. **리팩토링 안정성** - 기능 변경 시 회귀 방지
3. **문서화 역할** - 코드의 의도와 사용법 명시
4. **설계 개선** - 테스트하기 쉬운 코드는 좋은 설계

**Q: Mock과 Stub의 차이는?**
A:
- **Mock**: 호출되었는지 검증 (행위 검증)
- **Stub**: 미리 정의된 값 반환 (상태 검증)

**Q: 게임에서 테스트하기 어려운 부분은?**
A: "렌더링, 물리 시뮬레이션, 실시간 입력 등은 직접 테스트가 어렵습니다. 이런 부분은 로직과 분리하여 핵심 게임 로직만 단위 테스트하고, 시각적 부분은 통합 테스트나 수동 테스트로 보완합니다."

**Q: TDD(Test-Driven Development)를 사용해봤나요?**
A: "Red-Green-Refactor 사이클로 먼저 실패하는 테스트를 작성하고, 통과시키는 최소 코드를 구현한 후 리팩토링하는 방식입니다. 요구사항이 명확한 게임 로직 부분에서 활용했습니다."

---

## 💡 실무 적용 팁

### 1. 게임 개발에서의 테스트 범위
```csharp
// 테스트하기 좋은 부분
- 게임 로직 (체력, 데미지 계산, 아이템 시스템)
- 데이터 변환 (저장/로드, 직렬화)
- 알고리즘 (AI 행동, 경로 찾기)
- 비즈니스 규칙 (레벨업 조건, 보상 계산)

// 테스트하기 어려운 부분
- 렌더링, 애니메이션
- 사용자 입력 처리
- 네트워크 통신
- 파일 시스템 접근
```

### 2. 테스트 작성 가이드라인
```csharp
// 좋은 테스트명 - 무엇을_어떤조건에서_어떤결과
[Test]
public void Player_WhenHealthReachesZero_ShouldMarkAsDead() { }

// 하나의 테스트는 하나의 검증
[Test]
public void AddItem_ShouldIncreaseInventoryCount()
{
    // 한 가지만 검증
    Assert.AreEqual(expectedCount, actualCount);
}

// 테스트 데이터는 명확하게
var player = new Player(health: 100); // 매직 넘버 대신 의미 있는 값
```

---

## 🔗 관련 개념
- [의존성 주입]({{ site.baseurl }}/posts/whatis-dependency-injection/)

---

*이 문서는 단위 테스트의 기본 개념과 게임 개발에서의 활용법을 다룹니다. 더 고급 테스트 기법과 CI/CD 연동은 별도 포스트에서 다룰 예정입니다.*