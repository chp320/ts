# Spring Security 구현 오류
* 도서 <스프링 부트 3 백엔드 개발자 되기: 자바 편> 내용 학습 중 스프링 시큐리티 항목에서 오류 발생해서 정리하는 글
* OAuth 인증 관련 부분은 자주 변경이 있나보다.. 나중에 까먹을지 모르는 나를 위해 정리함

## 환경
* SpringBoot: 3.1.5

## 오류 로그
- 하하하 이번에도 filterChain 관련 오류가 발생했다. 콘솔에서 찍은 로그를 보면 아래와 같다...
```
2023-12-05T14:30:15.567+09:00 ERROR 27038 --- [           main] o.s.b.d.LoggingFailureAnalysisReporter   : 

***************************
APPLICATION FAILED TO START
***************************

Description:

Method filterChain in com.skyfox83.springbootdeveloper.config.WebOAuthSecurityConfig required a bean of type 'org.springframework.security.oauth2.client.registration.ClientRegistrationRepository' that could not be found.


Action:

Consider defining a bean of type 'org.springframework.security.oauth2.client.registration.ClientRegistrationRepository' in your configuration.


Process finished with exit code 1
```

* 결론부터 말하면 상기 오류는.. '들여쓰기 오류' 였다.
  * 보통 아래와 같은 레벨로 yml 파일을 작성해야 하는데, 나의 경우 security: 가 spring: 과 동일한 레벨에 위치해 있었다.. 이를 아래와 동일 레벨로 수정 후 다시 빌드 후 실행하니 정상!!! 😹
``` yml
spring:
  security:
   oauth2:
     client:
       registration:
         google:
           client-id:
           client-secret: 
           scope:
             - email
             - profile
```


### 변경 전
![image](https://github.com/chp320/ts/assets/47440517/b33b353d-a76c-4d6c-a3fa-b7160d9da8d3)

### 변경 후
![image](https://github.com/chp320/ts/assets/47440517/b3980409-5d87-4daf-b5bf-9eb19218ecfd)


