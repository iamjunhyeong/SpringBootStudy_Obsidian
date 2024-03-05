
### **Radis (REmote Dictionary Server)**

메모리 기반의 `Key-Value`구조의 비정형 데이터를 저장하고 관리하기 위한 오픈 소스 기반의 비관계형 데이터베이스 관리 시스템

#### 특징
- 오픈 소스 소프트웨어이다.
- 디스크가 아닌 메모리 기반의 데이터 저장소이다.
- No SQL & Cache 솔루션이며 메모리 기반으로 구성된다.
- 명시적으로 삭제, Exprie를 설정하지 않으면 데이터는 영구적으로 보존된다.
- 여러대의 서버 구성이 가능하다.
- 데이터베이스로 사용될 수 있으며 Cache로도 사용될 수 있다.
- 성능은 서버에 따라 다르나 초당 2만 ~ 10만회 수행한다.

#### **주요 커맨드**

- `keys *` : 현재 Key값 확인
- `get {key}` : Key에 해당하는 Value 조회
- `hgetall {key}` : Key값으로 저장된 모든 필드를 조회
- `hget {key} {column}` : Key값으로 저장된 필드를 조회
- [https://redis.io/commands](https://redis.io/commands)


##### 커스터 마이징
`application.properties` 내 `spring.redis.*` 설정

```
// application.properties
 
spring.redis.*=value
```


#### **예제**
#### 1. 의존성 추가
```
// pom.xml
 
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

#### 2. Docker 이용 Redis 저장소 생성

```
// Terminal
 
// 저장소 생성
// 기본 포트 : 6379
$ docker run -p 6379:6379 --name redis_boot -d redis
 
// 저장소 실행
$ docker exec -i -t redis_boot redis-cli
```


#### 3. Account 객체 및 저장소 생성

```
// Account.java
 
@RedisHash("accounts")
public class Account {
    @Id 
    private String id;
    private String username;
    private String email;
 
    getter/setter
}
 
// AccountRepository.java
 
public interface AccountRepository extends CrudRepository<Account, String> {
}
```

#### 4. RedisRunner.java 작성
```
// RedisRunner.java
 
@Component
public class RedisRunner implements ApplicationRunner {
    @Autowired
    StringRedisTemplate redisTemplate;	// Spring Data Redis가 제공하는 RedisTemplate 구현체 
 
    @Autowired
    AccountRepository accountRepository;
 
    @Override
    public void run(ApplicationArguments args) throws Exception {
        // opsForValue() : value 관련 opertaion 들을 제공하는 함수
        ValueOperations<String, String> values = redisTemplate.opsForValue();
        values.set("Jinseo", "jsk");
        values.set("springboot", "2.0");
        values.set("hello", "world");
 
        Account account = new Account();
        account.setEmail("sun7190@naver.com");
        account.setUsername("Jinseo");
        
        // 저장
        accountRepository.save(account);
 
        // 저장 시 발생한 id를 이용해 데이터 조회
        Optional<Account> byId = accountRepository.findById(account.getId());
        System.out.println(byId.get().getUsername());
        System.out.println(byId.get().getEmail());
    }
}
```


