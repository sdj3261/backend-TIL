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

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

### Test-Matrix

지속적 배포를 위한 전제 조건 중 하나인 개발자 테스트는 아래와 같은 영역에 대해 테스트를 자동화하고 새로운 기능은 테스트와 함께 개발되어야 한다.



<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

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

테스트 더블이란?



The generic term he uses is a [Test Double](http://xunitpatterns.com/Test%20Double.html) (think stunt double). Test Double is a generic term for any case where you replace a production object for testing purposes. There are various kinds of double that Gerard lists:

* Dummy objects are passed around but never actually used. Usually they are just used to fill parameter lists.
* Fake objects actually have working implementations, but usually take some shortcut which makes them not suitable for production (an [InMemoryTestDatabase](https://martinfowler.com/bliki/InMemoryTestDatabase.html) is a good example).
* Stubs provide canned answers to calls made during the test, usually not responding at all to anything outside what's programmed in for the test.
* Spies are stubs that also record some information based on how they were called. One form of this might be an email service that records how many messages it was sent.
* Mocks are pre-programmed with expectations which form a specification of the calls they are expected to receive. They can throw an exception if they receive a call they don't expect and are checked during verification to ensure they got all the calls they were expecting.







### 궁금한 점?

1. MockBean vs SpyBean

