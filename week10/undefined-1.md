# 인증

### 목차

* Identifier
* [**PostgreSQL**](https://www.postgresql.org/) **(MySQL, MariaDB와 두드러지는 차별점)**
* OAuth 2.0
* Bearer Token 옵션
* SecurityFilterChain
* `@Configuration`
* `@EnableWebSecurity`
* Filter vs OncePerRequestFilter
* FilterChain
* SecurityContext

### 강의정리

스프링  시큐리티의   기본인증 방식은 인증 정보를 암호화하지 않고 매번 반복적으로 보내기 때문에 위험하다. 따라서 로그인 이후에는 임시로 쓸 수 있게 발급된 Access Token을 사용해야 한다. OAuth 2.0 이후로는 HTTP 헤더를 통해 Bearer Token으로 전달한다.

```
#Spring Security 적용전
http localhost:8080/api

#Spring Security 적용후
http -a <username>:<password> localhost:8080/api

http -A bearer -a <accessToken> localhost:8080
```

1. **Identifier**:
   * 식별자는 객체, 변수, 함수 등을 구별하기 위해 사용되는 이름입니다. 보안 측면에서, 식별자는 사용자, 세션, 자원 등을 구별하는 데 사용됩니다.

**PostgreSQL (MySQL, MariaDB와 두드러지는 차별점)**:

|                       | MySQL                                               | **PostgreSQL**                |
| --------------------- | --------------------------------------------------- | ----------------------------- |
| **ORDBMS와 RDBMS 비교**  | 관계형 데이터베이스 관리 시스템 (RDBMS)                           | 객체 관계형 데이터베이스 관리 시스템(ORDBMS)  |
| **ACID 규정 준수**        | 대부분의 엔진은 ACID 규정 준수를 제공하지만 MyISAM은 ACID를 지원하지 않습니다. | 완벽한 지원                        |
| **백업 및 복구**           | 백업 및 복구 기능을 제공합니다.                                  | 효율적인 백업 및 복구 기능으로 정평이 나 있습니다. |
| **크로스 플랫폼**           | 대응                                                  | UNIX 기반 시스템에 최적               |
| **확장 기능 및 플러그인**      | 다양하게 있음                                             | 확장성으로 유명한 PostGIS 등           |
| **외부 키**              | 지원되지만 MyISAM에서는 지원되지 않습니다.                          | 완벽한 지원                        |
| **인덱싱 기법**            | 다양한 기법 사용 가능                                        | GIN 및 GiST와 같은 고급 유형 제공       |
| **SQL 데이터 유형**        | 표준 유형 사용 가능                                         | 더 다양하고, 배열, hstore 포함         |
| **저장 프로시저**           | 지원 있음                                               | PL/pgSQL 언어로 더욱 고급화           |
| **트리거**               | 지원 있음                                               | 유연하고 다국어 지원                   |
| **뷰**                 | 지원 있음                                               | 구체화된 뷰 제공                     |

**OAuth 2.0**:

* OAuth 2.0은 인터넷 사용자가 하나의 서비스(예: Google, Facebook)의 자격 증명을 사용하여 다른 서비스에 안전하게 액세스할 수 있게 하는 표준입니다. 이는 사용자의 비밀번호를 공개하지 않고 제한된 액세스 권한을 부여합니다.

**Bearer Token 옵션**:

* Bearer Token은 OAuth 2.0에서 사용되는 인증 방식 중 하나입니다. 클라이언트가 서버에 요청을 보낼 때, HTTP 헤더에 'Bearer'라는 단어와 함께 토큰을 포함시켜 서버에 제출합니다. 이 토큰은 사용자의 인증 정보를 나타냅니다. ex) Authorization : Bearer ekcasdadsad21312321das

**SecurityFilterChain**:

#### SecurityFilterChain

Spring Security는 간단한 설정을 통해 기본 인증을 비활성화하고, 우리가 원하는 인증 절차를 추가할 수 있다.

WebSecurityConfig 클래스를 만들어서 인증되지 않은 사용자가 접근할 수 없게 세팅하자(인가). 기존에는 인증되지 않은 경우 401 Unauthorized 에러(인증되지 않음)가 발생했지만, 이제부터는 403 Forbidden 에러(권한 없음)가 발생하게 된다. 기본 인증을 사용한다고 세팅하지 않았기 때문에, 자연스럽게 기본 인증이 비활성화된다.

```
@Configuration
@EnableWebSecurity
public class WebSecurityConfig {
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http)
            throws Exception {
        http.authorizeHttpRequests().anyRequest().authenticated();

        return http.build();
    }
}
```

SecurityFilterChain은 요청이 처리되는 동안 다양한 보안 필터를 순서대로 커스텀하게적용하는 메커니즘입니다. 이 체인을 통해 인증 및 권한 부여 과정을 관리합니다.

&#x20;@Configuration&#x20;

클래스가 애플리케이션의 설정 정보를 포함하고 있음을 나타냅니다. 이는 보안 구성을 포함한 다양한 설정을 정의하는 데 사용됩니다.

**@EnableWebSecurity**:

이 어노테이션은 Spring Security 설정을 활성화시키며, 보안 관련 커스텀 설정을 적용할 때 사용됩니다.

1. **Filter vs OncePerRequestFilter**:
   * 'Filter'는 요청 및 응답에 대한 전처리와 후처리를 담당합니다. Spring에서 'OncePerRequestFilter'는 이름에서 알 수 있듯이, 하나의 요청에 대해 단 한 번만 실행됩니다.&#x20;

### OncePerRequestFilter

SecurityFilterChain은 HTTP 요청에 대해 여러 Filter를 먼저 실행할 수 있게 만들어져 있다. 서블릿   필터를 간단히 쓸 수 있도록 Spring Web은 `OncePerRequestFilter`라는 추상 클래스를 제공한다. 이렇게 하면 doFilterInternal 메서드 하나만 구현하면 된다.

`AccessTokenAuthenticationFilter`라는 클래스를 만들자. 여기선 일단 기본 코드만 쓰고 아무 것도 하지 않겠다. (주의: 다음 Filter를 실행하는 걸 잊지 말 것!)

```java
@Component
public class AccessTokenAuthenticationFilter extends OncePerRequestFilter {
    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain)
            throws ServletException, IOException {
        // TODO: 인증 처리

        **filterChain.doFilter(request, response);**
    }
}
```

아직 인증 처리하는 부분을 만들진 않았지만, 이 Filter를 SecurityFilterChain에 추가하자. 우리의 인증 필터를 기본 인증 필터보다 먼저 실행되도록 추가하면 된다.

```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig {
    private final AccessTokenAuthenticationFilter authenticationFilter;

    public WebSecurityConfig(
            AccessTokenAuthenticationFilter authenticationFilter) {
        this.authenticationFilter = authenticationFilter;
    }

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http)
            throws Exception {
        **http.addFilterBefore(
                authenticationFilter, BasicAuthenticationFilter.class);**

        http.authorizeHttpRequests().anyRequest().authenticated();

        return http.build();
    }
}
```

우리는 모든 요청에 대해 인증된 사용자만 인가하도록 설정을 했고, Filter에서 인증됐음을 알리는 방법을 알아내야 한다.

Spring Security는 인증 여부 등의 정보를 관리하고 Controller 등에 전달할 수 있도록 SecurityContext를 사용한다. 여기엔 `Authentication` 객체를 넣어줄 수 있는데, `UsernamePasswordAuthenticationToken` 클래스를 사용하면 간단히 `Authentication` 객체를 만들 수 있다.

AccessTokenAuthenticationFilter의 doFilter 메서드에서 인증 처리를 해보자.

```java
@Override
protected void doFilterInternal(HttpServletRequest request,
                                HttpServletResponse response,
                                FilterChain filterChain)
        throws ServletException, IOException {
    Authentication authentication =
            UsernamePasswordAuthenticationToken.authenticated(
                    "User", null, List.of());

    SecurityContextHolder.getContext()
            .setAuthentication(authentication);

    filterChain.doFilter(request, response);
}
```

`Authentication` 객체는 Principal, Credentials, Authorities로 구성되는데, 일반적으론 Principal로 사용자를 지정하고, Authorities로 권한(을 묘사하는 역할)을 지정한다.

무조건 인증이 되게 했으니 이번에는 올바르게 인증되지 않은 경우에 대한 테스트가 깨진다. 이제 제대로 인증하게 만들어 보자.

### AccessTokenService

Access Token을 이용해 인증하기 위해 우리는 다음과 같은 작업을 추가해야 한다:

1. HTTP 헤더에서 Access Token을 얻는다.
2. Access Token이 올바른지 확인하고, 이를 통해 사용자가 누구인지 알아내 Authentication 객체를 만든다.
3. 올바르게 인증이 됐다면 SecurityContext에 Authentication 객체를 추가한다.

* **SecurityContext**:  SecurityContext는 인증된 사용자의 세부 정보를 저장합니다. 이 컨텍스트를 통해 애플리케이션 내에서 현재 사용자의 인증 정보에 접근할 수 있습니다.

AccessTokenAuthenticationFilter에서 HTTP 헤더를 분석하고, AccessTokenService를 이용해 Authentication 객체를 얻는다.

```
@Component
public class AccessTokenAuthenticationFilter extends OncePerRequestFilter {
    private static final String AUTHORIZATION_PREFIX = "Bearer ";

    private final AccessTokenService accessTokenService;

    public AccessTokenAuthenticationFilter(
            AccessTokenService accessTokenService) {
        this.accessTokenService = accessTokenService;
    }

    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain)
            throws ServletException, IOException {
        String accessToken = parseAccessToken(request);

        Authentication authentication = accessTokenService
                .authenticate(accessToken);

        SecurityContextHolder.getContext()
                .setAuthentication(authentication);

        filterChain.doFilter(request, response);
    }

    private static String parseAccessToken(HttpServletRequest request) {
        return Optional.ofNullable(request.getHeader("Authorization"))
                .filter(i -> i.startsWith(AUTHORIZATION_PREFIX))
                .map(i -> i.substring(AUTHORIZATION_PREFIX.length()))
                .orElse("");
    }
}
```

