
### JPA

- 데이터는 java를 이해하지 못함
- JPA는 JAVA언어를 데이터베이스가 이해하게 함.
- 또한 Entity와 Repository를 통해 편의 제공.

==DTO== -> Entity -> Repository -> ==DB==

![[Pasted image 20240304204622.png]]


### 1. DTO를 엔티티로 변환하기

ArticleForm : DTO 작성 클래스
Article : 엔티티 작성 클래스

```java
@Controller
public class ArticleController {

    @Autowired // 스프링 부트가 미리 생성해놓은 객체를가져다가 자동연결
    private ArticleRepository articleRepository;

    @GetMapping("/articles/new")
    public String newAriticleForm() {
        return  "articles/new";
    }

    @PostMapping("/articles/create")
    public String createArticle(ArticleForm form) {
        System.out.println(form.toString());

        // 1. dto를 entity로 변환
        Article article = form.toEntity();

        // 2. repository에게 entity를 db안에 저장하게 함
        return "";
    }
}
```

### 2. 엔티티 작성

`id`라는 기본키를 추가했다. 

```java
package com.example.springboot_hongpark.entity;

import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.Id;

@Entity  // DB가 해당 객체를 인식 가능!
public class Article {

    @Id  // 대표값..  primary key
    @GeneratedValue // 1, 2, 3, ..... 자동 생성
    private Long id;

    @Column
    private String title;

    @Column
    private String content;

    public Article(Long id, String title, String content) {
        this.id = id;
        this.title = title;
        this.content = content;
    }

    public Article() {}

    @Override
    public String toString() {
        return "Article{" +
                "id=" + id +
                ", title='" + title + '\'' +
                ", content='" + content + '\'' +
                '}';
    }
}
```

### 3. Repository를 통해 엔티티 저장

```java
package com.example.springboot_hongpark.controller;

import com.example.springboot_hongpark.dto.ArticleForm;
import com.example.springboot_hongpark.entity.Article;
import com.example.springboot_hongpark.repository.ArticleRepository;
import org.antlr.v4.runtime.misc.LogManager;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;

@Controller
public class ArticleController {

    @Autowired // 스프링 부트가 미리 생성해놓은 객체를가져다가 자동연결
    private ArticleRepository articleRepository;

    @GetMapping("/articles/new")
    public String newAriticleForm() {
        return  "articles/new";
    }

    @PostMapping("/articles/create")
    public String createArticle(ArticleForm form) {
        System.out.println(form.toString());

        // 1. dto를 entity로 변환
        Article article = form.toEntity();

        // 2. repository에게 entity를 db안에 저장하게 함
        Article saved = articleRepository.save(article);
        System.out.println(saved.toString());
        return "";
    }
}
```

### 4. Repository 작성

## 📝 Repository

- JPA에서의 **DB Layer 접근자**를 의미한다.
- **인터페이스**를 생성 후 `JpaRepository<Entity 클래스, PK 타입>`을 상속하면 기본적인 CRUD 메소드가 자동으로 생성된다.
- `@Repository`를 추가할 필요가 없다.

```java
package com.example.springboot_hongpark.repository;

import com.example.springboot_hongpark.entity.Article;
import org.springframework.data.repository.CrudRepository;

public interface ArticleRepository extends CrudRepository<Article, Long> {

}

```

### 5. 의존성 주입

```java
@Controller  
public class ArticleController {  
  
@Autowired // 스프링 부트가 미리 생성해놓은 객체를가져다가 자동연결  
private ArticleRepository articleRepository;

...

```

