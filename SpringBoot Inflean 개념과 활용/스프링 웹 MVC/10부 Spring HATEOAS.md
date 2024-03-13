### **Spring HATEOAS**

Hypermedia As The Engine Of Application State

- `Server` : 현재 리소스와 연관된 링크 정보를 `Client`에게 제공
- `Client` : 연관된 링크 정보를 바탕으로 리소스에 접근
- 연관된 링크 정보
    - Relation
    - Hypertext Reference

#### 예시
```
// pom.xml
 
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-hateoas</artifactId>
</dependency>
```

```
// Hello.java
 
public class Hello {
    private String prefix;
    private String name;
 
    getter/setter
    toString()
}
```

```
// SampleController.java
 
@RestController
public class SampleController {
    @GetMapping(value="/hello")
    public Resource<Hello> hello() {
        Hello hello = new Hello();
        hello.setPrefix("Hey,");
        hello.setName("Jinseo");
 
        Resource<Hello> helloResource = new Resource<>(hello);
        helloResource.add(linkTo(methodOn(SampleController.class).hello()).withSelfRel());
 
        return helloResource;
    }
}

```

```
// SampleControllerTest.java
 
@RunWith(SpringRunner.class)
@WebMvcTest(SampleController.class)
public class SampleControllerTest {
    @Autowired
    MockMvc mockMvc;
 
    @Test
    public void hello() throws Exception {
        mockMvc.perform(MockMvcRequestBuilders.get("/hello"))
            .andDo(MockMvcResultHandlers.print())
            .andExpect(MockMvcResultMatchers.status().isOk())
            .andExpect(MockMvcResultMatchers.jsonPath("$._links.self").exists())
        ;
    }
}
```

#### 결과
```
// Result
 
{
    "prefix":"Hey,"
    ,"name":"Jinseo"
    ,"_links":
    {
    	"self":{"href":"http://localhost/hello"}
    }
}
```

### **ObjectMapper**

제공하는 리소스를 `JSON`으로 변환할 때 사용하는 인터페이스

`spring.jackson.*` 속성들로 `ObjectMapper`를 커스터마이징 할 수 있다.

### **LinkDiscovers**

`Client`에서 링크 정보를 `Rel`이름으로 찾을 때 사용할 수 있는 `XPath`를 확장해서 만든 HATEOAS용 `Client API`