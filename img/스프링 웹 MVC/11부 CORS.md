
### **SOP & CORS**

- SOP
    - Same Origin Policy 의 약자로, 같은 Origin에서만 접근이 가능한 정책
    - 예) `localhost:8080`에서 `localhost:8000`으로 접근 할 수 없다.
    - 기본적으로 SOP 정책
- CORS
    - Cross-Orgin Resource Sharing 의 약자로, 다른 Origin에서도 접근이 가능한 정책
- Origin
    - `URI Schema(http, https)` + `Hostname` + `Port`

#### 예시

## Server(`http://localhost:8080`)

```
// Application.java
// @CrossOrigin은 Controller나 Method에 추가하거나
// WebMvcConfigurer를 상속받아 구현한 전역 클래스에 추가하여 사용가능
 
@SpringBootApplication
@RestController
// @CrossOrigin(origins = "http://localhost:18080")	// Controller 단
public class Application {
 
	// @CrossOrigin(origins = "http://localhost:18080")	// Method 단
	@GetMapping("/hello")
	public String hello() {
		return "hello";
	}
	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
}
 
// WebConfig.java
 
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        // /** : 모든 요청
        registry.addMapping("/**").allowedOrigins("http://localhost:18080");
    }
}
```

## Client(`http://localhost:18080`)

```
// pom.xml
// jquery 추가
 
<dependency>
    <groupId>org.webjars.bower</groupId>
    <artifactId>jquery</artifactId>
    <version>3.3.1</version>
</dependency>
```

```
// index.html
 
<!DOCTYPE html>
<html>
<head>
    <meta charset='utf-8'>
    <title>Page Title</title>
</head>
<body>
    <h1>CORS Client</h1>
    <script src="/webjars/jquery/3.3.1/dist/jquery.min.js"></script>
    <script>
        $(function () {
            $.ajax("http://localhost:8080/hello")
                .done(function(msg) {
                    alert(msg);
                })
                .fail(function() {
                    alert("fail");
                });
        });
    </script>
</body>
</html>
```