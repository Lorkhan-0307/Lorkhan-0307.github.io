---
layout: post
title: "기술면접 대비 CS 공부 - Unreal"
date: 2025-10-06 10:00:00 +0900
categories: [Tech Interview, Study Plan]
tags: [c++, c-sharp, computer-science, algorithm, data-structure, operating-system, network, database, design-pattern, unity, unreal]
slug: csstudyforfinalpartseven
---

# 면접 대비 사전 QnA 정리 - Unreal Engine

<details markdown="1">
<summary><strong>1. Unreal Engine의 핵심 클래스 계층을 설명해주세요.</strong></summary>

**🧠 핵심 요약**

- 언리얼 엔진의 클래스 구조는 `UObject → AActor → APawn → ACharacter`로 확장된다.  
- `UObject`는 모든 언리얼 클래스의 루트이며, **GC(가비지 컬렉션)** 관리의 기본 단위이다.  
- `AActor`는 월드에 배치 가능한 객체, `APawn`은 제어 가능한 존재, `ACharacter`는 이동 로직과 콜라이더를 갖춘 특수한 Pawn이다.

---

**🔹 특징 및 상세설명**

- **UObject**: 모든 클래스의 기반이며, 월드 좌표나 렌더링 정보가 없는 순수 데이터 단위이다.  
- **AActor**: `UObject`를 상속하며, 월드에 존재할 수 있는 트랜스폼(위치/회전/스케일)을 가진다.  
- **APawn**: `AActor`의 하위 개념으로, `PlayerController`나 `AIController`에 의해 **조작 가능한 액터**다.  
- **ACharacter**: `APawn`을 확장한 클래스로, `CharacterMovementComponent`와 `CapsuleCollider`가 내장되어 있다.  
- 즉, 이 계층 구조는 “데이터(UObject) → 월드 객체(Actor) → 제어 가능 객체(Pawn) → 캐릭터 전용 로직(Character)”로 이어진다.

---

**💬 면접식 답변**

> 언리얼의 클래스 계층은 `UObject → AActor → APawn → ACharacter` 순으로 확장됩니다.  
> `UObject`는 메모리 관리 단위, `AActor`는 월드에 존재하는 엔티티,  
> `APawn`은 컨트롤러가 소유하는 조작 가능한 객체,  
> `ACharacter`는 이동 로직과 충돌 처리가 포함된 특수한 Pawn입니다.

</details>

---

<details markdown="1">
<summary><strong>2. UObject와 AActor의 차이는 무엇인가요?</strong></summary>

**🧠 핵심 요약**

- `UObject`는 게임 세계와 독립적인 데이터 객체이고, `AActor`는 실제 월드에 존재하는 엔티티이다.  
- `AActor`는 위치/회전/스케일 정보를 가진 반면, `UObject`는 그렇지 않다.  
- `UObject`는 경량 객체로 메모리 관리(GC)에 초점을 둔다.

---

**🔹 특징 및 상세설명**

- **UObject**: 엔진 내 모든 클래스의 루트로, **위치 정보나 물리 컴포넌트가 없다.** 데이터 자산, 매니저, 설정 객체 등에 쓰인다.  
- **AActor**: `UObject`를 기반으로 **월드에 스폰될 수 있는 물리적 객체**이며, 컴포넌트와 트랜스폼을 가진다.  
- `AActor`는 BeginPlay, Tick 등 **게임 루프에 참여**하지만, `UObject`는 그렇지 않다.  
- 따라서 `UObject`는 **비공간적 로직**, `AActor`는 **공간적 존재**로 구분된다.

---

**💬 면접식 답변**

> `UObject`는 엔진의 모든 객체의 기본 단위로, 월드에 존재하지 않는 데이터 중심 객체입니다.  
> 반면 `AActor`는 월드에 실제로 배치되며 트랜스폼과 컴포넌트를 가지고 동작하는 물리적 객체입니다.  
> 즉, `UObject`는 논리적, `AActor`는 공간적 존재라고 볼 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>3. Tick() 함수는 언제 호출되며, 어디에 사용하는 게 적절한가요?</strong></summary>

**🧠 핵심 요약**

- `Tick()`은 **매 프레임마다** 호출되며, 실시간 갱신 로직을 처리한다.  
- 과도한 `Tick` 사용은 성능 저하를 초래하므로, **이벤트 기반 업데이트**로 대체하는 것이 권장된다.

---

**🔹 특징 및 상세설명**

- 언리얼은 엔진 루프에서 `World::Tick()`을 매 프레임 호출하며, 그 안에서 각 `Actor`, `Component`의 `Tick()`을 실행한다.  
- `Tick()`은 주로 **위치 갱신, 애니메이션, 입력 반응** 등에 사용된다.  
- 필요 시 `PrimaryActorTick.bCanEverTick = false;` 로 비활성화 가능.  
- 대체 방법: 타이머(`FTimerManager`), 델리게이트, 이벤트, 또는 물리 시뮬레이션 기반 로직.  

---

**💬 면접식 답변**

> `Tick()`은 매 프레임 호출되어 캐릭터 이동, 카메라 추적 등 실시간 갱신이 필요한 로직에 사용됩니다.  
> 하지만 매 프레임 실행은 비효율적이므로, 이벤트 기반 타이머나 델리게이트로 대체해 최적화하는 것이 좋습니다.

</details>

---

<details markdown="1">
<summary><strong>4. Blueprint와 C++의 차이점과 상호 보완 방식은?</strong></summary>

**🧠 핵심 요약**

- Blueprint는 **시각적 스크립팅**, C++은 **저수준 로직과 성능 중심**이다.  
- 일반적으로 C++에서 시스템을 정의하고, Blueprint로 파생하여 게임 디자이너가 조작한다.

---

**🔹 특징 및 상세설명**

- **Blueprint**: 비프로그래머도 사용할 수 있는 노드 기반 비주얼 스크립팅 시스템.  
  - 장점: 빠른 프로토타이핑, 디자이너 친화적.  
  - 단점: 느린 실행 속도, 대규모 프로젝트에서 유지보수 어려움.  
- **C++**: 저수준 엔진 접근과 최적화를 위한 언어.  
  - 장점: 성능 우수, 정적 타입 검사, 대규모 시스템 구현 적합.  
  - 단점: 빌드 시간 길고, 초기 러닝커브 높음.  
- **혼합 전략**:  
  - 코어 시스템은 C++에서 작성.  
  - 게임플레이/밸런싱 부분은 Blueprint로 확장.  
  - 즉, “C++ → Blueprint 파생” 구조.

---

**💬 면접식 답변**

> 언리얼에서 C++은 저수준 로직과 최적화에, Blueprint는 고수준 제어와 빠른 콘텐츠 제작에 사용됩니다.  
> 일반적으로 핵심 시스템은 C++로 작성하고, 디자이너가 수정 가능한 영역은 Blueprint로 파생시켜 협업 효율을 높입니다.

</details>

---

<details markdown="1">
<summary><strong>5. GameMode, GameState, PlayerState의 차이를 설명해주세요.</strong></summary>

**🧠 핵심 요약**

- **GameMode** : 서버 전용 게임 규칙 및 흐름 관리  
- **GameState** : 클라이언트와 서버가 공유하는 게임 상태  
- **PlayerState** : 각 플레이어별 점수, 이름, 연결 상태 등 개별 정보 저장  

---

**🔹 특징 및 상세설명**

- **GameMode**
  - 서버 전용 클래스이며, 게임의 **규칙, 라운드 진행, 스폰 로직**을 관리한다.  
  - 클라이언트에는 존재하지 않으며, `GetWorld()->GetAuthGameMode()`로 접근 가능.
  - 예: 팀 구분, 점수 계산, 라운드 종료 조건 등.

- **GameState**
  - GameMode와 달리 **서버-클라이언트 양쪽에 존재**.  
  - 현재 점수판, 남은 시간, 라운드 상태 등 **공유 가능한 정보**를 보관한다.  
  - `Replicated` 속성을 통해 자동 동기화된다.

- **PlayerState**
  - 각 플레이어 고유의 정보(닉네임, 점수, 팀, Ping 등)를 저장.  
  - 클라이언트/서버 모두 존재하며, 멀티플레이 환경에서 각 플레이어를 식별할 때 사용된다.  

