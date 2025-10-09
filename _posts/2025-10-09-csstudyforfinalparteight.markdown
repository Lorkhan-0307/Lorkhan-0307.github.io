---
layout: post
title: "기술면접 대비 CS 공부 - Unity"
date: 2025-10-09 10:00:00 +0900
categories: [Tech Interview, Study Plan]
tags: [c++, c-sharp, computer-science, algorithm, data-structure, operating-system, network, database, design-pattern, unity, unreal]
slug: csstudyforfinalparteight
---

# 면접 대비 사전 QnA 정리 - Unity

# 🎮 Unity 면접 질문 50선 (Part 1 - 확장형)

---

<details markdown="1">
<summary><strong>1) Unity의 주요 렌더링 파이프라인 종류와 각각의 특징을 설명해주세요.</strong></summary>

**핵심 요약**  
Unity는 3가지 렌더링 파이프라인을 제공합니다 — **Built-in**, **URP**, **HDRP**.  
프로젝트의 성능, 품질, 타깃 플랫폼에 따라 선택해야 하며 SRP(Scriptable Render Pipeline) 기반입니다.

**특징 및 상세설명**  
- **Built-in Pipeline**: Unity의 전통적인 렌더링 시스템으로, Surface Shader나 커스텀 셰이더를 직접 작성해 사용합니다. 구조가 단순하고 레거시 자산이 많지만, 현대적인 GPU 기능과 SRP 기능이 부족합니다.  
- **URP (Universal Render Pipeline)**: 모바일 및 콘솔 환경에서도 성능을 유지하기 위해 경량화된 파이프라인입니다. Forward+ 렌더링 기반으로, 성능과 시각적 품질의 균형이 좋습니다.  
- **HDRP (High Definition Render Pipeline)**: 사실적인 조명, 반사, 볼륨 광원, 물리 기반 렌더링(PBR) 기능을 제공합니다. 하이엔드 PC나 콘솔 타깃에 적합하지만, 성능 비용이 높습니다.  
- **공통점**: SRP 기반이라 Render Pass나 Renderer Feature 등을 커스터마이즈하여 게임에 맞는 렌더링 파이프라인을 구성할 수 있습니다.

**면접식 답변**  
> Unity의 렌더링 파이프라인은 Built-in, URP, HDRP 세 가지로 나뉩니다.  
> Built-in은 오래된 표준 파이프라인으로, 직접 셰이더를 작성하는 데 유연하지만 구조가 단순하고 현대적인 GPU 기능을 지원하지 않습니다.  
> URP는 모바일과 콘솔, VR 환경에서 효율적인 성능을 내기 위해 최적화된 경량 파이프라인입니다. Forward 기반이라 단일 패스에서 여러 광원을 처리할 수 있고, 렌더링 구조도 단순해 최적화가 쉽습니다.  
> 반면 HDRP는 사실적인 조명과 반사, 복잡한 포스트 프로세싱을 지원해 AAA급 프로젝트나 시네마틱 퀄리티를 목표로 하는 프로젝트에 적합합니다.  
> 실무에서는 개발 환경과 플랫폼 성능, 그리고 시각적 품질 목표를 고려해 세 파이프라인 중 하나를 선택하거나, SRP를 직접 수정해 맞춤형 파이프라인을 구성하기도 합니다.

</details>

---

<details markdown="1">
<summary><strong>2) URP와 HDRP의 차이점은 무엇인가요?</strong></summary>

**핵심 요약**  
URP는 “성능 중심”, HDRP는 “품질 중심” 파이프라인입니다.  
렌더링 방식, 광원 처리, 셰이더 복잡도, 타깃 플랫폼이 다릅니다.

**특징 및 상세설명**  
- **URP**  
  - Forward Renderer 기반.  
  - 모바일, VR, 콘솔 등 성능 제약이 있는 환경에 적합.  
  - 조명 수가 제한적이며 복잡한 반사 표현은 어렵지만, 빠른 렌더링 속도를 자랑합니다.  
  - Custom Renderer Feature로 커스터마이징 용이.  
- **HDRP**  
  - Deferred Rendering + Physically Based Lighting.  
  - 복잡한 반사, 볼륨 조명, Subsurface Scattering 등 고품질 시각 효과 가능.  
  - 높은 하드웨어 요구사항.  
- **공통점**  
  - 둘 다 SRP 구조로 커스터마이징 가능.  
  - Shader Graph와 VFX Graph 지원.

**면접식 답변**  
> URP와 HDRP의 가장 큰 차이는 “성능 vs 품질”의 방향성입니다.  
> URP는 Forward Renderer 기반이라 조명 연산이 간단하고, 모바일 환경에서도 안정적인 성능을 보장합니다. 그래서 Switch나 VR 같은 제한된 디바이스에서 많이 사용됩니다.  
> 반면 HDRP는 Deferred 기반으로, 각 픽셀마다 복잡한 조명 연산과 반사, 섀도우 샘플링을 수행하기 때문에 하이엔드 그래픽에 적합합니다.  
> HDRP는 PBR, 볼륨 조명, SSAO, 물리적 반사 등 사실적인 광원 효과를 지원하지만, GPU와 메모리 부하가 크다는 점이 단점이죠.  
> 그래서 일반적으로 모바일/VR은 URP, PC/콘솔은 HDRP를 선택하고, 프로젝트 초기에 렌더링 파이프라인을 결정해야 효율적입니다.

</details>

---

<details markdown="1">
<summary><strong>3) GameObject와 Component의 관계를 설명해주세요.</strong></summary>

**핵심 요약**  
Unity의 핵심 구조는 **Entity-Component 기반**입니다.  
GameObject는 “컨테이너”, Component는 “행동과 데이터”입니다.

**특징 및 상세설명**  
- **GameObject**: Transform을 기본으로 포함하며, 이름, 태그, 계층 구조를 가진 객체입니다.  
- **Component**: GameObject에 부착되어 동작(Behavior)과 속성을 정의합니다.  
- Transform, Renderer, Collider, Script 등 모두 Component입니다.  
- 즉, GameObject 자체는 아무 기능이 없고, Component가 있어야 동작합니다.

**면접식 답변**  
> Unity의 GameObject는 단순히 컴포넌트를 담는 컨테이너이고, 실제 기능은 Component가 담당합니다.  
> 예를 들어, Transform은 위치와 회전을, Renderer는 시각적 표현을, Collider는 충돌 처리를 담당하죠.  
> 이런 구조 덕분에 Unity는 상속 대신 “조합”을 통해 객체 기능을 확장할 수 있습니다.  
> 예를 들어, Rigidbody와 Collider를 함께 붙이면 물리 객체가 되고, Animator를 추가하면 캐릭터로 발전시킬 수 있죠.  
> 이런 컴포넌트 기반 설계는 Unity의 확장성과 유지보수성을 높이는 핵심 철학입니다.

</details>

---

<details markdown="1">
<summary><strong>4) MonoBehaviour의 생명주기(Lifecycle)를 순서대로 설명해주세요.</strong></summary>

**핵심 요약**  
MonoBehaviour는 Unity 스크립트의 기본 단위로,  
**Awake → OnEnable → Start → Update → LateUpdate → OnDisable → OnDestroy** 순으로 실행됩니다.

**특징 및 상세설명**  
- **Awake()**: 객체가 생성되자마자 호출.  
- **OnEnable()**: 활성화 시점에 호출.  
- **Start()**: 첫 프레임 직전에 한 번만 호출.  
- **Update()**: 매 프레임 호출.  
- **LateUpdate()**: 모든 Update 이후 호출.  
- **FixedUpdate()**: 물리 연산용, 고정된 시간 간격.  
- **OnDisable(), OnDestroy()**: 비활성화 또는 삭제 시점에 호출.

**면접식 답변**  
> Unity에서 스크립트는 MonoBehaviour를 상속받아 동작하며, 생명주기 순서가 매우 중요합니다.  
> 먼저 객체가 생성되면 Awake가 호출되고, 활성화되면 OnEnable이 실행됩니다.  
> 게임이 시작되기 직전 Start가 한 번만 실행되고, 그 이후에는 매 프레임마다 Update가 호출됩니다.  
> 카메라 추적이나 후처리처럼 Update 이후에 처리해야 하는 로직은 LateUpdate에서 수행합니다.  
> 물리 연산은 프레임레이트와 무관하게 일정한 간격의 FixedUpdate에서 수행되며,  
> 객체가 비활성화되면 OnDisable, 삭제되면 OnDestroy가 실행되어 자원을 정리합니다.  
> 이런 순서를 명확히 이해하면 이벤트 중복, 초기화 타이밍 문제, 비활성화 시 버그 등을 예방할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>5) Awake(), Start(), OnEnable(), Update(), LateUpdate(), FixedUpdate()의 차이점은 무엇인가요?</strong></summary>

**핵심 요약**  
Unity의 주요 생명주기 함수들은 실행 시점이 다릅니다.  
초기화, 활성화, 물리 처리, 프레임 로직을 분리하기 위한 구조입니다.

**특징 및 상세설명**  
| 함수 | 실행 시점 | 주 목적 |
|------|-------------|----------|
| **Awake()** | 객체 생성 직후 | 변수 초기화, 의존 관계 설정 |
| **OnEnable()** | 활성화될 때 | Event 등록, 코루틴 시작 |
| **Start()** | 첫 프레임 전 | 초기화 완료 후 실행 |
| **Update()** | 매 프레임 | 주 로직, 입력 처리 |
| **LateUpdate()** | 모든 Update 후 | 카메라 추적, 후처리 |
| **FixedUpdate()** | 고정 시간 간격 | 물리 처리, Rigidbody 제어 |

**면접식 답변**  
> Awake는 객체가 생성될 때, OnEnable은 활성화될 때 호출됩니다.  
> Start는 모든 초기화가 완료된 후 첫 프레임 전에 실행되며,  
> Update는 매 프레임마다 호출되어 주로 입력이나 상태 갱신에 사용됩니다.  
> LateUpdate는 Update 이후에 호출되므로 카메라 추적이나 캐릭터 후처리용으로 자주 사용합니다.  
> FixedUpdate는 프레임레이트에 영향을 받지 않고 일정한 간격으로 실행되기 때문에 Rigidbody를 이용한 물리 계산에 사용됩니다.  
> 실무에서는 Awake에서 의존 객체 참조를, Start에서 초기 로직 실행을,  
> Update와 LateUpdate는 프레임 기반 로직에, FixedUpdate는 물리 전용 로직에 사용하는 패턴이 일반적입니다.

</details>

---

<details markdown="1">
<summary><strong>6) Script Execution Order의 역할과 관리 방법을 설명해주세요.</strong></summary>

**핵심 요약**  
Unity는 여러 스크립트가 동시에 존재할 때, 어떤 순서로 실행할지를 관리하기 위해 **Script Execution Order** 기능을 제공합니다.  
이 설정을 통해 초기화 순서나 Update 순서를 명시적으로 조정할 수 있습니다.

