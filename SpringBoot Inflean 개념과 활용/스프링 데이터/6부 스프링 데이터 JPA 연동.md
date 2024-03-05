### **Spring Data JPA 연동하기**

### **1. 의존성 추가**

```
// pom.xml
 
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
 
<!-- javax.xml.bind.JAXBException 해결 Java 9 이상에서 발생 -->
<dependency>  
	<groupId>javax.xml.bind</groupId>  
	<artifactId>jaxb-api</artifactId>  
	<version>2.3.0</version>  
</dependency>
 
<!-- Error creating bean with name 'entityManagerFactory' 해결 -->
<dependency>
    <groupId>org.javassist</groupId>
    <artifactId>javassist</artifactId>
    <version>3.27.0-GA</version>
</dependency>
```


### **2. @Entity Class 생성**

`Entity`는 RDBMS 내 데이터 테이블을 객체화 하는 것. 기본적으로 `id`, `getter`, `setter` 가 필요하다.

```
// Account.java

@Entity
public class Account {
    
    @Id @GeneratedValue // 자동으로 생성되는 값
    private Long id;
 
    private String username;
 
    private String password;
 
    getter/setter
    equals()
    hashCode()
}
```

### **3. Repository Interface 생성**

`Entity`의 CRUD 처리를 위한 인터페이스로 `JpaRepository`를 상속받아 구현한다.

```
// AccountRepository.java
 
// JpaRepository<Entity Type, id Type>
public interface AccountRepository extends JpaRepository<Account, Long> {
 
}
```

### **4. Database 연결**

`PostgreSQL`을 사용하고 접속 및 설정 방법은 [[4부 PostgreSQL]]와 같다.

```
// pom.xml
 
<!-- 실사용 데이터베이스 -->
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
</dependency>
```

```
// application.properties
 
// DB 접속정보 설정
spring.datasource.url=jdbc:postgresql://localhost:5432/springboot
spring.datasource.name=jun
spring.datasource.password=pass
```

### Spring Data JPA 테스트

JPA 테스트는 H2 인메모리 데이터베이스를 이용하여 구현한다.

```
// pom.xml
 
<!-- 테스트용 인메모리 데이터베이스 -->
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>test</scope> <!-- 테스트 환경에서만 사용하기 위함 -->
</dependency>
```

```
// AccountRepositoryTest.java
 
@RunWith(SpringRunner.class)
// 슬라이스 테스트 : 현재 AccountRepository 테스트이기 때문에 AccountRepository 관련된 Bean 만 등록하여 테스트를 만드는 것
// 슬라이스 테스트시에는 반드시 인메모리 데이터베이스가 필요하다.
@DataJpaTest
// @SpringBootApplication 찾아서 ComponentScan 다 하고 Application의 모든 Bean 다 등록.
// properties 파일에 설정된 DB 정보로 테스트
// @SpringBootTest
// @SpringBootTest(properties = "spring.datasource.url=~")
public class AccountRepositoryTest {
    @Autowired
    DataSource dataSource;
 
    @Autowired
    JdbcTemplate jdbcTemplate;
 
    @Autowired
    AccountRepository accountRepository;
 
    @Test
    public void di() throws SQLException {
        try (Connection connection = dataSource.getConnection()) {
            // Connection 정보 확인
            DatabaseMetaData metaData = connection.getMetaData();
            System.out.println(metaData.getURL());
            System.out.println(metaData.getDriverName());
            System.out.println(metaData.getUserName());
        }
 
        Account account = new Account();
        account.setUsername("Jun");
        account.setPassword("pass");
 
        Account newAccount = accountRepository.save(account);
        assertThat(newAccount).isNotNull();
    }
}
```

### **Query Method**

**findBy, countBy**

- `findBy~` : `Query Result` 요청하는 Method
- `countBy~` : `Query Result Count` 요청하는 Method

```
// AccountRepository.java
 
public interface AccountRepository extends JpaRepository<Account, Long> {
    Account findByUsername(String username);
}
```

```
// AccountRepositoryTest.java
 
@Test
public void di() throws SQLException {
    try (Connection connection = dataSource.getConnection()) {
        DatabaseMetaData metaData = connection.getMetaData();
        System.out.println(metaData.getURL());
        System.out.println(metaData.getDriverName());
        System.out.println(metaData.getUserName());
    }
 
    Account account = new Account();
    account.setUsername("Jun");
    account.setPassword("pass");
 
    Account newAccount = accountRepository.save(account);
    assertThat(newAccount).isNotNull();
 
    Account existingAccount = accountRepository.findByUsername(newAccount.getUsername());
    assertThat(existingAccount).isNotNull();
 
    Account nonExistingAccount = accountRepository.findByUsername("jsk");
    assertThat(nonExistingAccount).isNull();
}
```

### @Query

JPA를 통한 SQL 작성 시 사용

```
// AccountRepository.java
 
public interface AccountRepository extends JpaRepository<Account, Long> {
    // nativeQuery=true -> value에 SQL 작성, placeholder 이용하여 바인딩
    @Query(nativeQuery = true, value = "SELECT * FROM account WHERE username = '{0}'")
	Account findByUsername(String username);
}
```


### Optional<`T`>

`<T> 타입`의 객체를 포장해 주는 `Wrapper class`로 `Optional Instance`는 모든 타입의 참조 변수를 저장 가능하다.
```
// AccountRepository.java
 
public interface AccountRepository extends JpaRepository<Account, Long> {
	Optional<Account> findByUsername(String username);
}
```

```
// AccountRepositoryTest.java
 
@Test
public void di() throws SQLException {
    try (Connection connection = dataSource.getConnection()) {
        DatabaseMetaData metaData = connection.getMetaData();
        System.out.println(metaData.getURL());
        System.out.println(metaData.getDriverName());
        System.out.println(metaData.getUserName());
    }
 
    Account account = new Account();
    account.setUsername("Jun");
    account.setPassword("pass");
 
    Account newAccount = accountRepository.save(account);
    assertThat(newAccount).isNotNull();
 
    Optional<Account> existingAccount = accountRepository.findByUsername(newAccount.getUsername());
    assertThat(existingAccount).isNotEmpty();
 
    Optional<Account> nonExistingAccount = accountRepository.findByUsername("jsk");
    assertThat(nonExistingAccount).isEmpty();
}
```
