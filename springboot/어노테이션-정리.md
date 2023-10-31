# 어노테이션 정리
## 1. 어노테이션이란?
- 스프링 컨테이너가 관리하는 객체를 보통 '빈(bean)' 이라고 부름
- 이러한 스프링 컨테이너를 설정하는 방법은 1) XML 기반 설정 방법 2) JAVA 기반 설정 방법 이 있으며 '자바 기반 설정' 시 @로 시작하는 '어노테이션'을 통해 조작 가능하다.

## 2. 내용 정리
### @GetMapping
- HTTP GET 요청을 맵핑하는 @RequetMapping 의 축약 형태
  - 아래 예시에서 URI 패턴 "/test/sample" 과 일치하는 요청을 처리하는 welcome() 핸들러 메소드를 맵핑
  - @RequetMapping(value="/welcome", method=RequestMethod.GET) 의 축약형
``` java
@RequestMapping("/test")
public class TestController {
    @GetMapping("/sample")
    public String welcome() {
        return "Hello, World!";
    }
}
```

### @RequestMapping
- 정의된 URI 패턴으로 요청 인입 시 처리할 수 있는 컨트롤러를 맵핑
  - 아래 예시에서 URI 패턴 "/sample" 요청 시 TestController 클래스 호출할 수 있도록 맵핑
``` java
@RequestMapping("/sample")
public class TestController {
    ...
}
```


### @ResponseBody
- 반환 값을 HTTP response의 본문으로 처리하고 HTTP 응답
  - 별다른 설정이 없으면 스프링은 *Controller의 String 타입의 반환 값을 내부적으로 전달해야 하는 경로라고 간주함
  - 예를 들어 상기 <a href="#getmapping">@GetMapping</a> 의 예시에서 return "Hello, World!"; 는 "/Hello, World!" 경로를 핸들러를 통해 찾으려 하며, 당연히 없기 때문에 HTTP 404 에러가 발생함
  - 아래 예시에서 반환 값 "Hello, World!" 는 문자 그대로 화면에 출력됨
``` java
@ResponseBody
public String welcome() {
    return "Hello, World!";
}
```








