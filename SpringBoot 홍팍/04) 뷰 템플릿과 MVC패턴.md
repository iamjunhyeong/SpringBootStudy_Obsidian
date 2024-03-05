뷰템플릿 - 웹페이지 변수
MVC패턴 - 분야별 담당

### View 템플릿

화면을 담당하는 기술
웹페이지를 하나의 틀로 만들고 변수를 넣어 적용
- Controller : 처리 과정 
- Model : 데이터 관리

##### 위치 
- src/main/resources/template/.mustache

##### 머스테치
- 뷰 템플릿을 만들어주는 도구
- 마켓플레이스에서 플러그인 설치필요 

머스테치 변수사용
```
<body>  
	<h1>{{username}}님 반갑갑습니다.</h1>  
</body>
```

---
### Controller

특정 url를 매핑시킨다.

```
import org.springframework.stereotype.Controller;  
import org.springframework.web.bind.annotation.GetMapping;  
  
@Controller  
public class FirstController {  
  
@GetMapping("/hi")  
public String niceToMeetYou() {  
return "greetings";  
}  
}
```

---
### Model

- 모델이 데이터를 만든다. 

- 만든 데이터는 이제 변수로써 View에서 사용할 수 있다.

```  
@GetMapping("/hi")  
public String niceToMeetYou(Model model) {  
	model.addAttribute("username", "junhyeong");  
	return "greetings";  
}
```

