# CQRS

### 목차

* CQS
* CQRS
* SSOT(Single Source Of Truth)
* Materialized View
* OLTP(Online Transaction Processing)
* OLAP(Online Analytical Processing)
* Event Sourcing

### 강의 정리



\| **CQS (Command Query Separation)**

CQS는 시스템 설계에서 명령과 질의를 분리하는 원칙입니다. '명령'은 데이터를 변경하지만, 아무것도 반환하지 않으며, '질의'는 데이터를 반환하지만 변경하지 않습니다. 이 원칙은 코드의 명확성과 유지 보수성을 향상시킵니다.

Command와 Query를 분리하여 상태를 변경하는 메서드인지 아닌지에 따라 구분 한다.

도메인과 비즈니스 로직은 대부분 Command (C,U,D)

Query는 순수함수 (R)



\| **CQRS (Command Query Responsibility Segregation)**

CQRS는 CQS의 확장으로, 시스템을 명령을 처리하는 부분과 질의를 처리하는 부분으로 분리합니다. 이 접근 방식은 더 복잡한 시스템에서 유연성과 성능을 향상시킬 수 있습니다.

대규모 서비스에서는데   Command(SQL), Query(NoSQL)의  데이터베이스를 분리하는 곳도 있다고 한다.



\| **CQRS (Command Query Responsibility Segregation)**

CQRS는 CQS의 확장으로, 시스템을 명령을 처리하는 부분과 질의를 처리하는 부분으로 분리합니다. 이 접근 방식은 더 복잡한 시스템에서 유연성과 성능을 향상시킬 수 있습니다.

대규모 서비스에서는데   Command(SQL), Query(NoSQL)의  데이터베이스를 분리하는 곳도 있다고 한다.



**❗이렇게 Write Model과 Read Model을 구분하고, 애플리케이션의 상태가 바뀔 때마다(Command가 실행된 후 거의 바로) Read Model을 업데이트한다면 Command와 Query가 명확히 구분되고 트래픽 등이 비대칭적으로 발생하는 대부분의 서비스에서 도움이 된다.**



Write Model은 SSOT(Single Source Of Truth)일 때 유용하지만, Read Model은 상황에 맞게 적절한 기술을 통해 여러 개가 존재해도 된다. Read Model과 관련된 몇 가지 개념을 잠깐 짚고 넘어가자.

\| **SSOT (Single Source of Truth)**: SSOT는 모든 정보의 단일, 신뢰할 수 있는 출처를 의미합니다. 시스템 내의 데이터 중복과 불일치를 방지하기 위해 사용됩니다.

\| **Materialized View**: 물리적으로 저장된 데이터베이스의 뷰입니다. 이는 복잡한 쿼리의 결과를 저장하여, 데이터 조회 시 성능을 향상시킵니다.

\| **OLTP (Online Transaction Processing)**: OLTP는 온라인 거래 처리를 의미하며, 데이터베이스에서 실시간으로 거래를 처리하는 시스템입니다. 이 시스템은 빠른 성능과 데이터 무결성을 제공합니다.

\| **OLAP (Online Analytical Processing)**: OLAP는 복잡한 쿼리와 분석을 위한 데이터베이스 처리 방식입니다. 대규모의 데이터를 다루며, 비즈니스 인텔리전스와 데이터 마이닝에 자주 사용됩니다.

\| **Event Sourcing**: 이벤트 소싱은 시스템의 상태 변경을 이벤트(사건)로 저장하는 방식입니다. 이를 통해 시스템의 상태를 어느 시점으로도 되돌릴 수 있으며, 복잡한 시스템에서의 데이터 일관성과 이력 관리에 유용합니다.

상태의 변화를 이벤트의 연속으로 다루는 방식. 이벤트 자체는 과거형으로 표현하고, 과거의 이력이 모여서 현재의 상태를 계산할 수 있다.

예를 들면 다음과 같은 이벤트가 쌓였다면…

1. 장바구니에 상품 #1을 3개 담았다.
2. 장바구니에 상품 #2를 1개 담았다.
3. 장바구니에서 상품 #1을 1개 뺐다.

현재 상태는 다음과 같다: 장바구니에 상품 #1이 2개, 상품 #2가 1개 담겨 있다.

현재 상태를 빠르게 조회하거나 데이터를 분석하려면 Event Sourcing만으론 너무 어렵다. 따라서 Event Sourcing은 대부분 CQRS와 함께 사용된다.
