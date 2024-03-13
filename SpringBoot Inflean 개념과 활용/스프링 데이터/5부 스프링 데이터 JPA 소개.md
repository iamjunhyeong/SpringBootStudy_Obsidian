
### ORM

- `Object-Relational Mapping`의 약자로 객체와 릴레이션을 매핑할 때 발생하는 ==*개념적인 불일치==를 해결* 하는 패러다임
- 객체와 RDBMS 매핑, 객체와 DB 테이블이 매핑을 이루는 것

### **개념적인 불일치**

OOP적 구조(`사물을 추상화시켜 이해하려는 OOP적 사고방식`)와 SQL적 구조(`DataModel을 정형화하여 관리하려는 RDB`) 간의 불일치

**참고**
[http://hibernate.org/orm/what-is-an-orm/](http://hibernate.org/orm/what-is-an-orm/)

### **JPA(Java Persistence API)**

- Java ORM 기술에 대한 표준 인터페이스
- `Java Application`에서 `RDBMS`를 사용하는 방식을 정의한 인터페이스
- `Hibernate` 기반으로 만들어져 있다.

### **스프링 데이터 JPA**

`JPA`를 아주 쉽게 사용할 수 있게 스프링 데이터로 추상화시켜놓은것


**특징**
- `Repository` Bean 자동 생성
- `Query Method` 자동 구현
- `@EnableJpaRepositories` (스프링 부트 자동설정에 포함)

**참고**
[https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#preface](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#preface)
