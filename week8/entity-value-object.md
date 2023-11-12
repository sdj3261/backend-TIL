# Entity, Value Object

### 목차

* 전술적 설계 (Tactical Design)
* 엔티티 (Entity) vs 일반적으로 이야기하는 개체 (Entity)
* 값 객체 (Value Object, VO)
* 동일성(Identity)과 동등성(Equality)

### 강의 정리

\| 전술적 설계 (Tactical Design)

* 전술적 설계는 전략적 설계에서 도출된 Bounded Context와 Domain을 이용하여 Aggregate(집합) 패턴, Entity와 VO, Repogitory(저장소), 등을 구성하고 구현하는 활동을 의미한다.
* 다만 전략적 설계 패턴는 커다란 시스템을 만들 때는 더욱 중요하지만 작은 규모의 설계에서는 다룰 수 있는 주제가 아니다.
  * 전술적 설계만 다룰 경우 DDD Lite, Model Driven Design이라고 불린다.
* 도메인 모델의 대표적인 패턴&#x20;
  1. Entity
  2. Value Object
  3. Aggregate
  4. Repository

\| 엔티티 (Entity) vs 일반적으로 이야기하는 개체 (Entity)

* OOP에서의 대부분의 객체는 속성이 아니라 연속성, 식별성에 따라 정의된다.
* 식별자(Identifier)가 존재하고, 이를 통해 동일성(Identity)을 확인한다면 DDD의 Entity라고 할 수 있다.

\| 값 객체(VO)

VO는 도메인에서 한 개 또는 그 이상의 속성들을 묶어서 특정 값을 나타내는 객체를 말하며, 연속성, 식별성이 중요하지 않은 객체를 의미한다.

VO 의 특징

* 불변성 (Immutable)
  * VO는 수정자(Setter) 메소드를 가지지 않는다. 즉 불변하다.
* 동등성 (Equality)
  * 두 객체가 실제 다른 객체이더라도(동일성-identity을 갖지 않더라도) 논리적으로 값이 같다면 동등성을 갖는다. (자바의 equals 메서드를 속성 값이 같은지 구현)
* 자가 유효성 검사 (Self-Validation)
  * 모든 유효성 검사는 생성 시간에 이루어져야 한다.
  * 따라서 유효하지 않는 값으로 VO를 생성할 수 없다.

\| 동일성(Identity)과 동등성(Equality)

#### 동일성

* 비교 대상의 두 객체의 메모리 주소가 같음을 의미한다. (==)

동등성

* 비교 대상의 두 객체가 논리적으로 동일한 값을 나타내고 있는 지를 의미한다. (equals).
