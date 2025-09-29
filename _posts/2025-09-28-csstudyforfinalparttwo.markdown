---
layout: post
title: "기술면접 대비 CS 공부 - 01"
date: 2025-09-28 16:00:00 +0900
categories: [Tech Interview, Study Plan]
tags: [c++, c-sharp, computer-science, algorithm, data-structure, operating-system, network, database, design-pattern, unity, unreal]
slug: csstudyforfinalparttwo
---

# 면접 대비 사전 QnA 정리 - 엔진을 좀 더 중심적으로!


# 🔷 Unity 기술 질문 – 초급 (주니어 1-2년차)

---

### ✅ 기본 개념

<details>
<summary><strong>1) MonoBehaviour의 주요 라이프사이클 메서드들을 실행 순서대로 나열해주세요.</strong></summary>
<strong>A.</strong> Unity의 MonoBehaviour는 게임 오브젝트의 동작 단위를 정의하는 핵심 클래스입니다. 실행 순서는 Awake → OnEnable → Start → Update → LateUpdate → OnDisable → OnDestroy 순서로 진행됩니다. Awake는 객체가 생성될 때, Start는 첫 프레임 직전에, Update는 매 프레임마다, LateUpdate는 Update 이후에 호출되며, OnDisable과 OnDestroy는 오브젝트가 비활성화되거나 파괴될 때 실행됩니다.
</details>

---

<details>
<summary><strong>2) GameObject와 Component의 관계를 설명하고, 왜 이런 구조를 사용하는지 말씀해주세요.</strong></summary>
<strong>A.</strong> Unity에서 GameObject는 단순한 컨테이너이고, 실제 동작은 Component가 담당합니다. 예를 들어 Transform, Renderer, Collider 같은 기능은 모두 Component입니다. 이 구조는 컴포지션 기반 아키텍처를 제공해, 상속보다 유연하게 기능을 조합할 수 있는 장점이 있습니다.
</details>

---

<details>
<summary><strong>3) Prefab과 Instance의 차이점과 각각의 활용 사례를 설명해주세요.</strong></summary>
<strong>A.</strong> Prefab은 재사용 가능한 오브젝트 원형이고, Instance는 그 원형으로부터 생성된 복제본입니다. Prefab을 활용하면 UI 버튼, 몬스터, 아이템 등 반복 사용되는 자산을 관리하기 편리하며, Instance를 씬에서 배치해 수정할 수 있습니다. Prefab 변경은 모든 Instance에 반영되어 생산성이 높아집니다.
</details>

---

<details>
<summary><strong>4) Transform 컴포넌트의 position, localPosition, rotation의 차이점은 무엇인가요?</strong></summary>
<strong>A.</strong> position은 월드 좌표계에서의 절대 위치를 의미하고, localPosition은 부모를 기준으로 한 상대 위치입니다. rotation은 오브젝트의 월드 기준 회전값이고, localRotation은 부모 기준의 회전값입니다. 계층 구조가 깊을수록 local 단위가 유용합니다.
</details>

---

<details>
<summary><strong>5) Update, FixedUpdate, LateUpdate의 차이점과 각각 언제 사용해야 하나요?</strong></summary>
<strong>A.</strong> Update는 매 프레임 호출되며 일반적인 게임 로직에 적합합니다. FixedUpdate는 물리 엔진과 동기화된 고정 시간 간격으로 실행되므로 Rigidbody 기반 물리 연산에 사용합니다. LateUpdate는 Update 이후 실행되어 카메라 추적이나 애니메이션 후처리처럼 다른 업데이트 결과를 참조할 때 활용됩니다.
</details>

---

### ✅ 기본 시스템

<details>
<summary><strong>1) Unity의 씬 관리 시스템과 씬 전환 방법에 대해 설명해주세요.</strong></summary>
<strong>A.</strong> Unity는 Scene을 통해 게임 환경을 분리 관리합니다. 전환은 SceneManager.LoadScene() 또는 LoadSceneAsync()로 수행하며, Additive 모드를 사용하면 여러 씬을 동시에 로드해 멀티 씬 구조를 만들 수 있습니다. 이를 통해 로딩 화면, UI, 맵을 분리 관리할 수 있습니다.
</details>

---

<details>
<summary><strong>2) 태그(Tag)와 레이어(Layer)의 차이점과 활용 방법을 설명해주세요.</strong></summary>
<strong>A.</strong> Tag는 오브젝트 식별을 위해 사용되며 주로 GameObject.FindWithTag 등 검색에 쓰입니다. Layer는 충돌 감지, 카메라 Culling Mask 등 렌더링/물리 처리 최적화에 활용됩니다. 즉, Tag는 논리적 분류, Layer는 물리적/렌더링 제어 용도라고 구분할 수 있습니다.
</details>

---

<details>
<summary><strong>3) Rigidbody와 Collider를 이용한 물리 시뮬레이션의 기본 원리를 설명해주세요.</strong></summary>
<strong>A.</strong> Rigidbody는 질량, 힘, 속도 같은 물리 속성을 적용해 오브젝트를 Unity 물리 엔진에서 시뮬레이션하도록 합니다. Collider는 충돌 영역을 정의해 다른 오브젝트와 상호작용을 가능하게 합니다. Rigidbody와 Collider가 함께 있어야 물리적으로 충돌이 감지되고 반응할 수 있습니다.
</details>

---

<details>
<summary><strong>4) Unity의 좌표계(월드, 로컬, 스크린, 뷰포트)에 대해 설명해주세요.</strong></summary>
<strong>A.</strong> 월드 좌표는 전역 공간 기준이고, 로컬 좌표는 부모 오브젝트 기준입니다. 스크린 좌표는 모니터 픽셀 좌표로 (0,0)은 좌하단입니다. 뷰포트 좌표는 카메라 시야를 (0~1) 범위로 정규화한 좌표로, 해상도와 무관하게 상대적 위치를 표현합니다.
</details>

---

