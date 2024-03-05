

## @RequestBody / @ResponseBody 어노테이션
#### **클라이언트와 서버의 비동기 통신** 

![](https://blog.kakaocdn.net/dn/sxcOu/btq4eKsIpSZ/kntlVrm6YznC7PKyBHemH1/img.png)

 클라이언트에서 서버로 통신하는 메시지를 요청(request) 메시지라고 하며, 서버에서 클라이언트로 통신하는 메시지를 응답(response) 메시지라고 한다.

웹에서 화면전환(새로고침) 없이 이루어지는 동작들은 대부분 비동기 통신으로 이루어진다.

비동기통신을 하기위해서는 클라이언트에서 서버로 요청 메세지를 보낼 때, 본문에 데이터를 담아서 보내야 하고, 서버에서 클라이언트로 응답을 보낼때에도 본문에 데이터를 담아서 보내야 한다. 

**이 본문이 바로 body 이다.**

**즉, 요청본문 requestBody, 응답본문 responseBody 을 담아서 보내야 한다.** 

이때 본문에 담기는 데이터 형식은 여러가지 형태가 있겠지만 가장 대표적으로 사용되는 것이 JSON 이다.

즉, 비동기식 클라-서버 통신을 위해 JSON 형식의 데이터를 주고받는 것이다

출처: [https://cheershennah.tistory.com/179](https://cheershennah.tistory.com/179) 

---


### **HttpMessageConverters**

`HTTP 요청 본문 -> 객체` 또는 `객체 -> HTTP 응답 본문`으로 변경할때 사용

```
// 예시
{"username":"jinseo", "password":"123"} -> User Object
 
// User.java
public class User {
    private Long id;
    private String username;
    private String password;
    
    getter/setter
}
```

**특징**

- 요청에 따라 사용하는 `MessageConverter`가 달라진다.
- `Composition Object Type`일 경우 `JsonMessageConverter`사용 `String`일 경우 `StringMessageConverter`사용
- `⭐@RestController`사용 시 `@ResponseBody`생략 가능
- `@Controller`사용 시 `@ResponseBody`생략 할 경우 `MessageConverter`가 미적용 되어 `ViewResolver`에 의해 리턴값에 해당하는 `View`를 찾게된다.

#### Composition Object
	기존 클래스들을 조합해서 새로운 클래스를 만드는것 has-a 관계

