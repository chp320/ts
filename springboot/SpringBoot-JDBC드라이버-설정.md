# JDBC 드라이버
스프링 부트 애플리케이션에서 jdbc 드라이버로 DB에 직접 연결 시, pom.xml 파일에 의존성 추가 필요
``` xml
<dependencies>
...
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-jdbc</artifactId>
	</dependency>
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
	</dependency>
...
</dependencies>
```
  - 그런데 아래 메시지와 함께 오류가 뜨는 경우가 있다.

> 'dependencies.dependency.version' for mysql:mysql-connector-java:jar is missing.


* 최근 의존성 관련해서 MySQL JDBC 드라이버가 mysql:mysql-connector-java 에서 com.mysql:mysql-connector-j 로 변경되었다고 하니 아래와 같이 수정하자!
``` xml
<dependencies>
...
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-jdbc</artifactId>
	</dependency>
	<dependency>
		<groupId>com.mysql</groupId>
		<artifactId>mysql-connector-j</artifactId>
	</dependency>
...
</dependencies>
```

- 아니면 version을 명시하는 것도 방법일 수 있지만 권장하지 않음

``` xml
<dependencies>
...
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>8.0.33</version>
		<scope>runtime</scope>
	</dependency>
...
</dependencies>
```


관련 문서: [codejava.net][refLink]

[refLink]: https://codejava.net/frameworks/spring-boot/mysql-connector-java-dependency-version-is-missing