<details>
<summary><strong>5) Inspector에서 public 변수와 [SerializeField] 속성의 차이점은 무엇인가요?</strong></summary>
<strong>A.</strong> public 변수는 코드와 Inspector 양쪽에서 접근 가능하지만, 캡슐화가 약해집니다. [SerializeField]는 private 변수도 Inspector에 노출할 수 있어, 데이터 드리븐 설계는 가능하면서 외부 접근은 막아 안정성을 유지할 수 있습니다.
</details>

---

# 🔷 Unity 기술 질문 – 중급 (주니어 2-3년차, 미들 1-2년차)

---

### ✅ 메모리 & 성능

<details>
<summary><strong>1) Unity의 Garbage Collection은 언제 발생하며, GC를 최소화하는 방법은 무엇인가요?</strong></summary>
<strong>A.</strong> Unity의 GC는 관리 힙에 할당된 객체가 더 이상 참조되지 않을 때 발생하며, 보통 Mono/IL2CPP의 Mark-Sweep 기반으로 동작합니다. 빈번한 GC는 프레임 드랍을 유발할 수 있기 때문에, 문자열 연결 대신 StringBuilder 사용, 불필요한 new 호출 줄이기, 오브젝트 풀링, 구조체 활용 등을 통해 메모리 할당을 최소화하는 것이 중요합니다.
</details>

---

<details>
<summary><strong>2) Object Pooling을 구현할 때 고려해야 할 사항들을 설명해주세요.</strong></summary>
<strong>A.</strong> Object Pooling은 재사용 가능한 오브젝트를 미리 생성해 필요할 때 꺼내 쓰는 방식입니다. 고려해야 할 점은 초기화 시점에 필요한 수량을 미리 확보하는 것, 재사용 시 상태를 초기화하는 것, 풀의 크기를 동적으로 확장할 수 있게 하는 것, 메모리 누수가 없도록 반환 규칙을 명확히 하는 것입니다.
</details>

---

<details>
<summary><strong>3) Unity Profiler에서 주로 확인하는 지표들과 최적화 방법을 설명해주세요.</strong></summary>
<strong>A.</strong> Unity Profiler에서는 CPU 사용량, GPU 렌더링 시간, 메모리 사용량, GC Alloc, Draw Call 수 등을 확인합니다. CPU 최적화는 Update 호출 최소화, 이벤트 기반 코드 사용으로, GPU 최적화는 Batching, LOD, Shader 단순화로 수행합니다. 메모리 최적화는 오브젝트 풀링, 에셋 압축, 필요 없는 오브젝트 언로드로 달성할 수 있습니다.
</details>

---

<details>
<summary><strong>4) 메모리 누수가 의심될 때 어떤 방법으로 원인을 찾으시겠습니까?</strong></summary>
<strong>A.</strong> Unity Memory Profiler 패키지를 활용해 누수된 오브젝트를 추적하거나, GC Alloc 패널에서 반복적으로 증가하는 메모리 패턴을 확인합니다. 또한 Dispose를 누락한 Native 리소스, 이벤트 핸들러 해제 누락, DontDestroyOnLoad 객체의 중복 생성 여부 등을 점검합니다.
</details>

---

<details>
<summary><strong>5) Draw Call이 무엇이며, 이를 줄이는 방법들을 설명해주세요.</strong></summary>
<strong>A.</strong> Draw Call은 GPU에 오브젝트를 그리기 위한 명령입니다. 오브젝트마다 상태 변경이 필요하면 Draw Call이 늘어나므로 성능에 영향을 줍니다. 줄이는 방법으로는 Static/Dynamic Batching, GPU Instancing, 아틀라스 텍스처 사용, 머티리얼 공유, LOD 시스템 활용이 있습니다.
</details>

---

### ✅ 비동기 & 이벤트

<details>
<summary><strong>1) Coroutine과 async/await의 차이점과 각각의 사용 시나리오를 설명해주세요.</strong></summary>
<strong>A.</strong> Coroutine은 Unity 메인 루프에서 실행을 일시 정지하고 재개하는 방식으로, 주로 시간 지연, 프레임 단위 대기에 사용됩니다. async/await은 실제 비동기 연산(Task 기반)을 처리하며, 파일 I/O나 네트워크 요청 같이 엔진 외부 비동기 처리에 적합합니다. Coroutine은 엔진 친화적, async/await은 .NET 친화적이라고 볼 수 있습니다.
</details>

---

<details>
<summary><strong>2) Unity Event와 C# Event, Delegate의 차이점을 설명해주세요.</strong></summary>
<strong>A.</strong> C# Delegate는 함수 참조를 저장하는 타입이고, Event는 Delegate를 캡슐화하여 외부에서 직접 호출하지 못하도록 제한한 것입니다. UnityEvent는 에디터에서 바인딩할 수 있는 직렬화 가능한 이벤트 시스템입니다. 즉, Delegate는 코드 내부, Event는 코드 외부 구독용, UnityEvent는 에디터 친화적으로 활용됩니다.
</details>

---

<details>
<summary><strong>3) UniTask를 사용해본 경험이 있다면 기존 방식과의 차이점을 설명해주세요.</strong></summary>
<strong>A.</strong> UniTask는 async/await 기반 비동기 작업을 GC 할당 없이 최적화한 라이브러리입니다. 기본 Task보다 가볍고 Unity 메인 스레드와 자연스럽게 동작하며, Coroutine보다 구조적 오류 처리가 용이합니다. 특히 모바일 환경에서 GC Alloc을 줄여 성능 이점이 큽니다.
</details>

---

<details>
<summary><strong>4) 멀티스레딩 시 Unity API 호출 시 주의사항은 무엇인가요?</strong></summary>
<strong>A.</strong> Unity API 대부분은 메인 스레드 전용이므로 별도 스레드에서 호출하면 오류가 발생합니다. 따라서 멀티스레드 작업에서는 데이터 가공이나 비동기 연산만 처리하고, Unity API 호출은 Main Thread Dispatcher나 Unity의 Job System을 통해 메인 루프에서 실행되도록 해야 합니다.
</details>

---

