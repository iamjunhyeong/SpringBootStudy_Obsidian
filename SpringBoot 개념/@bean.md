[]()## 빈으로 등록하는 방법
[]()@SpringbootApplication
- Component Scaning
	- @Component
		- @Repository
		- @Service
		- @Controller
		- @Configuration
		- 등등 
- 또는 직접 일일히 XML이나 자바 설정 파일에 등록
- 자바설정파일:  @Configuration -> @Bean
- Repository 상속 : JPA기능을 통해 빈으로 등록
### 어떻게 꺼내쓰지?
- @Autowired 또는 @Inject
- 또는 ApplicationContext에서 getBean()으로 직접 꺼냄.

### 특징
- 오로지 "빈"들만 의존성 주입을 해준다.

---

스프링 IoC 컨테이너가 관리하는 객체.
빈들만 의존성 주입이 가능.