---

**💬 면접식 답변**

> GameMode는 서버에서 게임의 규칙과 흐름을 관리하고,  
> GameState는 이를 클라이언트에게 공유하는 상태 객체입니다.  
> PlayerState는 개별 플레이어의 정보를 관리하여 멀티플레이 환경에서 동기화를 담당합니다.  
> 즉, GameMode는 ‘게임 전체 로직’, GameState는 ‘공유 상태’, PlayerState는 ‘플레이어별 데이터’라고 볼 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>6. Component 시스템의 개념과 활용 사례를 설명해주세요.</strong></summary>

**🧠 핵심 요약**

- Component는 **재사용 가능한 기능 단위**로, Actor에 부착되어 기능을 확장한다.  
- `USceneComponent`는 공간 정보를, `UActorComponent`는 비공간 로직을 담당한다.

---

**🔹 특징 및 상세설명**

- **UActorComponent**
  - 모든 컴포넌트의 기반 클래스.  
  - Tick, BeginPlay 등의 기본 수명주기를 가진다.  
  - 예: HealthComponent, InventoryComponent 등 **논리적 시스템** 구현에 사용.

- **USceneComponent**
  - 위치, 회전, 스케일을 포함하는 공간 기반 컴포넌트.  
  - 예: Mesh, Light, Camera 등 **렌더링 및 물리적 객체** 구현에 사용.

- **활용 예시**
  - PlayerCharacter → `CameraComponent`, `SpringArmComponent`, `MovementComponent` 등으로 구성.
  - 재사용성과 모듈화를 극대화하여 유지보수를 쉽게 한다.

---

**💬 면접식 답변**

> 언리얼의 컴포넌트는 액터에 부착되어 동작을 확장하는 구조로,  
> `UActorComponent`는 비공간적 로직, `USceneComponent`는 공간 정보를 담당합니다.  
> 예를 들어, 캐릭터에 HealthComponent나 CameraComponent를 추가하여 기능을 모듈화할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>7. Level과 World의 차이를 설명해주세요.</strong></summary>

**🧠 핵심 요약**

- **Level**은 하나의 맵 파일(.umap), **World**는 여러 Level을 포함하는 실행 단위이다.  
- World는 게임 실행 중의 전체 공간, Level은 그 안의 **논리적 단위**이다.

---

**🔹 특징 및 상세설명**

- **Level**
  - 맵 하나를 의미하며, 액터와 라이트, 지형 등을 포함한다.  
  - Persistent Level, Sub Level로 나뉘며, 여러 개가 동시에 로드될 수 있다.
  - Level Streaming을 통해 비동기 로드/언로드 가능.

- **World**
  - 현재 실행 중인 모든 Level의 컨테이너.  
  - 하나의 게임 실행은 반드시 하나의 World를 가진다.
  - 서버/클라이언트 각각 World를 갖고 있으며, 그 안에서 네트워크 동기화가 이루어진다.

---

**💬 면접식 답변**

> Level은 하나의 맵 단위이며, World는 그 Level들을 포함하는 실행 단위입니다.  
> 예를 들어, Persistent Level에 Sub Level을 여러 개 로드하면 World가 이를 통합 관리합니다.  
> 즉, Level은 콘텐츠 단위, World는 실행 단위입니다.

</details>

---

<details markdown="1">
<summary><strong>8. PlayerController, AIController, 그리고 Possess()의 역할은 무엇인가요?</strong></summary>

**🧠 핵심 요약**

- **PlayerController**는 유저 입력을 처리하고,  
  **AIController**는 비플레이어 캐릭터의 행동을 제어한다.  
- `Possess()` 함수는 컨트롤러가 Pawn을 소유하여 조작할 수 있도록 한다.

---

**🔹 특징 및 상세설명**

- **PlayerController**
  - 키보드/마우스/패드 입력을 받아 Pawn에 전달.  
  - HUD나 카메라 제어도 담당.  
  - 멀티플레이 환경에서 클라이언트마다 하나씩 존재.

- **AIController**
  - Behavior Tree와 연계되어 비플레이어 캐릭터 제어.  
  - 네비게이션 시스템을 이용한 경로 탐색 수행.

- **Possess()**
  - `Controller → Pawn` 소유권 연결.  
  - 플레이어 전환, 리스폰, AI 스폰 등에 사용된다.  
  - 반대로 `UnPossess()`는 소유를 해제한다.

---

**💬 면접식 답변**

> PlayerController는 입력을 받아 Pawn을 제어하고,  
> AIController는 Behavior Tree를 통해 AI 캐릭터를 제어합니다.  
> `Possess()`는 컨트롤러가 Pawn의 소유권을 가져와 조작할 수 있도록 하는 핵심 함수입니다.

</details>

---

<details markdown="1">
<summary><strong>9. Subsystem이란 무엇이며, 어떤 종류가 있나요?</strong></summary>

**🧠 핵심 요약**

- Subsystem은 **전역적인 서비스 로직**을 관리하는 클래스 구조이다.  
- 범위에 따라 **EngineSubsystem, GameInstanceSubsystem, WorldSubsystem**으로 나뉜다.

---

**🔹 특징 및 상세설명**

- **EngineSubsystem**
  - 엔진 전체에서 공유되는 단일 객체.  
  - 예: 렌더링 매니저, 전역 로그 시스템 등.

- **GameInstanceSubsystem**
  - 게임 세션 동안 유지.  
  - 예: 세이브 시스템, 네트워크 매니저, UIController 등.

- **WorldSubsystem**
  - 월드별로 존재하며, Level Streaming이나 AI 네비게이션 관리에 사용.  

- 장점: Singleton 패턴을 대체하며, 자동 수명 관리 및 테스트 용이성을 제공.

---

**💬 면접식 답변**

> Subsystem은 전역적인 기능을 모듈화해 관리하는 시스템입니다.  
> 예를 들어, GameInstanceSubsystem은 게임 전반의 데이터를 관리하고,  
> WorldSubsystem은 각 월드별 로직을 담당합니다.  
> 이는 전통적인 싱글톤을 대체하는 안전한 구조입니다.

</details>

---

<details markdown="1">
<summary><strong>10. Unreal의 Garbage Collection(GC) 시스템은 어떻게 동작하나요?</strong></summary>

**🧠 핵심 요약**

- Unreal은 **UObject 기반의 GC 시스템**을 사용한다.  
- 참조되지 않는 객체를 주기적으로 스캔하여 해제하며,  
  C++의 `delete` 대신 안전하게 메모리를 관리한다.

---

**🔹 특징 및 상세설명**

- **Mark and Sweep 방식**
  - 루트 객체에서 시작해 참조 그래프를 탐색(Mark).  
  - 참조되지 않은 객체(Sweep)는 파괴된다.  
- **UPROPERTY()**
  - GC가 추적할 수 있도록 참조를 등록하는 매크로.  
  - 등록되지 않으면 GC가 객체를 해제할 수 있다.
- **특징**
  - UE의 GC는 Tick 루프 내에서 주기적으로 작동.  
  - UObject 외의 비관리 메모리는 여전히 수동 관리 필요.

---

**💬 면접식 답변**

> Unreal의 GC는 UObject 기반 참조 그래프를 이용한 Mark and Sweep 방식으로 동작합니다.  
> `UPROPERTY()`를 통해 참조 관계를 등록하면 GC가 안전하게 추적하며,  
> 개발자는 직접 delete를 호출하지 않아도 메모리 누수를 방지할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>11. UE5의 Garbage Collection(GC)과 Smart Pointer의 차이를 설명해주세요.</strong></summary>

**🧠 핵심 요약**  
- **GC**는 `UObject` 기반의 자동 메모리 관리 시스템.  
- **Smart Pointer**는 `UObject`가 아닌 일반 C++ 객체의 수명 관리를 담당한다.

---

**🔹 특징 및 상세설명**  

- **GC (Garbage Collection)**  
  - Unreal은 `UObject`를 루트로 하는 참조 그래프를 스캔해,  
    더 이상 참조되지 않는 객체를 제거한다.  
  - `UPROPERTY()`로 참조를 등록해야 GC가 추적 가능.  
  - 수동 삭제는 불필요하지만, 관리 외 객체(raw pointer)는 추적 불가.