<details>
<summary><strong>5) Invoke와 InvokeRepeating의 성능 이슈와 대안을 설명해주세요.</strong></summary>
<strong>A.</strong> Invoke와 InvokeRepeating은 문자열 기반 메서드 호출로 리플렉션 비용이 발생하며, 대량 사용 시 성능 문제가 생길 수 있습니다. 대안으로는 C# Delegate나 Timer, Coroutine을 사용하는 것이 좋습니다. 특히 반복 호출은 Coroutine으로 구현하는 것이 효율적입니다.
</details>

---

### ✅ 애니메이션 & UI

<details>
<summary><strong>1) Animator Controller의 State Machine과 Blend Tree의 활용법을 설명해주세요.</strong></summary>
<strong>A.</strong> Animator Controller는 상태 전이를 정의한 State Machine으로, 각 상태에 애니메이션을 배치해 조건에 따라 전환합니다. Blend Tree는 여러 애니메이션을 파라미터 값에 따라 자연스럽게 보간합니다. 예를 들어 캐릭터 속도에 따라 Idle → Walk → Run을 부드럽게 연결할 때 활용됩니다.
</details>

---

<details>
<summary><strong>2) Animation Rigging과 Timeline을 활용한 연출 시스템을 설명해주세요.</strong></summary>
<strong>A.</strong> Animation Rigging은 뼈대에 보조 리깅을 추가해 무기 잡기, IK 발 위치 보정 같은 동적 애니메이션을 가능하게 합니다. Timeline은 컷씬이나 이벤트 시퀀스를 구성하는 도구로, 애니메이션, 오디오, 카메라 전환을 시각적으로 제어할 수 있습니다. 둘을 결합하면 유연한 연출이 가능합니다.
</details>

---

<details>
<summary><strong>3) uGUI의 Canvas 렌더 모드별 차이점과 성능 영향을 설명해주세요.</strong></summary>
<strong>A.</strong> Canvas는 Screen Space - Overlay, Screen Space - Camera, World Space 모드가 있습니다. Overlay는 최상위 UI에 적합하고 카메라 설정과 무관합니다. Camera 모드는 특정 카메라 기준 UI 배치가 가능하고, World Space는 3D 환경에 UI를 배치할 수 있습니다. 성능상 Canvas 전체 리빌드가 비용이 크므로 UI를 분리해 관리하는 것이 중요합니다.
</details>

---

<details>
<summary><strong>4) UI 요소의 자동 리사이징을 위한 Layout 컴포넌트들의 활용법을 설명해주세요.</strong></summary>
<strong>A.</strong> Unity의 Horizontal/Vertical Layout Group, Content Size Fitter, Aspect Ratio Fitter 등을 활용하면 다양한 해상도와 기기 비율에서 자동으로 UI 크기를 조정할 수 있습니다. 이를 통해 반응형 UI를 쉽게 구현할 수 있습니다.
</details>

---

<details>
<summary><strong>5) NGUI에서 uGUI로 마이그레이션할 때 고려사항은 무엇인가요?</strong></summary>
<strong>A.</strong> NGUI는 커스텀 렌더링 기반이고, uGUI는 Unity 기본 Canvas 시스템을 기반으로 합니다. 마이그레이션 시에는 드로우콜 최적화 방식 차이, 에디터 친화성, 이벤트 시스템 변경을 고려해야 합니다. 특히 UI 이벤트 처리 방식이 다르므로 InputModule을 교체하는 부분에 주의해야 합니다.
</details>

---

# 🔷 Unity 기술 질문 – 고급 (미들 3년차+, 시니어)

---

### ✅ 아키텍처 & 패턴

<details>
<summary><strong>1) ScriptableObject의 활용 사례와 데이터 중심 설계 방법을 설명해주세요.</strong></summary>
<strong>A.</strong> ScriptableObject는 에디터에서 직렬화 가능한 데이터 컨테이너로, 코드와 데이터 분리를 가능하게 합니다. 게임 밸런스 값, 아이템 데이터, 이벤트 설정 등을 ScriptableObject로 관리하면 인스턴스 간 공유가 쉬워 메모리 사용이 줄어듭니다. 데이터 중심 설계는 로직을 변경하지 않고 데이터만 교체해 다양한 콘텐츠를 구현할 수 있다는 장점이 있습니다.
</details>

---

<details>
<summary><strong>2) Addressable Asset System의 장점과 메모리 관리 전략을 설명해주세요.</strong></summary>
<strong>A.</strong> Addressables는 에셋을 주소 기반으로 로드·언로드할 수 있는 시스템으로, 대규모 프로젝트에서 효율적인 메모리 관리와 업데이트가 가능합니다. 필요할 때만 에셋을 로드하고, 사용이 끝나면 Release로 메모리를 반환하여 메모리 누수를 방지할 수 있습니다. 원격 서버에서 에셋을 교체해 패치 효율성을 높일 수 있다는 점도 장점입니다.
</details>

---

<details>
<summary><strong>3) ECS (Entity Component System) 패턴과 Unity DOTS의 차이점을 설명해주세요.</strong></summary>
<strong>A.</strong> ECS는 데이터를 Entity, Component, System으로 나눠 관리하는 패턴으로, 캐시 효율성을 극대화합니다. Unity DOTS는 이를 엔진 차원에서 지원하는 구현체로, Job System과 Burst Compiler와 결합해 대규모 데이터를 병렬 처리할 수 있습니다. 즉, ECS는 개념이고 DOTS는 Unity가 제공하는 구체적 구현입니다.
</details>

---

<details>
<summary><strong>4) Dependency Injection을 Unity에서 구현하는 방법과 장점을 설명해주세요.</strong></summary>
<strong>A.</strong> Unity에서 DI는 Zenject 같은 라이브러리나 직접 Factory, Service Locator를 구현해 가능합니다. DI의 장점은 객체 간 결합도를 낮추고, 테스트 용이성과 유지보수성을 높이는 것입니다. 특히 UI나 게임 시스템에서 Mock 객체를 주입해 단위 테스트를 쉽게 할 수 있습니다.
</details>

