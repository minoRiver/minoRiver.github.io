---
layout: single
title:  "[TFM 모작]05. 매니저 생성 순서 조정 및 참조 연결"
author_profile: false
tag : [Unity, 게임개발]
categories : 게임개발
typora-root-url: ../
---

> #### 개요

각 매니저들에서 다른 매니저의 데이터를 사용하는 경우가 많습니다(특히 데이터 테이블 매니저를 필요로 하는 매니적 가 많음), 그렇다보니 각 매니저의 Awake 같은 이벤트 함수의 호출 순서도 중요해져서 GameManager 클래스에서 각 매니저 클래스를 생성하고 참조를 연결해주는 식으로 로직을 수정했습니다.



> #### 코드

코드는 이런 식으로 구성했습니다. 현재까지 매니저 클래스의 생성 순위는

1. 데이터 테이블 매니저(다른 매니저 클래스에서 정보를 가져가야 하기 때문에 초기화 작업이 끝나있는 상태여야 한다)
2. 이펙트 매니저(챔피언 매니저에서 필요로 함)
3. 그 외 나머지

![매니저 클래스 생성 및 참조연결 코드](/images/2023-04-06-first/매니저 클래스 생성 및 참조연결 코드.png)



> #### 결과

이렇게 매니저 클래스의 이벤트 함수 호출 순서를 조정하고 참조도 연결해줘서 싱글톤을 사용하지 않아도 각각의 매니저에서 다른 매니저를 참조할 수 있게 됐습니다.

최대한 싱글톤을 쓰지 않고 어떻게 해결할 수 있을지 고민하다보니 이런 식으로 쓴 것 같습니다.

이제 생성 순서로 인해 에러가 날 확률이 적어져서 작업에만 집중할 수 있을 것 같습니다.
