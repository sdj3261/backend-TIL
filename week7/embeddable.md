# Embeddable

### 목차

* Aggregate Mapping
* Value Object
* JPA 어노테이션
  * @Entity
  * @Table
  * @Id
  * @Embeddable
  * @Embedded

### 강의 정리

\| 객체지향 설계 원칙과 JPA의 데이터 관리 방식 연결

Aggregate Mapping & ValueObject

DDD에서 어그리게이트는 하나의 루트 엔티티와 여러 자식 엔티티 및 값 객체(Value Object)를 포함한다.

루트 엔티티를 통해 어그리게이트의 경계를 정의하고 모든 외부 접근은 루트 엔티티를 통해 이루어져야 한다.

* **관계 매핑**: 어그리게이트 내의 자식 엔티티와 값 객체들은 루트 엔티티와의 관계를 `@OneToMany`, `@OneToOne, @Embeddable` 등으로 매핑합니다.
* **일관성 유지**: JPA를 사용하면 어그리게이트의 일관성과 무결성을 유지하는 데 도움이 됩니다. 예를 들어, 트랜잭션 관리와 지연 로딩(lazy loading) 기능을 활용할 수 있습니다.

\| JPA Annotation

1. @Entity&#x20;

\-> @Entity 어노테이션은 JPA를 사용해 테이블과 매핑할 클래스에 붙여주는 어노테이션이다. 이 어노테이션을 붙임으로써 JPA가 해당 클래스를 관리하게 된다. 기본 생성자가 꼭 필요하며 final 사용 불가이다.

2. @Table&#x20;

\-> 연결할 테이블을 지정한다. name attribute를 통해 특정 테이블명을 지정 가능하다.

3. @Id&#x20;

\-> @Id는 특정 속성을 기본키로 설정하는 어노테이션이다

@GeneratedValue 어노테이션을 사용하면 기본값을 DB에서 자동으로 생성하는 전략을 사용 할 수 있다.

4. @Column

\-> @Column은 객체 필드를 테이블 컬럼과 매핑한다

5. @Enumerated

\-> @Enumerated는 자바 enum 타입을 매핑할 때 사용한다.

@Enumerated(value = EnumType.STRING) 을 통해 Enum이름으로 지정 가능하다.

6. @Transient

\-> @Transient 어노테이션을 붙인 필드는 DB에 저장하지도 조회하지도 않는다. 객체에 임시로 값을 보관하고 싶을 때 사용한다.

7. @Embeddable

* `@Embeddable`: 값 타입을 정의하는 곳에 표시
* `@Embedded`: 값 타입을 사용하는 곳에 표시
* **임베디드 타입은 기본 생성자가 필수**
* @Embeddable
* @Embedded

임베디드 타입 덕분에 **객체와 테이블을 아주 세밀하게(fine-grained) 매핑하는 것이 가능**합니다. 응집력을 높이고 결합도를 떨어뜨려 객체지향적인 코드를 만들기 쉽다.

