# Spring Security 구현 오류
- 도서 <스프링 부트 3 백엔드 개발자 되기: 자바 편> 내용 학습 중 스프링 시큐리티 항목에서 <background>11</background>
- org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'filterChain'

## 환경
- SpringBoot: 3.1.5


```
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'filterChain' defined in class path resource [com/skyfox83/springbootdeveloper/config/WebSecurityConfig.class]: Failed to instantiate [org.springframework.security.web.SecurityFilterChain]: Factory method 'filterChain' threw exception with message: This method cannot decide whether these patterns are Spring MVC patterns or not. If this endpoint is a Spring MVC endpoint, please use requestMatchers(MvcRequestMatcher); otherwise, please use requestMatchers(AntPathRequestMatcher).
```

```
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'springSecurityFilterChain' defined in class path resource [org/springframework/security/config/annotation/web/configuration/WebSecurityConfiguration.class]: Failed to instantiate [jakarta.servlet.Filter]: Factory method 'springSecurityFilterChain' threw exception with message: This method cannot decide whether these patterns are Spring MVC patterns or not. If this endpoint is a Spring MVC endpoint, please use requestMatchers(MvcRequestMatcher); otherwise, please use requestMatchers(AntPathRequestMatcher).
```

