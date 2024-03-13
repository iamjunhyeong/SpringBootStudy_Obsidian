`에러 핸들러`

### **Spring MVC ExceptionHandler**

#### 예시
```
// SampleException.java
 
// Custom Exception Class
public class SampleException extends RuntimeException {
    ...
}
```

```
// AppError.java
 
// Custom Error Info Class
public class AppError {
    private String message;
    private String reason;
 
    getter/setter
}
```

```
// SampleController.java
 
@Controller
public class SampleController {
    @GetMapping(value="/hello")
    public String hello() {
        throw new SampleException();    // Error throw
    }
 
    // 해당 Contoller 안에서 SampleException 예외가 발생하면 이 핸들러를 사용하겠다.
    @ExceptionHandler(SampleException.class)
    public @ResponseBody AppError sampleError(SampleException e) {
        AppError appError = new AppError();
        appError.setMessage("error.app.key");
        appError.setReason("I don't know");
        return appError;
    }
}
```

## 결과
```
// Result

{"message":"error.app.key","reason":"I don't know"}
```

## 전역 ExceptionHandler

클래스를 따로 만들고 `@ControllerAdvice` 추가하고 해당 클래스에 `ExceptionHandler`를 정의한다.

```
// 전역 ExceptionHandler
 
@ControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(해당 Exception.class)
    public @ResponseBody AppError sampleError1(해당 Exception e){
        ...
    }
    
    @ExceptionHandler(해당 Exception.class)
    public @ResponseBody AppError sampleError2(해당 Exception e){
        ...
    }
    
    ...
}
```

### **Spring Boot ExceptionHandler**

ExceptionHandler를 커스터마이징 하는 방법

- `ErrorController`타입의 핸들러를 구현한 후 빈으로 등록
- `BasicErrorController`를 상속받아 구현


### **Custom Error Page**

에러가 발생했을 때 응답의 상태값에 따라 다른 웹 페이지를 보여주고 싶을 때 사용

**특징**

- 파일 위치 `src/main/resources/static|templates/error/`
- 정확한 에러코드나 특정 구간 ==에러코드(`4xx, 5xx`)를 명칭==으로 하여 파일 생성
- 더 많은 커스터마이징을 하고 싶다면 `ErrorViewResolver` 구현