---

<details>
<summary><strong>5) 게임 데이터를 JSON, Binary, ScriptableObject 중 어떤 형태로 관리하시겠습니까?</strong></summary>
<strong>A.</strong> 데이터의 용도에 따라 다릅니다. JSON은 가독성과 수정 용이성이 뛰어나 서버와 통신하는 데이터에 적합합니다. Binary는 파일 크기가 작고 로딩 속도가 빨라 성능이 중요한 부분에 적합합니다. ScriptableObject는 Unity 에디터 친화적이고 공유 데이터 관리에 효율적입니다. 보통 혼합해 사용하는 것이 가장 효율적입니다.
</details>

---

### ✅ DOTS & 고급 시스템

<details>
<summary><strong>1) Unity Job System과 Burst Compiler의 동작 원리와 성능 이점을 설명해주세요.</strong></summary>
<strong>A.</strong> Job System은 멀티코어 환경에서 작업을 병렬로 나누어 실행할 수 있게 해주고, Burst Compiler는 IL 코드를 네이티브 코드로 변환해 SIMD 최적화와 메모리 접근 최적화를 수행합니다. 이 조합을 사용하면 물리 시뮬레이션이나 대규모 데이터 연산을 훨씬 빠르게 처리할 수 있습니다.
</details>

---

<details>
<summary><strong>2) ECS에서 System의 실행 순서 제어와 의존성 관리 방법을 설명해주세요.</strong></summary>
<strong>A.</strong> ECS에서는 UpdateBefore, UpdateAfter, UpdateInGroup 어트리뷰트를 통해 실행 순서를 제어합니다. 또한 JobHandle을 이용해 Job 간 의존성을 지정해 올바른 실행 순서를 보장합니다. 이를 통해 데이터 경합을 방지하고 예측 가능한 실행 순서를 유지할 수 있습니다.
</details>

---

<details>
<summary><strong>3) Unity DOTS에서 GameObject 기반 코드를 Entity 기반으로 변환하는 과정을 설명해주세요.</strong></summary>
<strong>A.</strong> 기존 GameObject 코드는 Entity Conversion Workflow를 통해 변환할 수 있습니다. ConvertToEntity 컴포넌트를 붙이면 GameObject가 Entity로 변환되며, MonoBehaviour는 대응하는 IComponentData로 치환됩니다. 이후 System에서 해당 데이터를 병렬로 처리하게 됩니다.
</details>

---

<details>
<summary><strong>4) Burst-compiled Job에서 사용할 수 없는 C# 기능들과 그 이유를 설명해주세요.</strong></summary>
<strong>A.</strong> Burst 컴파일된 Job에서는 GC 힙 할당, 가상 함수 호출, 참조 타입 사용, 예외 처리 같은 기능을 사용할 수 없습니다. 이는 성능 최적화를 위해 메모리 안전성과 예측 가능성을 보장하기 위한 제한입니다. 대신 struct 기반 IComponentData, NativeArray, value type 중심으로 작성해야 합니다.
</details>

---

<details>
<summary><strong>5) Unity NetCode for GameObjects와 기존 네트워킹 솔루션의 차이점은 무엇인가요?</strong></summary>
<strong>A.</strong> Unity NetCode는 DOTS 기반 네트워킹 프레임워크로, 대규모 멀티플레이어를 위해 최적화된 구조를 제공합니다. 기존 Mirror, UNet 같은 솔루션은 GameObject 기반으로 직관적이지만 대규모 동기화에 비효율적입니다. NetCode는 Entity 기반 구조와 예측, 보정 로직이 내장돼 있어 퍼포먼스와 확장성이 뛰어납니다.
</details>

---

### ✅ 에디터 & 커스터마이징

<details>
<summary><strong>1) 커스텀 에디터 창과 Inspector 확장을 구현하는 방법을 설명해주세요.</strong></summary>
<strong>A.</strong> UnityEditor 네임스페이스의 EditorWindow를 상속해 커스텀 에디터 창을 만들 수 있습니다. 또한 Editor 클래스를 상속하고 [CustomEditor] 어트리뷰트를 사용하면 특정 컴포넌트의 Inspector를 확장할 수 있습니다. 이를 활용하면 데이터 관리 툴, 디버깅 툴을 제작할 수 있습니다.
</details>

---

<details>
<summary><strong>2) AssetPostprocessor를 활용한 자동화 시스템 구현 사례를 설명해주세요.</strong></summary>
<strong>A.</strong> AssetPostprocessor는 에셋이 임포트될 때 후처리를 할 수 있도록 하는 클래스입니다. 예를 들어 텍스처 임포트 시 자동 압축 설정, 모델 스케일 조정, 네이밍 규칙 검사 같은 작업을 자동화할 수 있습니다. 이를 통해 아티스트와 개발자 간 워크플로우를 표준화할 수 있습니다.
</details>

---

<details>
<summary><strong>3) 빌드 파이프라인 커스터마이징과 에셋 번들 관리 전략을 설명해주세요.</strong></summary>
<strong>A.</strong> BuildPipeline 클래스를 활용하거나 ScriptableBuildPipeline을 이용하면 빌드 과정을 커스터마이징할 수 있습니다. 에셋 번들은 리소스를 묶어 효율적으로 로드·언로드할 수 있으며, Addressables과 결합해 원격 패치, 메모리 관리 전략을 체계화할 수 있습니다.
</details>

---

<details>
<summary><strong>4) Unity Package Manager를 활용한 모듈화 전략을 설명해주세요.</strong></summary>
<strong>A.</strong> 공통 기능을 별도 패키지로 분리해 UPM 패키지로 관리하면 프로젝트 간 재사용이 용이합니다. Git URL을 통한 공유, 버전 관리, 종속성 관리가 가능해 팀 협업에서 큰 장점을 줍니다. 특히 내부 공통 툴킷이나 SDK를 배포할 때 유용합니다.
</details>

---