**특징 및 상세설명**  
- Unity는 기본적으로 스크립트를 **알파벳 순서**로 실행하지만, 복잡한 종속 관계에서는 제어가 필요합니다.  
- Script Execution Order는 **Project Settings → Script Execution Order**에서 개별 스크립트의 **실행 우선순위**를 직접 설정할 수 있습니다.  
- 예: InputManager → PlayerController → CameraController 순으로 실행되도록 지정 가능.  
- 너무 많은 스크립트에 순서를 강제로 지정하면 유지보수성이 떨어지므로, 최소한으로 사용하는 것이 좋습니다.  
- `RuntimeInitializeOnLoadMethod` 속성을 통해 특정 함수를 **씬 로드 전** 혹은 **후**에 실행하도록 명시할 수도 있습니다.

**면접식 답변**  
> Unity의 Script Execution Order는 여러 스크립트가 공존할 때 실행 순서를 명시적으로 제어하는 기능입니다.  
> 예를 들어, InputManager가 PlayerController보다 먼저 입력을 받아야 하거나, CameraController가 모든 이동이 끝난 후 카메라를 따라가야 할 때 이 설정이 필요합니다.  
> 보통은 의존성이 낮은 스크립트 구조를 설계하는 게 이상적이지만, 불가피한 경우 Project Settings에서 순서를 지정하거나 `RuntimeInitializeOnLoadMethod`로 제어합니다.  
> 저는 개인적으로 Script Execution Order는 최소화하고, 가능하면 이벤트나 Observer 패턴으로 의존 관계를 느슨하게 유지하는 쪽을 선호합니다.

</details>

---

<details markdown="1">
<summary><strong>7) Prefab과 Instance의 차이점을 설명해주세요.</strong></summary>

**핵심 요약**  
**Prefab**은 원본 설계(Template),  
**Instance**는 Prefab으로부터 씬에 복제된 실제 객체입니다.

**특징 및 상세설명**  
- Prefab은 Project 뷰에 존재하는 **원본 에셋**입니다.  
- Instance는 Scene 뷰에 배치된 **실제 게임 오브젝트**이며, Prefab의 속성을 상속합니다.  
- Instance에서 일부 속성을 수정하면 “Overrides” 상태로 표시됩니다.  
- Prefab을 수정하면 해당 Prefab의 모든 인스턴스에 공통 적용됩니다.  
- Prefab Variant를 이용하면 부모 Prefab의 구조를 유지하면서 일부만 변경한 버전을 만들 수 있습니다.

**면접식 답변**  
> Prefab은 일종의 설계도, 즉 원본 데이터이고 Instance는 그 복제본입니다.  
> Unity에서는 Prefab이 프로젝트 리소스로 존재하며, Scene 안에서는 이를 인스턴스화하여 배치합니다.  
> 예를 들어, 적 캐릭터를 하나의 Prefab으로 만들어 두면, 수십 개의 적이 같은 구조를 공유하면서도 개별적인 위치나 체력 데이터를 가질 수 있습니다.  
> Instance에서 속성을 변경하면 Unity는 “Overrides”로 관리하며, 원본 Prefab을 수정하면 모든 인스턴스가 일괄적으로 업데이트됩니다.  
> 실무에서는 Prefab Variant를 통해 공통 구조를 유지하면서 일부 기능만 다른 오브젝트를 쉽게 관리합니다.

</details>

---

<details markdown="1">
<summary><strong>8) DontDestroyOnLoad()의 역할은 무엇인가요?</strong></summary>

**핵심 요약**  
씬이 전환되어도 오브젝트를 삭제하지 않고 유지하기 위해 사용하는 함수입니다.  
주로 **게임 매니저, 오디오 매니저, 네트워크 클라이언트** 등에 사용됩니다.

**특징 및 상세설명**  
- Unity는 씬이 전환될 때 기본적으로 모든 오브젝트를 파괴합니다.  
- `DontDestroyOnLoad(gameObject)`를 호출하면, 해당 오브젝트는 새로운 씬으로 이동해도 파괴되지 않습니다.  
- 단, 새로운 씬에서 같은 이름의 오브젝트가 또 생성될 수 있으므로 **Singleton 패턴**과 함께 사용해야 중복 방지가 가능합니다.  
- 이 오브젝트는 “DontDestroyOnLoad”라는 특별한 씬에 보관됩니다.

**면접식 답변**  
> Unity에서 씬이 전환될 때, 대부분의 GameObject는 자동으로 Destroy되지만,  
> `DontDestroyOnLoad()`를 사용하면 특정 오브젝트를 유지시킬 수 있습니다.  
> 예를 들어 BGM을 담당하는 AudioManager나 네트워크 클라이언트처럼,  
> 게임 전체에서 단 하나만 존재해야 하는 객체에 자주 사용하죠.  
> 하지만 씬 전환 시 중복 생성이 발생할 수 있으므로 일반적으로 Singleton 패턴을 함께 사용합니다.  
> 또한, 게임 종료 시까지 유지되는 오브젝트가 많아지면 메모리 관리가 어려워질 수 있어, 꼭 필요한 객체에만 제한적으로 적용하는 것이 좋습니다.

</details>

---

<details markdown="1">
<summary><strong>9) Unity의 좌표계(World, Local, Viewport, Screen)의 차이점을 설명해주세요.</strong></summary>

**핵심 요약**  
Unity는 오브젝트의 위치와 카메라 변환을 위해 **4가지 주요 좌표계**를 사용합니다.  
각 좌표계는 변환 단계와 카메라 공간에서의 표현 방법이 다릅니다.

**특징 및 상세설명**  
| 좌표계 | 설명 | 예시 |
|---------|-------|------|
| **Local Space** | 오브젝트 자신의 Transform 기준 | 자식 객체 회전 시 함께 회전 |
| **World Space** | 전역 좌표계 (모든 오브젝트의 기준) | 씬 전체의 위치 관계 표현 |
| **Viewport Space** | 카메라 뷰포트 기준 (0~1 범위) | UI나 시야 내 비율 계산 |
| **Screen Space** | 실제 픽셀 단위의 화면 좌표 | 마우스 좌표, UI 클릭 감지 |

**면접식 답변**  
> Unity에서는 위치를 표현할 때 4가지 좌표계를 사용합니다.  
> Local은 오브젝트 자체의 Transform 기준이고, World는 씬 전체의 전역 공간입니다.  
> Viewport는 카메라 시야를 (0,0)~(1,1) 범위로 정규화한 좌표로,  
> 특정 오브젝트가 화면 안에 있는지 계산할 때 유용합니다.  
> 마지막으로 Screen Space는 실제 모니터의 픽셀 좌표로,  
> 마우스 입력이나 UI 이벤트를 처리할 때 사용합니다.  
> 실무에서는 `Camera.WorldToScreenPoint()`나 `ScreenToWorldPoint()` 같은 함수를 활용해  
> 좌표계를 변환하면서 3D 오브젝트와 UI 간의 위치를 동기화합니다.

</details>

---

<details markdown="1">
<summary><strong>10) Transform과 RectTransform의 차이점을 설명해주세요.</strong></summary>

**핵심 요약**  
Transform은 **3D 객체의 위치·회전·크기**를 표현하고,  
RectTransform은 **UI 요소의 배치와 정렬**을 위해 확장된 클래스입니다.

**특징 및 상세설명**  
- **Transform**: 3D 공간에서 오브젝트의 위치(Position), 회전(Rotation), 스케일(Scale)을 관리합니다.  
- **RectTransform**: 2D UI용으로, Anchor, Pivot, SizeDelta 등의 추가 속성을 가집니다.  
- RectTransform은 Canvas 좌표계에 맞춰 UI의 상대적 크기, 위치, 정렬을 처리합니다.  
- 부모 Canvas의 크기에 따라 자동으로 리사이즈되므로 반응형 UI 제작에 적합합니다.

**면접식 답변**  
> Transform은 3D 오브젝트의 기본 공간 정보를 관리하는 컴포넌트입니다.  
> 반면 RectTransform은 UI 전용으로, Anchor와 Pivot, SizeDelta를 통해  
> 화면 크기에 따라 자동으로 위치나 크기를 조정할 수 있습니다.  
> 예를 들어 화면 비율이 달라도 버튼이 항상 오른쪽 하단에 유지되도록  
> Anchor를 설정하면, 다양한 해상도에서 레이아웃이 깨지지 않습니다.  
> 즉, Transform이 절대적인 3D 위치를 다룬다면, RectTransform은  
> 화면 비율과 부모 Canvas를 기준으로 한 상대적인 배치를 담당합니다.

</details>

---

<details markdown="1">
<summary><strong>11) Unity의 Garbage Collection(GC) 동작 방식과 성능 최적화 방법을 설명해주세요.</strong></summary>

**핵심 요약**  
Unity는 .NET 기반의 **Mark-and-Sweep 방식** 가비지 컬렉터를 사용하며,  
GC 발생 시 모든 스레드를 중단시키기 때문에 **GC 스파이크**가 프레임 드랍의 원인이 될 수 있습니다.

**특징 및 상세설명**  
- Unity의 GC는 **Mono GC** 또는 **IL2CPP**에서의 **Boehm GC**를 사용합니다.  
- **Mark-and-Sweep** 알고리즘으로, 살아있는 객체를 표시(Mark) 후 나머지를 해제(Sweep)합니다.  
- GC는 수동으로 제어할 수 없으며, 할당이 누적되면 특정 시점에 자동으로 실행됩니다.  
- **GC 스파이크**는 모든 스레드를 멈추는 “Stop the World” 현상으로, 프레임이 순간적으로 끊깁니다.  
- **최적화 방법**  
  - 불필요한 객체 생성을 줄인다 (예: `new`를 Update에서 사용하지 않기)  
  - **Object Pooling** 활용  
  - 문자열 결합 시 `StringBuilder` 사용  
  - 값 타입(`struct`) 사용으로 힙 할당 최소화  
  - GC 발생 타이밍을 `System.GC.Collect()`로 강제할 수도 있으나, 일반적으로 권장되지 않음

**면접식 답변**  
> Unity의 GC는 기본적으로 .NET의 Mark-and-Sweep 방식으로 동작하며,  
> GC가 실행되면 모든 스레드가 멈추기 때문에 프레임 드랍이 발생할 수 있습니다.  
> 특히 Update 루프에서 매번 `new`를 호출하거나 문자열을 자주 연결하는 코드는  
> 메모리 할당을 누적시켜 프레임 스파이크를 유발합니다.  
> 그래서 저는 실무에서 **Object Pooling**을 적극적으로 활용하고,  
> 컬렉션을 재사용하며, 불필요한 메모리 할당을 최소화합니다.  
> 또한 UI 문자열 변경이 잦을 경우 `StringBuilder`나 `TMP_Text.SetText()`를 사용해  
> 힙 할당을 피합니다.  
> GC를 강제 호출하는 건 일반적으로 비추천이지만,  
> 로딩 직후처럼 일시적으로 정지해도 괜찮은 시점에는 수동 호출로 메모리를 초기화하기도 합니다.

