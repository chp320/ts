# 세션 관리
- sticky session
- session clustering
- session clustering usring redis

## sticky session
- was 내 세션 생성
- 다중 환경에서 서버 문제로 기존 서버에서 다른 서버로 재접속될 경우 기존 세션은 사라지므로 고객 사용성 측면에서 불편함 초래
<img src="https://github.com/chp320/ts/assets/47440517/366a7a9e-da8b-4b9a-8a7c-0ae16af1f494" /> <br>
<img src="https://github.com/chp320/ts/assets/47440517/def68597-d88a-456f-b09e-43495f73ec27" />

## session clustering
- 다중 환경의 모든 서버에 세션 정보를 저장시켜서 다른 서버로 재접속되어도 세션을 유지하는 방법
- 단, 모든 서버에서 세션 정보를 가지면서 서버 메모리에 부하 발생 가능성
<img src="https://github.com/chp320/ts/assets/47440517/b76819dd-2479-4edd-b164-de99f7b1463c" />

## session clustering using redis
- 사용자의 접속 서버 경로가 변경되어도 별도 외부에 존재하는 redis에서 세션 관리하므로 사용자 입장에서는 변경을 인지 못함
<img src="https://github.com/chp320/ts/assets/47440517/6fc47e6a-01f1-4eca-9b99-c4350237eaa9" />

<hr>

### log for redis
- 호출~응답까지 약 0.00001 초

```
[2023-11-15 23:13:46:732852]  DEBUG 3821 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : GET "/redis", parameters={}
[2023-11-15 23:13:46:732858]  DEBUG 3821 --- [nio-8080-exec-1] o.s.d.r.core.RedisConnectionUtils        : Fetching Redis Connection from RedisConnectionFactory
[2023-11-15 23:13:46:732859]  DEBUG 3821 --- [nio-8080-exec-1] io.lettuce.core.RedisChannelHandler      : dispatching command AsyncCommand [type=HGETALL, output=MapOutput [output=AsyncCommand [type=HGETALL, output=MapOutput [output={}, error='null'], commandType=io.lettuce.core.protocol.Command], error='null'], commandType=io.lettuce.core.protocol.Command]
[2023-11-15 23:13:46:732860]  DEBUG 3821 --- [nio-8080-exec-1] i.l.core.protocol.DefaultEndpoint        : [channel=0x3cb31b73, /127.0.0.1:51228 -> localhost/127.0.0.1:6379, epid=0x1] write() writeAndFlush command AsyncCommand [type=HGETALL, output=MapOutput [output=[channel=0x3cb31b73, /127.0.0.1:51228 -> localhost/127.0.0.1:6379, epid=0x1], error='null'], commandType=io.lettuce.core.protocol.Command]
[2023-11-15 23:13:46:732860]  DEBUG 3821 --- [nio-8080-exec-1] i.l.core.protocol.DefaultEndpoint        : [channel=0x3cb31b73, /127.0.0.1:51228 -> localhost/127.0.0.1:6379, epid=0x1] write() done
[2023-11-15 23:13:46:732860]  DEBUG 3821 --- [ioEventLoop-4-1] i.l.core.protocol.CommandHandler         : [channel=0x3cb31b73, /127.0.0.1:51228 -> localhost/127.0.0.1:6379, epid=0x1, chid=0x1] write(ctx, AsyncCommand [type=HGETALL, output=MapOutput [output=[channel=0x3cb31b73, /127.0.0.1:51228 -> localhost/127.0.0.1:6379, epid=0x1, chid=0x1], error='null'], commandType=io.lettuce.core.protocol.Command], promise)
[2023-11-15 23:13:46:732861]  DEBUG 3821 --- [ioEventLoop-4-1] i.l.core.protocol.CommandEncoder         : [channel=0x3cb31b73, /127.0.0.1:51228 -> localhost/127.0.0.1:6379] writing command AsyncCommand [type=HGETALL, output=MapOutput [output=[channel=0x3cb31b73, /127.0.0.1:51228 -> localhost/127.0.0.1:6379], error='null'], commandType=io.lettuce.core.protocol.Command]
[2023-11-15 23:13:46:732861]  DEBUG 3821 --- [ioEventLoop-4-1] i.l.core.protocol.CommandHandler         : [channel=0x3cb31b73, /127.0.0.1:51228 -> localhost/127.0.0.1:6379, epid=0x1, chid=0x1] Received: 380 bytes, 1 commands in the stack
[2023-11-15 23:13:46:732861]  DEBUG 3821 --- [ioEventLoop-4-1] i.l.core.protocol.CommandHandler         : [channel=0x3cb31b73, /127.0.0.1:51228 -> localhost/127.0.0.1:6379, epid=0x1, chid=0x1] Stack contains: 1 commands
[2023-11-15 23:13:46:732861]  DEBUG 3821 --- [ioEventLoop-4-1] i.l.core.protocol.RedisStateMachine      : Decode done, empty stack: true
[2023-11-15 23:13:46:732861]  DEBUG 3821 --- [ioEventLoop-4-1] i.l.core.protocol.CommandHandler         : [channel=0x3cb31b73, /127.0.0.1:51228 -> localhost/127.0.0.1:6379, epid=0x1, chid=0x1] Completing command AsyncCommand [type=HGETALL, output=MapOutput [output={[B@74bb61d0=[B@1b7fe2e, [B@435f7b95=[B@390cae5e, [B@7da0036a=[B@444a4146, [B@2e2dafda=[B@75a756a}, error='null'], commandType=io.lettuce.core.protocol.Command]
[2023-11-15 23:13:46:732862]  DEBUG 3821 --- [nio-8080-exec-1] o.s.d.r.core.RedisConnectionUtils        : Closing Redis Connection.
```

