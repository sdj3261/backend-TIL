# 애플리케이션 수준의 보안

### 목차

* 인증과 인가
* HTTP Stateless (1주차 복습)
* Cookie vs localStorage
* Interceptor vs Filter in Spring
* 암호화와 복호화
  * 단방향 암호화
  * Hashing algorithm
  * Salt가 필요한 이유

### 강의 정리

애플리케이션 수준에서 꼭 챙겨야 하는 기본 보안 요소 3가지

1. 인증 (Authentication)
2. 인가 (Authorization)
3. 암호화 (Encryption)

\| 인증과 인가

❓인증&#x20;

`“컴퓨터 보안에서`` `**`인증`**`은 로그인 요청 등을 통해 통신 상에서 보내는 사람의 디지털 정체성을 확인하는 시도의 과정이다.”`

사용자가 누구인가?&#x20;

누구인지 증명하는 것을 인증 과정 이라고 한다.

한번 로그인을 하면 세션이 유지될 동안 다시 로그인을 하지 않아도 된다. 하지만 HTTP는 stateless를 전제로 한다. 각 요청은 개별적이고, 매 요청마다 “나는 누구인가?”라는 질문에 답을 줘야 한다. 즉, 매 요청마다 인증 절차가 있게 된다.

❓인가

인가는 허가이다.

ex) 현재 사용자가 관리자임을 증명해야 하고(인증), 관리자가 아닌 사용자는 관리자 페이지에 접근할 수 없게 해야 한다(인가).

❗인증과 인가 과정

1. 사용자 입장에서는 로그인 페이지를 통해 인증 절차를 밟게 되고, 접근이 거부될 때만 인가 과정이 있었음을 알 수 있게 된다.
2. 클라이언트 입장에서는 로그인 과정을 통해 인증 결과로 허가의 의미로  토큰을 얻게 되고(세션을 활용한다면 Session ID를 통해 간접적으로 전달), 이를 쿠키나 localStorage 등으로 웹 브라우저에서  관리하면서 매 요청마다 서버로 전달한다. 즉, 매 요청마다 인가 과정이 있다고 가정한다.\
   (토큰이 있니? 통과? 아니야? 403)
3. 서버 입장에서는 모든 요청에 대해 인증 작업이 수행된다. 로그인 등을 통해 발행한 토큰 등을 매번 확인해 사용자가 누구인지 알아내고(인증), 해당 사용자가 올바른 접근을 했는지 확인해서 허용 또는 금지한다(인가).

즉 백엔드에선 HTTP 프로토콜의  stateless로 인해 매 요청마다 인증과 인가 과정을 통해 보안 처리를 해야한다.

Spring Web MVC로 개발할 때는 Controller로 요청이 전달되기 전에 HandlerInterceptor로 우리가 원하는 코드를 먼저 실행할 수 있고, Spring Security를 사용하면 SecurityFilterChain을 통해 우리가 원하는 코드를 먼저 실행할 수 있다.



\|&#x20;

Jakarta Persistence API는 Java에서 사용하는 ORM 표준으로 인터페이스이다.

Hibernate, EclipseLink와 같은 구현체를 통해 jpa를 구현한다. 현재는 Hibernate를 가장 널리 사용한다.

\| Jakarta EE

자바EE는 상업용 플랫폼의 한계와 스프링 프레임워크와 같은 오픈소스 SW의 발전으로 빠르게 변화하는 기술 트렌드를 반영하지 못해 인기가 시들해지며 비영리 단체인 이클립스 재단에 자바 EE 프로젝트를 이관하면서 오픈소스 기반의 Jakarta EE로 변경되었다.

자카르타EE의 핵심 목표는 ‘클라우드 네이티브 환경을 위한 엔터프라이즈 자바 기술’로 마이크로서비스, 컨테이너 등의 최신 기술 트렌드를 반영하고자 하는 것이 목표이다'

\| Entity

엔티티는 기본적으로  테이블에 대응되는 클래스이며 실세계 객체를 모델링한 것을 의미하며 비즈니스 로직, 시스템 내의 데이터, 행동 등을 포함한다.

DB 세계와 관련: “Entities represent persistent data **stored in a relational database** automatically using container-managed persistence.”

OOP 세계와 관련: “An entity models a **business entity** or **multiple actions** within a single business process.”

OOP와 관련지어 생각을 안하게 되면 애매한 DB에 끌려가는 프로그램이 되기 십상이다.



### 궁금한 점?

1. \
   ￼\
   \
   OOP와 관련지어 생각을 안하게 되면 애매한 DB에 끌려가는 프로그램이 되기 십상이\
   ￼\
   ￼\
   ​\
   ￼\
   ￼\
   ￼\
   궁금한 점?\
   ￼\
   ￼\
   ￼\
   ￼\
   1.\
   ㅅ​\
   ￼