- **Smart Pointer**  
  - `TSharedPtr`, `TWeakPtr`, `TUniquePtr` 등 C++ 스타일 스마트 포인터를 제공.  
  - UObject 외의 일반 클래스나 데이터 구조를 안전하게 관리.  
  - 참조 카운트를 기반으로, 범위를 벗어나면 자동 해제된다.

- **차이점 요약**  

  | 구분 | Garbage Collection | Smart Pointer |
  |------|------------------|----------------|
  | 적용 대상 | UObject | 일반 C++ 객체 |
  | 관리 방식 | Mark & Sweep | 참조 카운트 |
  | 수명 제어 | 엔진 루프 기반 | 범위 기반 |
  | 코드 스타일 | 매크로 중심 | 템플릿 중심 |

---

**💬 면접식 답변**

> UE5의 GC는 `UObject` 기반 객체를 자동 관리하지만,  
> 일반 C++ 객체는 관리하지 않습니다.  
> 따라서 비-UObject 타입의 데이터는 `TSharedPtr`이나 `TWeakPtr`을 사용해 수명을 제어합니다.  
> 즉, GC는 엔진 레벨에서, Smart Pointer는 코드 레벨에서의 메모리 안전을 보장합니다.

</details>

---

<details markdown="1">
<summary><strong>12. UPROPERTY()의 역할과 주요 지정자들을 설명해주세요.</strong></summary>

**🧠 핵심 요약**  
- `UPROPERTY()`는 GC, 리플렉션, 시리얼라이제이션 시스템에 객체를 등록하기 위한 매크로다.  
- 에디터 노출, 블루프린트 접근, 네트워크 복제 등 다양한 지정자를 제공한다.

---

**🔹 특징 및 상세설명**

- **주요 역할**  
  - GC 추적 대상 등록  
  - Blueprint 및 에디터 노출  
  - 네트워크 동기화 지원 (Replication)

- **대표 지정자**
  - `VisibleAnywhere` : 읽기 전용으로 에디터에 노출  
  - `EditAnywhere` : 수정 가능  
  - `BlueprintReadWrite` : 블루프린트 접근 허용  
  - `Replicated` : 네트워크 동기화 대상 지정  
  - `Transient` : 저장 불가, 런타임 전용 변수

---

**💬 면접식 답변**

> `UPROPERTY()`는 Unreal의 리플렉션 시스템과 GC가 객체를 추적하도록 등록하는 매크로입니다.  
> 또한 에디터와 블루프린트 노출, 네트워크 복제 등을 지정할 수 있어  
> 언리얼 코드의 핵심적인 메타데이터 역할을 합니다.

</details>

---

<details markdown="1">
<summary><strong>13. UE5에서 발생할 수 있는 메모리 누수의 원인은 무엇이며, 어떻게 진단하나요?</strong></summary>

**🧠 핵심 요약**  
- Unreal의 메모리 누수는 주로 **비-UObject 포인터, Delegate 바인딩 누락, GC 등록 누락** 등에서 발생한다.  
- `Unreal Insights`, `MemReport`, `Stat Memory` 등을 통해 추적 가능하다.

---

**🔹 특징 및 상세설명**

- **주요 원인**
  - `new`로 생성 후 `delete` 미호출
  - UPROPERTY 누락 → GC 추적 불가
  - Delegate, Timer의 바인딩 해제 누락
  - TArray 등 컨테이너의 동적 할당 누락

- **진단 방법**
  - `Stat Memory`, `Stat UObject`로 실시간 메모리 사용 확인  
  - `MemReport -full` : 힙, UObject, 텍스처 등 상세 분석  
  - `Unreal Insights` : GC 주기, 힙 할당 타이밍 시각화 가능  

---

**💬 면접식 답변**

> Unreal에서의 메모리 누수는 주로 GC 추적 누락이나 Delegate 해제 누락으로 발생합니다.  
> 저는 `MemReport`나 `Unreal Insights`를 활용해 누수된 UObject나 메모리 블록을 추적하며,  
> 비-UObject 객체의 경우 스마트 포인터로 수명 관리를 명확히 합니다.

</details>

---

<details markdown="1">
<summary><strong>14. Draw Call이란 무엇이며, 이를 줄이는 방법은?</strong></summary>

**🧠 핵심 요약**  
- Draw Call은 GPU에 **렌더링 명령을 보내는 호출 단위**이다.  
- 오브젝트가 많을수록 성능에 큰 영향을 미친다.

---

**🔹 특징 및 상세설명**

- **Draw Call** = "CPU → GPU로 하나의 그리기 명령 전달"  
  - 예: 메시 1개, 머티리얼 1개, 라이트 1개마다 하나의 Draw Call  
- **병목 현상**
  - CPU가 GPU로 데이터를 보내는 횟수가 많아질수록 병목 발생  
  - 특히 모바일 플랫폼에서 영향이 크다.

- **최적화 방법**
  - **Batching (Instancing)** : 동일 머티리얼/메시 묶기  
  - **LOD (Level of Detail)** : 거리별 폴리곤 축소  
  - **Occlusion Culling** : 보이지 않는 오브젝트 제거  
  - **Static Mesh Merging** : 여러 메시를 하나로 통합  

---

**💬 면접식 답변**

> Draw Call은 GPU에 그리기 명령을 보내는 단위로,  
> 개수가 많을수록 CPU 오버헤드가 커집니다.  
> 따라서 저는 Instancing, LOD, Culling을 적극 활용하여  
> Draw Call 수를 최소화해 성능을 확보합니다.

</details>

---

<details markdown="1">
<summary><strong>15. LOD(Level of Detail) 시스템의 원리와 설정 방법을 설명해주세요.</strong></summary>

**🧠 핵심 요약**  
- LOD는 **거리에 따라 폴리곤 수를 줄이는 기술**이다.  
- 성능을 유지하면서 시각 품질을 효율적으로 관리한다.

---

**🔹 특징 및 상세설명**

- **원리**
  - 카메라 거리 기준으로 메시에 여러 LOD 버전을 사용  
  - 가까울수록 고해상도, 멀수록 저해상도 메시  
  - GPU 부하를 줄이는 핵심 기술

- **설정**
  - Static Mesh Editor에서 LOD 설정  
  - 자동 생성: `Generate LODs` 기능  
  - 수동 설정: 각 LOD별 폴리곤 비율 직접 지정  

---

**💬 면접식 답변**

> LOD는 카메라 거리별로 메시의 디테일을 줄이는 방식으로,  
> 시각 품질과 성능을 균형 있게 유지할 수 있습니다.  
> UE5에서는 자동 LOD 생성 기능으로 대규모 씬에서도 효율적으로 관리합니다.

</details>

---

<details markdown="1">
<summary><strong>16. Unreal Insights의 주요 기능과 활용 목적은 무엇인가요?</strong></summary>

**🧠 핵심 요약**  
- Unreal Insights는 엔진 내부의 **성능 분석 도구**로,  
  CPU/GPU/메모리 사용량과 GC, TaskGraph 등 시스템 활동을 시각적으로 분석할 수 있다.

---

**🔹 특징 및 상세설명**

- **주요 분석 항목**
  - 프레임 타임, 쓰레드 부하, 함수 호출 그래프  
  - GC 실행 타이밍, Tick 이벤트, 네트워크 지연  
- **활용 방법**
  - `Stat StartFile` / `Stat StopFile`로 트레이스 데이터 수집  
  - Unreal Insights에서 Timeline으로 시각화  
- **장점**
  - 병목 구간, 메모리 폭증 구간을 직관적으로 파악 가능  
  - 대규모 멀티스레드 환경 디버깅에 유용  

---

**💬 면접식 답변**

> Unreal Insights는 엔진 전반의 성능을 시각적으로 분석할 수 있는 도구입니다.  
> 저는 이를 활용해 Tick 부하나 GC 타이밍을 분석하여  
> 병목 지점을 찾아내고, 최적화 방향을 세웁니다.

</details>

---

