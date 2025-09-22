---
layout: post
title: "클린 코드(Clean Code)란? - 읽기 쉽고 유지보수하기 좋은 코드 작성법"
date: 2025-09-22 20:00:00 +0900
categories: [Tech Interview, Code Quality]
tags: [clean-code, refactoring, readable-code, maintainability, best-practices, game-development]
slug: whatis-clean-code
mermaid: true
---

# 클린 코드(Clean Code)란?

## 📌 학습 목표
- 클린 코드의 개념과 중요성 이해
- 읽기 쉬운 코드 작성을 위한 원칙들 습득
- 게임 개발에서의 클린 코드 적용 방법 학습
- 리팩토링을 통한 코드 품질 개선 기법 파악

## 📌 정의
**클린 코드(Clean Code)**는 **읽기 쉽고, 이해하기 쉽고, 수정하기 쉬운 코드**입니다. 로버트 마틴(Uncle Bob)이 제시한 개념으로, 단순히 동작하는 코드를 넘어서 **다른 개발자가 쉽게 이해하고 유지보수할 수 있는 코드**를 의미합니다.

## 📝 개념 정리
- **가독성(Readability)**: 코드를 읽는 사람이 쉽게 이해할 수 있음
- **단순성(Simplicity)**: 복잡하지 않고 직관적임
- **의도 표현(Intent)**: 코드가 무엇을 하는지 명확히 드러남
- **중복 제거(DRY)**: Don't Repeat Yourself - 중복 코드 최소화
- **책임 분리**: 각 함수와 클래스가 명확한 책임을 가짐

---

## 🔑 클린 코드의 핵심 원칙

### 1. 의미 있는 이름 사용
```csharp
// 나쁜 예 - 의미를 알 수 없는 이름
int d; // 경과 시간?
List<int> lst; // 무엇의 리스트?
void calc(); // 무엇을 계산?

// 좋은 예 - 의도가 분명한 이름
int elapsedTimeInSeconds;
List<Player> activePlayers;
void CalculatePlayerScore();

// 게임 개발 예시
// 나쁜 예
public class GM
{
    public void u() { } // 업데이트?
    public void r() { } // 렌더링?
    int hp; // 체력?
    int mp; // 마나?
}

// 좋은 예
public class GameManager
{
    public void UpdateGameLogic() { }
    public void RenderFrame() { }
    int currentHealth;
    int currentMana;
}
```

### 2. 함수는 작고 단일 책임을 가져야 함
```csharp
// 나쁜 예 - 너무 많은 일을 하는 함수
public void ProcessPlayer()
{
    // 입력 처리
    if (Input.GetKey(KeyCode.W)) player.position += Vector3.forward;
    if (Input.GetKey(KeyCode.S)) player.position -= Vector3.forward;

    // 체력 처리
    if (player.health <= 0)
    {
        player.isDead = true;
        ShowGameOverScreen();
    }

    // 애니메이션 처리
    if (player.isMoving) animator.Play("run");
    else animator.Play("idle");

    // 사운드 처리
    if (player.isAttacking) audioSource.PlayOneShot(attackSound);
}

// 좋은 예 - 책임을 분리한 함수들
public void UpdatePlayer()
{
    HandlePlayerInput();
    UpdatePlayerHealth();
    UpdatePlayerAnimation();
    UpdatePlayerAudio();
}

private void HandlePlayerInput()
{
    var input = GetPlayerInput();
    player.Move(input);
}

private void UpdatePlayerHealth()
{
    if (player.IsDead())
    {
        HandlePlayerDeath();
    }
}

private void UpdatePlayerAnimation()
{
    var animationState = player.IsMoving ? "run" : "idle";
    animator.Play(animationState);
}

private void UpdatePlayerAudio()
{
    if (player.IsAttacking)
    {
        audioManager.PlayAttackSound();
    }
}
```

