# 로그인 및 회원가입

### 목차

* CSRF(Cross Site Request Forgery)
* 자바 Date vs LocalDateTime
* Epoch Time
* Spring Security PasswordEncoder
* Argon2
* TSID

### 강의 정리

애플리케이션 수준에서 꼭 챙겨야 하는 기본 보안 요소 3가지

1. 인증 (Authentication)
2. 인가 (Authorization)
3. 암호화 (Encryption)

\| CSRF

❓CSRF(Cross Site Request Forgery)&#x20;

` ``` `

사용자가 누구인가?&#x20;

누구인지 증명하는 것을 인증 과정 이라고 한다.

한번 로그인을 하면 세션이 유지될 동안 다시 로그인을 하지 않아도 된다. 하지만 HTTP는 stateless를 전제로 한다. 각 요청은 개별적이고, 매 요청마다 “나는 누구인가?”라는 질문에 답을 줘야 한다. 즉, 매 요청마다 인증 절차가 있게 된다.

❓인가

인가는 허가이다.

ex) 현재 사용자가 관리자임을 증명해야 하고(인증), 관리자가 아닌 사용자는 관리자 페이지에 접근할 수 없게 해야 한다(인가).

❗인증과 인가 과정
