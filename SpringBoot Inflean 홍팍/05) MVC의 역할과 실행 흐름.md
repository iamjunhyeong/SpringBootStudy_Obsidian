
### MVC 역할

- `localhost:8080/hi` 로 요청하면 `Controller` 가 받음.

- 그러면 `hi`와 매핑되어있는 메서드를 실행.

- greetings를 템플릿에서 찾아감.
```java
return "greetings"; // 보여줄 view 페이지
```

- `{{username}}` 변수 사용을 모델을 통해서 등록시킴
```java
model.addAttribute("username", "junhyeong");
```

[]()[]()