# Spring Security 구현 오류
- 도서 <스프링 부트 3 백엔드 개발자 되기: 자바 편> 내용 학습 중 스프링 시큐리티 항목에서 <span style="background-color:#fff5b1">requestMatchers</span>이 발생해서 정리하는 글

## 환경
- SpringBoot: 3.1.5

## 오류 로그
- 교재에서 안내한 코드는 아래와 같다.


[WebSecurityConfig.java](https://github.com/shinsunyoung/springboot-developer/blob/8a0841c499848f03828932fdd0e21bc1660d79f5/chapter8/src/main/java/me/shinsunyoung/springbootdeveloper/config/WebSecurityConfig.java#L30){: target="_blank"}

```
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'filterChain' defined in class path resource [com/skyfox83/springbootdeveloper/config/WebSecurityConfig.class]: Failed to instantiate [org.springframework.security.web.SecurityFilterChain]: Factory method 'filterChain' threw exception with message: This method cannot decide whether these patterns are Spring MVC patterns or not. If this endpoint is a Spring MVC endpoint, please use requestMatchers(MvcRequestMatcher); otherwise, please use requestMatchers(AntPathRequestMatcher).
```

```
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'springSecurityFilterChain' defined in class path resource [org/springframework/security/config/annotation/web/configuration/WebSecurityConfiguration.class]: Failed to instantiate [jakarta.servlet.Filter]: Factory method 'springSecurityFilterChain' threw exception with message: This method cannot decide whether these patterns are Spring MVC patterns or not. If this endpoint is a Spring MVC endpoint, please use requestMatchers(MvcRequestMatcher); otherwise, please use requestMatchers(AntPathRequestMatcher).
```