<details markdown="1">
<summary><strong>17. Unreal에서 멀티스레딩은 어떻게 관리되나요? (TaskGraph / AsyncTask)</strong></summary>

**🧠 핵심 요약**  
- Unreal은 `TaskGraph`, `AsyncTask`, `FRunnable` 기반 멀티스레딩을 제공한다.  
- 병렬화된 연산으로 프레임 효율을 극대화한다.

---

**🔹 특징 및 상세설명**

- **TaskGraph System**
  - 엔진 내부의 기본 병렬화 프레임워크.  
  - 렌더링, 피직스, AI 등 작업을 여러 코어에 자동 분배.
  - 의존성 그래프 기반으로 실행 순서 관리.

- **AsyncTask**
  - 경량화된 비동기 작업.  
  - `Async(EAsyncExecution::ThreadPool, []{ ... })` 형태로 사용.

- **FRunnable**
  - 직접 스레드를 구현할 때 사용.  
  - 반복 루프나 네트워크 처리 등 지속적인 백그라운드 작업에 적합.

---

**💬 면접식 답변**

> Unreal은 TaskGraph를 통해 내부 시스템을 자동 병렬화하고,  
> `AsyncTask`나 `FRunnable`을 사용해 커스텀 비동기 처리를 구현합니다.  
> 저는 주로 I/O나 Pathfinding 같은 비실시간 작업에 Async를 활용합니다.

</details>

---

<details markdown="1">
<summary><strong>18. Level Streaming과 World Partition의 차이는 무엇인가요?</strong></summary>

**🧠 핵심 요약**  
- **Level Streaming** : 수동/스크립트 기반으로 레벨을 로드/언로드.  
- **World Partition** : UE5에서 새로 도입된 **자동 스트리밍 시스템**.

---

**🔹 특징 및 상세설명**

| 항목 | Level Streaming | World Partition |
|------|------------------|------------------|
| 로드 방식 | 수동/Trigger 기반 | 자동 셀 단위 스트리밍 |
| 구조 | SubLevel 다중 관리 | 하나의 World를 셀로 분할 |
| 협업 | 제한적 | 멀티 에디터 지원 |
| 장점 | 세밀한 제어 | 자동화 및 대규모 월드 지원 |

---

**💬 면접식 답변**

> Level Streaming은 수동으로 여러 맵을 관리하지만,  
> UE5의 World Partition은 월드를 자동으로 셀 단위로 나누어  
> 필요할 때만 로드하기 때문에 대규모 오픈월드에서도 메모리 효율이 좋습니다.

</details>

---

<details markdown="1">
<summary><strong>19. Tick 성능 최적화를 위한 방법에는 어떤 것이 있나요?</strong></summary>

**🧠 핵심 요약**  
- Tick 호출은 프레임마다 실행되므로, 최소화가 핵심이다.  
- **비활성화, 주기 조절, 이벤트 기반 전환**이 주요 전략이다.

---

**🔹 특징 및 상세설명**

- **Tick 최소화 방법**
  - `PrimaryActorTick.bCanEverTick = false`
  - `SetActorTickInterval(float Time)`으로 주기 조절
  - Delegate/Event 방식으로 전환

- **추가 최적화**
  - Update 타이밍 통합 (e.g., Manager 단위 처리)
  - Tick Group 조정으로 병목 해소

---

**💬 면접식 답변**

> Tick은 매 프레임 호출되므로 불필요한 Tick을 줄이는 것이 중요합니다.  
> 저는 Event 기반 처리로 전환하거나 Tick 간격을 조절해  
> CPU 부하를 효과적으로 줄이는 방식을 자주 사용합니다.

</details>

---

<details markdown="1">
<summary><strong>20. Unreal에서 Garbage Collection을 수동으로 실행하거나 제어할 수 있나요?</strong></summary>

**🧠 핵심 요약**  
- `CollectGarbage()` 함수를 통해 수동 실행 가능하지만, 빈번한 호출은 성능 저하를 초래한다.

---

**🔹 특징 및 상세설명**

- **수동 실행 방법**
  ```cpp
  CollectGarbage(GARBAGE_COLLECTION_KEEPFLAGS);
  ```
  - 필요 시점에 명시적 호출 가능 (e.g., 맵 전환 직후)

- **주의점**
  - GC는 멀티스레드 환경에서 일시적인 프레임 드랍을 유발할 수 있다.
  - Unreal의 기본 주기는 보통 몇 초 간격으로 자동 실행된다.

---

**💬 면접식 답변**

> Unreal에서는 `CollectGarbage()`를 호출해 수동으로 GC를 실행할 수 있지만,  
> 일반적으로 엔진 루프에서 자동 관리되는 것이 권장됩니다.  
> 저는 주로 맵 전환 직후나 대량 UObject가 해제된 뒤에만 수동 호출을 고려합니다.

</details>

---

<details markdown="1">
<summary><strong>21. Unreal의 Replication(복제) 시스템은 무엇인가요?</strong></summary>

**🧠 핵심 요약**  
- Replication은 **서버가 클라이언트에 객체 상태를 동기화하는 시스템**이다.  
- 서버 권한을 가진 객체만이 복제 대상이 되며, `Replicated` 플래그와 `Server/Client` 함수 매크로로 제어된다.

---

**🔹 특징 및 상세설명**  
- **기본 원리**
  - 서버가 authoritative(권위자) 역할을 수행.  
  - 클라이언트는 해당 정보를 주기적으로 수신하여 자신의 상태를 업데이트.  
- **복제 종류**
  - **Property Replication** : 변수 값 동기화 (`UPROPERTY(Replicated)`)  
  - **Function Replication (RPC)** : 원격 함수 호출 (`Server`, `Client`, `NetMulticast`)  
- **조건부 복제**
  - `COND_SkipOwner`, `COND_OwnerOnly` 등으로 대상 제한 가능.  

---

**💬 면접식 답변**  
> Unreal의 Replication은 서버가 클라이언트로 객체의 상태를 전달하여 동기화하는 시스템입니다.  
> 예를 들어, 플레이어의 위치나 체력 같은 값은 `Replicated` 속성을 통해 자동으로 복제됩니다.  
> 모든 동기화는 서버 권한을 기반으로 이루어지며, 이는 치팅 방지와 일관성을 보장합니다.

</details>

---

<details markdown="1">
<summary><strong>22. RPC(Remote Procedure Call)의 종류와 차이를 설명해주세요.</strong></summary>

**🧠 핵심 요약**  
- Unreal은 네트워크 통신을 위해 `Server`, `Client`, `NetMulticast` RPC를 제공한다.  
- RPC는 서버-클라이언트 간 **원격 함수 호출 메커니즘**이다.

---

**🔹 특징 및 상세설명**  

| 종류 | 호출 주체 | 실행 위치 | 주요 용도 |
|------|------------|------------|------------|
| `Server` | 클라이언트 → 서버 | 서버 | 서버 권한 작업 요청 |
| `Client` | 서버 → 특정 클라이언트 | 클라이언트 | UI 업데이트, 이펙트 등 |
| `NetMulticast` | 서버 → 모든 클라이언트 | 전부 | 광역 동기화 (예: 폭발, 사운드) |

- RPC는 `UFUNCTION` 매크로에 `Server`, `Client`, `Reliable/Unreliable`을 함께 사용.  
- `Reliable`은 손실 없이 전송되지만, 네트워크 부하가 커질 수 있다.

---

**💬 면접식 답변**  
> RPC는 클라이언트와 서버 간 함수 호출을 네트워크를 통해 수행하는 방식입니다.  
> `Server`는 서버에서 실행되며, `Client`는 특정 클라이언트에서,  
> `NetMulticast`는 모든 클라이언트에서 동시에 실행됩니다.  
> 주로 상태 변화, UI 업데이트, 광역 효과 전달 등에 사용합니다.

</details>

---

<details markdown="1">
<summary><strong>23. Client Prediction과 Server Reconciliation은 무엇인가요?</strong></summary>

**🧠 핵심 요약**  
- **Client Prediction** : 클라이언트에서 입력을 예측해 즉각 반응.  
- **Server Reconciliation** : 서버의 결과와 비교 후 보정(sync)하는 단계.

---

