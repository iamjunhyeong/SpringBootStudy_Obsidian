
### **ViewResolver**

**ContentNegotiatingViewResolver**

  들어오는 요청의 `Accept Header`에 따라 응답을 변환할 때 사용하고,
  경우에 따라 `Accept Header`를 제공하지 않을경우 `format`이라는 파라미터명의 값으로 리턴 포맷을 결정한다.

#### Accept Header
	`Browser` 또는 `Client`가 어떠한 타입의 응답을 원한다 라고 서버에게 알려주는것


#### XML 리턴타입으로 할 경우 의존성 추가
```
// pom.xml
 
<dependency>
    <groupId>com.fasterxml.jackson.dataformat</groupId>
    <artifactId>jackson-dataformat-xml</artifactId>
    <version>2.9.6</version>
</dependency>
```


```
// UserControllerTest
 
@RunWith(SpringRunner.class)
@WebMvcTest(UserController.class)
public class UserControllerTest {
    @Autowired
    MockMvc mockMvc;
 
    @Test
    public void createUser_XML() throws Exception {
        String userJson = "{\"username\":\"jinseo\",\"password\":\"12345\"}";
        mockMvc.perform(MockMvcRequestBuilders.post("/users/create")
                .contentType(MediaType.APPLICATION_JSON_UTF8)
                .accept(MediaType.APPLICATION_XML)
                .content(userJson))
            .andExpect(MockMvcResultMatchers.status().isOk())
            .andExpect(MockMvcResultMatchers.xpath("/User/username").string("jinseo"))
            .andExpect(MockMvcResultMatchers.xpath("/User/password").string("12345"))
        ;
    }
}
```


