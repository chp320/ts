# Spring Security 구현 오류
- 도서 <스프링 부트 3 백엔드 개발자 되기: 자바 편> 내용 학습 중 스프링 시큐리티 항목에서 오류 발생해서 정리하는 글

## 환경
- SpringBoot: 3.1.5

## 오류 로그
- 교재에서 안내한 코드는 아래와 같다. <a href="https://github.com/shinsunyoung/springboot-developer/blob/8a0841c499848f03828932fdd0e21bc1660d79f5/chapter8/src/main/java/me/shinsunyoung/springbootdeveloper/config/WebSecurityConfig.java#L30" target="_blank">교재 소스 보기</a>

```
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'filterChain' defined in class path resource [com/skyfox83/springbootdeveloper/config/WebSecurityConfig.class]: Failed to instantiate [org.springframework.security.web.SecurityFilterChain]: Factory method 'filterChain' threw exception with message: This method cannot decide whether these patterns are Spring MVC patterns or not. If this endpoint is a Spring MVC endpoint, please use requestMatchers(MvcRequestMatcher); otherwise, please use requestMatchers(AntPathRequestMatcher).
```

* 이 오류는 스프링 시큐리티 버전 문제이다.
* 교재 환경은 스프링 부트 3.0.2 였는데, 나는 3.1.5 버전으로 구성되어 있다. 버전 up이 되면서 authorizeRequests 가 deprecate 되면서 발생한 문제이다.
* 수정은 아래와 같이 하였다.

### 변경 전
``` java
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        return http
                .authorizeRequests()    // 인증, 인가 설정
                    .requestMatchers("/login", "/signup", "/user").permitAll()  // requestMatchers() 에 파라미터로 전달된 url에 대해 설정
                    .anyRequest().authenticated()
                .and()
                .formLogin()    // 폼 기반 로그인 설정
                    .loginPage("/login")    // 로그인 페이지 경로 설정
                    .defaultSuccessUrl("/articles") // 로그인 완료 후 이동할 경로 설정
                .and()
                .logout()   // 로그아웃 설정
                    .logoutSuccessUrl("/login") // 로그아웃 완료 후 이동할 경로 설정
                    .invalidateHttpSession(true)    // 로그아웃 이후 세션 삭제 여부
                .and()
                .csrf().disable()   // csrf 비활성화 (실습용. csrf 공격 방지를 위해서는 활성화하는게 좋음) - token을 사용하는 방식에서는 csrf를 비활성화
                .build();
    }
```
### 변경 후
``` java
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        return http
                .csrf(AbstractHttpConfigurer::disable)
                .authorizeHttpRequests(authorizationManagerRequestMatcherRegistry -> authorizationManagerRequestMatcherRegistry
                        .requestMatchers(
                                new AntPathRequestMatcher("/login"),
                                new AntPathRequestMatcher("/signup"),
                                new AntPathRequestMatcher("/user")
                        ).permitAll()
                        .anyRequest().authenticated()
                )
                .build();
    }
```

#### 요약하면 아래와 같다.
```
- authorizeRequests -> authorizeHttpRequests 로 변경
- requestMatchers()에 인수로 넘긴 항목을 AntPathRequestMatcher()에 단건으로 넘겨 객체 생성
```

## 오류 로그2
- 위와 같이 수정하고 빌드하니 또다른 오류가 발생했다.

```
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'springSecurityFilterChain' defined in class path resource [org/springframework/security/config/annotation/web/configuration/WebSecurityConfiguration.class]: Failed to instantiate [jakarta.servlet.Filter]: Factory method 'springSecurityFilterChain' threw exception with message: This method cannot decide whether these patterns are Spring MVC patterns or not. If this endpoint is a Spring MVC endpoint, please use requestMatchers(MvcRequestMatcher); otherwise, please use requestMatchers(AntPathRequestMatcher).
```

* 이번에도 requestMatcher 사용법으로 인한 오류로 다시 수정해본다.

### 변경 전
``` java
    @Bean
    public WebSecurityCustomizer configure() {
        return (web) -> web.ignoring()
                .requestMatchers(toH2Console())
                .requestMatchers("/static/**");
    }
```
### 변경 후
``` java
    @Bean
    public WebSecurityCustomizer configure() {
        return (web) -> web.ignoring()
                .requestMatchers(toH2Console())
                .requestMatchers(
                        new AntPathRequestMatcher("/img/**"),
                        new AntPathRequestMatcher("/css/**"),
                        new AntPathRequestMatcher("/js/**")
                );
    }
```

## 해결 완료!!


##### 참고 사이트
https://github.com/spring-projects/spring-security/issues/13609

https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/config/annotation/web/builders/HttpSecurity.html#authorizeHttpRequests()

https://github.com/shinsunyoung/springboot-developer/issues/5

https://sennieworld.tistory.com/109

https://velog.io/@shdbtjd8/Spring-This-method-cannot-decide-whether-these-patterns-are-Spring-MVC-patterns-or-not

https://velog.io/@404-nut-pound/Spring-Security-AntPathRequestMatcher-오류-처리

