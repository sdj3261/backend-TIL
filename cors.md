# CORS

### 목차
- CORS 란
    - 동일 출처 정책
    - JSONP
    - `Access-Control-Allow-Origin`
- `@CrossOrigin`


### 강의 정리

실제 REST API 사용 경험! <BR>
많은 문제가 생기는데 대표적인 CORS(Cross Origin Resource Sharing) 에러에 대해 살펴보자 <BR>
Same-Origin Policy(동일 출처 정책)<br>
동일 오리진 정책
웹 브라우저가 처리하는 보안 정책으로 얻으려는 리소스의 출처가 현재 요청하려는 페이지와 서버가 다르면 에러가 난다.(포트포함, 이미 서버는 처리 끝냄)<br>

=> 해결 방법 : 서버의 REST API Response 헤더에 Access-Control-Allow-Origin Header를 추가한다.
B/E에서 여기(F/E의 주소)에서 요청한 것은 괜찮아요! 라고 설명해주는것 <br>

JSONP (JSON with Padding):

CORS 이전에 동일 출처 정책 문제를 해결하기 위해 사용되던 방식입니다. <br>
웹 페이지가 다른 도메인의 데이터를 읽을 수 있게 하는 데 사용되는 패턴입니다. <br>
JSONP는 스크립트 태그를 통해 데이터를 요청하며, 응답은 콜백 함수를 실행하는 JavaScript 코드 형식으로 옵니다. <br>
하지만 JSONP는 보안 문제와 제한된 요청 방식(GET만 가능) 때문에 현대의 웹 개발에서는 덜 사용됩니다. <br>


Access-Control-Allow-Origin:

CORS를 구현할 때 사용되는 HTTP 응답 헤더 중 하나입니다.
이 헤더는 특정 원본(출처)이 리소스에 접근하도록 허용하는지를 나타냅니다.
예를 들어, 헤더에 * 값이 설정되면 모든 원본이 리소스에 접근할 수 있습니다. 특정 도메인을 지정하면 해당 도메인만 접근이 허용됩니다.
@CrossOrigin:

Java의 Spring 프레임워크에서 사용되는 어노테이션입니다.
이 어노테이션을 사용하면 Spring 기반의 웹 서비스에서 CORS 설정을 쉽게 할 수 있습니다.
예를 들어, @CrossOrigin(origins = "http://example.com")과 같이 설정하면 http://example.com <BR>도메인만 
해당 리소스에 접근이 가능하게 됩니다.


### 궁금한 점?














***

### 궁금한 점?
