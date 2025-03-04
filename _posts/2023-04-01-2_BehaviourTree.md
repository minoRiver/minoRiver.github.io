---
layout: single
title:  "[Unity로 행동 트리 구현]01. 행동 트리란?"
author_profile: false
tag : [Unity, 개발]
categories : Unity
typora-root-url: ../
---

> ## 개요

오늘은 팀파이트 매니저의 AI 기능을 구현하기 위해 행동 트리에 대해 알아보고 정리해봤습니다.

AI 로직을 어떤 것을 사용할지 고민하다가 행동 트리를 지금 구현해두면 앞으로 게임 개발 시 유용하게 사용할 것 같아서 공부하고 그 내용을 바탕으로 행동 트리에 대해 정리해봤습니다.

그리고 AI를 공부하려고 많은 자료를 찾으면서 가장 많이 봤던 FSM과 BT는 어떤 차이에 대해서도 간략하게 정리해봤습니다.



행동 트리는 <a href="https://www.youtube.com/watch?v=nKpM98I7PeM&list=PLyBYG1JGBcd009lc1ZfX9ZN5oVUW7AFVy&index=11&ab_channel=TheKiwiCoder">Unity Create Behaviour Tree</a> 와 <a href="https://docs.unrealengine.com/5.0/ko/behavior-trees-in-unreal-engine/">Unreal의 행동 트리</a>를 참고했습니다. 이 2개만 참고해도 잘 구성해놔서 구현하는데 큰 어려움은 없었던 것 같습니다.



> ## 행동 트리(Behaviour Tree, BT)

캐릭터의 행동 로직을 트리 구조로 만들어 트리 안에서 실행할 행동을 결정하는 하나의 알고리즘입니다.

행동 트리는 노드(Node) 단위로 구성되는데 노드에는 여려 가지 종류가 있습니다.

행동 트리를 사용하면 캐릭터의 AI를 구현할 수 있습니다.

행동 트리는 블랙보드의 데이터를 가지고 실행할 행동을 결정합니다.

![행동 트리 예시](/images/2023-04-01-second/행동 트리 예시.png)



> ## 블랙보드(Blackboard)

AI에서 사용하는 데이터들을 저장하는 집합소입니다.



> ## FSM과 BT의 차이

구현해보면서 느끼는 가장 차이점은 구성 요소 단위의 차이와 연결된 구조의 차이가 가장 크다고 느꼇습니다.

<br>



#### 1. 구성 요소 단위 차이

FSM의 경우 상태 단위로 구현하는 것이 일반적이고 BT의 경우 행동(혹은 조건) 단위로 구현하기 때문에 이 것 때문에 BT를 쓸 때 코드 재사용하기 좋다고 생각했습니다.

예를 들어 근처를 방황하다가 적을 발견 시 추적해 공격하는 몬스터가 있다고 가정했을 때(장애물X)

FSM으로 구현하게 된다면 Patrol, Chase, Attack 3개의 상태를 만들고 각각의 상태에서 해야할 행동을 정의할 것입니다.



이 것을 BT로 구현하는 경우 Move, LookTarget, ComputeNextPatrolPoint, Attack 행동으로 나눌 수 있습니다.

Patrol의 경우 ComputeNextPatrolPoint로 다음 이동할 지점을 구한 뒤 Move(방향)을 실행시키면 정해진 방향으로 이동할 것이고, Chase의 경우에도 LookTarget으로 타겟을 바라본 뒤 Move(Forward)을 실행시키면 타겟을 향해 추적할 것입니다. Attack의 경우 LookTarget을로 적을 바라본 뒤 Attack()을 실행시켜주면 적을 바라보면서 그 방향으로 공격하게 됩니다.



이런 식으로 행동 단위로 구성하다보니 필요하면 재사용할 수 있어 상태가 복잡해지면 더 편하게 구현할 수 있겠다는 생각이 들었습니다.

<br>

#### 2. 연결된 구조의 차이

FSM은 Transition으로 상태끼리 연결되며 BT는 트리 구조이기 때문에 부모 자식 관계로 연결됩니다.

이 차이로 인해 복잡한 행동 패턴을 정의해야 할 때 가독성적인 측면에서 차이가 발생한다고 생각합니다.

<br>

- 아름답지만 어지럽긴 하다.

![FSM 어지러운 연결](/images/2023-04-01-second/FSM 어지러운 연결-1680333380981-15.jpg)



- 좀 더 실행 흐름이 보인다.

![BT 구성도](/images/2023-04-01-second/BT 구성도.png)
