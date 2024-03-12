
### 삭제 버튼 생성

```
<div id="comments-list">
  {{#commentDtos}}
    <div class="card m-2" id="comments-{{id}}">
      <div class="card-header">
        {{nickname}}
        <button type="button"
                class="btn btn-sm btn-outline-primary"
                data-bs-toggle="modal"
                data-bs-target="#comment-edit-modal"
                data-bs-id="{{id}}"
                data-bs-nickname="{{nickname}}"
                data-bs-body="{{body}}"
                data-bs-article-id="{{articleId}}">수정</button>
        <!-- 댓글 삭제 버튼 -->
        <button type="button"
                class="btn btn-sm btn-outline-danger comment-delete-btn">삭제</button>
      </div>
      <div class="card-body">
        {{body}}
      </div>
    </div>
  {{/commentDtos}}
</div>
```

#### 쿼리셀렉터

CSS 클래스 선택하기 -> .클래스명

Ex)
```
const commentDeleteBtns = document.querySelectorAll(".comment-delete-btn");
```

#### querySelectorAll

하나만 가져오는 querySelector와 달리 여러개를 가져온다.

```
<!-- 댓글 삭제 -->
<Script>
{
  // 삭제 버튼 선택
  const commentDeleteBtns = document.querySelectorAll(".comment-delete-btn");
  // 삭제 버튼 이벤트를 처리
  commentDeleteBtns.forEach(btn => {
    // 각 버튼의 이벤트 처리를 등록
    btn.addEventListener("click", () => {
      console.log("삭제 버튼이 클릭되었습니다..!");
    });
  });
}
</script>
```


### 삭제 댓글 id 가져오기

#### getAttribute 

버튼에 `data-comment-id` 속성을 가져옴

#### 백틱 문자열

``console.log(`삭제 버튼 클릭: ${commentId}번 댓글`) == "삭제 버튼 클릭: " + commentId + "번 댓글";`



```
      // 삭제 댓글 id 가져오기
      const commentId = commentDeleteBtn.getAttribute("data-comment-id");
      console.log(`삭제 버튼 클릭: ${commentId}번 댓글`);
```


`event.srcElement == event.target`

```
<!-- 댓글 삭제 -->
<Script>
{
  // 삭제 버튼 선택
  const commentDeleteBtns = document.querySelectorAll(".comment-delete-btn");
  // 삭제 버튼 이벤트를 처리
  commentDeleteBtns.forEach(btn => {
    // 각 버튼의 이벤트 처리를 등록
    btn.addEventListener("click", (event) => {
      // 이벤트 발생 요소를 선택
      const commentDeleteBtn = event.target;
      // 삭제 댓글 id 가져오기
      const commentId = commentDeleteBtn.getAttribute("data-comment-id");
      console.log(`삭제 버튼 클릭: ${commentId}번 댓글`);
    });
  });
}
</script>
```

#### target.remove와 window.location.reload() 차이