<details>
<summary><strong>5) 에디터에서만 실행되는 코드와 런타임 코드를 분리하는 방법을 설명해주세요.</strong></summary>
<strong>A.</strong> UNITY_EDITOR 전처리기를 사용하거나 Editor 전용 어셈블리 정의 파일을 만들어 분리할 수 있습니다. 이렇게 하면 빌드 시 에디터 전용 코드는 포함되지 않아 최종 빌드 크기와 실행 안전성이 보장됩니다.
</details>

---

# 🔶 Unreal Engine 기술 질문 – 초급 (주니어 1-2년차)

---

### ✅ 기본 개념

<details>
<summary><strong>1) Blueprint와 C++의 차이점과 각각의 사용 시나리오를 설명해주세요.</strong></summary>
<strong>A.</strong> Blueprint는 비주얼 스크립팅으로 빠른 프로토타입 제작과 디자이너 친화적 개발에 적합합니다. C++은 퍼포먼스가 중요한 시스템, 복잡한 게임 로직, 네트워크 처리, 최적화 영역에 주로 사용됩니다. 일반적으로 핵심 시스템은 C++로, 콘텐츠 제작과 이벤트 로직은 Blueprint로 작성하는 하이브리드 방식을 많이 사용합니다.
</details>

---

<details>
<summary><strong>2) Actor, Pawn, Character 클래스의 계층 구조와 역할을 설명해주세요.</strong></summary>
<strong>A.</strong> Actor는 월드에 배치 가능한 모든 오브젝트의 기본 클래스입니다. Pawn은 Actor를 상속받아 플레이어나 AI가 조종 가능한 존재를 의미합니다. Character는 Pawn을 확장해 이동, 캡슐 콜리전, 애니메이션 시스템이 내장된 고수준 클래스입니다. 즉, Actor → Pawn → Character 순서로 특화됩니다.
</details>

---

<details>
<summary><strong>3) Component 시스템과 Actor에 Component를 추가하는 방법을 설명해주세요.</strong></summary>
<strong>A.</strong> Unreal은 컴포지션 기반 아키텍처로, Actor에 다양한 Component를 붙여 기능을 확장합니다. 예를 들어 MeshComponent로 외형을 표현하고, AudioComponent로 소리를 재생할 수 있습니다. Component는 생성자에서 CreateDefaultSubobject로 추가하거나, 에디터에서 드래그해 붙일 수 있습니다.
</details>

---

<details>
<summary><strong>4) Level, World, Map의 차이점과 관계를 설명해주세요.</strong></summary>
<strong>A.</strong> Level은 지형, 오브젝트, 라이트 같은 게임 환경 단위입니다. World는 하나 이상의 Level을 포함해 전체 게임 세션을 관리합니다. Map은 저장된 Level 데이터 파일(.umap)입니다. 즉, Map은 저장 단위, Level은 구성 단위, World는 실행 단위라고 할 수 있습니다.
</details>

---

<details>
<summary><strong>5) GameMode, GameState, PlayerState의 역할과 차이점을 설명해주세요.</strong></summary>
<strong>A.</strong> GameMode는 서버에서 게임 규칙과 흐름을 정의하는 클래스입니다. GameState는 모든 클라이언트와 공유되는 게임 진행 상황을 관리합니다. PlayerState는 각 플레이어별 점수, 팀 정보 같은 상태를 동기화합니다. GameMode는 서버 전용, GameState와 PlayerState는 네트워크 동기화를 담당하는 것이 큰 차이입니다.
</details>

---

### ✅ 기본 시스템

<details>
<summary><strong>1) UE5의 좌표계와 단위 시스템에 대해 설명해주세요.</strong></summary>
<strong>A.</strong> UE5는 기본적으로 좌표계가 X-앞, Y-오른쪽, Z-위 방향입니다. 단위는 1 Unreal Unit = 1 cm이며, 이로 인해 실제 스케일과 물리 계산이 자연스럽게 맞춰집니다. 물리 엔진과도 일관성이 있어 현실감 있는 시뮬레이션이 가능합니다.
</details>

---

<details>
<summary><strong>2) Material과 Material Instance의 차이점과 활용법을 설명해주세요.</strong></summary>
<strong>A.</strong> Material은 셰이더와 속성을 정의한 원본 자산입니다. Material Instance는 원본을 상속받아 파라미터 값만 변경할 수 있어, 동일한 셰이더를 재사용하면서 성능 비용은 줄이고 다양성을 확보할 수 있습니다. 색상, 텍스처 교체 같은 작업에 적합합니다.
</details>

---

<details>
<summary><strong>3) Blueprint의 Event Graph와 Construction Script의 차이점은 무엇인가요?</strong></summary>
<strong>A.</strong> Event Graph는 런타임 시 동작하는 로직을 정의하는 곳입니다. Construction Script는 에디터에서 Actor가 배치되거나 속성이 변경될 때 실행되어 초기화나 반복 배치를 자동화하는 데 사용됩니다. 즉, 전자는 플레이 시점, 후자는 디자인 시점에 실행됩니다.
</details>

---

<details>
<summary><strong>4) Collision과 Overlap의 차이점과 각각의 사용 사례를 설명해주세요.</strong></summary>
<strong>A.</strong> Collision은 물리적으로 충돌이 발생해 반응하는 경우를 의미하고, Overlap은 충돌은 발생하지 않지만 두 영역이 겹쳤을 때 이벤트만 발생합니다. Collision은 벽 충돌이나 물리 반응에, Overlap은 트리거 존이나 아이템 줍기 같은 상황에 사용됩니다.
</details>

---

<details>
<summary><strong>5) UE5의 기본 조명 시스템과 라이트 타입별 특징을 설명해주세요.</strong></summary>
<strong>A.</strong> UE5에는 Directional Light, Point Light, Spot Light, Rect Light가 있습니다. Directional은 태양광처럼 무한한 방향광, Point는 전구처럼 모든 방향으로, Spot은 원뿔 형태로, Rect는 직사각형 영역광을 표현합니다. 여기에 Lumen이 도입되면서 글로벌 일루미네이션과 반사가 실시간으로 가능해졌습니다.
</details>

---