### 3. 주석보다는 코드로 설명
```csharp
// 나쁜 예 - 주석에 의존
public void Attack(Enemy target)
{
    // 크리티컬 확률 계산
    float critChance = (luck * 0.1f) + (level * 0.02f);
    bool isCritical = Random.Range(0f, 1f) < critChance;

    // 기본 데미지 계산
    int baseDamage = strength * weaponDamage;

    // 크리티컬이면 데미지 2배
    if (isCritical)
        baseDamage *= 2;

    target.TakeDamage(baseDamage);
}

// 좋은 예 - 코드 자체가 설명
public void Attack(Enemy target)
{
    bool isCriticalHit = RollForCriticalHit();
    int damage = CalculateBaseDamage();

    if (isCriticalHit)
        damage = ApplyCriticalMultiplier(damage);

    target.TakeDamage(damage);
}

private bool RollForCriticalHit()
{
    float criticalChance = CalculateCriticalChance();
    return Random.Range(0f, 1f) < criticalChance;
}

private float CalculateCriticalChance()
{
    return (luck * LUCK_CRIT_MODIFIER) + (level * LEVEL_CRIT_MODIFIER);
}

private int CalculateBaseDamage()
{
    return strength * currentWeapon.Damage;
}

private int ApplyCriticalMultiplier(int damage)
{
    return damage * CRITICAL_DAMAGE_MULTIPLIER;
}
```

### 4. 매직 넘버 제거
```csharp
// 나쁜 예 - 매직 넘버
public void RegenerateHealth()
{
    if (timeSinceLastRegen > 5.0f) // 5초는 무엇?
    {
        health += 10; // 10은 무엇?
        timeSinceLastRegen = 0;
    }
}

public bool CanLevelUp()
{
    return experience >= level * 100; // 100은 무엇?
}

// 좋은 예 - 상수로 의미 부여
private const float HEALTH_REGEN_INTERVAL = 5.0f;
private const int HEALTH_REGEN_AMOUNT = 10;
private const int EXPERIENCE_PER_LEVEL_MULTIPLIER = 100;

public void RegenerateHealth()
{
    if (timeSinceLastRegen > HEALTH_REGEN_INTERVAL)
    {
        health += HEALTH_REGEN_AMOUNT;
        timeSinceLastRegen = 0;
    }
}

public bool CanLevelUp()
{
    int requiredExperience = level * EXPERIENCE_PER_LEVEL_MULTIPLIER;
    return experience >= requiredExperience;
}
```

---

## 🎮 게임 개발에서의 클린 코드 적용

### 1. 게임 상태 관리
```csharp
// 나쁜 예 - 복잡한 상태 처리
public class GameController : MonoBehaviour
{
    public int gameState; // 0=메뉴, 1=플레이, 2=일시정지, 3=게임오버

    void Update()
    {
        if (gameState == 0)
        {
            // 메뉴 로직
            if (Input.GetKeyDown(KeyCode.Space))
                gameState = 1;
        }
        else if (gameState == 1)
        {
            // 게임 플레이 로직
            if (Input.GetKeyDown(KeyCode.Escape))
                gameState = 2;
            if (player.health <= 0)
                gameState = 3;
        }
        else if (gameState == 2)
        {
            // 일시정지 로직
            if (Input.GetKeyDown(KeyCode.Escape))
                gameState = 1;
        }
        // ... 복잡한 조건문 계속
    }
}

// 좋은 예 - 명확한 상태 관리
public enum GameState
{
    MainMenu,
    Playing,
    Paused,
    GameOver
}

public class GameStateManager : MonoBehaviour
{
    public GameState CurrentState { get; private set; }

    void Update()
    {
        switch (CurrentState)
        {
            case GameState.MainMenu:
                HandleMainMenuState();
                break;
            case GameState.Playing:
                HandlePlayingState();
                break;
            case GameState.Paused:
                HandlePausedState();
                break;
            case GameState.GameOver:
                HandleGameOverState();
                break;
        }
    }

    private void HandlePlayingState()
    {
        if (Input.GetKeyDown(KeyCode.Escape))
            ChangeState(GameState.Paused);

        if (player.IsDead())
            ChangeState(GameState.GameOver);
    }

    public void ChangeState(GameState newState)
    {
        ExitCurrentState();
        CurrentState = newState;
        EnterNewState();
    }
}
```

