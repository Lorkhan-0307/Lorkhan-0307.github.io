---
layout: post
title:  "UE Study 3.Udemy 강의 2일차"
date:   2025-02-17 18:00:00 +0900
categories: GameProgramming
tags: [C++, UE5]
---


# 언리얼의 구조(FrameWork)

Unity와 비교해보자.
Unity는 Scene이 있고, MonoBehavior가 있다.
여기에서 Prefab이나 GameObject, Manager 등등을 생성한다.

그렇기에 MonoBehavior가 기본이라고 할 수 있다.


## 그렇다면 UE는?

Unity는 이에 따라 상대적으로 자유롭고, Scene이라는 도화지 위에 Monobehavior로 자유롭게 그리면 된다.

하지만 UE에서는 Level(Map은 조금 더 큰 의미. Map 안에는 Level, SubLevel 등이 존재하며, 여러개의 Level이 존재하는 Map도 존재할 수 있다)이라는 개념이 들어간다.
이에는 pawn, Actor, UObject 등의 고정된 클래스를 상속받은 클래스를 만들어야 한다.
UI면 User-Widget을 상속받아야 한다.

즉, 기본적인 Framework가 존재하며, 이 규칙에 맞는 작업을 해야 한다.


UE를 조금 더 자세하게 알아보자.

Unreal에서 메인된 흐름을 결정하는 것은 GameInstance이다(Unity로 비교하면 GameManager).

이 안에는 SubSystem이라는 것이 들어간다. 이것이 Unity의 Singleton과 유사하다고 생각하면 된다. 

이 때의 SubSystem은 종류가 여러개이며, 각 상황에 맞추어 사용해야 한다. 

Gameinstance subsystem이 있다면, 이는 게임이 시작할 때부터 종료될 때까지 지속적으로 유지되는 것이다.

혹은, 전투와 관련된 데이터를 Singleton으로 만들고 싶다고 가정하자.(명칭 : Battle Subsystem)

하지만 이 기능은 로비 등의 위치에서는 필요가 없으므로, GameInstance Subsystem이 아닌, Level Subsystem을 사용한다. 그러면 전투 레벨에서만 활용하도록 제작할 수 있다.

* #pragma once 와는 다르다.
pragma once의 경우에는 컴파일이 중복되지 않도록 방지하는 역할이 주된 역할이기 때문에, 이는 완전 다른 행위이다. *

## UWorld란?

UWorld의 개념 : Level이 생성되면 World가 생성이 된다 (일종의 공간).

Unity에서는 하나가 생기지만, Unreal에서는 3개가 생긴다.
이 World는 Unreal에서 각각의 역할이 분할된다.
Actor, 기믹, 오브젝트, Lighting 등이 이 맵 안에서 생길 수 있다. 이 안에서, 이 데이터가 있는 World가 있고, 두번째 월드는 Rendering World이다. 데이터를 가지고 화면에 그려주는 역할을 한다.(Unreal을 킬때 NoRendering 옵션을 사용하면 이 Rendering World를 생성하지 않는다. 이는 주로 데디 서버에서 활용된다.)
세번째 월드는 Physics World이다. 물리적 연산, Collider를 처리한다. 

이러한 분할적 월드의 생성은 효율적인 부분도 있지만, 데디 서버를 위함이 더 크다. 데디 서버에서는 1, 3번 월드만 생성해도 되므로 상대적으로 부담이 감소하기 때문이다.

1, 2, 3번 월드는 한 프레임마다 서로의 대한 syncronization 작업을 거친다.


## Actor 개념

World 안에는 다양한 Actor가 존재하고, 이 Actor 안에는 다양한 ActorComponent가 존재한다.
이 Actor마다 Tick이 돈다.

Unity에서 Object가 비활성화되면 Update가 멈추지만, UE에서는 Tick을 Actor가 비활성화될때, 이를 멈출지 말지 관리할 수 있다.

Tick Manager(확인 필요)가 존재하여, 이것들이 Actor가 생성될때 Actor의 Tick을 TickManager가 호출한다. Actor는 Tick 함수를 정의 및 등록만 한다.

Actor Component도 내부에 Tick이 있지만, 이름이 살짝 다르다.(찾아보자!)


## 그래서 Unreal의 Framework는 어떻게 구성되어 있는가?


먼저 GI(Game Instance)에서 게임의 Rule을 정립한다.
그 다음에, GI에 여러개의 UWorld가 존재할 것이며, 이 내부에는 다수의 Actor가 존재할 것이며, 또한 UObject도 존재할 것이다. playerController가 붙을 수 있는 Pawn이 있다.

UI는 UserWidget을 통해서 화면에 붙이며 이를 통해 HUD를 구성할 수 있을 것이다.

이것들이 Unreal 파일 생성하면 생성되는 Unreal의 기본 구조이다.


## PlayerState와 PlayerController

PlayerController는 Input을 넣고, 이에 맞는 Animation과 Effect를 집어넣는다.

PlayerState에는 GameData, State 등을 집어넣는다.
이들은 보통 Actor에 붙어있다.

getPlayerState 함수가 있다. 이에 Default 0이 들어간다.

