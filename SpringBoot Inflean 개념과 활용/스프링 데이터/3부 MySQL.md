[]()### **지원하는 DBCP**

**DBCP**

`Connection Pool`이라는 공간을 만들어 `Connection`객체들을 담아놓고 차후 사용자가 `DataBase`에 접속을 시도하면 `Connection Pool`에 담겨있는 `Connection` 객체를 하나하나 꺼내주는 방법

**종류**

- HikariCP
- Tomcat CP
- Commons DBCP2

**DBCP 설정 방법**

- `spring.datasource.hikari.*`
- `spring.datasource.tomcat.*`
- `spring.datasource.dbcp2.*`

---
### MySQL

```
// pom.xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```

```
// application.properties
// DB 접속정보 설정
spring.datasource.url=jdbc:mysql://localhost:3306/springboot
spring.datasource.username=jinseo
spring.datasource.password=pass
```

```
// MySQLRunner.java
 
@Component
public class MySQLRunner implements ApplicationRunner {
 
    @Autowired
    DataSource dataSource;
 
    @Autowired
    JdbcTemplate jdbcTemplate;
 
    @Override
    public void run(ApplicationArguments args) throws Exception {
        try(Connection connection = dataSource.getConnection()) {
            String url = connection.getMetaData().getURL();
            String userName = connection.getMetaData().getUserName();
    
            System.out.println("URL : " + url);
            System.out.println("UserName : " + userName);
    
            Statement statement = connection.createStatement();
            String sql = "CREATE TABLE USER(ID INTEGER NOT NULL, NAME VARCHAR(255), PRIMARY KEY (ID))";
            statement.executeUpdate(sql);
        }
 
        jdbcTemplate.execute("INSERT INTO USER VALUES (1, 'Jinseo')");
    }
}
```

`Docker`를 통해 MySQL 서버를 구성하고 `H2`이용한 테스트와 동일하게 실행.