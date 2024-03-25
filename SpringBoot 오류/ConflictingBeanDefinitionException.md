# ConflictingBeanDefinitionException
파일을 이동하거나 삭제하는 작업 이후 `ConflictingBeanDefinitionException`를 만날 때가 있다.

일반적으로 ConflictingBeanDefinitionException는 중복으로 클래스가 존재할 대 발생하는 예외이다. 

이 때 에러에서 알려주는 중복된 클래스를 찾아 삭제해주면 된다. 하지만 가끔 파일을 이동만 했을 뿐인데 에러가 발생하는 경우가 있다. 중복 된 클래스가 없을때도 말이다..!

> 원인은 이전 빌드된 클래스 파일이 남아있기 때문이다 !
> 해결 방법은 이전 빌드된 클래스 파일을 없애줘야 한다 !


## 해결
```java
./gradlew clean
```

위 명령어로 프로젝트를 초기 상태로 되돌릴 수 있다!