**🔹 특징 및 상세설명**  
- **Client Prediction**
  - 입력 지연(Latency)을 줄이기 위해 클라이언트가 미리 움직임을 계산.  
  - 예: 이동, 점프, 공격 애니메이션 등 즉시 반응성 확보.
- **Server Reconciliation**
  - 서버에서 실제 결과를 계산 후 클라이언트에 전달.  
  - 클라이언트는 차이를 감지하면 보정 수행(스냅 또는 보간).

- **문제점 & 해결**
  - **Desync** 발생 시 스냅핑(Snapping)이 눈에 띄는 현상 발생.  
  - 스무딩 알고리즘, Dead Reckoning 보간으로 완화.

---

**💬 면접식 답변**  
> Prediction은 네트워크 지연을 보완하기 위한 핵심 기술로,  
> 클라이언트가 입력을 즉시 반영하고 서버의 결과를 나중에 맞추는 방식입니다.  
> 서버와 차이가 생기면 Reconciliation 과정에서 보정이 이뤄집니다.  
> 이 덕분에 지연이 큰 환경에서도 부드러운 조작이 가능합니다.

</details>

---

<details markdown="1">
<summary><strong>24. Unreal에서 네트워크 Relevancy(관련성)란 무엇인가요?</strong></summary>

**🧠 핵심 요약**  
- Relevancy는 **서버가 어떤 객체를 어느 클라이언트에 전송할지 결정하는 기준**이다.  
- 즉, “해당 클라이언트에게 필요한 데이터만 복제한다”는 개념이다.

---

**🔹 특징 및 상세설명**  
- **기준**
  - 거리 기반 : `NetCullDistanceSquared`  
  - 소유권 기반 : PlayerController의 관찰 범위  
  - 수동 제어 : `SetNetDormancy`, `AlwaysRelevant` 속성 활용 가능  
- **장점**
  - 불필요한 데이터 전송 감소 → 네트워크 최적화  
  - 서버 부하 완화 및 대규모 멀티플레이 효율 향상  

---

**💬 면접식 답변**  
> Relevancy는 클라이언트에게 필요한 객체만 복제하기 위한 시스템입니다.  
> 보통 거리에 따라 전송을 제한하거나, 플레이어가 소유한 객체만 업데이트하도록 설정합니다.  
> 이 덕분에 대규모 게임에서도 네트워크 부하를 효율적으로 줄일 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>25. Dedicated Server와 Listen Server의 차이는 무엇인가요?</strong></summary>

**🧠 핵심 요약**  
- **Dedicated Server** : 전용 서버, 클라이언트 역할 없음.  
- **Listen Server** : 호스트가 서버이자 클라이언트 역할을 동시에 수행.

---

**🔹 특징 및 상세설명**  

| 항목 | Dedicated Server | Listen Server |
|------|------------------|----------------|
| 서버 전용 여부 | 전용 | 호스트 = 서버 |
| 성능 | 안정적, 확장 가능 | 낮음 (호스트 의존) |
| 사용 예시 | 상용 멀티플레이 | 로컬 테스트, 소규모 게임 |
| 동기화 | 완전한 서버 권위 | 클라이언트 영향 있음 |

---

**💬 면접식 답변**  
> Dedicated Server는 완전한 서버 전용 구조로, 가장 안정적인 네트워크 환경을 제공합니다.  
> 반면 Listen Server는 호스트 플레이어가 서버 역할을 겸하므로 테스트나 협동형 게임에 적합합니다.  
> 대규모 온라인 게임에서는 주로 Dedicated 방식을 사용합니다.

</details>

---

<details markdown="1">
<summary><strong>26. Unreal의 GAS(Gameplay Ability System)는 무엇인가요?</strong></summary>

**🧠 핵심 요약**  
- GAS는 **스킬, 버프, 상태이상** 등을 통합적으로 관리하는 시스템이다.  
- 네트워크, Attribute, Effect, Tag 구조로 구성되어 있다.

---

**🔹 특징 및 상세설명**  
- **핵심 구성요소**
  - `GameplayAbility` : 스킬 로직 정의  
  - `GameplayEffect` : 수치 변화, 지속시간, 조건  
  - `AttributeSet` : 캐릭터의 능력치  
  - `GameplayTag` : 조건 필터 및 효과 구분  

- **장점**
  - 네트워크 복제 내장  
  - 모듈형 구조로 재사용성 높음  
  - 예측(Prediction) 시스템 지원  

---

**💬 면접식 답변**  
> GAS는 능력치, 스킬, 상태이상 등 게임플레이 로직을 통합 관리하는 시스템입니다.  
> AttributeSet, Effect, Tag 구조로 나뉘며, 예측 및 복제 기능이 내장되어 있어  
> 멀티플레이에서도 안정적이고 일관된 스킬 처리가 가능합니다.

</details>

---

<details markdown="1">
<summary><strong>27. GameplayEffect의 Duration 타입에는 어떤 것이 있나요?</strong></summary>

**🧠 핵심 요약**  
- Duration은 **효과의 지속 방식**을 결정한다.  
- `Instant`, `Duration`, `Infinite` 세 가지가 있다.

---

**🔹 특징 및 상세설명**  

| 타입 | 설명 | 예시 |
|------|------|------|
| `Instant` | 즉시 적용 후 종료 | 즉발 데미지 |
| `Duration` | 일정 시간 동안 지속 | 버프/디버프 |
| `Infinite` | 영구 적용, 수동 해제 필요 | 패시브 효과 |

---

**💬 면접식 답변**  
> GameplayEffect는 효과의 지속 방식을 Instant, Duration, Infinite 세 가지로 구분합니다.  
> 예를 들어, 공격은 Instant, 버프는 Duration, 패시브는 Infinite 방식으로 설정합니다.

</details>

---

<details markdown="1">
<summary><strong>28. AttributeSet이란 무엇이며, 능력치 변경 시 어떤 순서로 실행되나요?</strong></summary>

**🧠 핵심 요약**  
- AttributeSet은 캐릭터의 **스탯 데이터(체력, 마나 등)**를 저장하는 클래스이다.  
- 값 변경 시 특정 함수들이 순차적으로 호출된다.

---

**🔹 특징 및 상세설명**  
1. `PreAttributeChange()` : 값이 적용되기 전에 호출  
2. `PostGameplayEffectExecute()` : 효과 적용 후 실행  
3. `OnRep_` 함수 : 클라이언트에 복제될 때 호출  

- **주의점**  
  - 직접 변수 수정보다 `ApplyGameplayEffectToTarget()` 사용 권장  
  - 서버 권한에서만 값 변경 가능  

---

**💬 면접식 답변**  
> AttributeSet은 캐릭터 능력치를 저장하는 클래스이며,  
> 값이 변할 때 PreAttributeChange → PostEffectExecute → OnRep 순으로 호출됩니다.  
> 이 구조를 통해 서버-클라이언트 간 데이터 일관성을 유지합니다.

</details>

---

<details markdown="1">
<summary><strong>29. GAS에서 GameplayTag의 역할은 무엇인가요?</strong></summary>

**🧠 핵심 요약**  
- GameplayTag는 **효과, 조건, 상태를 구분하는 문자열 기반 식별자**이다.  
- 복잡한 조건문을 간결하게 관리할 수 있다.

---

**🔹 특징 및 상세설명**  
- **구조**
  - 계층적 구조 : `State.Stunned`, `Ability.Attack.Melee`  
  - 빠른 비교 가능 (`HasTag`, `MatchesTagExact`)  
- **활용 예시**
  - 캐릭터 상태 제어 (ex: “Stunned” → 입력 차단)
  - 조건 분기 (ex: “Buff.SpeedUp”이 있으면 이동속도 +10%)

---

**💬 면접식 답변**  
> GameplayTag는 상태나 조건을 문자열 기반으로 관리하는 시스템입니다.  
> 예를 들어, “Stunned” 태그가 있으면 입력을 막고,  
> “Buff.DefenseUp” 태그가 있으면 방어력을 증가시키는 식으로 간결하게 제어할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>30. GAS의 Client Prediction은 어떻게 동작하나요?</strong></summary>

