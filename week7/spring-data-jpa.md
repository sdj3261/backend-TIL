# Spring Data JPA

### 목차

* Spring Data JPA
* Dao 와 Repository 차이
* JPA Repository와 DDD의 Repository
* Spring AOP
* @Transactional

### 강의 정리

\| Spring Data JPA

Spring Data JPA를 사용하면 JPA 기반 리포지토리를 쉽게 구현할 수 있습니다. 이 모듈에서는 JPA 기반 데이터 액세스 계층에 대한 향상된 지원을 다룹니다. 데이터 액세스 기술을 사용하는 Spring 기반 애플리케이션을 더 쉽게 구축할 수 있습니다.

\| Dao 와 Repository 차이

DAO는 영속성의 추상화이고, Repository는 객체들 집합(collection)의 추상화이다. DAO는 storage system에 더 가까운 개념이고 Repository는 도메인 객체에 가까운 개념이다. Repository가 상대적으로 더 high-level의 개념이다. 그러므로 Repository는 DAO를 사용해 구현할 수 있으나, 그 반대는 구현할 수 없다.

|JPA Repository와 DDD의 Repository

JPA Repository는 데이터베이스와의 상호작용을 추상화하는 인터페이스로 CRUD, 페이징 , 정렬, 쿼리 메소드를 제공하여 DB 작업을 간소화하고 표준화한다.

DDD Repository는 도메인 모델의 일부로 컬렉션 처럼 작동하여 도메인 엔티티의 지속성을 관리한다. 도메인 로직과 데이터 지속성 사이의 분리를 강조하며 인프라계층과의 결합을 최소화 한다.

\| Spring AOP , @Transactional

* 클래스를 설계하다보면 로깅, 보안, 트랜잭션 등 여러 클래스에서 공통적으로 사용하는 부가 기능들이 생긴다. 이들은 주요 비즈니스 로직은 아니지만, 반복적으로 여러 곳에서 쓰이는 데 이를 **흩어진 관심사(Cross Cutting Concerns)**라고 한다.

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

1. target에 대한 호출이 들어오면 AOP proxy가 이를 가로채서(intercept) 가져온다.
2. AOP proxy에서 Transaction Advisor가 commit 또는 rollback 등의 트랜잭션 처리를 한다.
3. 트랜잭션 처리 외에 다른 부가 기능이 있을 경우 해당 Custom Advisor에서 그 처리를 한다.
4. 각 Advisor에서 부가 기능 처리를 마치면 Target Method를 수행한다.
5. interceptor chain을 따라 caller에게 결과를 다시 전달한다.

[https://velog.io/@ann0905/AOP%EC%99%80-Transactional%EC%9D%98-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC](https://velog.io/@ann0905/AOP%EC%99%80-Transactional%EC%9D%98-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC)



### 궁금한 점?

1. 인터페이스 만으로 어떻게 Repository의 메서드를 사용할 수 있을까?

스프링 데이터 JPA가 `JpaRepository` 인터페이스를 상속받는 리포지토리 인터페이스를 만나면 리플렉션을 활용해 `SimpleJpaRepository` 클래스를 기반으로 동적으로 프록시를 생성해서 잘 동작할 수 있었던 것이다.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

스프링은 사용자가 정의한 Repository 인터페이스를 구현하고 SimpleJpaRepository를 target으로 포함하는 Proxy를 동적으로 만들어주면서 그 Proxy를 Bean으로 등록해주고 연관관계 설정이 필요한 곳에 주입도 해주어 개발자에게편리성을 제공한다.
