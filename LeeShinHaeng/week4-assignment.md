# 4주차 과제

## 기술 블로그 & GitHub README

### 기술 블로그

- 링크
    - https://velog.io/@snhng
- velog 선택 이유
    - 직관적이고, 빠른 세팅
    - 많은 개발자의 사용
    - 마크다운 언어 지원

### GitHub README

- 링크
    - https://github.com/LeeShinHaeng/LeeShinHaeng/blob/master/README.md
- 참조
    - https://80000coding.oopy.io/865f4b2a-5198-49e8-a173-0f893a4fed45
    - https://velog.io/@somm/Github-readme-꾸미기
    - https://github.com/Kimhanyeol/Kimhanyeol/blob/main/README.md

---

## Logging

### 로그란?

- 개념
  - 런타임 동안 로그 메시지를 기록하고 저장하는 데 사용되는 메커니즘
  - 시스템 운영에 대한 기록

- 목적
  - 디버깅, 시스템 에러 추적, 성능, 문제점 향상 등의 목적으로 사용

- 언제 사용?
  - 로직의 흐름, 예외 등등을 파악하고 싶을 때
  - 주로 예외 상황


### 자바에서의 로그

1. Logger 클래스를 생성한다.
   - 편의를 위해서 모든 레벨의 로그를 저장하는 코드로 작성
   - java.util.logging.Logger 클래스에서 제공하는 getLogger() 메서드를 사용
2. 로그를 생성할 파일을 생성 및 지정
   - 싱글톤 패턴으로 생성자에서 생성
    

### 스프링부트에서의 로그

- 스프링부트는 자바 친화적인 프레임워크지만 Logger 클래스 외에 로그를 남기는 다양한 방식을 지원한다.
- 그중 대표적인 것이 바로 logback!
  - Slf4j의 구현체
- 사용법
  1. 로그 사용
    - 클래스위에 @Slf4j 어노테이션을 붙이기 (Lombok 사용)   
    - 로거 팩토리에서 직접 가져오기
        ```
        private static final Logger log = LoggerFactory.getLogger(Log.class);
  2. Springboot 로그 설정
    1. resourece 디렉터리 밑에 logback-spring.xml을 읽고, 로그를 만듭니다. 
    2. 위의 logback-spring.xml이 없다면, .yml파일 혹은 .properties 파일을 읽고, 로그를 만듭니다. 
    3. 만약 두 개 다 있다면, .yml파일 먼저 읽고, logback-spring.xml 파일에서 추가되는 부분을 사용합니다. 
  3. 세부 설정
    - 레벨 지정
      - TRACE  <  DEBUG  <  INFO  <  WARN  <  ERROR  <  FATAL
    - Appender 
      - 로그의 출력 위치를 지정
    - rollingPolicy
      - max-file-size: 로그 파일 1개의 최대 용량(size)를 설정
      - max-history: 로그 파일을 유지할 기간(일수)을 설정
      - totalSizeCap
    - 로그 파일 이름 설정
      - logging.file.name : 로그 파일의 이름



### 로그의 사용 예시 (스프링, 롬복 사용)

- 입력값 로그로 남기기 (간단히)
```
@Slf4j
public class UserController {
    private final UserService UserService;

    public void saveUser(UserCreateRequest request) {
      UserService.save(new User(request.getName(), request.getAge()));
      log.info("name={}, age={}",request.getName(), request.getAge());
    }
}
```
결과
```
name=aaa, age=20
```

### 참조
- https://berom.tistory.com/213
- https://sdesigner.tistory.com/100
- https://wikidocs.net/163118
- https://loosie.tistory.com/829


---