# Unit Test & Spring Test

### 목차

* V-Model
* Test-Matrix
* 내적품질(테스트 코드 작성)을 높이면 좋은 이유
* Junit
* 단위 테스트(Unit Test) 및 E2E 테스트
* 통합 테스트(Integration Test)
* @Autowired
* @SpyBean
* MockMvc 및 MockBean
* @WebMvcTest

### 강의 정리

우리가 프로그래밍을 하는 이유는 무엇일까?

➡️ 비즈니스 문제를 해결하기 위해서

문제를 잘 정의하고 해결된 모습을 그려 볼 수 있다.&#x20;

### V-Model과 Test-Matrix

V-Model은 소프트웨어의 개발 프로세스로 폭포수 모델의 확장 형태로 각 단계는 매우 유용하다.

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

### Test-Matrix

지속적 배포를 위한 전제 조건 중 하나인 개발자 테스트는 아래와 같은 영역에 대해 테스트를 자동화하고 새로운 기능은 테스트와 함께 개발되어야 한다.



<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

우리가 일반적으로 말하는 품질은 개발의 외적 품질을 이야기 하지만(편의성, 디자인내적 품질도 매우 중요하다. 당장에 큰 성과가 나오는건 아니지만 외적 품질을 끌어올리거나 문제에 대해 대응하고 유지  보수하기 좋아져 장기적으로 생산성을 높일 수 있다.&#x20;

생산속도를 유지하려면 품질을 저하시키지 않고 소프트웨어를 더 빠르게 제공할 수 있는 방법을 찾아야 합니다. 소프트웨어가 언제든지 프로덕션 환경에 출시될 수 있도록 자동으로 보장하는 방식인 지속적 전달이 도움이 될 수 있습니다. 지속적 전달을 사용하면 빌드 파이프라인을 사용하여 소프트웨어를 자동으로 테스트하고 테스트 및 프로덕션 환경에 배포합니다. 구축부터 테스트, 배포, 인프라까지 모든 것을 자동화하는 것이 생산량을 늘리는 유일한 방법이다.



### 단위 테스트(Unit Test)&#x20;

Junit5는 자동화 된 테스트를 지원하는 도구지만 통합테스트와 E2E 테스트에도 사용된다.

```
@Test
void sqrtIter() {
	DecimalFormat decimalFormat = new DecimalFormat("0.######");
	assertThat(decimalFormat.format(sut.sqrtIter(1, 2))).isEqualTo("1.414216");
	assertThat(decimalFormat.format(sut.sqrtIter(1, 3))).isEqualTo("1.732143");
	assertThat(decimalFormat.format(sut.sqrtIter(1, 4))).isEqualTo("2");
}

@Test
void sqrt() {
	DecimalFormat decimalFormat = new DecimalFormat("0.######");
	assertThat(decimalFormat.format(sut.sqrt(2))).isEqualTo("1.414216");
	assertThat(decimalFormat.format(sut.sqrt(3))).isEqualTo("1.732143");
	assertThat(decimalFormat.format(sut.sqrt(4))).isEqualTo("2");
}
```

앞으로 Spring과 Mockito 등을 적극적으로 쓰는 테스트 코드를 많이 작성할 거지만, 기본은 여기에 있다. 단순한 Unit Test가 제일 많아야 하고, 이를 통해 신뢰할 수 있는 토대를 구축해야 한다.



**단위 테스트**는 응용 프로그램에서 테스트 가능한 가장 작은 소프트웨어를 실행하여 예상대로 동작하는지 확인하고&#x20;

**통합테스트**는 개발자가 변경할 수 없는 외부 라이브러리 까지 묶어 검증할 때 사용한다. ex) DB,인프라

스프링부트에서는 @SpringBootTest를 통해 통합 테스트 수행이 가능하다.

**인수 테스트**는 사용자 시나리오에 맞춰 수행하는 테스트이다.





스프링과 Junit을 이용해 테스트 코드를 작성하다보면 테스트 환경 구현 코드까지 작성하여 실제 테스트 할 코드보다 환경을 구현하는 코드가 더 길어 복잡한 경우가 많다.

이러한 문제 영역을 해결 하기 위해 Mockito  라이브러리가 있다.



@Mock, @MockBean, @Spy, @SpyBean, @InjectMocks&#x20;

스프링에서는 보통 DI를 통해 프레임워크에 위임해 객체를 생성하여 직접적으로 Mocking 테스트 환경을 만들기 쉽지 않다.&#x20;

@MockBean은 가짜 객체를 만들어 stub을 해주지만 @SpyBean은 stub한 메서드 외에 객체의 실제 메서드를 사용할 수 있다. given에서 선언한 코드 외에 전부 실제 객체의 것을 사용하여 스파이를 하나 심어둔 것 처럼 이해하면 된다. 대신에 SpyBean은 실제 구현된 객체를 감싸기 때문에 Spring Context에 실제 구현체가 등록되어 있어야 한다.

