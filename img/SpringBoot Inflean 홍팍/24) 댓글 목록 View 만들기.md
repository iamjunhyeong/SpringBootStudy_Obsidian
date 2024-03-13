상세페이지에서 댓글 목록을 만들도록 한다.


`show.mustache `
```
// 

<a href="/articles/{{article.id}}/edit" class="btn btn-primary">Edit</a>  
<!--GET, a 태그 즉, 링크를 사용 !-->  
<a href="/articles/{{article.id}}/delete" class="btn btn-danger">Delete</a>  
<a href="/articles" class="btn btn-primary">Go to Article List</a>  
  
{{>comments/_comments}}   <-- 추가 -->
...

```

`comments/_comments.mustache`
```
<div>
    <!-- 댓글 목록 -->
    {{>comments/_list}}

    <!-- 새 댓글 작성 -->
    {{>comments/_new}}
</div>
```

`_list.mustache`
```
<div id="comments-list">
    {{#commentDtos}}
        <div class="card m-2" id="comments-{{id}}">
            <div class="card-header">
                {{nickname}}
            </div>
            <div class="card-body">
                {{body}}
            </div>
        </div>
    {{/commentDtos}}
</div>

```


모델에 등록하기 위해 ArticleController에 코드를 추가한다.
`ArticleController.java`
```
    @GetMapping("/articles/{id}")
    public String show(@PathVariable Long id, Model model) {
        log.info("id = " + id);

        // 1. id로 데이터를 가져옴.
        Article articleEntity = articleRepository.findById(id).orElse(null);
        List<CommentDto> commentDtos = commentService.comments(id);

        // 2. 가져온 데이터를 모델에 등록.
        model.addAttribute("article", articleEntity);
        model.addAttribute("commentDtos", commentDtos);

        // 3. 보여줄 페이지 설정.
        return "articles/show";
    }
```

![[Pasted image 20240310182245.png]]