</details>

---

<details markdown="1">
<summary><strong>12) Object Pooling 기법이란 무엇이며, 왜 사용하는가?</strong></summary>

**핵심 요약**  
Object Pooling은 **자주 생성·삭제되는 객체를 미리 만들어두고 재사용**하는 메모리 최적화 기법입니다.  
GC 스파이크 방지와 성능 향상이 주요 목적입니다.

**특징 및 상세설명**  
- Unity에서 Instantiate/Destroy는 GC 부하가 크고 메모리 파편화를 유발합니다.  
- 풀링 시스템은 필요한 만큼 미리 오브젝트를 생성하고, 비활성화 상태로 보관하다가 필요 시 활성화합니다.  
- 사용이 끝나면 다시 풀에 반환(Return)합니다.  
- **활용 예시**: 총알, 적 스폰, 이펙트, UI 팝업, 네트워크 패킷 등.  
- Unity 2021 이후에는 **Generic ObjectPool<T>** API도 공식 지원합니다.

**면접식 답변**  
> Object Pooling은 Instantiate와 Destroy로 인한 메모리 부하를 줄이기 위한 핵심 최적화 기법입니다.  
> 게임에서 총알, 파티클, UI 같은 오브젝트는 매우 자주 생성·삭제되는데,  
> 매번 힙에 새로 할당하고 해제하면 GC가 자주 발생하고 프레임 드랍이 생깁니다.  
> 그래서 풀링 시스템을 통해 일정 개수를 미리 생성해두고,  
> 필요할 때 활성화했다가 다시 반환하는 구조를 사용합니다.  
> 저는 주로 `Queue<GameObject>`를 활용하거나, Unity의 `ObjectPool<T>` 클래스를 사용해  
> 풀을 효율적으로 관리합니다.  
> 이런 방식은 메모리 재사용뿐만 아니라 Instantiate/Destroy 호출을 대폭 줄여  
> 실시간 성능을 안정화하는 데 매우 효과적입니다.

</details>

---

<details markdown="1">
<summary><strong>13) Draw Call과 Batching의 관계를 설명해주세요.</strong></summary>

**핵심 요약**  
Draw Call은 GPU에 “렌더링 명령”을 요청하는 횟수이며,  
Batching은 이를 **병합하여 Draw Call 수를 줄이는 최적화 기술**입니다.

**특징 및 상세설명**  
- Draw Call은 CPU → GPU로 전달되는 렌더 명령입니다.  
- 오브젝트마다 머티리얼이 다르면 Draw Call이 증가합니다.  
- **Batching 기법**  
  - **Static Batching**: 움직이지 않는 오브젝트를 하나로 합침.  
  - **Dynamic Batching**: 동일 머티리얼을 가진 작은 Mesh를 실시간으로 묶음.  
  - **GPU Instancing**: 동일 Mesh를 여러 번 렌더할 때 GPU가 반복 처리.  
- Draw Call이 많으면 CPU 병목이 발생하므로, Batching은 성능 최적화의 핵심 요소입니다.

**면접식 답변**  
> Draw Call은 GPU에게 “이 오브젝트를 그려라”라고 요청하는 명령 횟수입니다.  
> 수백 개의 오브젝트가 각각 다른 머티리얼을 사용하면 Draw Call이 폭발적으로 증가해  
> CPU가 렌더링 명령을 준비하는 데 많은 시간을 소비하게 됩니다.  
> 그래서 Unity에서는 Static Batching과 Dynamic Batching, 그리고 GPU Instancing을 통해  
> 여러 오브젝트를 하나의 Draw Call로 묶어 효율적으로 렌더링합니다.  
> 예를 들어, 같은 머티리얼을 사용하는 나무 100개를 Instancing으로 렌더링하면  
> Draw Call이 100 → 1로 줄어듭니다.  
> 다만 Dynamic Batching은 CPU 부하가 늘고, Static Batching은 메모리를 더 차지하므로  
> 상황에 맞게 병행하는 것이 중요합니다.

</details>

---

<details markdown="1">
<summary><strong>14) Static Batching, Dynamic Batching, GPU Instancing의 차이점을 설명해주세요.</strong></summary>

**핵심 요약**  
세 기술 모두 Draw Call을 줄이기 위한 최적화이지만,  
대상 객체와 처리 시점이 다릅니다.

**특징 및 상세설명**  
| 구분 | 대상 | 처리 방식 | 특징 |
|------|-------|------------|------|
| **Static Batching** | 움직이지 않는 오브젝트 | 빌드 시 Mesh 병합 | CPU 부하 적음, 메모리 사용 많음 |
| **Dynamic Batching** | 작은 Mesh, 동일 머티리얼 | 실시간 CPU 병합 | CPU 오버헤드 있음 |
| **GPU Instancing** | 동일 Mesh 반복 렌더 | GPU 단에서 반복 처리 | 효율적, Shader 지원 필요 |

**면접식 답변**  
> 세 가지 모두 Draw Call을 줄이는 방식이지만, 접근 방식이 다릅니다.  
> Static Batching은 움직이지 않는 오브젝트를 빌드 시점에 하나의 큰 Mesh로 합쳐서  
> 런타임에 Draw Call을 줄입니다. 대신 메모리 사용량이 증가하고, 움직이는 객체에는 부적합합니다.  
> Dynamic Batching은 CPU가 매 프레임마다 작은 Mesh를 묶어 렌더링하는 방식이라  
> 오브젝트가 적을 때만 효율적입니다.  
> GPU Instancing은 동일한 Mesh를 여러 번 렌더할 때 GPU가 반복 명령을 알아서 처리합니다.  
> 이 방식은 메모리 효율이 뛰어나고, 수많은 동일 오브젝트(예: 풀, 나무, NPC)에 이상적입니다.  
> 즉, Static은 고정물, Dynamic은 소규모 실시간, Instancing은 대규모 반복 객체에 적합하다고 정리할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>15) Overdraw이란 무엇이며, 이를 줄이는 방법은?</strong></summary>

**핵심 요약**  
Overdraw은 동일 픽셀이 여러 번 렌더링되는 현상으로,  
GPU 성능 저하의 주된 원인 중 하나입니다.

**특징 및 상세설명**  
- 2D UI나 투명 객체(Transparent Shader)에서 특히 심각합니다.  
- 여러 오브젝트가 같은 픽셀을 덮어쓰면 GPU는 불필요하게 여러 번 계산합니다.  
- **해결 방법**  
  - **Z-Prepass** 또는 **Depth Test**를 적극 활용.  
  - **Opaque → Transparent 순서로 렌더링**.  
  - **알파 블렌딩 최소화**, UI 캔버스 분리.  
  - **Shader에서 discard 최소화**, Particle의 투명도 관리.  
  - **SRP Batcher**를 이용한 최적화도 가능.

**면접식 답변**  
> Overdraw은 같은 픽셀이 여러 번 그려지는 현상으로,  
> 특히 투명 셰이더나 파티클이 많은 장면에서 GPU 부하를 급격히 증가시킵니다.  
> 예를 들어, 5개의 반투명 이펙트가 같은 영역을 덮으면, GPU는 같은 픽셀을 5번 연산해야 합니다.  
> 이를 줄이기 위해 Depth Test를 활성화하거나, 불투명 오브젝트를 먼저 렌더링해  
> Z-Buffer에 깊이를 기록한 뒤 투명 객체를 뒤에서 앞으로 그리는 방식을 사용합니다.  
> 또한 UI는 여러 캔버스로 분리해 재렌더링 범위를 최소화하고,  
> 불필요한 알파 블렌딩과 discard 연산을 피하는 것도 중요합니다.  
> 결국 Overdraw은 “보이지 않는 픽셀을 그리지 않기 위한 싸움”이라고 요약할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>18) LayerMask와 Physics.Raycast의 관계를 설명해주세요.</strong></summary>

**핵심 요약**  
LayerMask는 Raycast, Physics 연산 시 **특정 레이어만 선택적으로 감지**하도록 도와주는 필터 역할을 합니다.

**특징 및 상세설명**  
- **LayerMask**는 비트마스크(32bit)로, 특정 오브젝트 그룹만 검사할 수 있습니다.  
- **Physics.Raycast**는 광선을 쏴서 충돌된 오브젝트 정보를 반환합니다.  
- `Physics.Raycast(origin, direction, out hit, distance, layerMask)` 형태로 사용합니다.  

**예시 코드**
```csharp
int layerMask = 1 << LayerMask.NameToLayer("Enemy");
if (Physics.Raycast(ray, out hit, 100f, layerMask))
{
    Debug.Log("Enemy hit!");
}
```

**면접식 답변**  
> Unity의 Raycast는 기본적으로 모든 Collider를 검사하지만,  
> LayerMask를 지정하면 특정 레이어만 감지할 수 있습니다.  
> 예를 들어 Enemy 레이어만 탐지하거나, UI나 배경을 무시할 수 있죠.  
> 내부적으로 LayerMask는 32비트 비트마스크로 관리되어,  
> 여러 레이어를 OR 연산으로 조합할 수도 있습니다.  
> 저는 보통 `1 << LayerMask.NameToLayer("Enemy")` 형태로 Mask를 만들고,  
> `Physics.Raycast()`의 마지막 인자로 전달해 성능을 최적화합니다.  
> 특히 레이캐스트를 자주 사용하는 슈팅 게임이나 AI 시야 감지 시스템에서는  
> LayerMask 필터링이 퍼포먼스 개선에 큰 차이를 만듭니다.

</details>

---

<details markdown="1">
<summary><strong>19) Rigidbody의 Update와 FixedUpdate 중 어디서 움직여야 하나요?</strong></summary>

**핵심 요약**  
Rigidbody는 **FixedUpdate()**에서 움직여야 합니다.  
Update()는 프레임마다 실행되어, 물리 계산과 동기화되지 않습니다.

**특징 및 상세설명**  
- Update()는 **매 프레임** 호출되며, FPS에 따라 속도가 달라집니다.  
- FixedUpdate()는 **고정된 시간 간격**(기본 0.02초)으로 호출되어, 물리 계산과 동기화됩니다.  
- Rigidbody 이동은 `AddForce()`, `MovePosition()` 등 물리 기반 함수를 통해 수행해야 합니다.  
- Transform을 직접 수정하면 PhysX와 불일치가 발생합니다.

**예시 코드**
```csharp
void FixedUpdate()
{
    rb.MovePosition(rb.position + moveDirection * speed * Time.fixedDeltaTime);
}
```

**면접식 답변**  
> Rigidbody는 물리 엔진인 PhysX가 고정된 시간 간격으로 계산하기 때문에  
> Update가 아니라 FixedUpdate에서 움직여야 합니다.  
> Update는 프레임레이트에 따라 호출 횟수가 다르기 때문에  
> 동일한 입력이라도 FPS에 따라 이동 속도가 달라질 수 있죠.  
> 반면 FixedUpdate는 일정한 간격으로 호출되어 물리 연산과 정확히 동기화됩니다.  
> 예를 들어 플레이어 이동은 FixedUpdate에서 `AddForce()`로 처리하고,  
> 입력 감지는 Update에서 받은 후 FixedUpdate로 넘기는 구조가 일반적입니다.  
> 이렇게 하면 부드럽고 일관된 물리 움직임을 구현할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>20) Physics.RaycastHit 구조체에는 어떤 정보가 담겨있나요?</strong></summary>

