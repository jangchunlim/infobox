
# Restful 
자원을 이름으로 구분하여 해당 자원의 상태를 주고 받는 모든 것을 의미한다.
1. 자원의 표현
2. 상태 전달

rest는 기본적으로 웹의 기존 기술과 http 프로토콜을 그대로 활용하기 때문에 웹의 장점을 최대한 활용할 수 있는 아키텍처 스타일이다.
rest는 네트워크 상에서 client와 server 사이의 통신 방식 중 하나이다.

Http uri 를 통해 자원을 명시하고, Http method(post, get, put, delete)를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것을 의미한다.

### rest 의 특징
1. Uniform (유니폼 인터페이스)
Uniform Interface는 URI로 지정한 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수행하는 아키텍처 스타일
uri 에서 i 는 식별자(identifier)
url 에서 l 은 위치(locator)
2. Stateless (무상태성)
상태가 있다 없다는 의미는 사용자나 클라이언트의 컨택스트를 서버쪽에 유지 하지 않는다는 의미한다.
세션이나 쿠키등을 별도로 관리하지 않기 때문에 API서버는 요청만을 들어오는 메시지로만 처리하기 때문에 구현이 단순하다.
3. Cacheable (캐시 처리 가능)
REST의 가장 큰 특징 중 하나는 HTTP라는 기존 웹표준을 그대로 사용한다.
HTTP가 가진 캐싱 기능이 적용 가능하다. HTTP 프로토콜 표준에서 사용하는 Last-Modified태그나 E-Tag를 이용하면 캐싱 구현이 가능하다.
4. Self-descriptiveness (자체 표현 구조)
REST의 또 다른 큰 특징 중 하나는 REST API 메시지만 보고도 이를 쉽게 이해 할 수 있는 자체 표현 구조로 되어 있다는 것
5. Client - Server Architecture (클라이언트 - 서버 구조)
REST 서버는 API를 제공하고, 제공된 API를 이용해서 비즈니스 로직 처리 및 저장을 책임진다.
클라이언트의 경우 사용자 인증이나 컨택스트(세션,로그인 정보)등을 직접 관리하고 책임진다.
서로간의 의존성이 줄어들게 된다.
6. 계층형 구조
클라이언트 입장에서는 REST ApI 서버만 호출한다.
REST 서버는 다중 계층으로 구성될 수 있다. 예를 들어 보안, 로드 밸런싱, 암호화, 사용자 인증등등 추가하여 구조상의 유연성을 줄 수 있다.

### rest가 필요한 이유
애플리케이션 분리 및 통합
다양한 클라이언트의 등장

### uri설계 시 주의할 점
1. 되도록 소문자 사용
2. 하이픈(-) 사용으로 가독성 높임
3. (_)사용 금지
4. 확장자 사용 금지
```
http://api.example.com/device-management/managed-devices.xml  /*Do not use it*/
http://api.example.com/device-management/managed-devices 	/*This is correct URI*/
```
5. path의 마지막에는 (/) 사용 금지
6. CRUD function names 를 사용하지 말 것
```
HTTP GET http://api.example.com/device-management/managed-devices  //Get all devices
HTTP POST http://api.example.com/device-management/managed-devices  //Create new Device
HTTP GET http://api.example.com/device-management/managed-devices/{id}  //Get device for given Id
HTTP PUT http://api.example.com/device-management/managed-devices/{id}  //Update device for given Id
HTTP DELETE http://api.example.com/device-management/managed-devices/{id}  //Delete device for given Id
```
7. URI collenction을 구분하기 위해 쿼리를 사용할 것
```
http://api.example.com/device-management/managed-devices
http://api.example.com/device-management/managed-devices?region=USA
http://api.example.com/device-management/managed-devices?region=USA&brand=XYZ
http://api.example.com/device-management/managed-devices?region=USA&brand=XYZ&sort=installation-date
```

### http 응답 코드

200 : 정상적으로 수행  
201 : 자원 생성 요청. 성공적으로 수행됨  

400 : 클라이언트 요청이 부적절할 경우 응답 코드  
401 : 클라이언트가 권한이 없는 자원을 요청하였을 때 응답 코드  
403 : 보호되는 자원을 요청하였을 때 응답 코드  
405 : 클라이언트가 요청한 리소스에는 사용 불가능한 Method를 이용했을 때 응답 코드  

301 : 클라이언트가 요청한 리소스에 대한 uri 가 변경되었을 때 응답 코드  
500 : 서버에 뭔가 문제가 있을 때 사용하는 응답 코드  


uri에 직접 담는 것은 restful 하지 않다.
ex)  
X : PUT /dogs/1/isSick  
O : PUT /dogs/1?isSick=true   

무엇(명사)을 생성하는지 생각해보아야 한다.
ex)  
X : POST /login  
X : POST /users/login  
O : POST /session  

### 기존의 method 호출방식과 비교  

<img width="852" alt="스크린샷 2020-03-15 오후 8 06 10" src="https://user-images.githubusercontent.com/60742564/76700191-7fc8a580-66f8-11ea-9edb-fc6414815a27.png">
