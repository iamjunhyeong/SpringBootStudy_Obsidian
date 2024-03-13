

JSON을 자바 객체로 변환 -> 자바 객체를 JSON으로 변환

요청 JSON -> DTO
응답 반환 DTO -> JSON

## 디버깅 AOP 적용

**DebuggingAspect**의 적용 범위 변경

service -> api로 변경

```
// 31강 메소드 선택 : api 패키지의 모든 메소드  
@Pointcut("execution(* com.example.springboot_hongpark.api.*.*(..))")
```

![[Pasted image 20240313192506.png]]

commant 추가 시 log가 잘 뜬다


## 객체를 JSON으로
![[Pasted image 20240313192627.png]]



## JSON을 객체로 
![[Pasted image 20240313192707.png]]