```
@SpringBootTest
@AutoConfigureMockMvc
class PostControllerTest {
	@Autowired
	private MockMvc mockMvc;
	
	@Autowired
	private PostRepository postRepository;
	
	@Test
	public void list() throws Exception {
		this.mockMvc.perform(get("/posts"))
			.andExpect(status().isOk())
			.andExpect(content().string(
				containsString("테스트입니다")
			));
	}
	
	@Test
	public void create() throws Exception {
		String json = """
					{
						"title": "새 글",
						"content": "제곧내"
					}
					""";
		
		int oldSize = postRepository.findAll().size();
	
		this.mockMvc.perform(
			post("/posts")
				.contentType(MediaType.APPLICATION_JSON)
				.content(json)
			)
			.andExpect(status().isCreated());
		
		int newSize = postRepository.findAll().size();
	
		assertThat(newSize).isEqualTo(oldSize + 1);
	}
}
```

### 테스트 더블이란?

영화 촬영시 위험한 역할을 대신하는 스턴트 더블에서 비롯되었으며 테스트가 어려울때 대신 테스트를 진행하도록 만들어주는 객체를 말한다.

테스트 더블의 종류

테스트 더블은 크게 **Dummy**, **Fake**, **Stub**, **Spy**, **Mock**으로 나눈다.

#### 1. Dummy

* 가장 기본적인 테스트 더블이다.
* 인스턴스화 된 객체가 필요하지만 기능은 필요하지 않은 경우에 사용한다.
* Dummy 객체의 메서드가 호출되었을 때 정상 동작은 보장하지 않는다.
* 객체는 전달되지만 사용되지 않는 객체이다.

정리하면 인스턴스화된 객체가 필요해서 구현한 가짜 객체일 뿐이고, 생성된 Dummy 객체는 정상적인 동작을 보장하지 않는다.

간단한 예시를 통해 알아보자.

```java
public interface PringWarning {
    void print();
}
```

```java
public class PrintWarningDummy implements PrintWarning {
    @Override
    public void print() {
        // 아무런 동작을 하지 않는다.
    }
}
```

실제 객체는 PrintWarning 인터페이스의 구현체를 필요하지만, 특정 테스트에서는 해당 구현체의 동작이 전혀 필요하지 않을 수 있다. 실제 객체가 로그용 경고만 출력한다면 테스트 환경에서는 전혀 필요 없기 때문이다.

이런 경우에는 `print()` 가 아무런 동작을 하지 않아도 테스트에는 영향을 미치지 않는다.

이처럼 동작하지 않아도 테스트에는 영향을 미치지 않는 객체를 Dummy 객체라고 한다.

#### 2. Fake

* 복잡한 로직이나 객체 내부에서 필요로 하는 다른 외부 객체들의 동작을 단순화하여 구현한 객체이다.
* 동작의 구현을 가지고 있지만 실제 프로덕션에는 적합하지 않은 객체이다.

정리하면 동작은 하지만 실제 사용되는 객체처럼 정교하게 동작하지는 않는 객체를 말한다.

간단한 예시를 통해 알아보자.

```java
@Entity
public class User {
    @Id
    private Long id;
    private String name;
    
    protected User() {}
    
    public User(Long id, String name) {
        this.id = id;
        this.name = name;
    }
    
    public Long getId() {
        return this.id;
    }
    
    public String getName() {
        return this.name;
    }
}
```

```java
public interface UserRepository {
    void save(User user);
    User findById(long id);
}
```

```java
public class FakeUserRepository implements UserRepository {
    private Collection<User> users = new ArrayList<>();
    
    @Override
    public void save(User user) {
        if (findById(user.getId()) == null) {
            user.add(user);
        }
    }
    
    @Override
    public User findById(long id) {
        for (User user : users) {
            if (user.getId() == id) {
                return user;
            }
        }
        return null;
    }
}
```

테스트해야 하는 객체가 데이터베이스와 연관되어 있다고 가정해보자.

그럴 경우 실제 데이터베이스를 연결해서 테스트해야 하지만, 실제 데이터베이스 대신 가짜 데이터베이스 역할을 하는 FakeUserRepository를 만들어 테스트 객체에 주입하는 방법도 있다. 이렇게 하면 테스트 객체는 데이터베이스에 의존하지 않으면서도 동일하게 동작을 하는 가짜 데이터베이스를 가지게 된다.

이처럼 실제 객체와 동일한 역할을 하도록 만들어 사용하는 객체가 Fake이다.

#### 3. Stub

