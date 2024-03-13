
# AOP란?

AOP (Aspect-Oriented Programming)는 소프트웨어 개발에서 관심사의 분리를 위해 사용되는 프로그래밍 패러다임 중 하나입니다. 관심사의 분리란, **프로그램의 기능 구현과 이를 지원하는 부가 기능의 구현을 분리하는 것을 의미**합니다.

**OP는 핵심 기능 구현 코드에 영향을 주지 않으면서, 부가 기능을 추가할 수 있도록 해준다.

 **AOP는 Cross-cutting concern(공통 관심사)라고 불리는 부가 기능들을 분리하여 구현하고, 이를 필요한 시점에 핵심 기능 구현 코드와 결합한다

AOP의 구현 방법 중 하나는, 프로그램 코드에 직접 코드를 삽입하는 것이 아니라, 특정 Pointcut(적용 대상)과 Advice(부가 기능)을 정의하고, 이를 Aspect(관심 모듈)라는 단위로 묶어서 사용하는 것이다. 

Aspect는 공통적으로 사용되는 부가 기능들을 모아놓은 것으로, 애플리케이션 전체에서 반복적으로 사용되는 기능들을 Aspect로 정의하고, 필요한 곳에서 적용할 수 있다.

AOP는 **로깅, 보안, 트랜잭션 관리** 등 다양한 분야에서 활용되며, 코드의 가독성과 유지보수성을 높여줍니다. 또한, AOP를 사용하면, 중복 코드를 줄일 수 있고, 개발 생산성을 높일 수 있다.

대표적인 예로 `@Transactional`있다.




특정 로직을 주입한다!
![[Pasted image 20240313143216.png]]


**주요 어노테이션**
![[Pasted image 20240313143649.png]]


먼저 이번 테스트에서는 h2db를 사용한다
```
# 09강: h2 DB, 웹 콘솔 설정
spring.h2.console.enabled=true
# 15강: data.sql 적용을 위한 설정(스프링부트 2.5 이상)
spring.jpa.defer-datasource-initialization=true
# 17강: JPA 로깅 설정
## 디버그 레벨로 쿼리 출력
logging.level.org.hibernate.SQL=DEBUG
## 이쁘게 보여주기
spring.jpa.properties.hibernate.format_sql=true
## 파라미터 보여주기
logging.level.org.hibernate.type.descriptor.sql.BasicBinder=TRACE
## 고정 url 설정
## 30강
spring.datasource.url=jdbc:h2:mem:testdb
# 28강: PostgreSQL 연동
#spring.datasource.url=jdbc:postgresql://localhost:5432/firstproject_db
#spring.datasource.username=postgres
#spring.datasource.password=postgres
spring.datasource.data=classpath:/data.sql
## 30강
spring.datasource.initialization-mode=always
spring.jpa.hibernate.ddl-auto=create-drop
```


## 기존 코드의 문제점

```
...
@Slf4j
@Service
public class CommentService {

    @Autowired
    private CommentRepository commentRepository;

    @Autowired
    private ArticleRepository articleRepository;
    ...
    @Transactional
    public CommentDto create(Long articleId, CommentDto dto) {
        log.info("입력값 => {}", articleId);
        log.info("입력값 => {}", dto);

        // 게시글 조회(혹은 예외 발생)
        Article article = articleRepository.findById(articleId)
                .orElseThrow(() -> new IllegalArgumentException("댓글 생성 실패! 대상 게시글이 없습니."));

        // 댓글 엔티티 생성
        Comment comment = Comment.createComment(dto, article);

        // 댓글 엔티티를 DB로 저장
        Comment created = commentRepository.save(comment);

        // DTO로 변경하여 반환
//        return CommentDto.createCommentDto(created);
        CommentDto createdDto = CommentDto.createCommentDto(created);
        log.info("반환값 => {}", createdDto);
        return createdDto;
    }
    ...
}
```

다음과 같이 메소드마다 로그를 계속 찍어야하는 번거로움이 있다.


## 입력값 로깅 AOP

#### @Aspect
AOP 클래스 선언 : 부가 기능을 주입하는 클래스

#### @Component 
Ioc 컨테이너가 해당 객체를 생성 및 관리

#### @Before("cut()")
실행 시점 설정 -> cut()의 대상이 수행되기 이전에 실행한다!

#### joinPoint

##### getArgs()

##### getTarget()

##### getSignature()


```
package com.example.springboot_hongpark.aop;

import lombok.extern.slf4j.Slf4j;
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.stereotype.Component;

@Aspect // AOP 클래스 선언 : 부가 기능을 주입하는 클래스
@Component // Ioc 컨테이너가 해당 객체를 생성 및 관리
@Slf4j
public class DebuggingAspect {

    // 대상 메소드 선택: CommentService#create()
    @Pointcut("execution(* com.example.springboot_hongpark.service.CommentService.create(..))")
    private void cut() {}

    // 실행 시점 설정: cut()의 대상이 수행되기 이전
    @Before("cut()")
    public void loggingArgs(JoinPoint joinPoint) { // cut() 의 대상 메소드

        // 입력값 가져오기
        Object[] args = joinPoint.getArgs();

        // 클래스명
        String className = joinPoint.getTarget()
                .getClass()
                .getSimpleName();

        // 메소드명
        String methodName = joinPoint.getSignature()
                .getName();

        // 입력값 로깅하기
        // CommentService#create()의 입력값 => 5
        // CommentService#create()의 입력값 => CommentDto(id=null, ...)
        for (Object obj : args) {
            log.info("{}#{}의 입력값 => {}", className, methodName, obj);
        }
    }
}
```


## 반환값 로깅 AOP

#### @AfterReturning
실행 시점 설정 : cut()에 지정된 대상 호출 성공 후이다

```
    // 실행 시점 설정 : cut()에 지정된 대상 호출 성공 후!
    @AfterReturning(value = "cut()", returning = "returnObj")
    public void loggingReturnValue(JoinPoint joinPoint,     // cut()의 대상 메소드
                                   Object returnObj) {      // 리턴값
        // 클래스명
        String className = joinPoint.getTarget()
                .getClass()
                .getSimpleName();

        // 메소드명
        String methodName = joinPoint.getSignature()
                .getName();

        // 반환값 로깅
        // CommentService#create()의 반환값 => CommnetDto(id=10, ...)
        log.info("{}#{}의 반환값 {}", className, methodName, returnObj);

    }
```


## AOP 대상 범위 변경

