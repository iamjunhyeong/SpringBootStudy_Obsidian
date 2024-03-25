# data.sql 오류
> 스프링 부트 2.5 버전 이상에서는 data.sql이 바로 동작하지 않습니다. 이를 해결하려면 application.properties에 다음을 추가하면 됩니다. 

```java
spring.jpa.defer-datasource-initialization=true
```
