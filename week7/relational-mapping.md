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

### 궁금한 점?

1. N+1 문제 해결방법