**핵심 요약**  
RaycastHit은 광선이 충돌한 오브젝트에 대한 **위치, 법선, 거리, Collider 정보**를 포함하는 구조체입니다.

**특징 및 상세설명**  
- 주요 필드:
  - **point**: 충돌 지점의 월드 좌표  
  - **normal**: 충돌 표면의 법선 벡터  
  - **distance**: Ray 발사 지점에서의 거리  
  - **collider**: 충돌된 Collider 참조  
  - **rigidbody / transform**: 충돌된 오브젝트의 컴포넌트 참조  

**예시 코드**
```csharp
if (Physics.Raycast(ray, out RaycastHit hit))
{
    Debug.Log($"Hit at {hit.point} with {hit.collider.name}");
}
```

**면접식 답변**  
> RaycastHit 구조체는 레이캐스트가 어떤 오브젝트와 부딪혔는지를 나타내는 핵심 데이터입니다.  
> 내부에는 충돌한 지점의 좌표(`point`), 표면의 법선(`normal`), 충돌까지의 거리(`distance`),  
> 그리고 해당 오브젝트의 Collider와 Transform이 포함되어 있습니다.  
> 예를 들어 슈팅 게임에서는 RaycastHit의 `point`를 이용해 총알 이펙트를 배치하고,  
> `normal`을 통해 피탄 방향을 계산합니다.  
> 또한 `hit.collider.CompareTag("Enemy")`를 통해 충돌한 객체의 타입을 판별할 수 있습니다.  
> 실무에서는 레이캐스트 결과를 이용해 다양한 상호작용 — 클릭, 감지, 공격, 시야 체크 등을  
> 매우 세밀하게 제어합니다.

</details>

---

<details markdown="1">
<summary><strong>21) Coroutine은 무엇이며, 어떻게 동작하나요?</strong></summary>

**핵심 요약**  
Coroutine은 **시간 지연이나 비동기적인 작업 흐름을 표현**하기 위한 Unity 고유의 제어 구조입니다.  
메인 스레드를 멈추지 않고, 프레임 단위로 **일시 중단(yield)** 되었다가 재개됩니다.

**특징 및 상세설명**  
- C#의 `IEnumerator` 인터페이스를 기반으로 작동합니다.  
- `yield return` 키워드를 사용해 특정 조건, 시간, 또는 프레임 단위로 일시 중단할 수 있습니다.  
- Unity의 **메인 스레드에서 실행**되며, 실제로는 병렬 처리가 아닌 **프레임 단위 협력형 멀티태스킹**입니다.  
- 주로 **시간 기반 이벤트**(예: 애니메이션, 쿨타임, 시각 효과 등)에 사용됩니다.

**예시 코드**
```csharp
IEnumerator SpawnEnemy()
{
    while (true)
    {
        Instantiate(enemyPrefab);
        yield return new WaitForSeconds(2f); // 2초마다 반복
    }
}

void Start()
{
    StartCoroutine(SpawnEnemy());
}
```

**면접식 답변**  
> Coroutine은 Unity에서 시간을 다루기 위한 가장 직관적인 방법입니다.  
> 일반적인 함수는 한 번에 끝나지만, Coroutine은 `yield` 키워드를 통해 실행을 일시 중단하고  
> 다음 프레임 혹은 일정 시간 후 다시 이어서 실행할 수 있습니다.  
> 이때 CPU가 멈추는 것이 아니라, Unity의 메인 루프가 다음 프레임에서 해당 루틴을 재개합니다.  
> 예를 들어, 적을 3초마다 소환하거나, 화면을 페이드 아웃하는 경우에 자주 사용됩니다.  
> 내부적으로는 `IEnumerator`로 관리되어, 각 Coroutine의 상태를 Unity가 지속적으로 체크하며 스케줄링합니다.

</details>

---

<details markdown="1">
<summary><strong>22) Coroutine과 async/await의 차이는 무엇인가요?</strong></summary>

**핵심 요약**  
Coroutine은 **Unity 프레임 루프 기반의 일시 정지 시스템**,  
async/await는 **C# 런타임 수준의 비동기 작업(Task 기반)**입니다.

**특징 및 상세설명**  
| 구분 | Coroutine | async/await |
|------|------------|-------------|
| 실행 스레드 | Unity 메인 스레드 | 별도의 스레드 또는 Task 스케줄러 |
| 제어 단위 | 프레임/시간 단위 (`yield`) | Task 완료 시점 (`await`) |
| 병렬 실행 | 불가 (동시 실행은 흉내) | 가능 (실제 비동기 처리) |
| 예외 처리 | Unity 로그 | try/catch 지원 |
| 대표 사용 | 시각 효과, 시간 기반 처리 | 파일 I/O, 웹 요청, DB 접근 등 |

**예시 코드**
```csharp
// Coroutine
IEnumerator FadeOut()
{
    yield return new WaitForSeconds(1f);
    canvas.alpha = 0;
}

// Async
async Task LoadData()
{
    var json = await File.ReadAllTextAsync(path);
    Debug.Log(json);
}
```

**면접식 답변**  
> Coroutine은 Unity의 메인 루프에 통합된 일시 정지 기반 제어 방식이고,  
> async/await는 C#의 Task 시스템을 활용한 비동기 실행입니다.  
> Coroutine은 프레임 단위 제어에 탁월하지만, 진짜 비동기 스레드를 생성하지는 않습니다.  
> 반면 async/await는 CPU나 I/O 연산을 비동기적으로 수행해 메인 스레드 부하를 줄입니다.  
> Unity에서는 게임 오브젝트 제어나 렌더링은 메인 스레드에서만 가능하므로  
> async는 보통 데이터 로딩, 웹 통신, 저장 작업에 사용하고,  
> Coroutine은 시각적 타이밍 조절이나 UI 트랜지션에 더 자주 활용됩니다.

</details>

---

<details markdown="1">
<summary><strong>23) Update()와 LateUpdate(), FixedUpdate()의 차이점을 설명해주세요.</strong></summary>

**핵심 요약**  
세 메서드는 모두 **프레임 루프의 서로 다른 타이밍**에서 호출되며,  
주로 입력, 물리, 카메라 제어 순서에 따라 구분됩니다.

**특징 및 상세설명**  
| 함수명 | 호출 시점 | 주요 용도 |
|---------|-------------|-------------|
| **Update()** | 매 프레임 | 입력 처리, 비물리 이동 |
| **FixedUpdate()** | 고정 시간 간격 | 물리 연산 및 Rigidbody 제어 |
| **LateUpdate()** | 모든 Update 이후 | 카메라 추적, 최종 보정 |

**면접식 답변**  
> Unity의 프레임 루프는 Update → Physics(FixedUpdate) → Rendering(LateUpdate) 순으로 돌아갑니다.  
> Update는 매 프레임 호출되어 입력 처리나 로직 계산에 적합합니다.  
> FixedUpdate는 일정한 물리 시뮬레이션 간격으로 실행되기 때문에 Rigidbody 이동에 사용됩니다.  
> LateUpdate는 모든 Update가 끝난 후 호출되어, 예를 들어 캐릭터의 위치가 변한 후 카메라가 그를 추적할 때 유용합니다.  
> 이렇게 세 단계를 분리하면, 입력-물리-렌더링 간 타이밍 불일치를 최소화할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>24) Time.deltaTime과 Time.fixedDeltaTime의 차이는 무엇인가요?</strong></summary>

**핵심 요약**  
deltaTime은 **프레임 간 경과 시간**,  
fixedDeltaTime은 **물리 연산 간 고정된 시간 간격**입니다.

**특징 및 상세설명**  
- **Time.deltaTime**  
  - 한 프레임이 렌더링되는 데 걸린 실제 시간(초)  
  - FPS에 따라 값이 달라짐  
  - Update()에서 주로 사용  
- **Time.fixedDeltaTime**  
  - 물리 엔진의 고정된 시뮬레이션 간격 (기본 0.02초)  
  - FixedUpdate()에서 사용  

**예시 코드**
```csharp
void Update()
{
    transform.Translate(Vector3.forward * speed * Time.deltaTime);
}

void FixedUpdate()
{
    rb.MovePosition(rb.position + move * Time.fixedDeltaTime);
}
```

**면접식 답변**  
> deltaTime은 “지난 프레임과 현재 프레임 사이의 시간”을 의미합니다.  
> 즉, FPS가 낮을수록 deltaTime이 커지고, 높을수록 작아지죠.  
> 반면 fixedDeltaTime은 항상 일정하게 유지되어, 물리 연산을 안정적으로 처리할 수 있게 합니다.  
> 그래서 Rigidbody 이동은 FixedUpdate에서 Time.fixedDeltaTime을 써야 하고,  
> 단순 Transform 기반 이동은 Update에서 Time.deltaTime을 곱해야  
> 프레임레이트에 상관없이 일정한 속도를 유지할 수 있습니다.

</details>

---

<details markdown="1">
<summary><strong>25) Update() 최적화를 위한 대표적인 기법에는 어떤 것이 있나요?</strong></summary>

**핵심 요약**  
Update는 프레임마다 호출되므로, **함수 호출 최소화**와 **불필요한 연산 제거**가 핵심입니다.

**특징 및 상세설명**  
- 불필요한 **Find(), GetComponent()** 호출 제거 → 캐싱 필수  
- 매 프레임마다 **할당(new, string)** 금지 → GC 부하 방지  
- 이벤트 기반 처리로 Update 의존도 줄이기  
- `InvokeRepeating()`이나 Coroutine 활용  
- LOD / Distance Culling / DeltaTime 기반 간헐 실행  

**예시 코드**
```csharp
void Update()
{
    if (Time.frameCount % 10 == 0) // 10프레임마다 실행
        CheckDistance();
}
```

**면접식 답변**  
> Update는 모든 활성화된 MonoBehaviour마다 프레임마다 호출되기 때문에,  
> 불필요한 연산이 누적되면 쉽게 CPU 병목이 생깁니다.  
> 그래서 자주 접근하는 컴포넌트는 Start()에서 미리 캐싱하고,  
> 매 프레임 new나 문자열 연산을 피하는 것이 좋습니다.  
> 또한 매 프레임마다 감시할 필요 없는 로직은 `InvokeRepeating()`이나 Coroutine으로 분리하면  
> 전체 프레임 부하를 크게 줄일 수 있습니다.  
> 실제 프로젝트에서는 오브젝트 풀링, 이벤트 기반 구조, DeltaTime 기반 간헐 실행 같은 기법을 조합해  
> Update의 호출량을 최소화합니다.

</details>

---

<details markdown="1">
<summary><strong>26) Unity의 UI 시스템은 어떻게 동작하나요?</strong></summary>

