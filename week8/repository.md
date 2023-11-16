# Repository

### 목차

* 리파지터리 (Repository)
* Application Layer와 UI Layer

### 강의 정리

\| Repository

* 리파지터리는 도메인 모델의 영속성을 관리하는 역할을 합니다. 즉, 도메인 엔티티나 집합체(Aggregate)를 데이터베이스에 저장하고 검색하는 메커니즘을 제공합니다.
* 이는 도메인 레이어와 데이터 저장소 사이의 중간자 역할을 하며, 도메인 로직과 데이터베이스 로직을 분리합니다. 이로 인해 도메인 로직이 데이터베이스 구현 세부 사항으로부터 독립적으로 유지될 수 있습니다.
* 리파지터리는 일반적으로 인터페이스로 정의되고, 구현체는 인프라스트럭처 레이어에서 제공됩니다.
* 적절한 Aggregate를 발견하고, 적절히 책임을 나눌 수 있도록 Entity와 Value Object로 구성(또는 분해)하고, 이를 위한 Repository를 만듦으로써, 여러 기술 문제와 무관한 비즈니스 도메인에 집중할 수 있게 된다.
* Spring Data JPA는 이 둘을 만족시키기 위한 기능을 갖추고 있다.&#x20;
  1. Aggregate에 대해서만 Repository를 만들어도 안정적으로 작동할 수 있도록 기능을 제공한다.
  2. Persistence Context를 통해 Collection처럼 사용하는 게 가능하다.
  3. 아예 인터페이스만 만들면 나머지는 크게 신경 쓰지 않아도 되는 기능까지 제공한다.

\| Application Layer와 UI Layer

* Layered Architecture의 도움 없이는 도메인 모델을 Domain Layer에 격리하는 게 불가능하다.&#x20;
* 성공적인 Layered Architecture는 네 가지 개념적 계층으로 나뉜다.
  * Presentation Layer(UI Layer)
    * 사용자의 응답 및 요청을 처리
    * Web 등 구체적인 기술로 사용자와 소통하는 코드가 모인 곳
  * Application Layer
    * Repository와 Aggregate를 사용하는 코드가 모인 곳
  * Domain Layer&#x20;
    * 비즈니스 도메인에 집중한 코드를 모아둔 곳
  * Infrastructure Layer

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

