### **HTMLUnit**

`HTML`을 단일 테스트하기 위한 Tool로 `WebClient`를 이용하여 특정 페이지에 요청을 보내고 결과를 받아서 `HtmlPage`라는 인터페이스를 통해 `xml, text`등 여러가지로 가져올 수 있다.

**특징**

- `form`이 있는 경우 `form submit`테스트도 가능하다.
- 특정 브라우저 타입 부여 가능하다.
- html문서 내 요소들을 다양한 메소드를 이용해 가져올 수 있다.

#### 예시
```
// pom.xml
 
<dependency>
    <groupId>org.seleniumhq.selenium</groupId>
    <artifactId>htmlunit-driver</artifactId>
    <scope>test</scope>
</dependency>[]()
 
<dependency>
    <groupId>net.sourceforge.htmlunit</groupId>
    <artifactId>htmlunit</artifactId>
    <scope>test</scope>
</dependency>
```

```
// SampleControllerTest.java
 
@RunWith(SpringRunner.class)
@WebMvcTest(SampleController.class)
public class SampleControllerTest {
    @Autowired
    WebClient webClient;    // WebClient 생성 시 MockMvc 사용
 
    @Test
    public void hello() throws Exception {
        HtmlPage page = webClient.getPage("/hello");
        HtmlHeading1 h1 = page.getFirstByXPath("//h1");
        assertThat(h1.getTextContent()).isEqualToIgnoringCase("Jinseo");
 
    }
}
```

