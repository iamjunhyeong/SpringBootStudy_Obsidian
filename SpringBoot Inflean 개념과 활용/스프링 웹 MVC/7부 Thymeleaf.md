### 템플릿 엔진
- View 만들기
- code generation

### **Template Engine**

템플릿 양식과 특정 데이터 모델에 따른 입력 자료를 결합하여 원하는 결과 자료를 출력하는 소프트웨어 또는 컴포넌트를 말한다.

#### **특징**
- 기존 HTML코드에 비해 간단한 문법을 사용해 많은 코드를 줄일 수 있다.
- 재사용성이 높다.
- 유지보수에 용이하다.


#### **스프링 부트가 자동 설정을[]() 지원하는 Template Engine**
- FreeMarker
- Groovy
- ==Thymeleaf==
- Mustache

#### JSP를 권장하지 않는 이유

- JAR 패키징 할 때는 동작하지 않고, WAR 패키징을 해야 동작한다.
- `Undertow`는 JSP를 지원하지 않는다.
- [https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-jsp-limitations](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-jsp-limitations)

## Thymeleaf

비교적 최근에 만들어진 Template Engine으로 `Java Library`를 이용하여 `xml, xhtml, html5`문서를 생성하는 템플릿 엔진이다.

**특징**

- 독자적으로 HTML을 생성하기 때문에 테스트 시 렌더링 결과를 확인하기 좋다.
- HTML Tag 안에 문법을 사용하기 때문에 별도의 코딩이 필요 없다.
- 서버가 아닌 브라우저를 이용해 파일을 열어도 `th`를 무시하기 때문에 화면을 확인 할 수 있다.
- 템플릿 파일 기본 위치 : `/src/main/resources/template/`


#### 예시
```
// pom.xml
 
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```


```java
// SampleController.java
 
@Controller
public class SampleController {
    @GetMapping(value="/hello")
    public String hello(Model model) {
        model.addAttribute("name", "Jinseo");
        return "hello";
    }   
}
```

```java
// SampleControllerTest.java
 
@RunWith(SpringRunner.class)
@WebMvcTest(SampleController.class)
public class SampleControllerTest {
    @Autowired
    MockMvc mockMvc;
 
    @Test
    public void hello() throws Exception {
        // 요청 "/hello"
        // 응답
        // - 모델 name : Jinseo
        // - 뷰 : hello
 
        mockMvc.perform(MockMvcRequestBuilders.get("/hello"))
            .andExpect(MockMvcResultMatchers.status().isOk())
            .andDo(MockMvcResultHandlers.print())
            .andExpect(MockMvcResultMatchers.view().name("hello"))
            .andExpect(MockMvcResultMatchers.model().attribute("name", "Jinseo"))
            .andExpect(MockMvcResultMatchers.content().string(Matchers.containsString("Jinseo")))
        ;
    }
}
```

```java
// hello.html
 
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">⭐⭐⭐
<head>
    <meta charset='utf-8'>
    <meta http-equiv='X-UA-Compatible' content='IE=edge'>
    <title>Page Title</title>
    <meta name='viewport' content='width=device-width, initial-scale=1'>
    <link rel='stylesheet' type='text/css' media='screen' href='main.css'>
    <script src='main.js'></script>
</head>
<body>
    <h1 th:text="${name}">Name</h1>
</body>
</html>
```
