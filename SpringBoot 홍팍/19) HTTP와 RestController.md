
상태 코드
![[Pasted image 20240309140837.png]]

### JSON 구조

![[Pasted image 20240309140905.png]]

##### JSON URL
![[Pasted image 20240309141106.png]]

### @RestController
Rest API용 컨트롤러, JSON 반환

일반적인 `Controller`는 View 템플릿페이지를 반환한다, `RestController`는 `JSON url`를 받고 `ResponseEntity`를 반환하기 위한 `Controller`이다.

![[Pasted image 20240309141122.png]]
```
// /api/RestApiController.java

@RestController  
public class FirstApiController {  
  
	@GetMapping("/api/hello")  
	public String hello() {  
		return "hello world!";  
	}  
}
```
해당 url은 `hello world!`를 띄우고있다.

Talent Api tester에 가서 GET 해본다.

`http://jsonplaceholder.typicode.com/api/hello`
⭐ 이때 우리는 HTTPS가 아닌  HTTP 이므로  수정해준다.


