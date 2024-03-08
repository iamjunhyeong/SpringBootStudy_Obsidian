
JPA repository로 데이터에 접근 한다
![[Pasted image 20240308211230.png]]

## JPA 로깅 설정

#### 디버그 레벨로 쿼리 출력  
`loggin.level.org.hibernate.SQL=DEBUG  `
  
#### 이쁘게 보여주기  
`spring.jpa.properties.hibernate.format_sql=true  `
  
#### 파라미터 보여주기  
`logging.level.org.hibernate.type.descriptor.sql.BasicBinder=TRACE  `
  
## DB URL 고정 설정  

#### 유니크 URL 생성 X  
`spring.datasource.generate-unique-name=false  `
#### 고정 url 설정  
`spring.datasource.url=jdbc:h2:mem:testdb`


