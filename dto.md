# DTO

### 목차

- DTO (Data Transfer Object) 란
    - 프로세스 간 통신(IPC, Inter-Process Communication)
- “무기력한 도메인 모델” 이란 그리고 안티 패턴인 이유
- 자바빈즈(JavaBeans)
- EJB(Enterprise JavaBeans)
- Java의 record
- DAO
- ORM

### 강의 정리
서로 다른 프로세스(프로그램) 간에 포맷을 맞추기 위하여 우리는
JSON이나 DTO를 사용한다.

DTO(Data Transfer Object) : Object that carries data between processes in order to **reduce the number of method calls**.
remote interface, 프로세스간 통신 작업은 큰 오버헤드가 있으므로 이를 줄이기 위해 사용한다. <BR>

Data Transfer에 집중하면 꼭 remote interface가 아니여도 서로 다른 프로세스 간 데이터 전송에 쓰인다. <BR>
1. F/E와 B/E 사이 <BR> 
+ DTO 자체를 그대로 전송할 수는 없고 직렬화(마샬링)을 통해야 한다.
+ 어떤 직렬화 기술 사용할지? ex) XML, JSON
2. B/E와 DB 사이
+ SI기업에서 VO를 DTO란 의미로 사용하지만 현재는 Transfer Object로 불린다.(객체가 아닌 값 객체 Integer, Money, 불변)
+ DAO는 DB의 데이터에 접근하기 위한 객체를 가리킨다. DB에 접근하기 위한 로직을 분리하기 위해 사용한다. 직접 DB에 접근하여 data를 삽입, 삭제, 조회 등 조작할 수 있는 기능을 수행한다.
+ JPA를 지양하고 DDD를 따르는 사람 중 일부는 ORM은 Active Record + DTO처럼 접근한다.


아주 단순하게 setter getter로만 이루어진 것이 특징이며 제대로 된 객체가 아니라 그냥 무기력한 데이터 덩어리 이다.
Go,C++에서는 struct로 구분할 수 있었지만 자바에서는 불가능하다. (최신 자바는 record로 해결가능)

IPC(inter-process Communication) : 서로 다른 프로세스 간 통신 <BR>
ex) B/E F/E로 Tier 나눌때<BR>
IPC 사용 기술 : File,Socket(HTTP),원격 프로시저 호출

무기력한 도메인 모델 : 객체가 실제 도메인 모델이 갖는 풍부한 관계나 동작없이 getter setter과 같은 가방에 불과한 모델
이러한 모델은 어떤 비즈니스 논리도 넣지 말아야 한다.<BR>
안티패턴인 이유 : 객체기향 디자인의 기본 개념(협력,결합등)에 어긋난 절차적 디자인이다. 

자바 빈즈(JavaBeans) : 자바로 작성된 소프트웨어 컴포넌트 <br>
자바빈즈는 빌더 형식의 개발도구에서 가시적으로 조작이 가능하고 재사용 가능한 컴포넌트로 기존 DTO는 자바 빈즈를 따른다. <br>
자바빈즈의 관례 <br>
클래스는 직렬화, 기본생성자, get,set과 같은 메서드로 접근, 필요한 이벤트 처리 메서드 포함

EJB(Enterprise Java Beans) : 스프링 컨테이너에서의 빈으로 엔터프라이즈급 어플리케이션 개발을 단순하기 위해 발표한 스펙
<BR>
개발을 하다 보면 많은 객체를 만들게 되므로 이러한 비즈니스 객체를 컨테이너를 통해 필요할때마다 컨테이너로부터 객체를 주입 받는 식으로 관리 하면
효율적이기 때문에 탄생하게 되었다.
<BR>

자바의 Record란? <br>
   => 자바 16부터 등장하여 불변 데이터 객체를 쉽게 생성할 수 있도록 하는 새로운 유형의 클래스로 주로 DTO로 쓰인다.
   <BR>
+ 불필요한 코드를 제거할 수 있다. <br>
+ 멤버변수는 private final로 선언된다  <br>
+ 모든 멤버변수를 인수로 가지는 생성자를 자동으로 생성해 준다 <br>
+ getter, equals, hashcode, toString과 같은 기본 메소드를 제공한다 <br>
+ 인터페이스 구현은 가능하지만 상속은 불가능하다.

기존 DTO class와 record 비교

```
@Getter
@Setter
@NoArgsConstructor
public class QuestionSaveRequestDto {

    @NotBlank(message = "제목은 필수 입력 사항입니다.")
    @Size(max = 200, message = "제목이 너무 길어요.")
    private String title;
    @NotBlank(message = "내용은 필수 입력 사항입니다.")
    private String content;

    @Builder
    public QuestionSaveRequestDto(String title, String content) {
        this.title = title;
        this.content = content;
    }

    public Question toEntity(Member member) {
        return Question.builder()
                .title(title)
                .content(content)
                .member(member)
                .build();
    }
}


public record QuestionSaveRequestDto(
        @NotBlank(message = "제목은 필수 입력 사항입니다.")
        @Size(max = 200, message = "제목이 너무 길어요.")
        String title,

        @NotBlank(message = "내용은 필수 입력 사항입니다.") 
		String content
) {
    public Question toEntity(Member member) {
        return Question.builder()
                .title(title())
                .content(content())
                .member(member)
                .build();
    }
}
```