### 2. 플레이어 컨트롤러 리팩토링
```csharp
// 나쁜 예 - 모든 기능이 한 곳에
public class PlayerController : MonoBehaviour
{
    void Update()
    {
        // 이동 처리
        float h = Input.GetAxis("Horizontal");
        float v = Input.GetAxis("Vertical");
        transform.Translate(new Vector3(h, 0, v) * speed * Time.deltaTime);

        // 점프 처리
        if (Input.GetKeyDown(KeyCode.Space) && isGrounded)
        {
            rb.AddForce(Vector3.up * jumpForce);
            isGrounded = false;
        }

        // 공격 처리
        if (Input.GetMouseButtonDown(0))
        {
            Collider[] enemies = Physics.OverlapSphere(attackPoint.position, attackRange);
            foreach (Collider enemy in enemies)
            {
                if (enemy.CompareTag("Enemy"))
                {
                    enemy.GetComponent<Enemy>().TakeDamage(attackDamage);
                }
            }
        }

        // 애니메이션 처리
        bool isMoving = h != 0 || v != 0;
        animator.SetBool("IsMoving", isMoving);
    }
}

// 좋은 예 - 기능별 분리
public class PlayerController : MonoBehaviour
{
    private PlayerMovement movement;
    private PlayerCombat combat;
    private PlayerAnimation animation;

    void Awake()
    {
        movement = GetComponent<PlayerMovement>();
        combat = GetComponent<PlayerCombat>();
        animation = GetComponent<PlayerAnimation>();
    }

    void Update()
    {
        var input = GetPlayerInput();

        movement.HandleMovement(input);
        combat.HandleCombat(input);
        animation.UpdateAnimations(input);
    }

    private PlayerInput GetPlayerInput()
    {
        return new PlayerInput
        {
            Movement = new Vector2(Input.GetAxis("Horizontal"), Input.GetAxis("Vertical")),
            Jump = Input.GetKeyDown(KeyCode.Space),
            Attack = Input.GetMouseButtonDown(0)
        };
    }
}

public class PlayerMovement : MonoBehaviour
{
    public void HandleMovement(PlayerInput input)
    {
        if (input.Movement != Vector2.zero)
            Move(input.Movement);

        if (input.Jump && CanJump())
            Jump();
    }

    private void Move(Vector2 direction)
    {
        var movement = new Vector3(direction.x, 0, direction.y);
        transform.Translate(movement * moveSpeed * Time.deltaTime);
    }

    private bool CanJump()
    {
        return isGrounded;
    }

    private void Jump()
    {
        rigidbody.AddForce(Vector3.up * jumpForce);
        isGrounded = false;
    }
}
```

### 3. 아이템 시스템 클린 코드
```csharp
// 나쁜 예 - 타입별 하드코딩
public void UseItem(int itemId)
{
    if (itemId == 1) // 체력 포션
    {
        health += 50;
        if (health > maxHealth) health = maxHealth;
    }
    else if (itemId == 2) // 마나 포션
    {
        mana += 30;
        if (mana > maxMana) mana = maxMana;
    }
    else if (itemId == 3) // 공격력 버프
    {
        attackDamage *= 1.5f;
        // 10초 후 원복 타이머 설정...
    }
}

// 좋은 예 - 다형성 활용
public abstract class Item
{
    public string Name { get; protected set; }
    public abstract void Use(Player player);
}

public class HealthPotion : Item
{
    private readonly int healAmount;

    public HealthPotion(int healAmount)
    {
        Name = "Health Potion";
        this.healAmount = healAmount;
    }

    public override void Use(Player player)
    {
        player.Heal(healAmount);
    }
}

public class ManaPotion : Item
{
    private readonly int manaAmount;

    public ManaPotion(int manaAmount)
    {
        Name = "Mana Potion";
        this.manaAmount = manaAmount;
    }

    public override void Use(Player player)
    {
        player.RestoreMana(manaAmount);
    }
}

public class AttackBuff : Item
{
    private readonly float multiplier;
    private readonly float duration;

    public AttackBuff(float multiplier, float duration)
    {
        Name = "Attack Buff";
        this.multiplier = multiplier;
        this.duration = duration;
    }

    public override void Use(Player player)
    {
        player.ApplyAttackBuff(multiplier, duration);
    }
}

// 사용
public void UseItem(Item item)
{
    item.Use(this); // 각 아이템이 자신만의 방식으로 동작
}
```

