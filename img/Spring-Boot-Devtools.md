스프링 부트가 제공하는 Optional한 Tool


의존성 추가
```
// pom.xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
</dependency>
```

**특징**

- 캐시 설정을 개발 환경에 맞게 변경
- Restart : `classpath`에 있는 파일이 변경될 때마다 자동으로 재시작
    - Tomcat Cold Start 보다 빠르다.
    - `spring.devtools.restart.exclude` : Restart 예외 지정
    - `spring.devtools.restart.enabled` : Restart On/Off
- LiveReload : Restart 시 Browser 자동 Refresh
    - Browser Plugin 필요
    - `spring.devtools.liveload.enabled` : LiveReload On/Off
- Global Settings (⭐ *1순위 우선순위* )
    - `~/.spring-boot-devtools.properties` :application.properties 보다 우선순위가 더 높다.
- Remote Application

#### Restart 가 Tomcat Cold Start 보다 빠른 이유

스프링 부트는 Class Loader를 2개 사용한다.

1. Base Loader : Library, Dependency 를 읽는 Loader

2. Restart Loader : Application 을 읽는 Loader

Restart Loader 를 사용하여 Restart가 실행되기 때문에 Tomcat Cold Start 보다 빠르다.