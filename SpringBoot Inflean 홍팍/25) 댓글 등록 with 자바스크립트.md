
기존 게시글 등록은
HTML의 form 태그를 이용한 HTTP 요청 방식이였다.

반면 댓글 등록은
JS를 사용하여 HTTP 요청을 보낸다. 

과거에는 form 태그 요청 방식이
일반적이었으나,
최근 JS 요청 방식을 선호하는 추세라고 한다.


![[Pasted image 20240311134029.png]]

![[Pasted image 20240311133958.png]]

![[Pasted image 20240311134018.png]]

## new.mustache 작성

```
<div class="card m-2" id="comments-new">
    <div class="card-body">
        <!-- 댓글 작성 폼-->
        <form>
            <!-- 닉네임 입력 -->
            <div class="mb-3">
                <label class="form-label">닉네임</label>
                <input type="text" class="form-control form-control-sm" id="new-comment-nickname">
            </div>

            <!-- 댓글 본문 입력 -->
            <div class="mb-3">
                <label class="form-label">댓글 내용</label>
                <textarea type="text" class="form-control form-control-sm" rows="3" id="new-comment-body"></textarea>
            </div>

            <!-- 히든 인풋 -->
            {{#article}}
                <input type="hidden" id="new-comment-article-id" value="{{id}}">
            {{/article}}

            <!-- 전송 버튼 -->
            <button type="button" class="btn btn-outline-primary btn-sm" id="comment-create-btn">댓글 작성</button>
        </form>
    </div>
</div>

<script>
    {
        // 댓글 생성 버튼 변수화(id가 comment-create-btn인 대상)
        const commentCreateBtn = document.querySelector("#comment-create-btn");

        // 버튼 클릭 이벤트를 감지!
        commentCreateBtn.addEventListener("click", function () {
            // 새 댓글 객체 생성
            const comment = {
                nickname: document.querySelector("#new-comment-nickname").value,
                body: document.querySelector("#new-comment-body").value,
                article_id: document.querySelector("#new-comment-article-id").value
            };

            // 댓글 객체 출력
            console.log(comment);

            // fetch() - Talend API 요청을 JavaScript로 보내준다 !
            const url = "/api/articles/" + comment.article_id + "/comments";
            fetch(url, {
                method: "post",     // POST 요청
                body: JSON.stringify(comment),  // comment JS 객체를 JSON으로 변경하여 보냄
                headers: {
                    "Content-Type": "application/json"
                }
            }).then(response => {
                // http 응답 코드에 따른 메세지 ㅊ ㅜㄹ력
                const msg = (response.ok) ? "댓글이 등록되었습니다." : "댓글 등록 실패..!";
                alert(msg);
                // 현재 페이지 새로고침
                window.location.reload();
            });
        });
    }
</script>
```

###  querySelector


### fetch

Talend API 요청을 JavaScript로 보내준다!

```
const url = "/api/articles/" + comment.article_id + "/comments";
fetch(url, {
	method: "post",     // POST 요청
	body: JSON.stringify(comment),  // comment JS 객체를 JSON으로 변경하여 보냄
	headers: {
		"Content-Type": "application/json"
	}
})
```

