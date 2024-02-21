## Markdown

### 정의

- 일반 텍스트 문서에 서식 요소를 추가하는 데 사용할 수 있는 가벼운 마크업 언어

### 장점

1. 간결성
2. 별도의 도구없이 작성가능
3. 다양한 형태로 변환이 가능
4. 텍스트로 저장 → 가벼움 + 버전 관리 시스템을 이용 가능 (변경 이력 관리 가능)

### 단점

1. 표준의 부재 → 도구에 따라 변환 방식이나 생성물에 차이
2. 모든 HTML 마크업 대체 불가능

### 문법

- Heading
    
    # H1
    ## H2
    ### H3
    
- Bold
    
    **bold**
    
- Italic
    
    *italic*
    
- BockQuote
    
    > blockquote
    
- Ordered List
    
    
    1. First item
    2. Second item
    3. Third item
    
    
- Unorderd List
    
    
    - First item
    - Second item
    - Third item
    
    
- Code
    
    ``` code ``` 
    
- Horizonatal
    
    ---
    
- Link
    
    [title](https://www.example.com)
    
- Image
    
    ![alt text] (image.jpg)
    

### 참조

[https://www.markdownguide.org/](https://www.markdownguide.org/getting-started/)

https://gist.github.com/ihoneymon/652be052a0727ad59601

---

## Swagger

### 정의

- 서버로 요청되는 API 리스트를 HTML 화면으로 문서화하여 테스트 할 수 있는 라이브러리

### Swagger 전 후 차이

- 전
    - 백엔드에서 문서로 일일이 URL 및 Req, Res를 적어서 프론트로 전달
    - API를 변경할 때 마다 레퍼런스 문서를 변경
        - 번거로움
        - 생산성 저하
        - 수기로 작성하기 때문에 에러 발생 가능
- 후
    - API 문서 생성 시 개발자가 문서를 작성하지 않아도 됨
    - 쉽게 테스트 가능

### 사용법

1. yml 파일을 사용
    - 예시 코드
    
    ```yaml
    openapi: 3.0.0
    info:
      title: My API
      version: 1.0.0
    servers:
      - url: https://{env}.example.com/api/v1
        description: Server A
        variables:
          env:
            default: production
            enum:
              - production
              - staging
      - url: <https://api.example-2.com>
        description: Server B
    paths:
      # 현재 API 스펙에서 사용되는 경로들을 추가합니다.
      /users:
        get:
          summary: Get all users
          responses:
            200:
              description: A list of users
              content:
                application/json:
                  schema:
                    type: array
                    items:
                      $ref: '#/components/schemas/User'
    components:
      schemas:
        # 각 객체 타입에 대한 스키마를 정의합니다.
        User:
          type: object
          properties:
            id:
              type: integer
            name:
              type: string
    ```
    
    - 설명
        1. 기본 구조:
            - `openapi`: OpenAPI 사양의 버전을 명시
            - `info`: API 문서의 기본 정보
            - `servers`: API 서버들의 URL과 설명
        
        2. 경로(`paths`): 
        
           - API 엔드포인트 관련 정보
           - 각 경로에서 사용 가능한 HTTP 메서드를 명시 및 요청과 응답 매개변수를 정의
        
        3. 컴포넌트(`components`): 
        
           - 스키마, 응답, 매개변수, 요청 본문, 헤더 등의 재사용 가능한 API 구성 요소를 정의
           - `schemas`: JSON Schema를 사용하여 설명된 복잡한 데이터 타입을 정의
           - `responses`: 공통된 응답 메시지의 상태 코드와 설명
           - `parameters`: API의 여러 엔드포인트에서 재사용 될 수 있는 매개변수
           - `examples`: 응답 및 요청 본문 예시
           - `requestBodies`: 공통된 요청 본문
           - `headers`: 공통된 헤더를 정의
           - `securitySchemes`: API에서 사용되는 보안 방식을 정의
        
        4. 태그(`tags`): 
           - 각 API 엔드포인트를 논리적으로 그룹화하고 이름을 부여하는 데 사용
        
2. 소스 코드 내에서 swagger를 설정(Spring)
    - build.gradle에서 dependency 추가
        
        ```yaml
        dependencies {
          implementation('io.springfox:springfox-boot-starter:3.0.0')
        }
        ```
        
    - 예시 코드
        
        ```java
        @Configuration
        @EnableSwagger2
        public class SwaggerConfig {
          @Bean
          public Docket api() {
            return new Docket(DocumentationType.SWAGGER_2)
                    .select()
                    .apis(RequestHandlerSelectors.any())
                    .paths(PathSelectors.any())
                    .build();
          }
        }
        
        @Api(tags = "스웨거 튜토리얼")
        @RestController
        @RequestMapping("/api/v1")
        public class HelloController {
          @ApiOperation(value = "헬로 1")
          @GetMapping("/hello")
          public ResponseEntity<String> hello() {
            return ResponseEntity.ok("Hello World");
          }
        
          @ApiOperation(value = "헬로 2")
          @ApiResponses(value = {
            @ApiResponse(code = 200, message = "리턴 성공", response = String.class),
            @ApiResponse(code = 500, message = "서버 에러")
          })
          @PostMapping("/hello/{name}")
          public ResponseEntity<String> hello2(
            @ApiParam(value = "이름") @PathVariable String name
          ) {
            if (name.equals("aa")) {
              throw new IllegalArgumentException("유효하지 않은 이름입니다.");
            }
            String body = "Hello, " + name;
            return ResponseEntity.ok(body);
          }
        }
        ```
        
    - 설명
        - **`@EnableSwagger2`:** Swagger 기능을 활성화하는 어노테이션
        - **`@Api`:** 클래스 레벨에서 사용되며, 특정 리소스에 대한 간단한 설명을 제공
        - **`@ApiOperation`:** 메서드 레벨에서 사용되며, API 작업에 대한 짧은 설명을 제공
        - **`@ApiParam`:** API 매개 변수에 대한 메타데이터를 지정
        - **`@ApiResponse`:** 메서드 레벨에 사용되며, API 호출에 대한 가능한 응답 코드와 각 코드에 대한 설명을 제공합니다.
        - **`@ApiImplicitParam(s)`:** API에서 전달되는 암시적인 매개 변수 (헤더, 경로 변수, 쿼리 매개 변수 등)에 대한 설명을 제공

### 참조

https://yozm.wishket.com/magazine/detail/2195/

https://www.youtube.com/watch?v=Q27PGBYmHNA&list=WL&index=8

---