**🧠 핵심 요약**  
- 클라이언트가 **Ability를 미리 실행하고**, 서버 결과로 **정정(Reconcile)** 하는 구조다.  
- 입력 지연을 줄이고 부드러운 반응성을 확보한다.

---

**🔹 특징 및 상세설명**  
- **작동 과정**
  1. 클라이언트 입력 → Ability 실행(로컬 예측)  
  2. 서버 검증 후 결과 반환  
  3. 불일치 시 Rollback 및 재적용  
- **예시**
  - 즉시 시전 스킬, 근거리 공격, 점프 입력 등  
- **장점**
  - 서버 지연(Latency)을 숨겨 부드러운 조작감 제공  

---

**💬 면접식 답변**  
> GAS의 Client Prediction은 클라이언트가 능력을 미리 시전하고,  
> 서버가 이를 검증해 최종 확정하는 방식입니다.  
> 이 과정을 통해 지연 환경에서도 자연스러운 반응성을 유지할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>31. Animation Blueprint의 State Machine이란 무엇인가요?</strong></summary>

**🧠 핵심 요약**  
- **State Machine**은 애니메이션 상태 전이를 관리하는 시스템이다.  
- “Idle → Walk → Run → Jump”처럼 **상태(State)**와 **전이 조건(Transition)**을 정의한다.

---

**🔹 특징 및 상세설명**  
- **기본 구조**
  - 각 상태는 별도의 애니메이션(Sequence 또는 BlendSpace)을 재생한다.  
  - 상태 간 이동은 Bool, Speed, IsInAir 등 변수 조건으로 결정된다.  
- **장점**
  - 논리적인 상태 전이 관리 (if문 남발 방지)  
  - 디자이너 친화적인 시각화 인터페이스 제공  
- **예시**
  - `Speed > 200 → Run`, `IsInAir == true → JumpStart`  

---

**💬 면접식 답변**  
> Animation Blueprint의 State Machine은 캐릭터의 상태 전이를 시각적으로 관리하는 시스템입니다.  
> 예를 들어, Idle에서 Run으로 전환할 때 Speed 값을 조건으로 삼아 부드럽게 전이되며,  
> 이를 통해 복잡한 애니메이션 로직을 단순화할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>32. Blend Space란 무엇이며 언제 사용하나요?</strong></summary>

**🧠 핵심 요약**  
- **Blend Space**는 여러 애니메이션을 **파라미터 값에 따라 부드럽게 보간(Blend)**하는 시스템이다.  
- 주로 이동 속도(Speed), 방향(Direction)에 따라 사용된다.

---

**🔹 특징 및 상세설명**  
- **1D Blend Space** : 하나의 파라미터로 전환 (예: Speed → Idle↔Run)  
- **2D Blend Space** : 두 파라미터로 전환 (예: Speed, Direction → 8방향 이동)  
- **장점**
  - 자연스러운 애니메이션 전환  
  - 변수 기반의 실시간 보간  
- **활용**
  - 이동, 조준, 카메라 방향 등  

---

**💬 면접식 답변**  
> Blend Space는 여러 애니메이션을 변수에 따라 보간하는 시스템입니다.  
> 캐릭터가 움직일 때 Speed와 Direction 값을 기준으로 자연스럽게 걷기↔달리기↔회전 모션이 전환됩니다.

</details>

---

<details markdown="1">
<summary><strong>33. Anim Montage의 역할은 무엇인가요?</strong></summary>

**🧠 핵심 요약**  
- **Anim Montage**는 **특정 구간 애니메이션(스킬, 공격 등)**을 독립적으로 재생하기 위한 구조다.  
- 주로 Gameplay Ability System(GAS)와 연동해 사용된다.

---

**🔹 특징 및 상세설명**  
- **용도**
  - 공격, 회피, 스킬 등 하나의 클립을 독립 실행  
  - `Montage Play`, `Section Jump`, `Blend Out` 등 제어 가능  
- **이점**
  - 다른 애니메이션(State Machine)과 분리되어 독립 재생 가능  
  - Notifies를 통해 타이밍 제어 가능 (이펙트, 데미지 등)

---

**💬 면접식 답변**  
> Anim Montage는 공격이나 스킬 같은 단발성 애니메이션을 관리하는 구조입니다.  
> 일반적인 상태머신과 분리되어, Montage 단위로 독립 제어가 가능하며  
> Notifies를 통해 사운드, 이펙트 타이밍을 정확하게 동기화할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>34. Blueprint Interface와 Event Dispatcher의 차이점을 설명해주세요.</strong></summary>

**🧠 핵심 요약**  
- **Interface** : 클래스 간 “공통 함수 정의”  
- **Event Dispatcher** : 한 객체에서 다른 객체로 이벤트 브로드캐스트

---

**🔹 특징 및 상세설명**  

| 항목 | Blueprint Interface | Event Dispatcher |
|------|----------------------|------------------|
| 목적 | 함수 규격 통일 | 이벤트 전달 |
| 연결 방식 | 클래스 간 상속 기반 | Delegate 기반 바인딩 |
| 예시 | Door, Switch 인터페이스 | Button → UI 연동 |

- **Interface**는 구조화된 설계에 유용,  
  **Dispatcher**는 실시간 이벤트 통신에 적합.

---

**💬 면접식 답변**  
> Interface는 여러 객체에 동일한 함수를 정의하는 계약서 역할이고,  
> Event Dispatcher는 한 객체에서 발생한 이벤트를 다른 객체에 실시간으로 알리는 역할을 합니다.  
> 둘 다 통신 도구지만, Interface는 설계 중심이고 Dispatcher는 실행 중심입니다.

</details>

---

<details markdown="1">
<summary><strong>35. Unreal의 Garbage Collection(GC)은 어떻게 동작하나요?</strong></summary>

**🧠 핵심 요약**  
- Unreal은 **Mark & Sweep 방식**의 가비지 컬렉션을 사용한다.  
- `UObject` 기반의 모든 객체는 **참조 추적(Reference Tracking)**을 통해 관리된다.

---

**🔹 특징 및 상세설명**  
- **Mark 단계** : Root Set에서 시작해 참조 중인 객체를 표시  
- **Sweep 단계** : 표시되지 않은 객체를 삭제  
- **특징**
  - `UPROPERTY()`가 선언된 포인터만 추적됨  
  - `NewObject`, `DuplicateObject` 등만 GC 관리 대상  
  - Native C++ 포인터(`new`)는 수동 관리 필요  

---

**💬 면접식 답변**  
> Unreal의 GC는 Mark-Sweep 방식으로 작동하며,  
> 루트 객체에서 참조되지 않는 `UObject`는 Sweep 단계에서 제거됩니다.  
> `UPROPERTY`로 선언된 객체만 추적되기 때문에  
> 수동 포인터는 반드시 명시적으로 관리해야 합니다.

</details>

---

<details markdown="1">
<summary><strong>36. UE5의 Lumen은 어떤 방식으로 동작하나요?</strong></summary>

**🧠 핵심 요약**  
- **Lumen**은 실시간 글로벌 일루미네이션(GI) 및 반사 시스템이다.  
- **스크린 공간 추적 + 거리 필드 + SDF 캐시**를 결합한 하이브리드 방식이다.

---

**🔹 특징 및 상세설명**  
- **기술 구성**
  - Screen Space Global Illumination (SSGI)
  - Distance Field Tracing (Mesh Distance Field 기반)
  - Hardware Ray Tracing (지원 시 활용)
- **장점**
  - 라이트 베이크 없이 즉시 반응  
  - 동적 조명, GI, 반사 모두 실시간 대응  
- **단점**
  - 고성능 GPU 필요  
  - 완전한 정확성은 RT보다 낮음  

---

**💬 면접식 답변**  
> Lumen은 UE5에서 도입된 실시간 글로벌 일루미네이션 시스템입니다.  
> 스크린 공간과 거리 필드 정보를 혼합해 빛의 반사와 확산을 계산하며,  
> 베이크 없이도 환경 변화에 즉시 대응할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>37. Nanite의 핵심 원리와 장점은 무엇인가요?</strong></summary>