**핵심 요약**  
Unity의 UI 시스템(uGUI)은 **Canvas 기반의 렌더링 계층 구조**로,  
모든 UI 요소는 Canvas에서 **배치 → 정렬 → 렌더링** 단계를 거칩니다.

**특징 및 상세설명**  
- **Canvas**는 UI의 루트이며, 모든 UI 요소는 그 하위에서 렌더링됩니다.  
- Canvas의 렌더링 모드는 3가지:  
  - **Screen Space - Overlay**: 화면에 직접 그리기 (UI가 항상 카메라 위에)  
  - **Screen Space - Camera**: 지정 카메라를 통해 렌더링  
  - **World Space**: 3D 공간 내 배치 가능한 UI  
- Canvas 하위에 RectTransform 기반의 UI 요소(Image, Text, Button 등)가 배치됩니다.  
- 이벤트는 **EventSystem + GraphicRaycaster** 조합으로 처리됩니다.

**면접식 답변**  
> Unity의 UI 시스템은 Canvas를 중심으로 동작합니다.  
> 모든 UI는 Canvas 안에서 배치되고, 렌더링 순서와 정렬은 계층 구조에 따라 결정됩니다.  
> Screen Space - Overlay 모드는 HUD나 메뉴처럼 항상 화면 위에 그릴 때 사용하고,  
> Camera 모드는 특정 카메라에서 UI를 렌더링할 때,  
> World Space는 3D 공간 내 UI(예: NPC 이름표, 게임 내 단말기 등)에 사용합니다.  
> 또한 EventSystem과 GraphicRaycaster가 Input 이벤트를 감지하여  
> 클릭, 드래그, Hover 등의 UI 상호작용을 관리합니다.  
> 실무에서는 Canvas 분리와 배치 순서가 성능 최적화의 핵심 포인트입니다.

</details>

---

<details markdown="1">
<summary><strong>27) Canvas가 변경될 때 성능이 저하되는 이유는 무엇인가요?</strong></summary>

**핵심 요약**  
Canvas 내부의 요소가 변경되면 Unity가 **Canvas 전체를 다시 리빌드(Render Batch 재계산)** 하기 때문입니다.

**특징 및 상세설명**  
- UI 요소의 위치, 크기, 텍스트, 색상 등이 변경되면 Canvas가 **전체 갱신(rebuild)** 됩니다.  
- 특히 **동적 텍스트(Text, TMP)**가 잦은 경우 Draw Call이 급격히 증가합니다.  
- Canvas를 **세분화 분리**하면 변경된 부분만 다시 렌더링하게 할 수 있습니다.  
- `CanvasGroup`을 사용하면 하위 그룹 단위로 제어 가능.

**면접식 답변**  
> Canvas는 UI의 루트이자 렌더링 단위라, 내부 요소 중 하나라도 변경되면  
> 전체 UI 버퍼를 다시 계산해야 합니다.  
> 이 과정에서 버텍스 데이터를 다시 빌드하고 배치 순서를 재정렬하기 때문에 성능이 떨어집니다.  
> 그래서 실무에서는 정적 UI(배경, 프레임 등)와 동적 UI(체력바, 텍스트 등)를  
> 서로 다른 Canvas로 분리합니다.  
> 또한 CanvasScaler 설정, CanvasGroup, Retainer Box 등을 활용해  
> 불필요한 재빌드를 최소화하는 것이 중요합니다.  
> 이는 특히 모바일 디바이스에서 FPS 안정성에 큰 영향을 줍니다.

</details>

---

<details markdown="1">
<summary><strong>28) Draw Call이란 무엇이며, UI에서 Draw Call을 줄이는 방법은?</strong></summary>

**핵심 요약**  
Draw Call은 GPU에 **렌더링 명령을 요청하는 호출 단위**입니다.  
UI에서는 **배치(batch)** 단위가 분리될 때마다 Draw Call이 증가합니다.

**특징 및 상세설명**  
- 동일한 **재질(Material)**을 사용하는 UI는 자동으로 **Batch 처리**됩니다.  
- 서로 다른 Material, Shader, BlendMode를 사용하는 경우 Batch가 분리되어 Draw Call 증가.  
- Canvas 단위로 batching이 이루어짐 → Canvas 분리 시 Draw Call도 나뉨.  
- `SpriteAtlas`를 활용하면 텍스처 병합으로 Batch 효율을 높일 수 있습니다.  

**예시 코드**
```csharp
// UI가 자주 갱신되는 경우
canvas.additionalShaderChannels = AdditionalCanvasShaderChannels.TexCoord1;
```

**면접식 답변**  
> Draw Call은 CPU에서 GPU로 “이 오브젝트를 그려라”는 명령을 보낼 때마다 발생합니다.  
> Unity UI에서는 서로 다른 Material을 쓰면 Batch가 끊어지고 Draw Call이 늘어납니다.  
> 따라서 UI는 SpriteAtlas로 텍스처를 병합하고,  
> 동일한 Material을 사용하는 요소들을 하나의 Canvas에 배치하는 것이 중요합니다.  
> 또, 자주 갱신되는 UI는 별도의 Canvas로 분리해 불필요한 재빌드를 막습니다.  
> Draw Call을 줄이면 CPU-GPU 간 병목이 줄어 전체 렌더링 성능이 개선됩니다.  
> 실제로 모바일 프로젝트에서는 Draw Call이 100 이하로 유지되도록 설계하는 것이 일반적입니다.

</details>

---

<details markdown="1">
<summary><strong>29) Layout Group과 Content Size Fitter의 차이점은 무엇인가요?</strong></summary>

**핵심 요약**  
Layout Group은 **UI 배치 자동화**,  
Content Size Fitter는 **자식 콘텐츠 크기에 따른 부모 자동 확장**을 담당합니다.

**특징 및 상세설명**  
| 구성요소 | 역할 | 사용 예시 |
|-----------|--------|-----------|
| **Horizontal/Vertical Layout Group** | 자식 UI를 일정한 간격, 정렬로 자동 배치 | 버튼 목록, 인벤토리 슬롯 |
| **Grid Layout Group** | 행과 열 기준 자동 정렬 | 카드, 타일 UI |
| **Content Size Fitter** | 콘텐츠 크기에 따라 부모 UI 크기 자동 조정 | 텍스트 박스, 채팅창 |

**면접식 답변**  
> Layout Group은 자식 UI를 자동으로 정렬하고 간격을 맞추는 역할을 합니다.  
> 반면 Content Size Fitter는 자식의 크기에 맞춰 부모의 크기를 조절하죠.  
> 예를 들어, 채팅창이나 리스트뷰처럼 콘텐츠 양이 늘어날 때  
> Content Size Fitter를 사용하면 스크롤 영역이 자동으로 확장됩니다.  
> 하지만 두 컴포넌트를 동시에 잘못 사용하면 무한 루프나 Rebuild가 반복될 수 있으므로,  
> ScrollRect와 조합 시 Anchor와 Pivot을 정확히 설정하는 것이 중요합니다.

</details>

---

<details markdown="1">
<summary><strong>30) TextMeshPro는 기존 UI Text보다 어떤 점이 우수한가요?</strong></summary>

**핵심 요약**  
TextMeshPro(TMP)는 **SDF(Signed Distance Field)** 기반 벡터 렌더링을 통해  
텍스트 품질과 성능을 모두 개선한 고급 UI 텍스트 시스템입니다.

**특징 및 상세설명**  
- **해상도 무관** 텍스트 렌더링 (SDF를 이용해 폰트 확대 시에도 선명함 유지)  
- **RichText 지원**, **Gradient / Outline / Shadow** 등 다양한 시각 효과 제공  
- **Mesh 캐싱**으로 CPU 오버헤드 감소  
- **Dynamic Font Asset** 시스템으로 런타임 폰트 확장 가능  

**예시 코드**
```csharp
using TMPro;

public class TextExample : MonoBehaviour
{
    [SerializeField] private TMP_Text label;

    void Start()
    {
        label.text = "<color=yellow>Hello</color> TMP!";
        label.outlineWidth = 0.1f;
    }
}
```

**면접식 답변**  
> TextMeshPro는 Unity의 기본 UI Text보다 품질과 유연성이 훨씬 뛰어납니다.  
> SDF(Signed Distance Field) 기술을 사용해 어떤 해상도에서도 글자가 깨지지 않고 선명하게 표시됩니다.  
> 또한 Gradient, Shadow, Outline, RichText 태그 등 다양한 시각 효과를 지원하죠.  
> 성능 면에서도 TMP는 텍스트를 Mesh로 관리해 재빌드를 최소화하고,  
> Dynamic Font Asset을 통해 다국어 폰트도 실시간으로 로드할 수 있습니다.  
> 실제로 모든 최신 Unity 프로젝트에서는 TextMeshPro가 사실상 표준 텍스트 시스템으로 사용됩니다.

</details>

---

<details markdown="1">
<summary><strong>31) Animator Controller의 기본 구조와 State Machine의 동작 원리를 설명해주세요.</strong></summary>

**핵심 요약**  
Animator Controller는 캐릭터의 애니메이션 상태 전환을 관리하는 **상태 머신(State Machine)**입니다.  
각 상태는 특정 애니메이션 클립을 재생하며, **전이(Transition)**를 통해 상태 간 이동합니다.

**특징 및 상세설명**  
- Animator는 **State(상태)**, **Transition(전이)**, **Parameter(매개변수)**, **Layer(계층)**로 구성됩니다.  
- `Animator.SetBool()`, `SetTrigger()`, `SetFloat()` 등으로 파라미터 값을 변경해 전이를 트리거합니다.  
- 전이에는 조건(Conditions)과 전환 시간(Transition Duration)을 설정할 수 있습니다.  
- 여러 Layer를 통해 상체/하체 등 **부분 애니메이션 분리**가 가능합니다.  

**면접식 답변**  
> Animator Controller는 캐릭터의 상태를 시각적으로 관리하는 State Machine 구조입니다.  
> 각 상태는 특정 애니메이션을 재생하고, 파라미터에 따라 다른 상태로 전환됩니다.  
> 예를 들어 Idle → Walk → Run은 `Speed` 파라미터를 기반으로 Blend나 전이가 일어납니다.  
> 또 여러 Layer를 활용하면 상체는 공격, 하체는 이동처럼 분리된 제어가 가능합니다.  
> 실제 프로젝트에서는 복잡한 Animator보다 코드 기반의 StateMachineBehaviour나 Playable API로  
> 전환을 직접 제어해 유지보수성을 높이기도 합니다.

</details>

---

<details markdown="1">
<summary><strong>32) Blend Tree란 무엇이며 어떤 상황에서 사용하나요?</strong></summary>

**핵심 요약**  
Blend Tree는 여러 애니메이션 클립을 **파라미터 값에 따라 실시간으로 보간(Blend)** 하는 구조입니다.  
주로 **이동 속도나 방향에 따른 자연스러운 애니메이션 전환**에 사용됩니다.

