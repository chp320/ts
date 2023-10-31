# 스프링부트 기동 시 Flyway 관련 오류
## 1. Flyway 란?

## 2. 오류 현상
- MySQL JDBC 드라이버 테스트를 위해 테스트코드 작성 후 실행 시 오류 발생
  - 그 중 아래 Caused by 부분에서 스키마를 찾을 수 없다는 내용이 보임
  > Caused by: org.flywaydb.core.api.FlywayException: Found non-empty schema(s)
- 테스트 환경
  > SpringBoot 2.7 버전 <br>
  > Flyway <br>
  > MySQL 8.1.0 버전 <br>
```
Error starting ApplicationContext. To display the conditions report re-run your application with 'debug' enabled.
2023-10-31 12:00:30.719 ERROR 31879 --- [  restartedMain] o.s.boot.SpringApplication               : Application run failed

org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'flywayInitializer' defined in class path resource [org/springframework/boot/autoconfigure/flyway/FlywayAutoConfiguration$FlywayConfiguration.class]: Invocation of init method failed; nested exception is org.flywaydb.core.api.FlywayException: Found non-empty schema(s) `app_messages` but no schema history table. Use baseline() or set baselineOnMigrate to true to initialize the schema history table.
        at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1804) ~[spring-beans-5.3.30.jar:5.3.30]
        at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:620) ~[spring-beans-5.3.30.jar:5.3.30]
        at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:542) ~[spring-beans-5.3.30.jar:5.3.30]
        at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:335) ~[spring-beans-5.3.30.jar:5.3.30]
        at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:234) ~[spring-beans-5.3.30.jar:5.3.30]
        at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:333) ~[spring-beans-5.3.30.jar:5.3.30]
        at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:208) ~[spring-beans-5.3.30.jar:5.3.30]
        at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:322) ~[spring-beans-5.3.30.jar:5.3.30]
        at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:208) ~[spring-beans-5.3.30.jar:5.3.30]
        at org.springframework.context.support.AbstractApplicationContext.getBean(AbstractApplicationContext.java:1160) ~[spring-context-5.3.30.jar:5.3.30]
        at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:911) ~[spring-context-5.3.30.jar:5.3.30]
        at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:583) ~[spring-context-5.3.30.jar:5.3.30]
        at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.refresh(ServletWebServerApplicationContext.java:147) ~[spring-boot-2.7.17.jar:2.7.17]
        at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:732) ~[spring-boot-2.7.17.jar:2.7.17]
        at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:409) ~[spring-boot-2.7.17.jar:2.7.17]
        at org.springframework.boot.SpringApplication.run(SpringApplication.java:308) ~[spring-boot-2.7.17.jar:2.7.17]
        at org.springframework.boot.SpringApplication.run(SpringApplication.java:1300) ~[spring-boot-2.7.17.jar:2.7.17]
        at org.springframework.boot.SpringApplication.run(SpringApplication.java:1289) ~[spring-boot-2.7.17.jar:2.7.17]
        at app.message.appmessage.AppMessageApplication.main(AppMessageApplication.java:10) ~[classes/:na]
        at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[na:na]
        at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) ~[na:na]
        at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[na:na]
        at java.base/java.lang.reflect.Method.invoke(Method.java:566) ~[na:na]
        at org.springframework.boot.devtools.restart.RestartLauncher.run(RestartLauncher.java:50) ~[spring-boot-devtools-2.7.17.jar:2.7.17]
Caused by: org.flywaydb.core.api.FlywayException: Found non-empty schema(s) `app_messages` but no schema history table. Use baseline() or set baselineOnMigrate to true to initialize the schema history table.
        at org.flywaydb.core.Flyway$1.execute(Flyway.java:153) ~[flyway-core-8.5.13.jar:na]
        at org.flywaydb.core.Flyway$1.execute(Flyway.java:124) ~[flyway-core-8.5.13.jar:na]
        at org.flywaydb.core.FlywayExecutor.execute(FlywayExecutor.java:205) ~[flyway-core-8.5.13.jar:na]
        at org.flywaydb.core.Flyway.migrate(Flyway.java:124) ~[flyway-core-8.5.13.jar:na]
        at org.springframework.boot.autoconfigure.flyway.FlywayMigrationInitializer.afterPropertiesSet(FlywayMigrationInitializer.java:66) ~[spring-boot-autoconfigure-2.7.17.jar:2.7.17]
        at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.invokeInitMethods(AbstractAutowireCapableBeanFactory.java:1863) ~[spring-beans-5.3.30.jar:5.3.30]
        at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1800) ~[spring-beans-5.3.30.jar:5.3.30]
        ... 23 common frames omitted
```

## 3. 확인 사항
### 1) flyway에 대한 설정 누락
- flyway 를 사용하기 위해서는 application.properties 에 설정 정보를 등록해야 하는데 누락되었다.
### 2) SpringBoot 버전 차이
- 스프링부트 버전 2와 그 이후 버전은 application.properties 에 등록 시 차이가 있다.
- 결론부터 말하자면, 버전 2에서는 "spring.flyway." 접두사로 작성해야 하고, 그 이후 버전은 "flyway." 접두사만 사용하면 된다.

참고: [stackoverflow][stackLink]

[stackLink]: https://stackoverflow.com/questions/53172123/flyway-found-non-empty-schemas-public-without-schema-history-table-use-bas

### 3) 그래서 어떻게 등록하면 되는데?
``` properties
// 버전 2
spring.flyway.baseline-on-migrate = true
spring.flyway.baseline-version = 0
// 버전 3
flyway.baseline-on-migrate = true
flyway.baseline-version = 0
```



