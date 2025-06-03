# Cloudinary

Cloudinary는 클라우드 기반의 미디어 관리 서비스로, 파일 업로드 및 이미지 및 비디오 관리 기능을 제공한다.

#### 기능 및 특징

1. **다양한 파일 형식 지원**: Cloudinary는 이미지, 비디오, 오디오 등 다양한 형식의 파일을 업로드하고 관리할 수 있습니다.
2. **자동 변환 및 최적화**: 업로드된 파일은 자동으로 변환되어 다양한 기기 및 브라우저에서 최적화된 형태로 제공될 수 있습니다.
3. **API 및 SDK 제공**: Cloudinary는 RESTful API와 다양한 프로그래밍 언어(JavaScript, Python, Ruby 등)를 위한 SDK를 제공합니다.
4. **보안**: 안전한 업로드, HTTPS 지원, 개인정보 보호 등 보안 기능을 제공합니다.
5. **저장 및 전송**: 클라우드 기반 스토리지를 사용하여 파일을 저장하고, CDN(Content Delivery Network)을 통해 전 세계적으로 빠른 파일 전송을 지원합니다.

#### 파일 업로드 과정

1. **업로드 설정**: Cloudinary 계정에 로그인하고, API 키 및 기타 필요한 설정을 구성합니다.
2. **클라이언트 사이드 구현**:
   * JavaScript SDK를 사용하여 클라이언트 측에서 직접 파일 업로드를 구현할 수 있습니다.
   * `<input type="file">`을 사용하여 사용자로부터 파일을 받고, Cloudinary의 업로드 API를 호출하여 파일을 업로드합니다.
3. **서버 사이드 구현** (선택적):
   * 서버 사이드에서 파일을 처리한 후 Cloudinary로 전송하는 것도 가능합니다.
   * 스프링과 같은 백엔드 프레임워크에서 Cloudinary의 SDK를 사용하여 서버 측에서 파일 업로드를 처리할 수 있습니다.
4. **응답 및 관리**:
   * 업로드가 완료되면 Cloudinary는 파일 URL, 상태, 메타데이터 등을 포함하는 응답을 반환합니다.
   * Cloudinary 대시보드에서 업로드된 파일을 관리하고 필요한 경우 추가 처리(예: 이미지 크롭, 형식 변환)를 수행할 수 있습니다.

```
import com.cloudinary.Cloudinary;
import com.cloudinary.utils.ObjectUtils;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Service;

@Service
public class CloudinaryService {

    private Cloudinary cloudinary;

    public CloudinaryService(@Value("${cloudinary.cloud_name}") String cloudName,
                             @Value("${cloudinary.api_key}") String apiKey,
                             @Value("${cloudinary.api_secret}") String apiSecret) {
        cloudinary = new Cloudinary(ObjectUtils.asMap(
                "cloud_name", cloudName,
                "api_key", apiKey,
                "api_secret", apiSecret));
    }

}

```

```
public class CloudinaryService {
public Map upload(MultipartFile file) throws IOException {
    return cloudinary.uploader().upload(file.getBytes(), ObjectUtils.emptyMap());
}

```
