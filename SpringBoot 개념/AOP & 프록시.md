흩어진 코드를 한 곳으로 모아

코드를 작성하지않은 메소드에 적용시키는 방법 

### 다양한 AOP 구현 방법
- 컴파일 A.java-------(AOP)------> A.class (AspectJ)
- 바이트코드 조작 A.java-> A.class ------(AOP)------> 메모리 (AspectJ)
- 프록시 패턴 (스프링 AOP에서 자동으로 빈이 등록될때 이루어짐)
 ---
 
## 프록시 패턴

프록시 
- '대리', '대신'이라는 뜻을 가지며, 프로토콜에 있어서는 **대리 응답** 등에서 사용하는 개념.
- - 클라이언트와 서버 사이에 존재하며, 중계기로서 대리로 통신을 수행하는 것을 `Proxy`라고 하며, 그 중계 기능을 하는 주체를 `Proxy Server`라고 한다.



