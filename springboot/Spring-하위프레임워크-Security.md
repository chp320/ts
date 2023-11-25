# 스프링 시큐리티
- 스프링 기반의 인증/인가/권한을 담당하는 스프링 "하위 프레임워크"

## 사전 지식
### 인증과 인가
- 인증(authentication): 사용자의 신원을 입증하는 과정으로 사용자가 '누구인지 확인하는 과정'을 의미
- 인가(authorization): 사용자가 시스템을 이용하기 위해 적절한 '권한을 가지고 있는지 확인하는 과정'을 의미

### 스프링 시큐리티
- 보안 관련 옵션을 다수 제공하며
- CSRF 공격, 세션 고정 공격을 방어하고, 요청 헤더도 보안 처리해줌
  - CSRF 공격: 사용자의 권한을 가지고 '특정 동작'을 수행하도록 유도하는 공격
  - 세션 고정 공격: 사용자의 인증 정보를 탈취하거나 변조하는 공격
 
#### 특징
- 스프링 시큐리티는 "필터" 기반으로 동작
- 스프링 시큐리티 필터 구조
<img src="https://github.com/chp320/ts/assets/47440517/271810e5-43bd-4780-bd1b-a3c2cf72baf1" />

* UsernamePasswordAuthenticationFilter
  - 아이디와 패스워드가 넘어오면 인증 요청을 위임하는 '인증 관리자' 역할 수행
* FilterSecurityInterceptor
  - 권한 부여 처리를 위임해 접근 제어 결정을 쉽게하는 '접근 결정 관리자' 역할 수행
* SecurityContextHolder
  - 인증 완료된 후 Authentication 객체를 저장하는 곳
* 그 외 필터에 대한 설명은 아래 도표 참고
<img src="https://github.com/chp320/ts/assets/47440517/bf6a7e78-ab1a-497e-aa24-08aac08ad4ec" />



### 스프링 시큐리티 - 인가 방식
1. 폼 로그인 방식 (기본)
2. OAuth2.0 방식
3. JWT 방식


### Note.
- 스프링 시큐리티에서 사용자 인증/인가 정보는 UserDetails 객체에 담음 -> 이 클래스를 implements 해서 override 하면 됨


### 아이디/패스워드 기반 폼 로그인 인증 절차 예시
<img src="https://github.com/chp320/ts/assets/47440517/7a331345-5cf9-421a-8abf-7e7d28d90f46" />

1. 사용자가 폼에 아이디/패스워드 입력 후 전송 - HTTPServletRequest
   - AuthenticationFilter 가 전송된 값(아이디/패스워드)에 대해 유효성 검사 수행
2. 유효성 검사 끝나면 실제 구현체인 UsernamePasswordAuthenticationToken을 생성
3. 2에서 생성한 '인증용 객체'인 UsernamePasswordAuthenticationToken을 전달
4. AuthenticationProvider에 전달
5. 사용자 정보(아이디)를 UserDetailsService로 전달하며
6. 전달받은 데이터로 DB에 있는 사용자 정보 조회
7. 실제 인증 처리 진행 (입력 정보 - UserDetails 정보 비교)