**🧠 핵심 요약**  
- **Nanite**는 **하이 폴리곤 메시를 자동으로 최적화하는 가상화 지오메트리 시스템**이다.  
- LOD를 자동 관리하여 수십억 개의 폴리곤도 실시간 렌더링 가능하다.

---

**🔹 특징 및 상세설명**  
- **Virtualized Geometry Pipeline**
  - GPU가 직접 메시 데이터를 스트리밍  
  - 보이는 영역만 LOD 수준별로 로드  
- **장점**
  - 수작업 LOD 생성 불필요  
  - 메모리와 드로우콜 대폭 절감  
  - 시네마틱 품질 자산도 인게임 사용 가능  

---

**💬 면접식 답변**  
> Nanite는 폴리곤 수에 상관없이 실시간 렌더링이 가능한 가상화 지오메트리 기술입니다.  
> GPU가 자동으로 필요한 영역만 로드하므로  
> 고해상도 모델도 성능 저하 없이 사용할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>38. Occlusion Culling과 Frustum Culling의 차이를 설명해주세요.</strong></summary>

**🧠 핵심 요약**  
- **Frustum Culling** : 카메라 시야 밖의 객체 제거  
- **Occlusion Culling** : 시야 안이지만 다른 객체에 가려진 객체 제거

---

**🔹 특징 및 상세설명**  

| 항목 | Frustum Culling | Occlusion Culling |
|------|------------------|--------------------|
| 기준 | 카메라 시야 영역 | 가시성 (Visibility) |
| 처리 위치 | CPU | GPU |
| 예시 | 시야 바깥의 건물 | 벽 뒤의 NPC |

---

**💬 면접식 답변**  
> Frustum Culling은 카메라 밖의 객체를 제거하는 1차 필터링이고,  
> Occlusion Culling은 카메라 안이라도 가려진 객체를 제거하는 2차 필터링입니다.  
> 두 기법을 함께 사용하면 드로우콜을 크게 줄일 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>39. Level Streaming이란 무엇이며, 어떤 방식으로 사용되나요?</strong></summary>

**🧠 핵심 요약**  
- **Level Streaming**은 하나의 월드를 여러 서브 레벨로 분리해  
  **필요한 시점에만 로드/언로드**하는 시스템이다.

---

**🔹 특징 및 상세설명**  
- **방식**
  - `Blueprint Trigger` : 트리거 기반 수동 로드  
  - `World Composition` : 자동 거리 기반 로드  
  - `Level Streaming Volume` : 카메라 위치 기반 로드  
- **장점**
  - 메모리 절약 및 로딩 최적화  
  - 대규모 월드에서도 Seamless 전환 가능  

---

**💬 면접식 답변**  
> Level Streaming은 월드를 여러 레벨로 나누어 필요한 순간에만 로드하는 시스템입니다.  
> 예를 들어, 플레이어가 특정 구역에 접근하면 그 구역만 동적으로 로드되어  
> 메모리와 성능을 효율적으로 관리할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>40. Unreal의 Build 과정에서 Generate Files는 왜 필요한가요?</strong></summary>

**🧠 핵심 요약**  
- **Generate Files**는 Unreal의 리플렉션 시스템(Reflection)을 위해  
  **UCLASS / UFUNCTION / UPROPERTY** 메타 정보를 생성하는 과정이다.

---

**🔹 특징 및 상세설명**  
- UnrealHeaderTool(UHT)이 실행되어  
  `.generated.h` 파일을 생성 → 메타데이터를 등록한다.  
- 이 과정이 없으면 UE의 리플렉션, GC, Blueprint 통합이 동작하지 않는다.  
- 즉, **UObject 기반 시스템의 핵심 전처리 과정**이다.

---

**💬 면접식 답변**  
> Generate Files는 Unreal의 리플렉션 데이터를 생성하기 위한 과정입니다.  
> UHT가 클래스의 메타 정보를 파싱해 `.generated.h` 파일을 만들며,  
> 이를 통해 GC, Blueprint, Reflection 같은 엔진 핵심 기능이 작동합니다.

</details>

---

<details markdown="1">
<summary><strong>41. Subsystem이란 무엇이며 종류에는 어떤 것이 있나요?</strong></summary>

**🧠 핵심 요약**  
- Subsystem은 **Unreal의 전역 관리 객체 시스템**으로, 특정 라이프사이클에 따라 자동 초기화된다.  
- 대표적으로 **GameInstanceSubsystem, WorldSubsystem, EngineSubsystem** 등이 있다.

---

**🔹 특징 및 상세설명**  

| 종류 | 라이프사이클 | 사용 예시 |
|------|---------------|-----------|
| `UGameInstanceSubsystem` | 게임 실행 전역 유지 | 세이브, 네트워크 매니저 |
| `UWorldSubsystem` | 월드 단위 | 날씨, NPC 스폰, 포그 제어 |
| `UEngineSubsystem` | 엔진 전체 | 리소스 관리, 로깅 시스템 |

- **장점**
  - 싱글톤 패턴 대체 가능  
  - GC 및 의존성 관리 자동화  
  - 모듈 단위로 기능 분리 용이  

---

**💬 면접식 답변**  
> Subsystem은 Unreal에서 전역 기능을 안전하게 관리하기 위한 객체 시스템입니다.  
> 예를 들어 `GameInstanceSubsystem`은 세이브/설정 데이터를 관리하고,  
> `WorldSubsystem`은 월드 내 날씨나 이벤트를 제어하는 데 사용됩니다.  
> 싱글톤보다 안전하고 Unreal의 라이프사이클과 자연스럽게 동작합니다.

</details>

---

<details markdown="1">
<summary><strong>42. Unreal Build System(UBT)의 역할은 무엇인가요?</strong></summary>

**🧠 핵심 요약**  
- UBT(Unreal Build Tool)는 Unreal 프로젝트의 **모듈 빌드, 의존성 관리, 코드 생성**을 담당한다.  
- Make/CMake와 달리 Unreal의 리플렉션과 모듈 구조를 자동 처리한다.

---

**🔹 특징 및 상세설명**  
- **주요 기능**
  - `.Build.cs` 파일을 기반으로 모듈 간 의존성 분석  
  - 엔진 모듈과 프로젝트 모듈을 분리하여 병렬 빌드 수행  
  - `Target.cs`를 통해 Editor, Game, Server 빌드 타겟 지정  
- **추가 요소**
  - UnrealHeaderTool(UHT) → Reflection 데이터 생성  
  - UnrealPak → 패키징 시점 리소스 압축  

---

**💬 면접식 답변**  
> Unreal Build Tool은 엔진과 프로젝트 모듈을 자동으로 빌드하고,  
> 리플렉션 데이터와 종속성을 처리하는 Unreal의 전용 빌드 시스템입니다.  
> 일반적인 Make보다 복잡한 모듈 구조를 효율적으로 관리할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>43. Unreal의 리플렉션 시스템(Reflection)은 왜 필요한가요?</strong></summary>

**🧠 핵심 요약**  
- Reflection은 **런타임에 클래스, 함수, 변수의 메타데이터를 인식하고 조작할 수 있게 하는 시스템**이다.  
- Unreal의 **Blueprint, GC, 네트워크 복제** 기능이 모두 Reflection 기반으로 작동한다.

---

**🔹 특징 및 상세설명**  
- `UCLASS`, `USTRUCT`, `UPROPERTY`, `UFUNCTION` 등의 매크로를 통해 메타데이터를 생성한다.  
- UnrealHeaderTool(UHT)이 `.generated.h`를 만들어 리플렉션 정보를 포함시킨다.  
- **활용 예시**
  - 에디터에서 속성 노출 (UPROPERTY)
  - Blueprint에서 함수 호출
  - Replication에서 변수 복제 추적  

---

**💬 면접식 답변**  
> Reflection은 Unreal에서 클래스 정보를 런타임에 읽어오는 시스템입니다.  
> Blueprint, GC, 네트워크 복제 등이 모두 Reflection을 기반으로 하기 때문에,  
> Unreal에서는 단순한 매크로처럼 보여도 엔진 내부에서는 메타데이터 등록 작업이 이뤄집니다.

</details>

---

<details markdown="1">
<summary><strong>44. Unreal에서 멀티스레딩을 구현하는 방법에는 어떤 것이 있나요?</strong></summary>

