mysql 비밀번호 재설정 
====================
### 서비스를 중지한다.  
service mysqld stop  

### 인증 생략 옵션 / 안전모드로 데몬을 실행한다.
& /usr/bin/mysqld_safe --skip-grant-tables &


### 콘솔로 들어간다.

/usr/bin/mysql -u root mysql


### 패스워드 변경하는 부분은 윈도우와 동일하다. 

5.7버전 미만

UPDATE mysql.user SET password=PASSWORD('패스워드') WHERE user='root'; 
FLUSH PRIVILEGES; 
quit
Colored by Color Scripter


5.7버전 이상

UPDATE mysql.user SET authentication_string=PASSWORD('패스워드') WHERE user='root'; 
FLUSH PRIVILEGES;
quit
Colored by Color Scripter


### 패스워드를 입력해야 접속할 수 있도록 서비스를 재시작한다.

service mysqld restart