# 🔶 Unreal Engine 기술 질문 – 중급 (주니어 2-3년차, 미들 1-2년차)

---

### ✅ 메모리 & 성능

<details>
<summary><strong>1) UE5의 Garbage Collection 시스템과 UObject의 생명주기를 설명해주세요.</strong></summary>
<strong>A.</strong> Unreal은 Mark-Sweep 기반 GC를 사용하며, UObject는 참조 카운팅이 아닌 리플렉션 기반 참조 추적을 통해 관리됩니다. UObject는 생성 시 루트 세트에 등록되며, 참조가 끊어지고 GC 사이클이 돌면 메모리에서 해제됩니다. 다만 Native C++ 객체는 직접 관리해야 하며, GC와 혼동하지 않도록 주의해야 합니다.
</details>

---

<details>
<summary><strong>2) UPROPERTY 매크로의 주요 지정자들과 메모리 관리에 미치는 영향을 설명해주세요.</strong></summary>
<strong>A.</strong> UPROPERTY는 GC가 UObject 참조를 추적할 수 있도록 메타데이터를 부여합니다. 예를 들어 VisibleAnywhere, EditAnywhere, BlueprintReadWrite는 에디터/블루프린트 접근성을 제어합니다. 또한 UPROPERTY를 선언하지 않으면 GC가 추적하지 못해 메모리 누수가 발생할 수 있습니다.
</details>

---

<details>
<summary><strong>3) Smart Pointer (TSharedPtr, TWeakPtr, TUniquePtr)의 사용법과 차이점을 설명해주세요.</strong></summary>
<strong>A.</strong> TSharedPtr은 참조 카운트를 증가시켜 소유권을 공유하는 스마트 포인터입니다. TWeakPtr은 소유권을 갖지 않고 SharedPtr 유효성만 확인할 때 사용합니다. TUniquePtr은 단일 소유권만 허용하며 이동만 가능하고 복사는 불가능합니다. Unreal에서는 UObject는 GC가 관리하므로 주로 비-UObject 타입에서 스마트 포인터를 활용합니다.
</details>

---

<details>
<summary><strong>4) Level Streaming과 World Composition을 활용한 대용량 월드 관리 방법을 설명해주세요.</strong></summary>
<strong>A.</strong> Level Streaming은 특정 Level을 필요할 때만 로드하거나 언로드하여 메모리를 절약하는 방식입니다. World Composition은 대규모 오픈월드에서 여러 Level을 계층적으로 관리하는 시스템으로, LOD 전환과 스트리밍을 자동화합니다. 이를 통해 퍼포먼스를 유지하면서 대규모 맵을 구현할 수 있습니다.
</details>

---

<details>
<summary><strong>5) UE5의 프로파일링 도구들(Unreal Insights, Stat 명령어)의 활용법을 설명해주세요.</strong></summary>
<strong>A.</strong> Stat 명령어는 CPU, GPU, Draw Call, 메모리, 네트워크 같은 지표를 실시간으로 확인할 수 있습니다. Unreal Insights는 이벤트 기반 추적 툴로 프레임 단위 성능 병목을 시각적으로 분석할 수 있습니다. 두 툴을 함께 사용하면 최적화 대상을 구체적으로 찾아낼 수 있습니다.
</details>

---

### ✅ 네트워킹 & 멀티플레이

<details>
<summary><strong>1) UE5의 Replication 시스템과 네트워크 속성 동기화 방법을 설명해주세요.</strong></summary>
<strong>A.</strong> Replication은 서버 권위 기반으로 속성과 함수 호출을 클라이언트와 동기화하는 기능입니다. UPROPERTY에 Replicated 키워드를 붙여 변수 상태를 자동 동기화할 수 있고, OnRep 함수를 통해 동기화 시 추가 로직을 실행할 수 있습니다. 이를 통해 멀티플레이어 환경에서 상태 일관성을 유지합니다.
</details>

---

<details>
<summary><strong>2) RPC (Remote Procedure Call)의 종류와 각각의 사용 시나리오를 설명해주세요.</strong></summary>
<strong>A.</strong> RPC는 NetMulticast, Client, Server 세 가지가 있습니다. Server RPC는 클라이언트가 서버에 요청할 때, Client RPC는 서버가 특정 클라이언트에만 명령할 때, NetMulticast는 서버가 모든 클라이언트에 동시에 명령할 때 사용됩니다. 각각 상황에 맞게 네트워크 트래픽을 효율적으로 제어해야 합니다.
</details>

---

<details>
<summary><strong>3) Client Prediction과 Server Reconciliation의 개념과 구현 방법을 설명해주세요.</strong></summary>
<strong>A.</strong> Client Prediction은 입력에 대한 결과를 클라이언트에서 즉시 반영해 지연을 줄이는 방식입니다. 이후 서버가 authoritative 결과를 보내면, Server Reconciliation을 통해 차이를 보정합니다. 이는 이동, 슈팅 같은 빠른 액션에서 체감 반응성을 높이면서 동기화를 유지하는 핵심 기법입니다.
</details>

---

<details>
<summary><strong>4) 네트워크 관련성(Network Relevancy)과 컬링 시스템을 설명해주세요.</strong></summary>
<strong>A.</strong> Network Relevancy는 클라이언트에 꼭 필요한 정보만 전송하도록 범위를 제한하는 시스템입니다. 플레이어 시야 밖의 Actor는 업데이트를 생략해 트래픽을 줄입니다. 이와 함께 NetCullDistanceSquared 설정으로 네트워크 효율을 높일 수 있습니다.
</details>

---

<details>
<summary><strong>5) Dedicated Server와 Listen Server의 차이점과 각각의 장단점을 설명해주세요.</strong></summary>
<strong>A.</strong> Dedicated Server는 서버 전용 실행으로, 공정성과 안정성이 높고 대규모 멀티플레이에 적합합니다. Listen Server는 호스트 플레이어가 동시에 서버 역할을 하므로 설정이 간단하지만, 호스트가 나가면 세션이 종료되고 공정성이 떨어집니다. 따라서 개발 환경이나 소규모 테스트에서는 Listen, 상용 서비스에서는 Dedicated이 주로 사용됩니다.
</details>