ORM(object-relational mapping)
객체 지향 프로그래밍과 관계형 데이터베이스 사이의 불일치 문제를 해결하기 위한 기술 또는 프로그래밍 기법으로 DB 테이블을 객체로, 행과 컬럼을 속성으로
매핑하여 프로그램 내에서 사용할 수 있다. <BR>

ORM의 주요 특징:
1. 객체-데이터베이스 매핑: ORM 도구는 데이터베이스의 테이블과 객체 지향 언어의 클래스 간의 매핑을 제공합니다. 이로 인해 개발자는 SQL 쿼리를 직접 작성하는 대신 객체 지향 언어의 문법을 사용하여 데이터베이스와 상호 작용할 수 있습니다.

2. 데이터베이스 독립성: 많은 ORM 도구는 다양한 데이터베이스 시스템을 지원합니다. 이로 인해 동일한 코드를 사용하여 다양한 데이터베이스에 연결할 수 있습니다.

3. 코드 재사용 및 유지 보수성: ORM은 데이터 접근 로직을 모듈화하여 코드의 재사용성을 향상시키며, 유지 보수를 용이하게 합니다.

ORM의 장점:
1. 생산성: 개발자는 SQL 쿼리를 직접 작성하는 대신 더 높은 수준의 API를 사용하여 데이터베이스와 상호 작용할 수 있어 개발 시간이 단축됩니다.

2. 객체 지향 설계 지원: ORM을 사용하면 객체 지향적인 설계와 관계형 데이터베이스를 자연스럽게 통합할 수 있습니다.

3. 데이터베이스 변경의 유연성: 데이터베이스의 구조나 유형이 변경되더라도 ORM 설정만 변경하면 애플리케이션 코드에는 큰 영향을 미치지 않습니다.

ORM의 단점:
1. 성능 이슈: ORM 도구가 생성하는 SQL 쿼리는 항상 최적화된 것이 아닐 수 있습니다. 때로는 복잡한 쿼리나 특정 데이터베이스 최적화 기능이 필요한 경우에는 ORM의 제한에 직면할 수 있습니다.
2. 복잡성: 매우 복잡한 데이터베이스 스키마나 복잡한 쿼리의 경우 ORM의 추상화가 제한적일 수 있습니다.

***

### 궁금한 점?
1. Record(DTO)에서 상속을 필요로하는 경우에는 어떻게 처리하는것이 좋을까?
+ Record에서 상속을 피하고 인터페이스를 활용하기 
+ Record대신 class 활용하기 
2. ORM Data mapper(JPA) 패턴 VS Active Record + DTO 사용 <BR>
=> : Active Record 패턴에서는 모델 객체 자체가 데이터베이스 테이블의 레코드를 나타내며, 
CRUD (Create, Read, Update, Delete) 작업을 수행하는 메서드를 포함하고 있습니다. <BR>
코드가 직관적이며 모델 클래스에 비즈니스 로직과 데이터베이스 연산이 모두 포함될 수 있음. <BR>

단점: 복잡한 도메인 로직이나 복잡한 데이터베이스 연산이 필요한 경우에는 한계가 있다 <BR>
Data Mapper 패턴 (JPA가 사용하는 패턴):  <BR>
=> Data Mapper 패턴은 데이터베이스와 도메인 객체 (모델) 간의 관계를 중재하는 별도의 객체인 'Mapper'를 사용하여 두 시스템을 분리합니다. <BR>
도메인 객체는 데이터베이스에 대해 알지 못하며, 순수한 비즈니스 로직만 포함할 수 있습니다. <BR>
Mapper 객체가 도메인 객체와 데이터베이스 간의 변환을 처리합니다. <BR>
예: Java의 JPA와 Hibernate, .NET의 Entity Framework 등이 이 패턴을 사용합니다. <BR>
장점: 비즈니스 로직과 데이터베이스 연산이 엄격하게 분리되므로, 도메인 로직의 복잡성이나 데이터베이스 구조의 변화에 대해 더 유연하게 대응할 수 있습니다. <BR>


Active Record 패턴은 객체가 자신의 데이터 표현과 데이터베이스에 대한 연산을 직접 관리하는 반면, Data Mapper 패턴은 이 두 가지 책임을 엄격하게 분리하여 데이터베이스 연산과 객체 표현을 관리하는 데 중재자 역할을 하는 Mapper 객체를 사용합니다.
