### **WebJAR**

`Client`에서 사용하는 `Web Library`를 JAR 파일로 패키징한 것으로 의존성을 추가하여 사용할 수 있다.


**특징**
- JVM 기반의 웹 애플리케이션에서 클라이언트측 의존성을 손쉽게 관리할 수 있음
- JVM 기반 빌드툴을 사용하여 클라이언트측 의존성을 다운로드
- 사용하고 있는 클라이언트측 의존성을 파악할 수 있음
- 수동적인 종속성을 자동으로 해결하고 선택적으로 RequireJS를 통해 적재
- 메이븐 중앙저장소를 통해 배포된다.
- [JSDELIVR](http://www.jsdelivr.com/) 에서 제공하는 공개 CDN 사용이 가능하다.


*예시*

```
// pom.xml
 
<dependency>
    <groupId>org.webjars.bower</groupId>
    <artifactId>jquery</artifactId>
    <version>3.3.1</version>
</dependency>
```

```
<!-- resources/static/hello.html -->
 
<!DOCTYPE html>
<html>
<head>
    <meta charset='utf-8'>
    <meta http-equiv='X-UA-Compatible' content='IE=edge'>
    <title>Page Title</title>
    <meta name='viewport' content='width=device-width, initial-scale=1'>
    <link rel='stylesheet' type='text/css' media='screen' href='main.css'>
    <script src='main.js'></script>
</head>
<body>
    Hello Static Resource
    <script src="/webjars/jquery/3.3.1/dist/jquery.min.js"></script>
    <script>
        $(function() {
            alert("ready!");
        });
    </script>
</body>
</html>
```



### WebJAR의 버전 명시를 생략
> `pom.xml`에 `webjars-locator-core`의존성을 추가한 후 /webjars/jquery/3.3.1/dist/jquery.min.js`를 `/webjars/jquery/dist/jquery.min.js`로 변경하면 된다.

