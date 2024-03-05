
### PostgreSQL
```
// pom.xml
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
</dependency>
```

```
// application.properties
// DB 접속정보 설정
spring.datasource.url=jdbc:postgresql://localhost:5432/springboot
spring.datasource.username=jinseo
spring.datasource.password=pass
```

#### PostgreSQL 설치 및 서버 실행 (docker)

```
docker run -p 5432:5432 -e POSTGRES_PASSWORD=pass -e POSTGRES_USER=jun -e POSTGRES_DB=springboot --name postgres_boot -d postgres

docker exec -i -t postgres_boot bash

su - postgres

psql springboot

데이터베이스 조회
\list

테이블 조회
\dt

쿼리
SELECT * FROM account;
```

