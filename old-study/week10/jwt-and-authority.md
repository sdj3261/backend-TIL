# JWT & Authority

### 목차

* JWT
* Claims-based identity
* 대칭키 암호화 / 공개키 암호화
* `@Secured`

\|  JWT

JWT(Json Web Token)는 말그대로 웹에서 사용되는 JSON 형식의 토큰에 대한 표준 규격으로주로 사용자의 인증(authentication) 또는 인가(authorization) 정보를 서버와 클라이언트 간에 안전하게 주고 받기 위해서 사용됩니다.

### JWT 구조 (Json Web Token) <a href="#jwt" id="jwt"></a>

하나의 JWT 토큰은 헤더(header)와 페이로드(payload), 서명(signature) 이렇게 세 부분으로 이루어진다.

```
<헤더>.<페이로드>.<서명>
```

JWT 토큰은 네트워크로 전송되야 하기 때문에 공간을 적게 차지하는 것이 유리하다. 따라서 jwt 토큰의 key는 3글자로 지정하여 표시한다.

아래는 전형적인 JWT 토큰의 디코딩 결과

헤더

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

페이로드

```json
{
  "sub": "1234567890",
  "lat": 1516239022
}
```

JWT에서 자주 사용되는 JSON 키 이름은 다음과 같습니다.

* `sub` 키: 인증 주체(subject)
* `iss` 키: 토큰 발급처
* `typ` 키: 토큰의 유형(type)
* `alg` 키: 서명 알고리즘(algorithm)
* `iat` 키: 발급 시각(issued at)
* `exp` 키: 만료 시각expiration time)
* `aud` 키: 클라이언트(audience)



❓장단점?

JWT는 토큰 자체에 사용자의 정보가 저장되어 있어있기 때문에 서버 입장에서 토큰을 검증만 해주면 됩니다.   반면에 쿠키와 세션을 사용할 때는 서버 단에 로그인한 모든 사용자의 세션을 DB나 캐시(cache)에 저장해놓고 쿠키로 넘어온 세션 ID로 사용자 데이터를 매번 조회해야만 하죠.

따라서 사용자 인증을 위해서 추가로 투자해야하는 인프라 비용을 크게 절감하고 보안에도 유리하다.

\| Claims-based identity

JWT의 Payload 부분은 사용자의 정보Claims을 담고 있어, 사용자의 신원 정보 및 권한을 포함할 수 있습니다.

\|  `@Secured`\
Spring Security에서 특정 메서드나 클래스에 보안 제약을 적용하기 위해 사용되는 어노테이션입니다. 이를 통해 특정 역할(Role)이나 권한(Authority)을 가진 사용자만이 메서드에 접근할 수 있도록 제한할 수 있습니다.

* 사용자의 역할이 "ROLE\_ADMIN"인 경우, 이 정보는 JWT의 Claim으로 포함됩니다.
* 서버 측에서는 JWT를 받고 그 안의 Claims를 해석하여 사용자의 권한을 확인합니다. `@Secured("ROLE_ADMIN")`과 같은 어노테이션이 적용된 메서드는 해당 역할을 가진 사용자만 접근할 수 있도록 제한합니다.

JWT (JSON Web Tokens)는 사용자 인증 정보와 권한을 안전하게 전송하기 위한 메커니즘으로, 다음 세 부분으로 구성됩니다: Header, Payload, Signature.

* **대칭키 암호화 (HMAC)**: JWT의 Signature 부분은 HMAC (Hash-based Message Authentication Code)를 사용하여 생성될 수 있습니다. 이 경우, 토큰의 생성과 검증에 동일한 비밀키를 사용합니다. 대칭키 암호화는 비교적 간단하고 빠르지만, 키 관리가 중요한 이슈가 됩니다.
* **공개키 암호화 (RSA, ECDSA)**: JWT는 또한 RSA나 ECDSA와 같은 공개키 암호화 알고리즘을 사용하여 서명할 수 있습니다. 이 경우, 토큰을 생성할 때는 개인키를 사용하고, 토큰을 검증할 때는 공개키를 사용합니다. 이 방법은 키 관리 측면에서 대칭키 방식보다 안전하지만, 암호화 및 복호화 과정이 더 복잡하고 느릴 수 있습니다.
