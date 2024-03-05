
### @Autowired / @Inject
생성자에도 사용가능. (( 권장방법 ))
스프링 5 이상부터는 생성자에 생략 가능
```
@Autowired  # 생성자 사용
private final Repository repo;
# 생성자를 통해 받을때는 final 사용권장


public Controller(Repository repo) {
	this.repo = repo;
}
```

```
@Autowired  # 필드에 사용
private Repository repo;
```

```
@Autowired  # setter에도 사용 가능
public void setRepo(Repository repo) {
	this.repo = repo;
}
```
