# Relational Mapping

목차

* N + 1 problem
* DDD의 Aggregate
* `CascaseType.ALL`
* `orphanRemoval`
* Event Sourcing
* JPA 어노테이션
  * @OneToMany
  * @JoinColumn

### 강의 정리

\| Relational Mapping

데이터 모델 Entity의 관계를 객체 참조로 간단히 활용할 수 있게 해준다. 일반적으로는 DDD의 Aggregate를 구현하기 위해 CascadeType.ALL과 orphanRemoval=true를 함께 사용하는 게 강력히 권장된다.

일정 규모 이상의 시스템이라면 Command와 Query를 구분해서 다루는 게 좋다.&#x20;

조회 성능이 중요할떄는JPA Entity와 Relationship Mapping을 활용하는 방식을

CUD 성능이중요할때는 JdbcTemplate 등 SQL 매퍼를 적극 활용하는 방식을 추천한다

\| N+1 Problem

연관 관계에서 발생하는 이슈로 연관 관계가 설정된 엔티티를 조회할 경우에 조회된 데이터 갯수(n) 만큼 연관관계의 조회 쿼리가 추가로 발생하여 데이터를 읽어오게 된다. 이를 N+1 문제라고 한다.&#x20;

\| DDD의 Aggregate

Person이 Item을 “소유”하고, Item을 개별적으로 접근하는 일은 절대 없도록 하자. DDD에선 이런 걸 Aggregate란 개념으로 다룬다(자세한 건 DDD Lite 때 다룰 예정).&#x20;

```
@OneToMany(cascade = CascadeType.ALL, orphanRemoval = true) 
@JoinColumn(name = "person_name") 
@OrderBy("name") 
private List items = new ArrayList<>();
```

\| cascade = CascadeType.ALL, orphanRemoval = true

cascade : 연산이 자식 엔티티에도 적용되야함을 의미한다 ALL은 모든 연산

orphanRemoval : 부모엔티티와의 관계가 끊어지면 자식엔티티는 자동으로 삭제된다.

이를 통해 개발자는 엔티티 간 관계의 유지보수를 쉽게 관리할 수 있다.

### 궁금한 점?

1. N+1 문제 해결방법

* &#x20;1\. Fetch Join

```
@Query("select o from Owner o join fetch o.cats")
List<Owner> findAllJoinFetch();
```

Inner Join 으로 실행된다.

단점 : 우리가 연관관계 설정해놓은 FetchType을 사용할 수 없다는 것이다. Fetch Join을 사용하게 되면 데이터 호출 시점에 모든 연관 관계의 데이터를 가져오기 때문에 FetchType을 Lazy로 해놓는것이 무의미하다. 또한, 페이징 쿼리를 사용할 수 없다. 하나의 쿼리문으로 가져오다 보니 페이징 단위로 데이터를 가져오는것이 불가능하다.

* &#x20;2\. EntityGraph

Outer Join 으로 실행된다.

### Fetch Join과 EntityGraph 주의할 점

Fetch Join과 EntityGraph는 JPQL을 사용하여 JOIN문을 호출한다는 공통점이 있다. 또한, 공통적으로 카테시안 곱(Cartesian Product)이 발생하여 Owner의 수만큼 Cat이 중복 데이터가 존재할 수 있다. 그러므로 중복된 데이터가 컬렉션에 존재하지 않도록 주의해야 한다.



{% hint style="success" %}
#### 그렇다면 어떻게 중복된 데이터를 제거할 수 있을까?

* 컬렉션을 Set을 사용하게 되면 중복을 허용하지 않는 자료구조이기 때문에 중복된 데이터를 제거할 수 있다.
* JPQL을 사용하기 때문에 distinct를 사용하여 중복된 데이터를 조회하지 않을 수 있다.
{% endhint %}

* &#x20;3\. BatchSize

`size`는 IN절에 올수있는 최대 인자 개수를 말한다. 만약 Cat의 개수가 10개라면 위의 IN절이 2번 실행될것이다.

* BatchSize는 연관관계의 데이터 사이즈를 정확하게 알 수 있다면 최적화할 수 있는 size를 구할 수 있겠지만 사실상 연관 관계 데이터의 최적화 데이터 사이즈를 알기는 쉽지 않다.
