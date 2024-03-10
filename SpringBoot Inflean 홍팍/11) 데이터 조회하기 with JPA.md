
C==R==UD
## Read

@PathVariable()
	url주소 Path로 부터 입력된다라는 뜻
```
// ArticleController.java

@GetMapping("/articles/{id}")  
public String show(@PathVariable Long id) {  
	log.info("id = " + id);
	return "";  
}
```

### 1. id로 데이터를 가져옴

findById의 반환타입이 Optional<`T`>이여서 `.orElse(null)`를 붙이도록 해서 오류 없앰.

`orElse(null)` : 데이터가 없는 경우 null 반환

```
// 1. id로 데이터를 가져옴.  
Article articleEntity = articleRepository.findById(id).orElse(null);
```


### 2. 가져온 데이터를 모델에 등록
```
// 2. 가져온 데이터를 모델에 등록.  
model.addAttribute("article", articleEntity);
```


### 3. 보여줄 페이지를 설정
```
// 3. 보여줄 페이지를 설정.
return "articles/show";
```


####
```
	// ArticleController.java
...
	
	@GetMapping("/articles/{id}")
    public String show(@PathVariable Long id, Model model) {
        log.info("id = " + id);

        // 1. id로 데이터를 가져옴.
        Article articleEntity = articleRepository.findById(id).orElse(null);

        // 2. 가져온 데이터를 모델에 등록.
        model.addAttribute("article", articleEntity);

        // 3. 보여줄 페이지 설정.
        return "articles/show";
    }
}
```

### View 생성
`{{#article}}`로 데이터 가져옴.

```
{{>layouts/header}}

<table class="table">
    <thead>
    <tr>
        <th scope="col">ID</th>
        <th scope="col">Title</th>
        <th scope="col">Content</th>
    </tr>
    </thead>
    <tbody>  
    {{#article}}     <--- 데이터 가져옴 --->
        <tr>
            <th>{{id}}</th>
            <td>{{title}}</td>
            <td>{{content}}</td>
        </tr>
    {{/article}}
    </tbody>
</table>

{{>layouts/footer}}
```