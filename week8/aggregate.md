# Aggregate

### 목차

* 집합체 (Aggregate)
* 불변식
* 트랜잭션적 일관성과 결과적 일관성

### 강의 정리

\| ORM

* 객체와 관계형 데이터베이스 간을 매핑하는 기술, 도구를 의미한다. 객체 생성, 상태 감지, 생명주기 관리 기능을 포함한다.&#x20;
* ORM 도구를 통해 SQL문을 자동으로 생성하여 다양한 DBMS에 유연하게 대응하기 때문에 비즈니스 로직에 집중하고 유지보수 하기 좋다.&#x20;
* 결과적으로, 무기력한 도메인 객체가 아닌 객체지향 원칙을 따르는 도메인 객체를 영속화할 수 있게 되었다.

\| JPA

Jakarta Persistence API는 Java에서 사용하는 ORM 표준으로 인터페이스이다.

Hibernate, EclipseLink와 같은 구현체를 통해 jpa를 구현한다. 현재는 Hibernate를 가장 널리 사용한다.

\| Jakarta EE

자바EE는 상업용 플랫폼의 한계와 스프링 프레임워크와 같은 오픈소스 SW의 발전으로 빠르게 변화하는 기술 트렌드를 반영하지 못해 인기가 시들해지며 비영리 단체인 이클립스 재단에 자바 EE 프로젝트를 이관하면서 오픈소스 기반의 Jakarta EE로 변경되었다.

자카르타EE의 핵심 목표는 ‘클라우드 네이티브 환경을 위한 엔터프라이즈 자바 기술’로 마이크로서비스, 컨테이너 등의 최신 기술 트렌드를 반영하고자 하는 것이 목표이다'

\| Entity

엔티티는 기본적으로  테이블에 대응되는 클래스이며 실세계 객체를 모델링한 것을 의미하며 비즈니스 로직, 시스템 내의 데이터, 행동 등을 포함한다.

DB 세계와 관련: “Entities represent persistent data **stored in a relational database** automatically using container-managed persistence.”

OOP 세계와 관련: “An entity models a **business entity** or **multiple actions** within a single business process.”

OOP와 관련지어 생각을 안하게 되면 애매한 DB에 끌려가는 프로그램이 되기 십상이다.



### 궁금한 점?

1. ORM과 DB중심 개발의 차이점과 장단점은 무엇일까?

* 자바의ORM(JPA)은 객체 지향 프로그래밍 중심으로 데이터 접근을 추상화하여 데이터베이스와 독립성을 가지게 만들어준다. 단점으로는 성능 문제가 있으며 복잡한 쿼리나 튜닝이 필요한 경우가 있을 수 있다.
* DB 중심 개발은 직접적인 데이터베이스 접근을 통해 DB 스키마에 의존을 통해 성능 최적화와 직접적인 제어가 가능한 것이 특징이다. 단점으로는 모든 SQL을 직접 작성하여 개발 복잡성이 증가하고 쿼리의 유지보수가 어려울 수 있다. 또한 특정 DB에 종속성을 가지게 된다.
