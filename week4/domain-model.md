# Domain Model

### 목차

* 직렬화(Serialization)
* 마샬링
* JSON

### 강의 정리

객체를 그 자체로 DB에 저장하거나 네트워크로 전송하는 것은 불가능하다. 객체를 데이터화하여 바이너리라면 Byte Stream, 텍스트라면 XML,JSON을 주로 사용한다. 직렬화와 마샬링은 거의 같은 뜻이지만 Java에서 마샬링은 특수하게 다룬다.

직렬화 VS 마샬링

* 직렬화는 역직렬화를 통해 객체 또는 데이터의 복사본을 만들 수 있다.
* 마샬링은 직렬화와 같거나 원격 객체로 복원할 수 있다. 원격 객체의 경우에 메서드 호출은 RPC 가 된다.

JSON(Javascript Object Notation) Dogulas Crockford가 만든 데이터 포맷으로 사람이 읽기 쉽고 기계도 파싱하기 쉽다. 대부분 Json.parse(역직렬화) 와 stringify(직렬화)로 안전하게 사용한다. 기본적으로 key-value 쌍으로 자바에서는 Map과 비슷하지만 스키마 관리 및 타입 안전성을 DTO를 사용한다.

* 생성 : DTO(자바) -> Converter -> JSON String
* 해석 : JSON String -> Converter -> DTO

컨버터로 Java에선 Jackson이란 도구가 유명하고 스프링부트는 의존성 추가로 간단하게 사용 가능하다. @JsonProperty로 속성 이름 지정 가능하고 다른 객체(DTO)를 포함할수 있다.

DTO는 여러곳에서 사용될수 있고 Tier(Remote 환경이 아닌 상황)에서 레이어 사이나 인터페이스에서도 활용되며 데이터 전송이라는 의미면 틀리지 않다.

Jackson ObjectMapper를 통해 DTO <-> JSON 변환하며 필드명은 getter 메서드의 이름을 따르는데에 주의하자

* JSON 형식 예시

```
{
    "id": 1234,
    "title": "제목",
    "content": "test"
}
```

***

### 궁금한 점?

1. @ComponentScan Spring IOC 컨테이너에게 특정 패키지를 스캔하도록 지시하여 해당 패키지에 있는 어노테이션이 붙은 클래스를 Spring Bean으로 자동 등록한다. 이러한 Bean으로 등록된 객체는 Spring IOC 컨테이너를 통해 어디서나 주입 받아 사용 가능하다.

```
@Component
public class SomeClass {
    private final SomeBean someBean;

    @Autowired
    public SomeClass(SomeBean someBean) {
        this.someBean = someBean;
    }
}

@Component
public class SomeClass {
    private final SomeBean someBean;

    @Autowired
    public SomeClass(SomeBean someBean) {
        this.someBean = someBean;
    }
}
```

2. 생성자 주입의 장점 :

&#x20;  1\. 불변성(Immutability): 생성자 주입을 사용하면, 해당 필드를 final로 선언할 수 있습니다. 이는 객체가 생성된 후 해당 필드의 상태가 변경되지 않도록 보장하며, 객체의 불변성을 증진시킵니다. 불변 객체는 스레드 안전성을 갖게 되어 병렬 처리 환경에서도 안정적으로 사용할 수 있습니다.

&#x20;  2\. 의존성 명확성: 생성자를 통해 주입받는 의존성이 명시적으로 드러나므로, 해당 클래스가 어떤 의존성을 필요로 하는지 쉽게 파악할 수 있습니다.

&#x20;  3\. 순환 의존성(Circular Dependency) 감지: 생성자 주입을 사용할 경우, 순환 의존성이 발생하면 Spring 컨테이너는 애플리케이션 시작 시점에 오류를 발생시킵니다. 따라서 순환 의존성 문제를 조기에 발견하고 수정할 수 있습니다.

&#x20;  4\. Null 안전성: 생성자를 통해 의존성이 제대로 주입되지 않았을 경우 객체 생성 자체가 실패하게 됩니다. 따라서 Null Pointer Exception(NPE)의 발생 가능성을 줄일 수 있습니다.

\
&#x20;  5\. 필드 주입이나 세터 주입에 비해 오버헤드 감소: 생성자 주입은 객체 생성 시점에 한 번만 호출되므로, 필드나 세터를 통한 주입 방식보다 추가적인 메서드 호출 오버헤드가 발생하지 않습니다.\
