# 직렬화와 Jackson ObjectMapper

### 목차
- 직렬화(Serialization)
- 마샬링
- JSON

### 강의 정리
객체를 그 자체로 DB에 저장하거나 네트워크로 전송하는 것은 불가능하다. 
객체를 데이터화하여 바이너리라면 Byte Stream, 텍스트라면 XML,JSON을 주로 사용한다.
직렬화와 마샬링은 거의 같은 뜻이지만 Java에서 마샬링은 특수하게 다룬다.

직렬화 VS 마샬링 
- 직렬화는 역직렬화를 통해 객체 또는 데이터의 복사본을 만들 수 있다. 
- 마샬링은 직렬화와 같거나 원격 객체로 복원할 수 있다. 원격 객체의 경우에 메서드 호출은 RPC 가 된다.

JSON(Javascript Object Notation)
Dogulas Crockford가 만든 데이터 포맷으로 사람이 읽기 쉽고 기계도 파싱하기 쉽다. 
대부분 Json.parse(역직렬화) 와 stringify(직렬화)로 안전하게 사용한다.
기본적으로 key-value 쌍으로 자바에서는 Map과 비슷하지만 스키마 관리 및 타입 안전성을 DTO를 사용한다.

- 생성 : DTO(자바) -> Converter -> JSON String
- 해석 : JSON String -> Converter -> DTO

컨버터로 Java에선 Jackson이란 도구가 유명하고 스프링부트는 의존성 추가로 간단하게 사용 가능하다.
@JsonProperty로 속성 이름 지정 가능하고 다른 객체(DTO)를 포함할수 있다.

DTO는 여러곳에서 사용될수 있고 Tier(Remote 환경이 아닌 상황)에서 레이어 사이나 인터페이스에서도 활용되며
데이터 전송이라는 의미면 틀리지 않다.

Jackson ObjectMapper를 통해 DTO <-> JSON 변환하며 필드명은 getter 메서드의 이름을 따르는데에 주의하자
- JSON 형식 예시
```
{
    "id": 1234,
    "title": "제목",
    "content": "test"
}
```



***

### 궁금한 점?
1. @ComponentScan