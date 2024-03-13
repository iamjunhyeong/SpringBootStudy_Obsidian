
`댓글 수정`페이지를 만들고, `REST API`를 호출하여 처리한다.

자바스크립트의 이벤트 처리를 통해 해결

![[Pasted image 20240311140641.png]]


### Form

#### modal trigger button 
Modal를 띄우는 버튼이다. 동시에 데이터도 함께 넘기고 있다.

```
<div class="card-header">
	{{nickname}}
	<!-- Button trigger modal -->
	<button type="button"
			class="btn btn-sm btn-outline-primary"
			data-bs-toggle="modal"
			data-bs-target="#comment-edit-modal"
			. . .
</div>
```


Form 부분 전체 코드
```
<div id="comments-list">
    {{#commentDtos}}
        <div class="card m-2" id="comments-{{id}}">
            <div class="card-header">
                {{nickname}}
                <!-- Button trigger modal -->
                <button type="button"
                        class="btn btn-sm btn-outline-primary"
                        data-bs-toggle="modal"
                        data-bs-target="#comment-edit-modal"
                        data-bs-id="{{id}}"
                        data-bs-nickname="{{nickname}}"
                        data-bs-body="{{body}}"
                        data-bs-article-id="{{articleId}}">수정</button>
            </div>
            <div class="card-body">
                {{body}}
            </div>
        </div>
    {{/commentDtos}}
</div>
```

### Modal
다음과 같이 따로 작은 창을 띄워주는 것을 말한다.

![[Pasted image 20240311141945.png]]

#### Modal 틀
```
<!-- Modal -->
<div class="modal fade" id="comment-edit-modal" tabindex="-1">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">댓글 수정</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                <form>
					. . . 
                </form>
            </div>
        </div>
    </div>
</div>
```


#### 전체 코드
```
<!-- Modal -->
<div class="modal fade" id="comment-edit-modal" tabindex="-1">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">댓글 수정</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                <!-- 댓글 작성 폼-->
                <form>
                    <!-- 닉네임 입력 -->
                    <div class="mb-3">
                        <label class="form-label">닉네임</label>
                        <input type="text" class="form-control form-control-sm" id="edit-comment-nickname">
                    </div>

                    <!-- 댓글 본문 입력 -->
                    <div class="mb-3">
                        <label class="form-label">댓글 내용</label>
                        <textarea type="text" class="form-control form-control-sm" rows="3" id="edit-comment-body"></textarea>
                    </div>

                    <!-- 히든 인풋 -->
                    <input type="hidden" id="edit-comment-id">
                    <input type="hidden" id="edit-comment-article-id">

                    <!-- 전송 버튼 -->
                    <button type="button" class="btn btn-outline-primary btn-sm" id="comment-update-btn">수정 완료</button>
                </form>
            </div>
        </div>
    </div>
</div>
```


### Modal 이벤트 처리

이것 또한 부트스트랩에 있는 양식을 이용했다.

```
// 모달 요소 선택  
const commentEditModal = document.querySelector("#comment-edit-modal");  
  
// 모달 이벤트 감지  
commentEditModal.addEventListener("show.bs.modal", function(event) {  
  
	// 트리거 버튼 선택  
	const triggerBtn = event.relatedTarget;
	...
}
```



#### 전체 코드
```
<!--Bootstrap에 양식 틀이 주어진다.-->
<!-- 모달 이벤트 처리 -->
<Script>
    {
        // 모달 요소 선택
        const commentEditModal = document.querySelector("#comment-edit-modal");

        // 모달 이벤트 감지
        commentEditModal.addEventListener("show.bs.modal", function(event) {

            // 트리거 버튼 선택
            const triggerBtn = event.relatedTarget;

            // 데이터 가져오기
            const id = triggerBtn.getAttribute("data-bs-id");
            const nickname = triggerBtn.getAttribute("data-bs-nickname");
            const body = triggerBtn.getAttribute("data-bs-body");
            const articleId = triggerBtn.getAttribute("data-bs-article-id");
            //console.log(`${id}, ${nickname}, ${body}, ${articleId}`);

            // 데이터를 반영
            document.querySelector("#edit-comment-nickname").value = nickname;
            document.querySelector("#edit-comment-body").value = body;
            document.querySelector("#edit-comment-id").value = id;
            document.querySelector("#edit-comment-article-id").value = articleId;
        });
    }

    {
        // 수정 완료 버튼
        const commentUpdateBtn = document.querySelector("#comment-update-btn");

        // 클릭 이벤트 감지 및 처리
        commentUpdateBtn.addEventListener("click", function () {
            // 수정 댓글 객체 생성
            const comment = {
                id: document.querySelector("#edit-comment-id").value,
                nickname: document.querySelector("#edit-comment-nickname").value,
                body: document.querySelector("#edit-comment-body").value,
                article_id: document.querySelector("#edit-comment-article-id").value
            };
            console.log(comment);

            // 수정 REST API 호출 - fetch()
            const url = "/api/comments/" + comment.id;
            fetch(url, {
                method: "PATCH",                // PATCH 요청
                body: JSON.stringify(comment),  // 수정된 댓글 객체를 JSON으로 전달
                headers: {
                    "Content-Type": "application/json"
                }
            }).then(response => {
                // http 응답 코드에 따른 메시지 출력
                const msg = (response.ok) ? "댓글이 수정 되었습니다." : "댓글 수정 실패!";

                // 현재 페이지를 새로고침
                window.location.reload();
            });
        });
    }
</script>
```