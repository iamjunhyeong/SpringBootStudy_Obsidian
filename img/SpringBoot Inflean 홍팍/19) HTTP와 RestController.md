
상태 코드
![[Pasted image 20240309140837.png]]

### JSON 구조

![[Pasted image 20240309140905.png]]

##### JSON URL
![[Pasted image 20240309141106.png]]

### @RestController
Rest API용 컨트롤러, JSON 반환

일반적인 `Controller`는 View 템플릿페이지를 반환한다, `RestController`는 `JSON url`를 받고 `ResponseEntity`를 반환하기 위한 `Controller`이다.

![[Pasted image 20240309141122.png]]
```
// /api/RestApiController.java

@RestController  
public class FirstApiController {  
  
	@GetMapping("/api/hello")  
	public String hello() {  
		return "hello world!";  
	}  
}
```
해당 url은 `hello world!`를 띄우고있다.

#### Talent Api tester에서 GET해보기

`http://localhost:8080/api/hello`
⭐ 이때 우리는 HTTPS가 아닌  HTTP 이므로  수정해준다.
![[Pasted image 20240309141932.png]]

### RestController와 Controller 차이

반환 타입이 다른것을 확인할 수 있다.
![[Pasted image 20240309142201.png]] 일반 Controller의 반환

### RestAPI - GET

전체 조회 & 아이디 하나 조회
```

    @Autowired // DI
    private ArticleRepository articleRepository;

    // GET
    @GetMapping("/api/articles")
    public List<Article> index() {
        return (List<Article>) articleRepository.findAll();
    }

    @GetMapping("/api/articles/{id}")
    public Article index(@PathVariable Long id) {
        return articleRepository.findById(id).orElse(null);
    }
```

### RestAPI - POST
### @RequestBody
- Requset의 Body에서 값을 받아온다.
- JSON형태로 POST로 데이터를 생성하려고 할때 Controller에서 사용하여 값을 받아야 제대로 나옴.

```
    // POST
    @PostMapping("/api/articles")
    public Article create(@RequestBody ArticleForm dto) {

        Article article = dto.toEntity();
        return articleRepository.save(article);
    }
```


### RestAPI - PATCH

```
    // PATCH
    @PatchMapping("/api/articles/{id}")
    public ResponseEntity<Article> update(@PathVariable Long id, @RequestBody ArticleForm dto) {

        // 1. 수정용 엔티티 생성
        Article article = dto.toEntity();
        log.info("id: {}, article: {}", id, article.toString());

        // 2. 대상 엔티티 조회
        Article  target = articleRepository.findById(id).orElse(null);

        // 3. 잘못된 요청 처리
        if (target == null || id != article.getId()) {
            // 400, 잘못된 요청 응답
            log.info("잘못된 요청 id {}, article {}", id, article.toString());
            return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(null);
        }

        // 4. 업데이트 및 정상 응답
        target.patch(article);
        Article updated = articleRepository.save(target);
        return ResponseEntity.status(HttpStatus.OK).body(updated);
    }
```

### RestAPI - DELETE
```
    // DELETE
    @DeleteMapping("/api/articles/{id}")
    public ResponseEntity<Article> delete(@PathVariable Long id) {
        // 대상 찾기
        Article target = articleRepository.findById(id).orElse(null);

        // 잘못된 요청 처리
        if (target == null) {
            return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(null);
        }

        // 대상 삭제
        articleRepository.delete(target);
        return ResponseEntity.status(HttpStatus.OK).build();
    }
```