---

## 💡 클린 코드 체크리스트

### 함수 작성 가이드라인
```csharp
// ✅ 좋은 함수의 특징
- 한 가지 일만 한다
- 작다 (20줄 이내 권장)
- 의미 있는 이름
- 매개변수는 3개 이하
- 부작용(Side Effect) 없음

// ❌ 피해야 할 것들
- 긴 함수 (스크롤 필요)
- 애매한 이름 (process, handle, manage 등)
- 많은 매개변수
- 함수 내에서 다른 객체 변경
```

### 클래스 설계 원칙
```csharp
// ✅ 좋은 클래스
public class Player
{
    // 명확한 책임
    // 적절한 크기
    // 응집도 높음
    // 결합도 낮음
}

// ❌ 나쁜 클래스
public class GameManager
{
    // 너무 많은 책임
    // 모든 게임 로직이 한 곳에
    // 수백 줄의 코드
}
```

---

## 🎯 실무에서 자주 묻는 면접 질문

**Q: 클린 코드란 무엇인가요?**
A: "읽기 쉽고 이해하기 쉬우며 유지보수하기 좋은 코드입니다. 의미 있는 이름, 작은 함수, 단일 책임 등의 원칙을 통해 다른 개발자가 쉽게 이해할 수 있는 코드를 작성하는 것을 의미합니다."

**Q: 클린 코드와 성능 사이의 트레이드오프는?**
A: "일반적으로 클린 코드가 성능에 큰 영향을 주지 않습니다. 오히려 명확한 구조로 인해 최적화 포인트를 찾기 쉬워집니다. 성능이 정말 중요한 부분에서만 가독성을 조금 희생할 수 있지만, 그 부분은 명확히 문서화해야 합니다."

**Q: 레거시 코드를 클린 코드로 바꾸는 방법은?**
A: "점진적 리팩토링을 통해 개선합니다. 먼저 테스트 코드를 작성하고, 작은 단위부터 함수명 개선, 중복 제거, 함수 분리 등을 진행합니다. 한 번에 모든 것을 바꾸려 하지 않고 안전한 단위로 나누어 진행합니다."

**Q: 게임 개발에서 클린 코드가 특히 중요한 이유는?**
A: "게임은 요구사항이 자주 바뀌고, 팀원들이 다양한 부분을 함께 작업하며, 런타임에 복잡한 상호작용이 일어납니다. 클린 코드를 통해 빠른 기능 추가, 버그 수정, 팀 협업이 원활해집니다."

---

## 🔧 리팩토링 실습 예제

### Before: 복잡한 게임 로직
```csharp
public class ComplexGameLogic : MonoBehaviour
{
    public void ProcessGame()
    {
        // 100줄이 넘는 복잡한 로직...
        if (player.health <= 0 && !gameOver)
        {
            gameOver = true;
            Time.timeScale = 0;
            gameOverUI.SetActive(true);
            if (score > highScore)
            {
                highScore = score;
                PlayerPrefs.SetInt("HighScore", highScore);
                newRecordUI.SetActive(true);
            }
            // 더 많은 로직...
        }
    }
}
```

### After: 클린하게 분리된 로직
```csharp
public class GameManager : MonoBehaviour
{
    private readonly ScoreManager scoreManager;
    private readonly UIManager uiManager;

    public void Update()
    {
        if (player.IsDead() && !IsGameOver)
        {
            EndGame();
        }
    }

    private void EndGame()
    {
        SetGameOverState();
        HandleScoreRecords();
        ShowGameOverUI();
    }

    private void SetGameOverState()
    {
        IsGameOver = true;
        Time.timeScale = 0;
    }

    private void HandleScoreRecords()
    {
        scoreManager.ProcessFinalScore(currentScore);
    }

    private void ShowGameOverUI()
    {
        uiManager.ShowGameOverScreen(scoreManager.IsNewRecord);
    }
}
```

---

## 🔗 관련 개념
- [SOLID 원칙]({{ site.baseurl }}/posts/whatis-solid/)

---
