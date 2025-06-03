# Hibernate

### 목차

* Hibernate
* 데이터 모델 / 객체 모델
* entityManager
* 트랜잭션
* JPQL

### 강의 정리

\| Hibernate

* persistence.xml에 아래와 같이 DB 접속정보 및 매핑 클래스를 지정해준다.

```
<persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence" version="2.1">
	<persistence-unit name="demo">
		<class>com.example.demo.models.Person</class>
		<properties>
			<property name="jakarta.persistence.jdbc.url"
				value="jdbc:postgresql://localhost:5432/postgres"/>
			<property name="jakarta.persistence.jdbc.user"
				value="postgres"/>
			<property name="jakarta.persistence.jdbc.password"
				value="password"/>
			</properties>
		</persistence-unit>
</persistence>
```

EntityManager 는 Thread-Safe 를 보장해야 한다. 동일한 EntityManager 를 가지고 멀티 스레드 환경에서 호출한다면 데이터가 어떻게 변경될지 모르기 때문

\-> 따라서EntitiyManagerFactory에서 싱글톤 방식을 통해 EntityManager를 만들어 쓰레드간 공유를 금지한다.

```
    // 엔티티 매니저가 있다고 가정.
    // 추후에 엔티티 매니저 팩토리와 함께 엔티티 매니저를 어떻게 생성하는지 설명.
    EntityManager em;
    EntityTransaction tx = em.getTransaction();
    
    try {
        // 엔티티 매니저에서 수행하는 모든 로직은 트랜잭선 안에서 수행돼야 한다.
        tx.begin();
        
        // 이렇게 하면 해당 엔티티 매니저의 영속성 컨텍스트에 위에서 만든 member 객체가 저장된다.  
        // 이제 member 엔티티는 엔티티 매니저의 관리 대상이 되고, 영속성을 가졌다고 말할 수 있다.
        em.persist(member);
        
        // 트랜잭션을 커밋한다.
        tx.commit();
    } catch(Exception e) {
        // 어떤 이유에서 오류가 났다면 트랜잭션을 롤백 시켜줘야한다.
        tx.rollback();
    } finally {
        // 엔티티 매니저를 종료시켜줘야 한다.  
        // 아마 더 이상 사용하지 않는 자원이므로 더 이상 사용하지 않는 자원이라고 표시하는 것 같다.
        // 그럼 아마 GC가 해당 엔티티 매니저 자원을 수거해가서 메모리에 반환하지 않을까??
        // 성능 상 문제가 있어서 이렇게 종료시켜줘야 하는 건지 모르겠다. 
        em.close();
    }
}
```

}

```
public class JpaTest {
	private EntityManagerFactory entityManagerFactory;
	
	private EntityManager entityManager;
	
	@BeforeEach
	void setUp() {
		entityManagerFactory = Persistence.createEntityManagerFactory("demo");
		entityManager = entityManagerFactory.createEntityManager();
	}
	
	@AfterEach
	void tearDown() {
		entityManager.close();
		entityManagerFactory.close();
	}
	
	@Test
	void query() {
		Person person = entityManager.find(Person.class, "견우");

		System.out.println("*".repeat(80));
		System.out.println(person);
		System.out.println("*".repeat(80));
	}
}
```



영속성 컨텍스트와 Dirty Checking

* 영속성 컨텍스트는 관리하고 있는 엔티티의 변화를 추적하고, 한 트랜잭션 내에서 변화가 일어나면 엔티티에 마킹한다. 그리고 트랜잭션이 끝나는 시점에 마킹한 것을 DB 에 반영해준다.
  * 이를 dirty checking 이라고 한다.
*   영속성 컨텍스트가 중간에서 쿼리들을 모았다가 한 번에 반영해주니 리소스 감소가 있음.

    * 이를 쓰기 지연이라 함.


* 동일 Transaction 에서 EntityManager 를 여러 번 호출해서 처리하면 같은 영속성 컨텍스트 에서 처리된다.
* 다른 Transaction 이라면 다른 영속성 컨텍스트에서 처리된다.&#x20;
