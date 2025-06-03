# 로그인 및 회원가입

### 목차

* CSRF(Cross Site Request Forgery)
* 자바 Date vs LocalDateTime
* Epoch Time
* Spring Security PasswordEncoder
* Argon2
* TSID

### 강의 정리

❗강의에서 나오는 코드는 직접 따라 치면서 흐름 알기

\| CSRF

❓CSRF(Cross Site Request Forgery)&#x20;

* CSRF는 공격자가 사용자가 이미 인증된 웹 애플리케이션에 대해 비인가 작업을 수행하도록 하는 공격입니다. 예를 들어, 공격자는 사용자가 로그인한 상태에서 악의적인 링크를 클릭하게 하여 민감한 작업(예: 비밀번호 변경)을 수행할 수 있습니다.
* 웹 애플리케이션은 CSRF 토큰을 사용하여 이러한 공격을 방지할 수 있습니다. CSRF 토큰은 서버에 의해 생성되고 클라이언트 측에서 요청과 함께 전송되어야 하는, 예측할 수 없는 고유한 문자열입니다.

하지만최근 많이 사용하는 REST API 방식은 쿠키나 세션에 의존하지 않는 경향이 크기 때문에 CSRF 공격에 대한 방어 설정을 비활성화시키는 경우가 많다고 합니다. 쿠키 대신에 로컬 스토리지(localStorage)와 요청 헤더(Request Header) 사용하거나, 세션 대신에 JWT(Json Web Token)을 사용하기 때문입니다.&#x20;

\| **자바 Date vs LocalDateTime**

LocalDateTime은 불변성을 보장한다. (Thread-Safe)

이 말은   상태를 변경할 때마다 새로운 객체를 생성하지 않고 기존 객체를 재사용할 수 있어 메모리 사용량이 감소할 수 있습니다.&#x20;

포맷 변경도 쉽고 Date는 timezone 지정이 필수이지만 불필요한 코드 작성이 줄어든다.

```
String string = "2019년 01월 10일";
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy년 MM월 dd일");
LocalDate date = LocalDate.parse(string, formatter);
System.out.println(date);

Request
@Getter
public class HoursRequestDto {

    @JsonFormat(pattern = "yyyy-MM-dd")
    private Date date;

    @JsonFormat(pattern = "HH:mm")
    private Date time;
}
Response
@Getter
public class HoursResponse {

    @JsonFormat(pattern = "yyyy-MM-dd")
    private LocalDate date;
    
    @JsonFormat(pattern = "HH:mm")
    private LocalTime time;
}
```

\| Epoch Time

[https://www.epochconverter.com/](https://www.epochconverter.com/)

#### 시간대 독립성의 이점

1. **일관성**: Unix Timestamp는 UTC를 기준으로 하므로, 전 세계 어디에서나 동일한 순간을 동일한 숫자 값으로 표현합니다. 이는 다른 시간대에 있는 시스템 간에 시간 데이터를 교환할 때 오류나 혼란을 줄여줍니다.
2. **변환 용이성**: Unix Timestamp는 시간대 변환 없이 바로 사용할 수 있으며, 필요한 경우 로컬 시간대로 쉽게 변환할 수 있습니다.
3. **간단한 계산**: 두 시간 사이의 차이를 계산하는 것이 간단해집니다. 두 Unix Timestamp의 차이를 구하면, 그것은 두 시간 사이의 초 단위 차이입니다.

#### 주의할 점

* **로컬 시간 표시**: Unix Timestamp는 UTC를 기준으로 하는 시간입니다. 사용자에게 시간을 표시할 때는 해당 사용자의 로컬 시간대로 변환해야 합니다. 예를 들어, UI에서 사용자에게 특정 이벤트의 시간을 보여줄 때는 사용자의 시간대에 맞게 변환된 시간을 사용해야 합니다.
* **프로그래밍 언어/플랫폼 지원**: 대부분의 현대 프로그래밍 언어와 시스템은 Unix Timestamp와 작업하기 위한 내장 함수나 라이브러리를 제공합니다. 예를 들어, 자바에서는 `java.time` 패키지를 사용하여 Unix Timestamp를 다룰 수 있습니다.

1. **프로그래밍 및 데이터 교환**:
   * Epoch Time은 다양한 프로그래밍 언어와 시스템 간의 시간 데이터 교환에 자주 사용됩니다. 특히, 시간대 차이나 날짜 형식의 차이가 문제가 될 수 있는 상황에서 Epoch Time은 일관된 시간 표현을 제공합니다.
2. **타임스탬프 저장**:
   * 데이터베이스에서 타임스탬프를 저장할 때 Epoch Time을 사용하기도 합니다. 이 방법은 시간 데이터를 간단하고 효율적으로 저장하고 조회할 수 있게 합니다.

**Spring Security PasswordEncoder**:

* Spring Security에서 `PasswordEncoder` 인터페이스는 비밀번호를 안전하게 저장하기 위한 메커니즘을 제공합니다. 이는 비밀번호를 해시하는 데 사용되며, 보통 **단방향 해시 알고리즘**을 사용합니다.
* `PasswordEncoder`는 원본 비밀번호를 안전한 형태로 변환하고, 입력받은 비밀번호가 저장된 해시와 일치하는지 확인하는 기능을 제공합니다.
* Argon2는 비밀번호 해싱을 위한 최신 알고리즘 중 하나입니다. Spring Security가 지원한다.  메모리, CPU, 그리고 병렬 처리 능력을 활용하여 해시를 생성합니다. 매우 안전하다는 평가

