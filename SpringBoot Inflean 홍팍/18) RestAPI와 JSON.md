
## Rest란?

**REST** 
	REST(Representational State Transfer)의 약자로 자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든 것을 의미한다.

1. HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고,
2. HTTP Method(POST, GET, PUT, DELETE, PATCH 등)를 통해
3. 해당 자원(URI)에 대한 CRUD Operation을 적용하는 것을 의미한다.

**CRUD Operation이란**  
	CRUD는 대부분의 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능인 Create(생성), Read(읽기), Update(갱신), Delete(삭제)를 묶어서 일컫는 말로   
	REST에서의 CRUD Operation 동작 예시는 다음과 같다.



## RestAPI란?

- REST의 원리를 따르는 API를 의미한다.

- 클라이언트에 구애받지않고 다양한 기기에서 호환을 가능하게 함.

- JSON 형태로 reponse 받는다
#### > XML과 JSON
 XML : 사용자 정의 `html`

JSON : `javascript`를 차용한 객체 표현식


### JSON-Placeholder

1. chome 확장 `Talend API tester` 설치

2. GET `https://jsonplaceholder.typicode.com/posts` 으로 게시글 100개 JSON으로 받기
	- 응답 성공시 status : 200 
	- 실패시 500, 404, ..

4. POST로 데이터 생성
```
{
	title : "제목",
	body : "내용",
	id : 101
}
```
- 응답 성공시 status : 201
- 실패시 500, ..

5. PATCH로 수정
```
{
	title : "제목수정",
	body : "내용수정",
	id : 101
}
```
- 성공시 200

6. Delete로 삭제
- 성공시 204