---

### ✅ 애니메이션 & 블루프린트

<details>
<summary><strong>1) Animation Blueprint의 State Machine과 Blend Space 활용법을 설명해주세요.</strong></summary>
<strong>A.</strong> Animation Blueprint의 State Machine은 Idle, Walk, Run 같은 상태 전환을 정의합니다. Blend Space는 속도나 방향 파라미터를 기반으로 여러 애니메이션을 보간합니다. 둘을 함께 사용하면 부드러운 이동 애니메이션 전환을 구현할 수 있습니다.
</details>

---

<details>
<summary><strong>2) Anim Notify와 Anim Montage를 활용한 애니메이션 이벤트 처리를 설명해주세요.</strong></summary>
<strong>A.</strong> Anim Notify는 애니메이션 특정 프레임에 이벤트를 삽입하는 방식으로, 발소리, 이펙트 재생 같은 즉각적 이벤트에 적합합니다. Anim Montage는 여러 애니메이션을 조합하고 구간을 제어할 수 있어 콤보 공격이나 컷씬 같은 시퀀스에 사용됩니다.
</details>

---

<details>
<summary><strong>3) Blueprint Interface와 Blueprint Communication 방법들을 설명해주세요.</strong></summary>
<strong>A.</strong> Blueprint Interface는 서로 다른 Blueprint 간에 공통 함수를 정의해 통신을 단순화합니다. 직접 참조 대신 메시지 기반 호출을 통해 결합도를 낮출 수 있습니다. 또한 Event Dispatcher나 Casting도 Blueprint 간 통신 방법으로 활용됩니다.
</details>

---

<details>
<summary><strong>4) Delegate와 Event Dispatcher의 차이점과 활용법을 설명해주세요.</strong></summary>
<strong>A.</strong> Delegate는 C++에서 함수 포인터 개념을 확장한 콜백 메커니즘이고, Event Dispatcher는 Blueprint에서 Delegate를 노출해 이벤트를 바인딩할 수 있는 기능입니다. Delegate는 코드 레벨에서, Event Dispatcher는 에디터 친화적으로 사용된다는 차이가 있습니다.
</details>

---

<details>
<summary><strong>5) Widget Blueprint와 UMG를 활용한 UI 시스템 구축 방법을 설명해주세요.</strong></summary>
<strong>A.</strong> UMG는 Unreal의 UI 프레임워크로, Widget Blueprint를 통해 UI 요소를 시각적으로 배치하고 스크립트로 제어할 수 있습니다. 이를 통해 버튼 클릭 이벤트 처리, 동적 텍스트 변경, HUD 구현이 가능합니다. 복잡한 UI는 C++ 기반 Slate와 연동해 확장할 수도 있습니다.
</details>

---

---
layout: post
title: "기술면접 대비 CS 공부 - Unreal (고급)"
date: 2025-09-29 15:00:00 +0900
categories: [Tech Interview, Unreal, Study Plan]
tags: [unreal, c++, gas, optimization, subsystem, lumen, nanite]
slug: cs-study-unreal-advanced
---

# 🔶 Unreal Engine 기술 질문 – 고급 (미들 3년차+, 시니어)

---

### ✅ GAS & 게임플레이 프레임워크

<details>
<summary><strong>1) Gameplay Ability System(GAS)의 주요 구성 요소들을 설명해주세요.</strong></summary>
<strong>A.</strong> GAS는 Ability, GameplayEffect, AttributeSet, GameplayTag 네 가지 핵심 요소로 구성됩니다. Ability는 실행 가능한 행동(스킬), GameplayEffect는 버프/디버프 같은 상태 변화, AttributeSet은 체력, 마나 같은 수치 집합, GameplayTag는 효과 식별과 조건 분기를 담당합니다. 이를 통해 대규모 멀티플레이 환경에서 일관성 있고 확장 가능한 전투 시스템을 구현할 수 있습니다.
</details>

---

<details>
<summary><strong>2) GameplayEffect의 Duration 타입별 차이점과 활용법을 설명해주세요.</strong></summary>
<strong>A.</strong> GameplayEffect는 Instant, Duration, Infinite 세 가지 유형이 있습니다. Instant는 즉시 효과(예: 즉시 피해), Duration은 일정 시간 동안 유지(예: 10초간 공격력 증가), Infinite는 무한 지속(예: 장비 버프)입니다. 필요에 따라 조합해 다양한 효과를 표현할 수 있습니다.
</details>

---

<details>
<summary><strong>3) GameplayTag 시스템의 장점과 활용 사례를 설명해주세요.</strong></summary>
<strong>A.</strong> GameplayTag는 문자열 대신 계층 구조 태그를 사용해 조건 분기를 단순화합니다. 예를 들어 "Status.Stun" 태그를 가진 캐릭터는 스턴 상태를 쉽게 판별할 수 있습니다. 중복 코드 없이 직관적으로 상태를 관리할 수 있어 대규모 프로젝트에서 유지보수성이 크게 향상됩니다.
</details>

---

<details>
<summary><strong>4) AttributeSet에서 Attribute 값 변경 시 실행되는 함수들의 순서를 설명해주세요.</strong></summary>
<strong>A.</strong> Attribute가 변경되면 PreAttributeChange → GameplayEffectExecution → PostGameplayEffectExecute 순으로 실행됩니다. PreAttributeChange는 값 변경 전 클램프 같은 검증, Execution은 실제 수치 연산, PostGameplayEffectExecute는 후처리(이펙트, 애니메이션 등)를 담당합니다. 이 순서를 이해해야 올바른 로직을 작성할 수 있습니다.
</details>

---

