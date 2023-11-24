# 스프링 하위 프레임워크

## 의미
- 스프링 하위 프레임워크는 주로 스프링 프레임워크의 여러 기능을 모듈화하고 제공하는 것을 의미
1. Spring MVC : 웹 애플케이션에서 모델-뷰-컨트롤러 아키텍처를 구현하는데 사용됨 (일반적인 스프링 의미)
2. Spring Data : 데이터 액세스 기술을 지원하며, JAP/MongoDB/Redis 등과 같은 다양한 데이터 저장소에 대해 간소화된 데이터 액세스 기능을 제공함
3. Spring Security : 보안 기능을 제공하여 인증(Authentication) 및 권한 부여(Authorization)를 관리함
4. Spring Batch : 대규모 배치 프로세싱을 위한 기능을 제공함
5. Spring Integration : 엔터프라이즈 시스템 간의 메시지 기반 통합을 지원하는 프레임워크

### 참고1. 스프링 부트
- 스프링 부트(Spring Boot)는 스프링(Spring) 프레임워크의 일부가 아닌 별도의 프로젝트로 독립적으로 존재
- 스프링 부트는 스프링 대비 쉬운 설정, 내장 서버, 자동 구성 등을 통해 빠르게 프로젝트를 시작하고 실행할 수 있도록 지원

### 참고2. 스프링 하위 프레임워크 사용
- 스프링 하위 프레임워크를 사용 시 별도 라이브러리 설치는 불필요하며,
- '모듈' 형태로 제공되기 때문에 필요한 모듈(ex. Spring Security, Spring Data, ..)을 프로젝트에 포함시키면 된다.
- 일반적으로 프로젝트 빌드 툴(ex. Maven, Gradle, ..)을 통해 "의존성" 추가
```
// 스프링 부트에서 스프링 시큐리티 Gradle에 의존성 설정 (build.gradle 파일)
dependencies {
  implementation 'org.springframework.boot:spring-boot-starter-security'
}
```

