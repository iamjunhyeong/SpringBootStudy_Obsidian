[]()[]()[JPA 이란?](https://dbjh.tistory.com/77) 

### JPA -> Hibernate -> Spring Data JPA로 쓰는 이유 
	다른 데이터베이스로 교체가 필요할때 의존성만 교체하면 된다.
	교체하기 용이함.

**Entity 클래스에서는 Setter메소드를 만들지 않는다.**
- 기본 구조는 생성자를 통해 최종값을 채운 후 DB에 삽입하는 것.
- 값 변경이 필요한 경우에는 해당 이벤트에 맞는 public 메소드를 호출하여 변경하는 것
- @Builder : 롬복에서 제공하는 어노테이션. 객체 생성시 가독성 높여줌.
```java
import lombok.Builder;
import lombok.Getter;

@Getter
@Builder
public class Person {
    private String firstName;
    private String lastName;
    private int age;
    private String address;
}

// 사용 예시
public class Example {
    public static void main(String[] args) {
        Person person = Person.builder()
                .firstName("John")
                .lastName("Doe")
                .age(30)
                .address("123 Main St")
                .build();

        System.out.println(person.getFirstName());
        System.out.println(person.getLastName());
        System.out.println(person.getAge());
        System.out.println(person.getAddress());
    }
}

```

- 영속성 컨텍스트
	엔티티를 영구 저장하는 환경. JPA의 엔티티 매니저가 활성화(기본 옵션)된 상태로 트랜잭션 안에서 데이터베이스에서 데이터를 가져오면 영속성 컨텍스트 유지된 상태.
- JPA Auditing
- Hibername
	JPA 구현체의 한 종류. 
- h2
	인메모리 관계형 데이터베이스. 메모리에서 실행되기 때문에 재시작할 때마다 초기화된다. 그래서 테스트 용도로 많이 사용.