데디서버가 애초에 설계되어 있기 때문에, 데디서버에서 1을 하면, 다른 유저의 정보를 가져올 수 있기 때문에, 유저의 정보는 0번을 기준으로 가져온다.


## 나나이트

이것이 유니티와의 가장 큰 차이다.

이를 이해하기 위해서는 렌더링에 대해 조금 이해해야 한다.

Rendering Order에 대해서 이야기햇다.

Unity 기분에서
3000 -> Opaque
4000 -> Transparency
이다.

이때, 가장 멀리 있는 Opaque를 그리고, Transparency를 그린다. 그 이유는, Transparency는 반투명이므로 겹친 부분은 다시그리게 된다. 근데 만약, 반투명한 것이 또 그 위에 있다면, 겹친 영역을 또 다시그려야 한다.

Redrawing, 중복 발생이 계속 발생한다. 그렇기 때문에, 변동이 적은 layer를 먼저 그리기 위해서 Opaque Layer를 먼저 그리게 된다.

그래서 라이팅에서 Transparency가 자원 소비량이 많다.

이에 나나이트와 연관성이 생긴다.


### 그래서 나나이트란?

Redrawing 이슈를 기억하고, 이에 추가적으로 프러스텀 컬링(절두체 컬링 / 내가 알고있는 오브젝트 컬링의 전 단계)을 생각해보자.

절두체 -> 카메라에서 나오는 화면의 범위 중 그 머리 부분을 잘라서 절두체라고 한다.

이것의 최종 진화체가 나나이트 라는 기술으로 생각하면 된다.

나나이트는 어떠한 기술이냐면...

카메라가 있고, 그 아래 Terrian 및 다양한 오브젝트가 있다고 하자.

이때, unity 기준에서는 화면상 겹쳐있는 다양한 오브젝트 전부를 그려야 하기 때문에, 멀리 있는 오브젝트의 경우에는 LOD(Level of Detail)를 사용한다.

하지만 나나이트는 이 LOD를 사용하지 **않는다!**

나나이트는 소프트웨어 렌더링이라고 하여, 화면 상에 존재하는 Mesh를 하드웨어적으로 전부 삼각형으로 조각을 낸다. 이후, 조각난 범위에서 카메라에 들어온 영역들만 그리는 것이다. 

그래픽 카드의 비용이 조금 더 들어가지만, LOD와 Culling의 비용에 비해 훨씬 적은 비용이 소비되는 것이다.

그래픽카드 캐싱을 통해서 이 조각조각을 저장하기 때문에 메모리가 아닌 GPU의 램의 크기가 더 많이 필요해진다.

화면 전환의 비율이 적으면 특히나 이 나나이트의 효율이 증가하고, 화면 전환의 비율이 높은 FPS등의 게임의 경우에는 화면마다 이 조각조각을 내는 비용이 증가하므로 효율이 감소한다.


## '루멘'이란 무엇인가?

먼저, Lighting에 대해 이해해보자.

### 빛의 표현 방법은?

고전 3D 게임에서는 Directional Light 이 존재할 때, 빛이 닿는 부분은 밝고, 아닌 부분은 어둡고, 뒤에 그림자가 생긴다.

하지만 현실에서는 차폐광(Ambient Occlusion)이 생긴다. 물체가 빛을 받으면, 해당 빛을 은은하게 반사한다. 그렇기에, 물체의 빛을 받지 않는 부분도 그라데이션처럼 밝기의 차이가 발생한다.

또한, Specular라는 개념이 있다.
반사시에, 빛과 수직인 부분이 존재하는데, 이 부분은 그냥 하얗게 빛나는 점처럼 보인다. 이를 스페큘러(Specular)라고 한다.

### 그렇다면 이에 따라 추가되는 것들은?

Metallic, Smoothness 라고 Unity에 있었다.

Metallic(금속성)은 다시말해 주변을 더 많이 반사하는 값을 이야기한다.

이들은 빛을 반사하는 값이나 표현에 대해 이야기하는 것을 말한다.

이를 통합하여 PBR(Physical Based Rendering) 이라고 한다.

실제 현실에서의 빛과 동일한 방식으로 계산하는 렌더링을 이야기한다.

Unity에서는 URP, HDRP가 있었고, 이중 HDRP는 PC에, URP는 모바일에 최적화가 되어있어 이에 따라 무게가 다르다.


### 빛을 연산하는 방법

게임개발 101

forward, deforward 연산

빛이 증가하면 할수록, **빛의 갯수 * 물체의 갯수** 만큼의 연산이 발생한다. 이것이 forward 렌더링.

deforward 연산은 이 모든 것을 1번 진행한다.

일단 모니터에 물체를 먼저 렌더링 한 뒤에, 빛이 비춰지는 영역들을 연산하고, 제일 마지막에 이 연산을 **한번** 진행하는 것으로 끝난다.

하지만, deforward는 빛의 갯수의 제한된 값이 존재하며, 모바일에서는 forward 연산만 지원하고 있다.

deforward를 최적화한 것이 메가라이트이다.


## '메가라이트'란 무엇인가?



## 추가 질문


Asset Store 등에서 UE importer를 사용해서 추출하도록 하자!