* Dummy 객체가 실제로 동작하는 것 처럼 보이게 만들어 놓은 객체이다.
* 인터페이스 또는 기본 클래스가 최소한으로 구현된 상태이다.
* 테스트에서 호출된 요청에 대해 미리 준비해둔 결과를 제공한다.

정리하면 테스트를 위해 프로그래밍된 내용에 대해서만 준비된 결과를 제공하는 객체이다.

간단한 예시를 통해 살펴보자.

> 위에서 사용한 UserRepository 인터페이스를 사용하겠다.

```java
public class StubUserRepository implements UserRepository {
    // ...
    @Override
    public User findById(long id) {
        return new User(id, "Test User");
    }
}
```

위의 코드처럼 StubUserRepository는 `findById()` 메서드를 사용하면 언제나 동일한 id값에 **Test User**라는 이름을 가진 User 인스턴스를 반환받는다.

테스트 환경에서 User 인스턴스의 name을 Test User만 받기를 원하는 경우 이처럼 동작하는 객체(UserRepository의 구현체)를 만들어 사용할 수 있다.

물론 이러한 방식의 단점은 테스트가 수정될 경우(`findById()` 메서드가 반환해야 할 값이 변경되야 할 경우) Stub 객체도 함께 수정해야 하는 단점이 있다.

우리가 테스트에서 자주 사용하는 Mockito 프레임워크도 Stub와 같은 역할을 해준다.

이처럼 테스트를 위해 의도한 결과만 반환되도록 하기 위한 객체가 Stub이다.

#### 4. Spy

* Stub의 역할을 가지면서 호출된 내용에 대해 약간의 정보를 기록한다.
* 테스트 더블로 구현된 객체에 자기 자신이 호출 되었을 때 확인이 필요한 부분을 기록하도록 구현한다.
* 실제 객체처럼 동작시킬 수도 있고, 필요한 부분에 대해서는 Stub로 만들어서 동작을 지정할 수도 있다.

정리하면 실제 객체로도 사용할 수 있고 Stub 객체로도 활용할 수 있으며 필요한 경우 특정 메서드가 제대로 호출되었는지 여부를 확인할 수 있다.

간단한 예시를 통해 살펴보자.

```java
public class MailingService {
    private int sendMailCount = 0;
    private Collection<Mail> mails = new ArrayList<>();

    public void sendMail(Mail mail) {
        sendMailCount++;
        mails.add(mail);
    }

    public long getSendMailCount() {
        return sendMailCount;
    }
}
```

MailingService는 sendMail을 호출할 때마다 보낸 메일을 저장하고 몇 번 보냈는지를 체크한다. 그리고 나중에 메일을 보낸 횟수를 물어볼 때 sendMailCount 변수에 저장된 값을 반환한다.

이처럼 자기 자신이 호출된 상황을 확인할 수 있는 객체가 Spy이다.

이 또한 Mockito 프레임워크의 `verify()` 메서드가 같은 역할을 한다.

#### 5. Mock

* 호출에 대한 기대를 명세하고 내용에 따라 동작하도록 프로그래밍 된 객체이다.

우테코에서 미션을 진행하면서 사용한 Mockito 프레임워크가 대표적인 Mock 프레임워크라 볼 수 있다.

SpringBoot에서의 Mockito 프레임워크의 사용 방법을 통해 Mock을 살펴보자.

> 위에서 사용한 UserRepository 인터페이스를 사용하겠다.

```java
@ExtendWith(MockitoExtension.class)
public class UserServiceTest {
    @Mock
    private UserRepository userRepository;
    
    @Test
    void test() {
        when(userRepository.findById(anyLong())).thenReturn(new User(1, "Test User"));
        
        User actual = userService.findById(1);
        assertThat(actual.getId()).isEqualTo(1);
        assertThat(actual.getName()).isEqualTo("Test User");
    }
}
```

위의 예제를 보면 UserService 인터페이스의 구현체가 `findById()` 메서드를 동작했을 때 어떤 결과를 반환할지를 결정할 수 있다.

> Mockito의 when 메서드에서 anyLong()이 아니라 정확한 값을 사용해서 특정 상황에 대한 테스트를 특정할 수 있다.



➡️ Mockito가제공해주는 편리한 기능을 어떤 상황에, 무엇을 사용해야 하는지 모르고 사용한다면 제대로 된 테스트를 작성할 수 없다. 테스트 더블로 어떤 것이 있고, 어떤 상황에서 사용하는 것이 옳은지 학습하고 사용한다면, 더 효율적이고 올바른 테스트 코드를 작성할 수 있다.



### 궁금한 점?

1. 테스트 코드 최적화하기&#x20;

➡️ [https://www.youtube.com/watch?v=N06UeRrWFdY](https://www.youtube.com/watch?v=N06UeRrWFdY)

2. 테스트하기 어려운 코드(외부API) 어떻게 처리할까?

➡️ 작성중..