**특징 및 상세설명**  
- 1D Blend Tree: 속도(speed)나 강도(intensity)처럼 하나의 파라미터를 기준으로 보간  
- 2D Blend Tree: 방향(x,y), 조이스틱 입력 등 두 개의 파라미터를 기반으로 보간  
- 런타임에서 파라미터 값에 따라 **가중치 기반의 선형 보간** 수행  
- 캐릭터 움직임(Idle → Walk → Run), 방향 전환에 자주 활용  

**예시 코드**
```csharp
animator.SetFloat("Speed", moveSpeed);
```

**면접식 답변**  
> Blend Tree는 여러 애니메이션을 부드럽게 연결하기 위한 시스템입니다.  
> 예를 들어 Speed 파라미터를 기준으로 Idle, Walk, Run을 하나의 트리로 구성하면  
> 캐릭터가 속도 변화에 따라 자연스럽게 애니메이션이 전환됩니다.  
> 2D Blend Tree는 방향 전환에도 유용해, 조이스틱 입력에 따라  
> 전후좌우 움직임을 부드럽게 제어할 수 있습니다.  
> Blend Tree는 전환이 자주 발생하는 움직임에서  
> 상태 간 전이보다 훨씬 부드러운 비주얼을 제공합니다.

</details>

---

<details markdown="1">
<summary><strong>33) Animation Rigging 시스템의 목적과 장점은 무엇인가요?</strong></summary>

**핵심 요약**  
Animation Rigging은 런타임에 **보조 리깅(Constraint)** 을 추가해  
캐릭터의 애니메이션을 **절차적으로 제어하거나 보정**하는 시스템입니다.

**특징 및 상세설명**  
- Unity의 **Animation Rigging Package**는 Multi-Aim, Two-Bone IK, LookAt 등 다양한 Constraint 제공  
- 런타임 애니메이션 위에 **동적 리깅**을 적용할 수 있어  
  - 시선 맞추기(LookAt)  
  - 손/발 위치 보정(IK)  
  - 무기 들기, 캐릭터 상호작용 등 구현 가능  
- Animator 클립 기반 움직임에 절차적 조정을 더해 **더 자연스러운 반응형 애니메이션** 구현 가능  

**면접식 답변**  
> Animation Rigging은 기존 애니메이션 클립 위에 런타임 보정을 더할 수 있는 시스템입니다.  
> 예를 들어 캐릭터가 총을 쏠 때 조준 방향으로 머리와 팔을 자동으로 회전시키거나,  
> 계단을 오를 때 발이 지면에 정확히 닿게 보정할 수 있습니다.  
> Two Bone IK나 Multi Aim 같은 Constraint를 조합하면  
> 수많은 상황에 유연하게 대응할 수 있죠.  
> 이 방식은 Animator 클립의 한계를 보완해,  
> “사전에 만들어진 모션 + 런타임 반응형 제어”를 결합할 수 있다는 점에서  
> 최근 Unity 캐릭터 시스템에서 필수 기술로 평가받습니다.

</details>

---

<details markdown="1">
<summary><strong>34) Timeline과 Animator의 차이는 무엇인가요?</strong></summary>

**핵심 요약**  
Timeline은 **시퀀스(Sequence)** 기반 연출 시스템이고,  
Animator는 **상태 기반(State Machine)** 애니메이션 제어 시스템입니다.

**특징 및 상세설명**  
| 구분 | Animator | Timeline |
|------|-----------|-----------|
| 구조 | State Machine 기반 | Time Sequence 기반 |
| 용도 | 반복적인 캐릭터 동작 | 컷씬, 연출, 이벤트 |
| 전환 | 파라미터 전이(Transition) | 시간 축 제어 |
| 제어 방식 | 런타임 로직 | 에디터 타임라인 |

- Timeline은 여러 Track을 병렬로 실행할 수 있으며, 애니메이션뿐 아니라  
  오디오, 카메라, 이펙트, 이벤트 호출 등을 한 장면에 통합 가능.

**면접식 답변**  
> Animator는 캐릭터의 지속적인 상태 전환(Idle, Walk, Run)을 관리하고,  
> Timeline은 특정 시점 기반의 컷씬이나 이벤트 연출을 담당합니다.  
> Timeline은 영화 편집기처럼 시간 축에 애니메이션, 카메라, 사운드를 배치해  
> 하나의 시퀀스로 재생할 수 있습니다.  
> 반면 Animator는 파라미터를 기반으로 반복적, 실시간 제어가 강점입니다.  
> 예를 들어 오프닝 연출은 Timeline으로, 플레이어 이동과 전투는 Animator로 처리하는 식으로  
> 두 시스템을 목적에 맞게 병행 사용하는 것이 일반적입니다.

</details>

---

<details markdown="1">
<summary><strong>35) Root Motion이란 무엇이며, 언제 사용하나요?</strong></summary>

**핵심 요약**  
Root Motion은 애니메이션 클립에 내장된 **이동/회전 정보를 실제 Transform에 반영**하는 기술입니다.  
즉, 캐릭터가 애니메이션에 따라 **자연스럽게 움직이게 하는 방식**입니다.

**특징 및 상세설명**  
- Animator의 루트 본(hip/root) 이동 값을 Transform.position에 적용  
- 이동이 코드(`transform.Translate()`)가 아닌 **애니메이션 데이터에 의해 제어**됨  
- 캐릭터가 미끄러지지 않고 **자연스러운 보행/회전 구현** 가능  
- `Animator.applyRootMotion`으로 활성화/비활성 제어 가능  

**예시 코드**
```csharp
void OnAnimatorMove()
{
    transform.position = animator.rootPosition;
    transform.rotation = animator.rootRotation;
}
```

**면접식 답변**  
> Root Motion은 애니메이션 클립의 루트 본 움직임을 실제 Transform에 반영하는 기능입니다.  
> 이를 통해 캐릭터가 코드 기반 이동이 아닌, 모션 캡처된 자연스러운 이동으로 보행할 수 있습니다.  
> 예를 들어, 달리기 애니메이션의 발걸음 길이와 속도가 코드 이동과 다를 경우  
> 캐릭터가 미끄러지는 듯한 현상이 발생하는데, Root Motion을 사용하면 이를 방지할 수 있습니다.  
> 하지만 멀티플레이 환경에서는 Root Motion이 서버 동기화 문제를 유발할 수 있어,  
> 보통 클라이언트에서는 비활성화하고 서버에서 이동을 계산하는 구조를 사용합니다.

</details>

---

<details markdown="1">
<summary><strong>36) Unity에서 Lighting은 그래픽스 파이프라인의 어느 단계에서 적용되나요?</strong></summary>

**핵심 요약**  
Lighting은 **Fragment(픽셀) 단계**에서 주로 적용되며,  
빛과 표면 간의 상호작용 결과를 계산해 픽셀 단위의 색상을 결정합니다.

**특징 및 상세설명**  
- Lighting 연산은 **지오메트리 처리 이후**, 픽셀 셰이더(Fragment Shader) 단계에서 수행됩니다.  
- 표면의 법선 벡터(Normal)와 Light의 방향(Directional/Point/Spot)을 이용해 밝기를 계산합니다.  
- Unity의 표준 파이프라인은 **Phong / Blinn-Phong / PBR(Physical Based Rendering)** 모델 기반.  
- Unity는 Baked, Mixed, Realtime 세 가지 조명 모드를 제공합니다.

**면접식 답변**  
> Lighting은 렌더링 파이프라인의 픽셀 단계에서 처리됩니다.  
> 버텍스 단계에서 위치와 노멀을 계산한 후, Fragment 단계에서 빛의 세기와 색상을 합산하죠.  
> 이 과정에서 표면의 반사율, 노멀, 재질 속성을 함께 고려합니다.  
> Unity에서는 Realtime Light가 픽셀 단위로 실시간 연산되지만,  
> Baked Light는 미리 계산되어 Lightmap으로 저장되어 최적화됩니다.  
> 즉, 조명은 지오메트리 이후 픽셀 렌더링 시점에 가장 큰 비중을 차지하는 연산입니다.

</details>

---

<details markdown="1">
<summary><strong>37) Baked Lightmap과 Realtime Light의 차이는 무엇인가요?</strong></summary>

**핵심 요약**  
Baked Lightmap은 **사전 계산된 정적 조명**,  
Realtime Light는 **런타임에서 매 프레임 계산되는 동적 조명**입니다.

**특징 및 상세설명**  
| 구분 | Baked Light | Realtime Light |
|------|--------------|----------------|
| 계산 시점 | 사전 계산 (Lightmap 저장) | 실행 중 실시간 연산 |
| 성능 | 빠름 (GPU 부하 적음) | 느림 (매 프레임 조명 계산) |
| 적용 대상 | 정적 오브젝트 | 동적 오브젝트 |
| 그림자 | Static Shadow | Dynamic Shadow 가능 |
| 사용 예시 | 배경, 건물 | 캐릭터, 이펙트, 이동광 |

**면접식 답변**  
> Baked Light는 씬의 조명을 미리 계산해 Lightmap에 굽는 방식입니다.  
> 정적인 환경에서는 퍼포먼스가 훨씬 뛰어나지만, 실시간 조명 변화에는 대응하지 못합니다.  
> 반면 Realtime Light는 매 프레임마다 픽셀 단위로 광량을 계산하므로  
> 시간대 변화, 이동광, 불빛 반짝임 같은 효과를 구현할 수 있습니다.  
> 그러나 GPU 부하가 크기 때문에, 일반적으로 Mixed 모드로  
> 정적 요소는 Baked, 동적 요소만 Realtime으로 처리하는 혼합 방식을 많이 씁니다.

</details>

---

<details markdown="1">
<summary><strong>38) Reflection Probe는 어떤 원리로 동작하나요?</strong></summary>

**핵심 요약**  
Reflection Probe는 씬의 주변 환경을 큐브맵으로 **렌더링하여 반사 정보를 저장**하고,  
이 데이터를 이용해 **물체 표면의 반사 효과**를 표현합니다.

**특징 및 상세설명**  
- 씬의 한 지점에서 여섯 방향(+X, -X, +Y, -Y, +Z, -Z)을 렌더링해 **큐브맵 생성**  
- 물체 표면의 반사 벡터와 큐브맵 좌표를 매칭하여 반사 색 계산  
- Static Probe와 Realtime Probe 두 가지 방식 존재  
  - Static: 빌드 타임에 굽기 (정적 반사)  
  - Realtime: 런타임 중 업데이트 (거울, 물표면 등)

**면접식 답변**  
> Reflection Probe는 씬의 환경을 큐브맵 형태로 캡처해,  
> 반사 표면이 그 환경을 비추는 것처럼 보이게 만드는 시스템입니다.  
> 예를 들어 금속 오브젝트나 유리 표면에 주변 색상을 자연스럽게 반영할 수 있죠.  
> 실시간 리플렉션이 필요한 경우 Realtime Probe를 사용하지만,  
> 이는 퍼포먼스 비용이 크므로 보통 정적 오브젝트에는 Baked Probe를 사용합니다.  
> Unity의 PBR 셰이더는 이 Reflection Probe 데이터를 이용해  
> 반사광을 정확하게 계산합니다.