**🧠 핵심 요약**  
- Unreal은 멀티스레딩을 위해 **FRunnable / AsyncTask / TaskGraph** 세 가지를 주요하게 제공한다.  
- 각각 목적과 사용 시점이 다르다.

---

**🔹 특징 및 상세설명**  

| 방식 | 설명 | 주요 용도 |
|------|------|-----------|
| `FRunnable` | 직접 스레드 생성, 완전한 제어 | 백그라운드 IO, 서버 |
| `AsyncTask` | 비동기 실행 (Thread Pool) | 단발성 비동기 작업 |
| `TaskGraph` | Unreal 내부 태스크 스케줄러 | 게임 루프 내 병렬화 |

- **주의사항**
  - Unreal 객체(`UObject`)는 게임 스레드에서만 안전하게 접근 가능  
  - 스레드 간 데이터 보호를 위해 `FCriticalSection`, `FEvent` 등 사용  

---

**💬 면접식 답변**  
> Unreal에서는 FRunnable, AsyncTask, TaskGraph를 사용해 멀티스레딩을 구현할 수 있습니다.  
> TaskGraph는 게임 루프 내 최적화된 병렬화를 제공하고,  
> FRunnable은 완전한 제어가 필요한 경우 직접 스레드를 생성할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>45. Unreal에서 Draw Call을 줄이기 위한 방법에는 어떤 것들이 있나요?</strong></summary>

**🧠 핵심 요약**  
- Draw Call은 GPU에 “렌더링 명령”을 내리는 횟수로, 많을수록 성능이 저하된다.  
- **Instancing**, **Batching**, **Material 통합**으로 감소시킬 수 있다.

---

**🔹 특징 및 상세설명**  
- **Instancing** : 동일한 메시를 여러 번 렌더링 시 GPU 단일 호출로 처리 (`InstancedStaticMeshComponent`)  
- **Batching** : 여러 메시를 하나로 병합 (`Hierarchical Instancing`)  
- **Material 통합** : 텍스처 아틀라스, 머티리얼 공유로 셰이더 스위칭 감소  
- **LOD 조정**, **Occlusion Culling**도 추가적인 보조 기법

---

**💬 면접식 답변**  
> Draw Call을 줄이기 위해 Instancing과 Material 통합을 적극 활용합니다.  
> 동일한 오브젝트는 InstancedStaticMesh로 묶고,  
> 머티리얼은 아틀라스로 통합하여 GPU의 명령 호출 횟수를 최소화합니다.

</details>

---

<details markdown="1">
<summary><strong>46. Unreal Editor 확장은 어떻게 이루어지나요?</strong></summary>

**🧠 핵심 요약**  
- Unreal Editor는 **Slate**와 **Editor Module**을 통해 확장 가능하다.  
- 커스텀 에디터 창, 툴바, 메뉴를 추가할 수 있다.

---

**🔹 특징 및 상세설명**  
- **Slate** : Unreal의 UI 프레임워크 (C++ 기반)  
- **Editor Utility Widget** : Blueprint로 간단한 에디터 UI 제작  
- **Plugin Module 확장**
  - `StartupModule()`에서 메뉴 등록  
  - `FMenuBuilder`로 에디터 명령 바인딩  
- **예시**
  - 자동 임포트 툴, 시각화 툴, 빌드 매니저

---

**💬 면접식 답변**  
> Unreal Editor는 Slate 프레임워크를 통해 커스텀 창이나 툴을 만들 수 있습니다.  
> 간단한 기능은 Editor Utility Widget으로 구현하고,  
> 복잡한 툴은 별도의 Editor Module로 제작해 빌드에 통합합니다.

</details>

---

<details markdown="1">
<summary><strong>47. Plugin 시스템의 구조와 장점은 무엇인가요?</strong></summary>

**🧠 핵심 요약**  
- Plugin은 **Unreal의 모듈 단위 확장 시스템**으로, 엔진 수정 없이 기능을 추가할 수 있다.  
- 기능별로 독립적이며, 코드 재사용성이 높다.

---

**🔹 특징 및 상세설명**  
- **Plugin 구성**
  - `.uplugin` : 메타데이터 정의 파일  
  - `Source/ModuleName` : 모듈 코드  
  - `Content/` : 리소스  
- **장점**
  - 다른 프로젝트로 손쉽게 이식 가능  
  - 엔진 코드 의존성 최소화  
  - 커뮤니티 배포 및 자동 업데이트 지원  

---

**💬 면접식 답변**  
> Plugin은 Unreal 기능을 엔진 수정 없이 추가할 수 있는 확장 구조입니다.  
> 예를 들어 커스텀 UI 시스템이나 Gameplay Framework를 플러그인으로 분리하면  
> 여러 프로젝트 간 재사용이 매우 쉬워집니다.

</details>

---

<details markdown="1">
<summary><strong>48. Unreal의 쿠킹(Cooking) 과정이란 무엇인가요?</strong></summary>

**🧠 핵심 요약**  
- Cooking은 게임 콘텐츠를 **플랫폼별 실행 포맷으로 변환**하는 과정이다.  
- UASSET 파일을 압축, 스트리밍, 패킹하여 최적화한다.

---

**🔹 특징 및 상세설명**  
- **과정**
  1. UAsset → Binary 변환  
  2. 리소스 압축 및 Shader 컴파일  
  3. Platform Target에 맞게 파일 구조 생성  
- **도구**
  - `UnrealPak`, `ShaderCompileWorker`, `CookOnTheFly`  
- **목적**
  - 로딩 속도 개선, 패키징 시 용량 최소화  

---

**💬 면접식 답변**  
> 쿠킹은 프로젝트 자산을 플랫폼별 실행 파일로 변환하는 과정입니다.  
> 이 과정에서 텍스처, 메시, 셰이더가 압축되고  
> 실제 런타임에서는 더 빠르고 안정적으로 로딩됩니다.

</details>

---

<details markdown="1">
<summary><strong>49. Unreal의 프로파일링 도구에는 어떤 것이 있나요?</strong></summary>

**🧠 핵심 요약**  
- Unreal은 CPU, GPU, 메모리, 네트워크 등 성능을 분석하기 위한 **다양한 프로파일러**를 제공한다.

---

**🔹 특징 및 상세설명**  

| 도구 | 주요 용도 |
|------|-----------|
| `Unreal Insights` | CPU/GPU 타임라인, 이벤트 분석 |
| `Stat Unit` | 프레임, 렌더, 게임 스레드 시간 확인 |
| `Stat GPU` | GPU 렌더링 비용 확인 |
| `MemReport` | 메모리 사용량 및 누수 추적 |
| `Stat Net` | 네트워크 트래픽 분석 |

---

**💬 면접식 답변**  
> Unreal에서는 Unreal Insights를 통해 CPU/GPU 병목을 분석하고,  
> `Stat Unit`, `Stat GPU` 명령으로 실시간 성능을 확인합니다.  
> 메모리나 네트워크 문제는 `MemReport`나 `Stat Net`을 통해 점검합니다.

</details>

---

<details markdown="1">
<summary><strong>50. Unreal에서 프로젝트 최적화를 위한 일반적인 전략은 무엇인가요?</strong></summary>

**🧠 핵심 요약**  
- 최적화는 **CPU, GPU, 메모리, I/O, 네트워크** 영역으로 나뉜다.  
- 각 영역에 맞는 분석 도구와 개선 전략이 필요하다.

---

**🔹 특징 및 상세설명**  
- **CPU** : Tick 최소화, Event 기반 전환  
- **GPU** : Draw Call 감소, LOD/Instancing 사용  
- **메모리** : Object Pooling, Level Streaming  
- **I/O** : Async 로딩, Asset 압축  
- **네트워크** : Replication 범위 제한, Delta Compression  

---

**💬 면접식 답변**  
> Unreal 최적화는 단순히 프레임 향상만이 아니라  
> CPU, GPU, 메모리 등 전반적인 리소스 효율화를 의미합니다.  
> Tick을 줄이고 Instancing을 사용하며, 네트워크는 Relevancy로 데이터 전송을 줄이는 식으로 접근합니다.

</details>
