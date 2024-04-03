# Tomcat 8080 오류
	https://truehong.tistory.com/5
```powershell
   netstat -ano | findstr 8080
```

- 8080에서 사용중인 프로세스 번호를 확인한다. 내 경우는 33608 

```powershell
   taskkill /f /pid 33608
```

- taskkill 을 이용하여 프로세스를 중단해 준다