### log for non-redis
- 호출~응답까지 약 0.0000002 초

```
[2023-11-15 23:21:48:7297592]  DEBUG 3937 --- [p-nio-80-exec-3] o.s.web.servlet.DispatcherServlet        : GET "/session-info", parameters={}
[2023-11-15 23:21:48:7297593]  DEBUG 3937 --- [p-nio-80-exec-3] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped to com.example.springdemo.common.intellijTool.SessionInfoController#sessionInfo(HttpServletRequest)
[2023-11-15 23:21:48:7297593]  INFO  3937 --- [p-nio-80-exec-3] c.e.s.c.i.SessionInfoController          : session name=email, value=test
[2023-11-15 23:21:48:7297593]  INFO  3937 --- [p-nio-80-exec-3] c.e.s.c.i.SessionInfoController          : sessionId=15D07C699BA9340548182D3006CBE464
[2023-11-15 23:21:48:7297593]  INFO  3937 --- [p-nio-80-exec-3] c.e.s.c.i.SessionInfoController          : maxInactiveInterval=1800
[2023-11-15 23:21:48:7297593]  INFO  3937 --- [p-nio-80-exec-3] c.e.s.c.i.SessionInfoController          : creationTime=Wed Nov 15 23:18:01 KST 2023
[2023-11-15 23:21:48:7297593]  INFO  3937 --- [p-nio-80-exec-3] c.e.s.c.i.SessionInfoController          : lastAccessedTime=Wed Nov 15 23:21:38 KST 2023
[2023-11-15 23:21:48:7297593]  INFO  3937 --- [p-nio-80-exec-3] c.e.s.c.i.SessionInfoController          : isNew=false
[2023-11-15 23:21:48:7297594]  DEBUG 3937 --- [p-nio-80-exec-3] m.m.a.RequestResponseBodyMethodProcessor : Using 'text/html', given [text/html, application/xhtml+xml, image/avif, image/webp, image/apng, application/xml;q=0.9, application/signed-exchange;v=b3;q=0.7, */*;q=0.8] and supported [text/plain, */*, text/plain, */*, application/json, application/*+json, application/json, application/*+json]
[2023-11-15 23:21:48:7297594]  DEBUG 3937 --- [p-nio-80-exec-3] m.m.a.RequestResponseBodyMethodProcessor : Writing ["세션 출력"]
[2023-11-15 23:21:48:7297594]  DEBUG 3937 --- [p-nio-80-exec-3] o.s.web.servlet.DispatcherServlet        : Completed 200 OK
```

##### 참고 사이트
https://developer111.tistory.com/69
