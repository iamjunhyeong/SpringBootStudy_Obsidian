

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
#### Burger.java
```
package com.example.springboot_hongpark.objectmapper;

import java.util.List;
import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.ToString;

@AllArgsConstructor
@ToString
@Getter
public class Burger {
    private String name;
    private int price;
    private List<String> ingredients;
}

```

#### BurgerTest.java
```
package com.example.springboot_hongpark.objectmapper;

import static org.junit.jupiter.api.Assertions.*;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import java.util.Arrays;
import java.util.List;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;

class BurgerTest {


    @Test
    public void 자바_객체를_JSON으로_변환() throws JsonProcessingException {
        // 준비
        List<String> ingredients = Arrays.asList("통새우 패티",  "순쇠고기 패티", "토마토", "스파이시 어니언 소스");
        Burger burger = new Burger("맥도날드 슈비버거", 5500, ingredients);
        ObjectMapper objectMapper = new ObjectMapper();

        // 수행
        String json = objectMapper.writeValueAsString(burger);

        // 예상
        String expected = "{\"name\":\"맥도날드 슈비버거\",\"price\":5500,\"ingredients\":[\"통새우 패티\",\"순쇠고기 패티\",\"토마토\",\"스파이시 어니언 소스\"]}";

        // 검증
        assertEquals(expected, json);
        System.out.println(json);
    }
}
```


## JsonNODE 활용하기