<details>
<summary><strong>5) GAS에서 네트워크 동기화와 Client Prediction 구현 방법을 설명해주세요.</strong></summary>
<strong>A.</strong> GAS는 Ability와 Effect가 서버 권위로 처리되며, 클라이언트는 Prediction Key를 통해 예상 결과를 미리 적용합니다. 서버는 authoritative 결과를 보내고 클라이언트는 이를 Reconciliation하여 차이를 보정합니다. 이를 통해 액션 게임에서 빠른 반응성과 동기화 안정성을 모두 확보할 수 있습니다.
</details>

---

### ✅ 고급 시스템 & 아키텍처

<details>
<summary><strong>1) Subsystem의 종류별 차이점과 각각의 사용 사례를 설명해주세요.</strong></summary>
<strong>A.</strong> Unreal의 Subsystem은 GameInstanceSubsystem, EngineSubsystem, WorldSubsystem 등으로 구분됩니다. GameInstanceSubsystem은 게임 전체에서 유지되는 매니저, EngineSubsystem은 엔진 전역 서비스, WorldSubsystem은 맵 단위 로직을 담당합니다. 이 구조는 전역 싱글톤보다 의존성 관리와 테스트 용이성이 뛰어납니다.
</details>

---

<details>
<summary><strong>2) TaskGraph와 멀티스레딩을 활용한 병렬 처리 구현 방법을 설명해주세요.</strong></summary>
<strong>A.</strong> TaskGraph는 Unreal의 비동기 작업 시스템으로, 작은 작업 단위를 스레드 풀에 분산 실행합니다. 예를 들어 AI 탐색, 파티클 계산 같은 독립적인 작업을 병렬 처리할 수 있습니다. 이를 활용하면 메인 게임 스레드의 부하를 줄이고 성능을 크게 개선할 수 있습니다.
</details>

---

<details>
<summary><strong>3) Slate UI 프레임워크와 UMG의 차이점을 설명해주세요.</strong></summary>
<strong>A.</strong> Slate는 C++ 기반의 로우레벨 UI 프레임워크로, 고성능·커스터마이즈가 가능하지만 구현 난이도가 높습니다. UMG는 Slate 위에 만들어진 비주얼 UI 툴로, 디자이너 친화적이고 빠른 제작이 가능합니다. 복잡한 시스템은 Slate, 일반적인 UI는 UMG를 사용합니다.
</details>

---

<details>
<summary><strong>4) UE5의 렌더링 파이프라인과 커스텀 셰이더 작성 방법을 설명해주세요.</strong></summary>
<strong>A.</strong> UE5 렌더링 파이프라인은 Geometry → Rasterization → Pixel 단계로, Lumen과 Nanite 같은 신기술이 통합되었습니다. 커스텀 셰이더는 HLSL 기반으로 작성해 Material Editor나 플러그인을 통해 적용할 수 있습니다. 이를 통해 엔진 기본 기능을 확장할 수 있습니다.
</details>

---

<details>
<summary><strong>5) Blueprint를 C++로 변환할 때 고려해야 할 사항들을 설명해주세요.</strong></summary>
<strong>A.</strong> Blueprint를 C++로 변환할 때는 성능 병목, 반복 로직, 네트워크 처리 같은 영역이 우선 대상입니다. 다만 디자이너가 자주 변경하는 로직은 Blueprint로 남겨 협업 효율성을 유지해야 합니다. 즉, "핵심 로직은 C++, 반복 콘텐츠는 BP" 원칙이 이상적입니다.
</details>

---

### ✅ 최적화 & 프로덕션

<details>
<summary><strong>1) Lumen과 Nanite의 동작 원리와 성능 최적화 방법을 설명해주세요.</strong></summary>
<strong>A.</strong> Lumen은 실시간 글로벌 일루미네이션과 반사를 제공하는 시스템으로, 기존 라이트 베이크를 대체합니다. Nanite는 가상화된 지오메트리 시스템으로, 수십억 폴리곤을 효율적으로 스트리밍합니다. 성능 최적화를 위해 Lumen에서는 품질 설정과 리플렉션 범위를 조정하고, Nanite에서는 디테일 레벨과 화면 크기 비율을 조정합니다.
</details>

---

<details>
<summary><strong>2) LOD (Level of Detail) 시스템과 자동 생성 설정 방법을 설명해주세요.</strong></summary>
<strong>A.</strong> LOD는 거리에 따라 간단한 모델로 교체해 성능을 향상시키는 기법입니다. UE5는 자동 LOD 생성 기능을 제공해 아티스트의 작업 부담을 줄입니다. 이를 통해 대규모 환경에서도 성능과 시각 품질을 균형 있게 유지할 수 있습니다.
</details>

---

<details>
<summary><strong>3) Occlusion Culling과 Frustum Culling의 최적화 방법을 설명해주세요.</strong></summary>
<strong>A.</strong> Frustum Culling은 카메라 시야 밖의 객체를 렌더링하지 않고, Occlusion Culling은 가려진 객체까지 제거합니다. UE5는 하드웨어 Occlusion 쿼리와 HZB(Hierarchical Z-Buffer)를 활용합니다. 이를 통해 불필요한 드로우콜을 줄여 성능을 크게 개선할 수 있습니다.
</details>

---

<details>
<summary><strong>4) UE5의 빌드 시스템과 쿠킹 프로세스 최적화 방법을 설명해주세요.</strong></summary>
<strong>A.</strong> 쿠킹은 콘텐츠를 플랫폼별로 패키징하는 과정입니다. 최적화를 위해 Incremental Cooking, Pak 파일 압축 최적화, 불필요한 에셋 제외 등을 활용합니다. 빌드 파이프라인을 자동화하면 배포 속도와 안정성이 향상됩니다.
</details>

---

<details>
<summary><strong>5) 플러그인 개발과 모듈 시스템을 활용한 코드 구조화 방법을 설명해주세요.</strong></summary>
<strong>A.</strong> 플러그인은 기능 단위로 코드와 에셋을 분리해 재사용성과 모듈화를 높입니다. 모듈 시스템은 Runtime, Editor, Developer 등으로 구분되어 빌드 시 필요한 기능만 로드할 수 있습니다. 이를 통해 대규모 프로젝트에서 유지보수와 확장이 쉬워집니다.
</details>

---

