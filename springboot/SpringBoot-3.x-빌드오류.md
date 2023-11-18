# 스프링부트 3.x에서 빌드 시 오류 조치

## 현상
- 스프링부트 3.1.5 에서 프로젝트 생성 후 IntelliJ가 자동으로 빌드 중 무려 600줄에 달하는 방대한 오류를 뱉었다.
```
A problem occurred configuring root project 'springboot-developer'.
> Could not resolve all files for configuration ':classpath'.
   > Could not resolve org.springframework.boot:spring-boot-gradle-plugin:3.1.5.
     Required by:
         project : > org.springframework.boot:org.springframework.boot.gradle.plugin:3.1.5
      > No matching variant of org.springframework.boot:spring-boot-gradle-plugin:3.1.5 was found. The consumer was configured to find a library for use during runtime, compatible with Java 11, packaged as a jar, and its dependencies declared externally, as well as attribute 'org.gradle.plugin.api-version' with value '8.4' but:
          - Variant 'apiElements' capability org.springframework.boot:spring-boot-gradle-plugin:3.1.5 declares a library, packaged as a jar, and its dependencies declared externally:
              - Incompatible because this component declares a component for use during compile-time, compatible with Java 17 and the consumer needed a component for use during runtime, compatible with Java 11
              - Other compatible attribute:
                  - Doesn't say anything about org.gradle.plugin.api-version (required '8.4')
...
(중략)
...
```

## 분석
- 우선 "No matching" 으로 시작하는 로그를 보면 현재 스프링부트 버전을 알 수 있다. (3.1.5)
- 엇.. 그런데 Java 17 어쩌고 적혀 있다. 나는 지금 버전이 11인데??
- 구글링해보니 스프링부트3.0부터는 Java 17부터 지원된다고 한다. 음.. 17버전을 깔아보자.
```
# 지금은 11 버전이다 ㅠㅠ
$ java -version
java version "11.0.15.1" 2022-04-22 LTS
Java(TM) SE Runtime Environment 18.9 (build 11.0.15.1+2-LTS-10)
Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11.0.15.1+2-LTS-10, mixed mode)
```

### 참고 - brew 이용해서 설치 진행
```
# openjdk17 버전이 있는지 확인
$ brew search openjdk@17

# 설치 진행
$ brew install openjdk@17
```
```
# 무수한 콘솔 로그를 찍고 설치 완료 후에 친절한 설명을 준다.
For the system Java wrappers to find this JDK, symlink it with
  sudo ln -sfn /usr/local/opt/openjdk@17/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk-17.jdk

openjdk@17 is keg-only, which means it was not symlinked into /usr/local,
because this is an alternate version of another formula.

If you need to have openjdk@17 first in your PATH, run:
  echo 'export PATH="/usr/local/opt/openjdk@17/bin:$PATH"' >> ~/.zshrc

For compilers to find openjdk@17 you may need to set:
  export CPPFLAGS="-I/usr/local/opt/openjdk@17/include"
```
```
# 심볼릭 링크 작성하고..
$ sudo ln -sfn /usr/local/opt/openjdk@17/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk-17.jdk
```
```
# PATH 설정을 위해 profile 도 수정한다.
$ vi ~/.zshrc

# 아래 내용 추가
export JAVA_HOME=/Library/Java/JavaVirtualMachines/openjdk-17.jdk/Contents/Home
export PATH=${PATH}:$JAVA_HOME/bin
```

```
# 설치 후 자바 버전 확인
$ openjdk version "17.0.9" 2023-10-17
OpenJDK Runtime Environment Homebrew (build 17.0.9+0)
OpenJDK 64-Bit Server VM Homebrew (build 17.0.9+0, mixed mode, sharing)
```

## jdk 17 설치 이후에도 안된다고?
- IntelliJ 설정을 확인하자!
- (Mac/Gradle 기준) Settings > Build,Execution,Deployment > Build Tools > Gradle 메뉴
  - Gradle JVM 이 방금 설치 진행한 jdk 17이상으로 설정되어 있는지 확인. 아니라면 드롭박스 선택해서 17 버전 이상을 선택하자!
- 다시 빌드 진행하고 성공 확인하자!!
```
  BUILD SUCCESSFUL in 21s
```
- 성공 했다 흑흑 ㅠㅠ







