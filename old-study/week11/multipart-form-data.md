# Multipart Form Data

### 목차

* Multipart FormData
* `@ModelAttribute`

### 강의 정리

애플리케이션 수준에서 꼭 챙겨야 하는 기본 보안 요소 3가지

1. Multipart FormData
2. `@ModelAttribute`

\| Multipart FormData

**1. multipart/form-data 방식**

* **클라이언트 사이드 (자바스크립트)**
  * HTML `<form>` 태그와 `<input type="file">`을 사용하여 사용자가 파일을 선택할 수 있게 합니다.
  * 자바스크립트를 사용하여 이 폼 데이터를 `FormData` 객체로 캡처하고, `fetch` API 또는 `XMLHttpRequest`를 사용하여 서버로 전송합니다.
* **서버 사이드 (스프링)**
  * 스프링 MVC에서는 `@Controller` 또는 `@RestController` 어노테이션을 사용하여 엔드포인트를 정의합니다.
  * `MultipartFile` 인터페이스를 사용하여 클라이언트로부터 전송된 파일 데이터를 받습니다.
  * 서버 측에서는 이 파일 데이터를 처리하고, 저장하거나 다른 작업을 수행합니다.

**2. fetch API 방식**

* **클라이언트 사이드 (자바스크립트)**
  * 파일 선택을 위해 `<input type="file">`을 사용합니다.
  * 선택된 파일을 `FormData` 객체에 추가하고, `fetch` API를 사용하여 서버로 전송합니다.
  * 이때 `headers`에 `Content-Type: multipart/form-data`를 설정할 필요가 없습니다. `FormData`를 사용하면 자동으로 설정됩니다.
* **서버 사이드 (스프링)**
  * 스프링에서의 처리 과정은 `multipart/form-data` 방식과 유사합니다.



#### **서버에서의 처리 방법**

```
@RestController
@RequestMapping("images")
public class ImageController {
    @PostMapping(consumes = MediaType.MULTIPART_FORM_DATA_VALUE)
    @ResponseStatus(HttpStatus.CREATED)
    public String create(
            @ModelAttribute UploadImageDto imageDto
    ) {
        MultipartFile multipartFile = imageDto.image();

        if (multipartFile == null || multipartFile.isEmpty()) {
            return "No image";
        }

        return multipartFile.getOriginalFilename();
    }
}
```

String id = TSID.Factory.getTsid().toString(); File file = new File("data/" + id + ".jpg");

try (OutputStream outputStream = new FileOutputStream(file)) { byte\[] content = multipartFile.getBytes(); outputStream.write(content); }



실제 파일의 바이너리 데이터를 consumes와 @ModelAttribute로 읽어주고 파일 객체를 만들어 서버저장소에 파일을 저장한다.



파일  관련 테스트코드는 아래와 같다.



```
@WebMvcTest(ImageController.class) class ImageControllerTest { 
@Autowired private MockMvc mockMvc;
@Test
@DisplayName("POST /images")
void create() throws Exception {
    String filename = "src/test/resources/files/test.jpg";

    MockMultipartFile file = new MockMultipartFile(
            "image", "test.jpg", "image/jpeg",
            new FileInputStream(filename));

    mockMvc.perform(multipart("/images")
                    .file(file))
            .andExpect(status().isCreated());

    // TODO: 지금은 컨트롤러에서 모두 처리하기 때문에 테스트하기 어렵지만,
    //       다른 객체에 파일 저장 기능을 위임하면 verify 테스트를 써주자.
}
```

}

#### 다른 파일 업로드 방법

1. **Base64 인코딩**
   * 클라이언트에서 파일을 Base64 문자열로 인코딩하고, 이 문자열을 서버로 전송합니다.
   * 서버에서는 이 Base64 문자열을 디코딩하여 실제 파일 형태로 변환합니다.
   * 이 방법은 파일 크기가 클 때 비효율적일 수 있습니다.
2. **WebSocket을 이용한 업로드**
   * 실시간 통신이 필요한 경우, WebSocket을 사용하여 파일 데이터를 전송할 수 있습니다.
   * 클라이언트에서는 파일을 조각내어 WebSocket을 통해 순차적으로 전송합니다.
   * 서버에서는 이 조각들을 수신하여 하나의 파일로 재조립합니다.
3. **Rest API를 통한 스트리밍 업로드**
   * 대용량 파일 업로드의 경우, 파일을 여러 부분으로 나누어 순차적으로 업로드하는 방법입니다.
   * 각 부분은 별도의 HTTP 요청으로 전송되며, 서버에서는 이를 순차적으로 조합합니다.
