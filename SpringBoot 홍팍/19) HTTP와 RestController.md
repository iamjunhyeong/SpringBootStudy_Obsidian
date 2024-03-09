
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

#### Talent Api tester에서 GET해보기

`http://localhost:8080/api/hello`
⭐ 이때 우리는 HTTPS가 아닌  HTTP 이므로  수정해준다.
![[Pasted image 20240309141932.png]]

### RestController와 Controller 차이

반환 타입이 다른것을 확인할 수 있다.
![[Pasted image 20240309142201.png]] 일반 Controller의 반환

### RestAPI - GET


