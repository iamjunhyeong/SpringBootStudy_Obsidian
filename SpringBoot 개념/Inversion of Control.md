일반적인 제어권 : "내가 사용할  의존성은 내가 만듦"

```
class Controller {
	private Repository = new Repository();
}
```

IoC: "내가 사용할 의존성을 누군가 해주겠지"
- 내가 사용할 의존성의 타입(또는 인터페이스)만 맞으면 상관없다.
- 그래야 내 코드 테스트 하기도 편함.

```
class Controller {
	private Repository repo;

	public Controller(Repository repo) {
		this.repo = repo;
	}
	
	// repo 사용가능
}

class ControllerTest {
	@Test
	public void create() {
		Repository repo = new Repository();
		Controller ctl = new Controller(repo);
	}
}
```