</details>

---

<details markdown="1">
<summary><strong>39) Unity의 Batching 기법에는 어떤 종류가 있고, 어떻게 작동하나요?</strong></summary>

**핵심 요약**  
Batching은 **여러 오브젝트의 드로우 콜을 하나로 합쳐서 처리하는 최적화 기법**입니다.  
대표적으로 **Static Batching**, **Dynamic Batching**, **GPU Instancing**이 있습니다.

**특징 및 상세설명**  
| 종류 | 특징 | 조건 |
|------|--------|--------|
| **Static Batching** | 변하지 않는 Mesh를 하나로 묶음 | `Static` 체크 필요 |
| **Dynamic Batching** | 크기/재질이 같은 작은 Mesh들을 한 번에 렌더링 | 버텍스 수 300 이하 |
| **GPU Instancing** | 동일 Mesh + 동일 Material을 GPU가 반복 렌더링 | 셰이더에서 Instancing 지원 필요 |

**면접식 답변**  
> Batching은 CPU에서 GPU로 가는 Draw Call을 줄이기 위한 핵심 최적화 기술입니다.  
> Static Batching은 정적인 오브젝트들을 미리 하나로 묶어 렌더링하고,  
> Dynamic Batching은 프레임마다 작은 오브젝트들을 묶어서 처리합니다.  
> 하지만 둘 다 CPU 메모리 부하가 생길 수 있기 때문에,  
> 대규모 오브젝트에는 GPU Instancing을 사용하는 것이 더 효율적입니다.  
> GPU Instancing은 동일한 Mesh를 GPU가 복제 렌더링하기 때문에  
> 수백 개의 동일한 나무나 적 캐릭터를 그릴 때 매우 유용합니다.

</details>

---

<details markdown="1">
<summary><strong>40) 그림자(Shadow) 시스템은 어떻게 작동하나요?</strong></summary>

**핵심 요약**  
그림자는 **빛이 닿지 않는 영역을 계산**하는 과정으로,  
주로 **Shadow Map**을 생성하여 픽셀별 깊이 비교를 통해 구현됩니다.

**특징 및 상세설명**  
- 렌더링 과정:  
  1. Light 시점에서 장면을 렌더링 → Depth 정보 저장 (Shadow Map)  
  2. 실제 카메라 시점에서 렌더링 시 Shadow Map과 비교  
  3. 픽셀이 그림자 영역에 있으면 조명 세기 감소  
- Unity는 Directional, Point, Spot Light 별로 Shadow Map 구조가 다름.  
- Soft Shadow는 주변 픽셀의 샘플링을 평균화해 부드러운 가장자리 표현.

**예시 코드**
```csharp
Shader "Custom/ShadowExample"
{
    SubShader
    {
        Pass
        {
            Tags { "LightMode" = "ShadowCaster" }
        }
    }
}
```

**면접식 답변**  
> Unity의 그림자 시스템은 Shadow Mapping 방식을 사용합니다.  
> 즉, 라이트 시점에서 한 번 씬을 렌더링해 깊이 정보를 저장하고,  
> 실제 카메라 렌더링 시 픽셀이 빛에 닿는지 Shadow Map을 참조해 판단합니다.  
> 이때 깊이 비교 결과로 픽셀이 라이트에 가려져 있으면 어둡게 처리하죠.  
> Point Light의 경우 큐브맵 형태로 6방향을, Directional Light는 Cascaded Shadow Map을 사용합니다.  
> 실무에서는 해상도, Distance, Bias 값을 조정해 Shadow Acne나 Peter-Panning 같은  
> 그림자 아티팩트를 방지하는 것이 중요합니다.

</details>

---

<details markdown="1">
<summary><strong>41) Shader란 무엇이며, Unity에서 어떤 종류가 있나요?</strong></summary>

**핵심 요약**  
Shader는 GPU에서 동작하는 **렌더링 명령 코드**로,  
오브젝트의 **색상, 조명, 질감, 반사** 등을 계산해 최종 화면 픽셀을 결정합니다.

**특징 및 상세설명**  
- Unity의 주요 Shader 종류  
  - **Surface Shader**: 표준 조명 모델 기반의 자동 처리형 셰이더  
  - **Vertex/Fragment Shader**: 완전한 커스텀 셰이더, 가장 유연함  
  - **Compute Shader**: 그래픽스가 아닌 일반 연산(GPGPU)에 사용  
- Shader는 GPU의 **병렬 연산**을 활용하며, **재질(Material)** 과 결합되어 작동함.  
- Unity에서는 HLSL을 사용하며, Shader Graph를 통해 시각적으로 작성 가능.

**면접식 답변**  
> Shader는 GPU가 화면 픽셀의 색상과 조명을 계산하도록 지시하는 프로그램입니다.  
> Unity에서는 Surface Shader가 일반적인 조명 모델을 자동 처리해주고,  
> Vertex/Fragment Shader는 개발자가 직접 수학적으로 표현할 수 있습니다.  
> 예를 들어 금속 재질의 반사광이나 Outline 같은 효과는 Vertex/Fragment 단계에서 구현하죠.  
> 또 Compute Shader는 그래픽스 외에도 물리 시뮬레이션이나 파티클 처리를 GPU로 수행할 때 사용합니다.  
> 실제로 프로젝트에서 성능을 위해 셰이더 최적화와 패스 구조를 이해하는 건 매우 중요합니다.

</details>

---

<details markdown="1">
<summary><strong>42) URP와 HDRP의 차이점은 무엇인가요?</strong></summary>

**핵심 요약**  
URP는 **범용성과 경량화**, HDRP는 **고품질 그래픽 표현**에 초점을 맞춘  
Unity의 **Scriptable Render Pipeline(SRP)** 구현체입니다.

**특징 및 상세설명**  
| 구분 | URP (Universal Render Pipeline) | HDRP (High Definition Render Pipeline) |
|------|--------------------------------|---------------------------------------|
| 목적 | 모바일/콘솔/크로스 플랫폼 | 하이엔드 PC, 콘솔 |
| 성능 | 가볍고 빠름 | 높은 퀄리티, 무거움 |
| 조명 모델 | 단순 PBR, 실시간 최적화 | 고급 물리 기반 조명 (Area Light, Volumetric) |
| 셰이더 작성 | Shader Graph 기반 | Shader Graph + Custom Node 가능 |
| 지원 기능 | Forward+, SSAO, Bloom 등 | Ray Tracing, Subsurface, Volumetrics 등 |

**면접식 답변**  
> URP는 모바일이나 중저사양 환경에서도 높은 퍼포먼스를 유지하기 위한 범용 렌더 파이프라인입니다.  
> 반면 HDRP는 AAA급 비주얼을 목표로 하는 고품질 렌더링용 파이프라인으로,  
> 복잡한 조명, 반사, Volumetric 효과 등을 지원합니다.  
> URP는 Forward+ 렌더링 기반으로 빠르지만, HDRP는 물리 기반 연산량이 많기 때문에  
> GPU 부하가 상당히 높습니다.  
> 그래서 플랫폼에 따라 URP/HDRP를 선택하며, 최근엔 URP에서도 SSAO, SSR, Decal 등  
> 고급 효과들이 점점 통합되고 있습니다.

</details>

---

<details markdown="1">
<summary><strong>43) Post Processing이란 무엇이며 어떤 효과들이 있나요?</strong></summary>

**핵심 요약**  
Post Processing은 **렌더링이 끝난 화면(Frame Buffer)에 추가 시각 효과를 적용하는 단계**입니다.  
즉, 최종 이미지를 보정하거나 연출을 강화하는 후처리 과정입니다.

**특징 및 상세설명**  
- Post Processing Stack에는 다양한 효과가 포함됨:  
  - **Bloom**: 밝은 부분을 퍼지게 만들어 빛 번짐 표현  
  - **Depth of Field**: 초점 거리 기반 배경 흐림  
  - **Color Grading**: 색보정 / 영화 톤 연출  
  - **Vignette**: 화면 가장자리 어둡게  
  - **Motion Blur / Chromatic Aberration** 등  
- 적용 순서에 따라 시각적 결과가 달라짐 (Stack 처리 구조).  
- HDRP/URP에서는 Volume Profile 기반으로 Post Processing을 관리함.

**예시 코드**
```csharp
using UnityEngine.Rendering;
using UnityEngine.Rendering.Universal;

VolumeProfile profile;
if (profile.TryGet(out Bloom bloom))
{
    bloom.intensity.value = 0.8f;
}
```

**면접식 답변**  
> Post Processing은 렌더링된 결과 이미지에 추가로 효과를 입히는 단계입니다.  
> 예를 들어 Bloom이나 Depth of Field 같은 효과를 통해  
> 게임의 영화적 분위기를 강화하거나 초점감을 줄 수 있습니다.  
> Unity에서는 URP/HDRP 모두 Volume 시스템으로 효과를 관리하고,  
> 카메라에 따라 다른 Volume Profile을 적용할 수 있어 유연합니다.  
> 실무에서는 씬 전환 시 톤이나 색감을 빠르게 바꿀 수 있어서  
> 연출 퀄리티를 결정짓는 중요한 요소입니다.

</details>

---

<details markdown="1">
<summary><strong>44) Render Queue란 무엇이며 왜 중요할까요?</strong></summary>

**핵심 요약**  
Render Queue는 Unity가 **오브젝트를 렌더링하는 순서를 제어하는 시스템**입니다.  
즉, 어떤 오브젝트가 **앞에 그려지고 뒤에 그려지는지**를 결정합니다.

**특징 및 상세설명**  
- 기본 Queue 값:  
  - **Geometry (2000)** – 불투명 오브젝트  
  - **AlphaTest (2450)** – 컷아웃  
  - **Transparent (3000)** – 반투명 오브젝트  
  - **Overlay (4000)** – UI, Post Processing  
- Shader의 `Queue` 태그나 Material의 `renderQueue` 값을 통해 제어 가능.  
- 렌더 순서를 조정하여 Z-fighting이나 투명도 겹침 문제 해결 가능.

**예시 코드**
```csharp
material.renderQueue = 3100; // 투명 오브젝트 우선 렌더링
```

**면접식 답변**  
> Render Queue는 GPU가 어떤 순서로 오브젝트를 그릴지 결정하는 시스템입니다.  
> 예를 들어 투명한 유리창을 불투명 벽보다 먼저 렌더링하면  
> 깊이 버퍼가 올바르지 않아 유리가 보이지 않는 문제가 생길 수 있습니다.  
> 그래서 불투명 오브젝트는 먼저, 투명 오브젝트는 나중에 그리도록  
> Render Queue를 명시적으로 조정해야 합니다.  
> 특히 UI나 Overlay 효과가 있는 씬에서는 Queue 값 관리가  
> 시각적 정확도와 퍼포먼스 모두에 큰 영향을 줍니다.

</details>

---

