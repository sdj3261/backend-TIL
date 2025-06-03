# SOLID

### 목차

* SOLID (객체 지향 설계)
  * SRP (Single Responsibility Principle, 단일 책임 원칙)
  * OCP (Open-Closed Principle, 개방-폐쇄 원칙)
  * LSP (Liskov Substitution Principle, 리스코프 치환 원칙)
  * ISP (Interface Segregation Principle, 인터페이스 분리 원칙)
  * DIP (Dependency Inversion Principle, 의존관계 역전 원칙)

### 강의 정리

\| 5가지 객체 지향 설계 원칙&#x20;

5가지 객체 지향 설계 원칙은 연결되어 있으며 특정 측면에서 이야기 하고 있다.

1. SRP (Single Responsibility Principle, 단일 책임 원칙)

* 한 클래스는 단 한 가지의 변경 이유만을 가져야 한다.

`User` 클래스는 사용자의 정보를 관리하는 책임만 가지며, `UserRepository`는 사용자 정보를 데이터베이스에 저장하는 책임을 가집니다. 이를 통해 각 클래스는 단 하나의 책임을 가집니다

SRP 를 따르면 우리는 테스트 하기 쉽고, 재사용하기 편하며 더 작고 이해하기 쉬운 클래스를 작성할 수 있다.&#x20;

2. OCP (Open-Closed Principle, 개방-폐쇄 원칙)

* 확장에는 열려 있고 수정에는 닫혀 있어야 한다. 객체 지향 설계의 상징이며 그 본질은 다형성이다.&#x20;

예시: 서비스 객체는 CartRepository를 통해 의존하며JdbcCartRepository , MongoCartRepository와 같은   구현체를 통해 DBMS가 바뀌더라도 확장에는 열려  있고  서비스   객체의 내부코드를 직접 수정할 필요는 없게 된다.

자바에서는 인터페이스를 통한 추상화를 통해 이 목표에 달성하려고 한다.&#x20;

3. LSP (Liskov Substitution Principle, 리스코프 치환 원칙)

* 타입 S의 각 객체 o1과 타입 T의 각 객체 o2가 있을 때, T로 프로그램 P를 정의했음에도 불구하고, o2를 o1로 치환할 때 P의 행위가 변하지 않으면, S는 T의 서브타입이다.

LSP를 지키고 있는지 파악하는 방법 중 하나는 상속이 “행위” 측면에서 Is-A 관계인지 파악하는 것이다. 정사각형은 직사각형인가? 우리의 수학적인 직관은 그렇다고 이야기하겠지만, 객체의 행위 측면에서 보면 둘은 엄격히 분리된다.&#x20;

3. ISP (Interface Segregation Principle, 인터페이스 분리 원칙)

응집력이 없는 커다란 인터페이스를 여러개의 작은 인터페이스로 나눈다.

```
public interface SearchProductRepository {
    List<Product> findAllByName(String name);
}

public interface CommandProductRepository {
    Product save(Product product);
}

// 원래는 이렇게 분리된 인터페이스를 구현하는 클래스가 있어야 함.
public class ProductRepository
        implements SearchProductRepository, CommandProductRepository {
}

// Spring Data JPA를 쓰니까 이렇게 쓸 수 있음.
public interface ProductRepository extends
        SearchProductRepository, CommandProductRepository,
        CrudRepository<Product, ProductId> {
}
```

3. DIP (Dependency Inversion Principle, 의존관계 역전 원칙)

* 상위 수준의 모듈은 하위 수준의 모듈에 의존해서는 안 된다. 둘 모두 추상화에 의존해야 한다.
* 추상화는 구체적인 사항에 의존해서는 안 된다. 구체적인 사항은 추상화에 의존해야 한다.

여기서 상위 수준의 모듈은 비즈니스 로직을 다루는 부분을 의미하고, 하위 수준의 모듈은 상위 수준의 모듈에서 호출하는 기술적인 부분을 의미한다.

`AddProductToCartService`가 `JdbcCartRepository`에 의존한다면, 비즈니스 로직에 가까운 Application Layer의 객체가 기술적인 문제를 다루는 Infrastructure Layer의 객체에 의존하는 꼴이 된다. 이렇게 되면 기술적인 문제가 바뀔 때 비즈니스 로직에 영향을 주게 된다. 따라서 의존성 방향을 \[Application Layer → Infrastructure Layer]에서 역전된&#x20;

\[Infrastructure Layer → Application Layer]로 만드는 설계가 필요하다.

인터페이스에 의존하면서 구체적인 인스턴스를 활용하기 위해서는 DI가 필요하고, Spring을 사용하면 아주 쉽게 DI와 DIP를 활용할 수 있게 된다(DI와 DIP의 I가 서로 다르다는 점에 주의!).



`❗ShoppingCartService`는 `CartRepository` 인터페이스에만 의존하고 있으며, 구체적인 구현체(`JdbcCartRepository`)에 의존하지 않습니다. 이것이 DIP의 핵심입니다. DIP를 따르면 새로운 구현체를 추가하거나 기존 구현체를 변경할 때 고수준 모듈을 수정하지 않아도 됩니다.

```
@Configuration
public class RepositoryConfig {

    @Bean(name = "jdbcCartRepository")
    public CartRepository jdbcCartRepository() {
        return new JdbcCartRepository();
    }

    @Bean(name = "mongoCartRepository")
    public CartRepository mongoCartRepository() {
        return new MongoCartRepository();
    }

    @Bean(name = "redisCartRepository")
    public CartRepository redisCartRepository() {
        return new RedisCartRepository();
    }
}


@Service
public class ShoppingCartService {
    private final CartRepository cartRepository;

    @Autowired
    public ShoppingCartService(CartRepository cartRepository) {
        this.cartRepository = cartRepository;
    }
}


@Autowired
public ShoppingCartService(@Qualifier("mongoCartRepository") CartRepository cartRepository) {
    this.cartRepository = cartRepository;
}


```