<details markdown="1">
<summary><strong>45) Material 시스템은 Shader와 어떤 관계를 가지며, 최적화 시 어떤 점을 고려해야 하나요?</strong></summary>

**핵심 요약**  
Material은 Shader의 **변수(Property)** 에 값을 전달해주는 **런타임 데이터 컨테이너**입니다.  
즉, Shader의 설계도에 구체적인 재질 값을 채워주는 역할을 합니다.

**특징 및 상세설명**  
- Material은 Shader의 프로퍼티(예: `_Color`, `_MainTex`)를 저장함.  
- 하나의 Shader를 여러 Material이 공유할 수 있음 (서로 다른 텍스처, 색상 적용 가능).  
- `MaterialPropertyBlock`을 사용하면 Draw Call을 늘리지 않고 Material 속성만 변경 가능.  
- `SharedMaterial`과 `Material`의 차이:  
  - `sharedMaterial`: 모든 인스턴스에 적용  
  - `material`: 복사본 생성 (개별 오브젝트만 변경)

**예시 코드**
```csharp
var block = new MaterialPropertyBlock();
block.SetColor("_Color", Color.red);
renderer.SetPropertyBlock(block);
```

**면접식 답변**  
> Material은 Shader에 구체적인 값을 전달해주는 데이터 그릇입니다.  
> 예를 들어 하나의 PBR Shader가 있더라도,  
> 재질에 따라 Diffuse, Metallic, Smoothness 등의 값이 달라질 수 있죠.  
> 이런 값을 Material이 관리하고, GPU로 전달합니다.  
> 최적화에서는 동일 Shader + 동일 Material을 최대한 공유해  
> Batching이 유지되도록 하는 것이 중요합니다.  
> 또한 속성만 다를 경우 `MaterialPropertyBlock`을 사용하면  
> 새로운 Material 인스턴스를 만들지 않아 Draw Call을 증가시키지 않습니다.  
> 이런 관리가 모바일 프로젝트에서는 특히 필수적인 최적화 포인트입니다.

</details>

---

<details markdown="1">
<summary><strong>46) Rigidbody란 무엇이며, Transform 이동과 어떤 차이가 있나요?</strong></summary>

**핵심 요약**  
Rigidbody는 Unity의 **물리 연산(Physics Simulation)** 을 적용받기 위한 컴포넌트로,  
Transform을 직접 조작하는 것보다 **물리 법칙 기반의 이동**을 제공합니다.

**특징 및 상세설명**  
- Rigidbody는 Unity의 **PhysX 엔진**에 의해 관리됩니다.  
- Transform을 직접 변경하면 물리 충돌이 무시되지만,  
  Rigidbody를 사용하면 충돌 감지 및 반응이 자동으로 계산됩니다.  
- `Rigidbody.MovePosition()` 및 `AddForce()` 등을 통해 힘 기반 이동 가능.  
- `isKinematic`이 `true`일 경우 물리 연산에서 제외되며, Transform 제어 가능.

**예시 코드**
```csharp
Rigidbody rb;

void FixedUpdate()
{
    rb.MovePosition(transform.position + moveDir * speed * Time.fixedDeltaTime);
}
```

**면접식 답변**  
> Rigidbody는 Unity에서 물리 연산을 적용받는 핵심 컴포넌트입니다.  
> Transform을 직접 수정하면 충돌 계산이 생략되어 캐릭터가 벽을 뚫는 문제가 생기지만,  
> Rigidbody를 통해 이동하면 PhysX 엔진이 충돌과 중력, 마찰 등을 자동으로 계산해줍니다.  
> `AddForce()`는 힘을 가하는 방식으로 자연스러운 가속/감속 효과를 낼 수 있고,  
> `MovePosition()`은 프레임 독립적인 이동이 가능합니다.  
> 그래서 캐릭터 컨트롤러나 투사체 같은 물리 객체는 항상 Rigidbody를 통해 제어하는 게 안정적입니다.

</details>

---

<details markdown="1">
<summary><strong>47) Collider의 종류와 차이를 설명해주세요.</strong></summary>

**핵심 요약**  
Collider는 물리 충돌 감지를 위한 **경계 영역(Bounding Volume)** 을 정의하는 컴포넌트입니다.  
형태와 목적에 따라 여러 종류가 있습니다.

**특징 및 상세설명**  
| Collider 종류 | 설명 | 사용 예시 |
|----------------|------|------------|
| **BoxCollider** | 직육면체 형태의 기본 충돌체 | 벽, 상자 |
| **SphereCollider** | 구 형태 | 공, 원형 트리거 |
| **CapsuleCollider** | 캡슐 형태 | 캐릭터 몸통 |
| **MeshCollider** | 메시 형태(정밀 충돌) | 지형, 복잡한 모델 |
| **WheelCollider** | 차량용, 회전/접지 계산 포함 | 자동차 물리 |
| **CharacterController** | 내장 Collider + 이동 제어 전용 | 3D 캐릭터 이동용 |

- `isTrigger`를 활성화하면 충돌 대신 **OnTriggerEnter** 이벤트 발생.  
- Collider는 Rigidbody와 함께 사용될 때 **Dynamic Collision** 이 발생함.

**면접식 답변**  
> Collider는 물리 엔진이 충돌을 감지할 수 있도록 정의한 경계 영역입니다.  
> 단순한 상자나 구 형태부터, MeshCollider처럼 복잡한 형태까지 다양하죠.  
> 캐릭터는 CapsuleCollider를, 배경은 MeshCollider를 주로 사용합니다.  
> `isTrigger`를 켜면 물리 충돌 대신 트리거 이벤트가 발생하므로,  
> 아이템 획득이나 존 감지 등에 자주 활용됩니다.  
> 실무에서는 퍼포먼스 때문에 MeshCollider보다는 단순 Collider를 조합해서 사용하는 경우가 많습니다.

</details>

---

<details markdown="1">
<summary><strong>48) Raycast의 원리와 활용 예시를 설명해주세요.</strong></summary>

**핵심 요약**  
Raycast는 **한 점에서 특정 방향으로 쏜 가상의 선(Ray)** 이 충돌하는 오브젝트를 감지하는 방식입니다.  
시선 탐지, 클릭 선택, 총알 명중 판정 등에 자주 사용됩니다.

**특징 및 상세설명**  
- `Physics.Raycast()`를 통해 Ray를 발사하고, 충돌 정보를 `RaycastHit` 구조체로 반환.  
- 3D에서는 `Physics.Raycast`, 2D에서는 `Physics2D.Raycast` 사용.  
- LayerMask를 통해 특정 오브젝트만 감지 가능.  
- Ray 길이를 지정해 거리 제한도 가능.

**예시 코드**
```csharp
Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
if (Physics.Raycast(ray, out RaycastHit hit, 100f))
{
    Debug.Log("Hit: " + hit.collider.name);
}
```

**면접식 답변**  
> Raycast는 한 점에서 특정 방향으로 가상의 선을 쏴서 충돌체를 감지하는 방식입니다.  
> FPS 게임의 총알 판정, 클릭한 오브젝트 선택, 시야 감지 등에서 자주 사용됩니다.  
> Unity에서는 `Physics.Raycast`로 쉽게 구현할 수 있고,  
> 반환되는 `RaycastHit`에는 충돌 지점, 노멀, 오브젝트 정보가 포함됩니다.  
> 또한 LayerMask를 이용하면 감지할 대상을 필터링할 수 있어 효율적인 충돌 처리가 가능합니다.

</details>

---

<details markdown="1">
<summary><strong>49) OnCollisionEnter와 OnTriggerEnter의 차이는 무엇인가요?</strong></summary>

**핵심 요약**  
두 메서드 모두 충돌 이벤트를 감지하지만,  
**OnCollisionEnter는 실제 물리 충돌**,  
**OnTriggerEnter는 트리거 통과** 시 호출됩니다.

**특징 및 상세설명**  
| 구분 | OnCollisionEnter | OnTriggerEnter |
|------|------------------|----------------|
| 충돌 조건 | 물리적으로 닿을 때 | isTrigger = true |
| Rigidbody 필요 | 하나 이상 필요 | 하나 이상 필요 |
| 충돌 반응 | 튕김, 마찰 등 물리 반응 있음 | 없음 (통과 가능) |
| 반환 데이터 | `Collision` (접촉면 정보) | `Collider` (대상 정보만) |

**예시 코드**
```csharp
void OnCollisionEnter(Collision col)
{
    Debug.Log("Hit: " + col.gameObject.name);
}

void OnTriggerEnter(Collider other)
{
    Debug.Log("Triggered: " + other.name);
}
```

**면접식 답변**  
> OnCollisionEnter는 실제 물리 충돌이 일어날 때 호출됩니다.  
> 즉, 두 오브젝트가 충돌체와 Rigidbody를 모두 가지고 있어야 하며,  
> PhysX 엔진이 반발력이나 마찰을 계산합니다.  
> 반면 OnTriggerEnter는 충돌 대신 단순 감지용으로,  
> `isTrigger`를 활성화하면 물체가 통과하더라도 이벤트만 발생하고 물리 반응은 없습니다.  
> 아이템 습득, 센서 감지, 존 입출력 처리 같은 곳에 자주 사용됩니다.

</details>

---

<details markdown="1">
<summary><strong>50) NavMesh 시스템의 원리와 사용 목적을 설명해주세요.</strong></summary>

**핵심 요약**  
NavMesh는 Unity의 **내장 경로 탐색(Pathfinding)** 시스템으로,  
AI 캐릭터가 장애물을 피해서 목표 지점까지 이동하도록 계산합니다.

**특징 및 상세설명**  
- NavMesh는 씬의 지형을 분석해 **이동 가능한 영역(Polygon Mesh)** 을 미리 굽습니다.  
- NavMesh Agent는 NavMesh 위를 따라 자동으로 이동하며, 회전/속도 제어 가능.  
- 경로 탐색은 **A* 알고리즘 기반**으로 수행됩니다.  
- NavMesh Obstacle을 추가하면 동적 장애물 회피도 가능.

**예시 코드**
```csharp
using UnityEngine.AI;

NavMeshAgent agent;
void Update()
{
    agent.SetDestination(target.position);
}
```

**면접식 답변**  
> NavMesh는 Unity의 내장 AI 경로 탐색 시스템으로,  
> 씬의 이동 가능한 지형을 분석해 NavMesh 데이터를 생성하고,  
> AI 캐릭터가 이 경로를 따라 움직이도록 돕습니다.  
> 내부적으로는 A* 알고리즘을 사용하며,  
> NavMeshAgent를 통해 자동으로 회전, 가속, 장애물 회피가 처리됩니다.  
> 예를 들어 적 캐릭터가 플레이어를 추적할 때,  
> 복잡한 구조물 사이를 자연스럽게 돌아오는 경로를 자동으로 계산할 수 있습니다.  
> 실무에서는 NavMesh를 동적 환경에서도 업데이트할 수 있도록  
> NavMesh Surface나 Obstacle 시스템을 함께 활용합니다.

</details>

---



