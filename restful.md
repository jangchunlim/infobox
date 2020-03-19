
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
RFC 3986은 URI 스키마와 호스트를 제외하고는 대소문자를 구별하도록 규정하기 때문   
```
http://api.example.org/my-folder/my-doc  //1
HTTP://API.EXAMPLE.ORG/my-folder/my-doc  //2
http://api.example.org/My-Folder/my-doc  //3
```
2. 하이픈(-) 사용으로 가독성 높임
```
http://api.example.com/inventory-management/managed-entities/{id}/install-script-location  //More readable
http://api.example.com/inventory-management/managedEntities/{id}/installScriptLocation  //Less readable
```
3. (_)사용 금지
```
http://api.example.com/inventory-management/managed-entities/{id}/install-script-location  //More readable
http://api.example.com/inventory_management/managed_entities/{id}/install_script_location  //More error prone
```
4. 확장자 사용 금지
```
http://api.example.com/device-management/managed-devices.xml  /*Do not use it*/
http://api.example.com/device-management/managed-devices 	/*This is correct URI*/
```
5. path의 마지막에는 (/) 사용 금지
```
http://api.example.com/device-management/managed-devices/
http://api.example.com/device-management/managed-devices 	/*This is much better version*/
```
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
8. 계층 구분을 위해 (/)를 사용할 것
```
http://api.example.com/device-management
http://api.example.com/device-management/managed-devices
http://api.example.com/device-management/managed-devices/{id}
http://api.example.com/device-management/managed-devices/{id}/scripts
http://api.example.com/device-management/managed-devices/{id}/scripts/{id}
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

### 에러코드는 자주 사용되는 몇개의 코드만 의미에 맞춰 사용하는 것이 좋다.   

Google GData
200 201 304 400 401 403 404 409 410 500
Netflix
200 201 304 400 401 403 404 412 500
Digg
200 400 401 403 404 410 500 503

에러메세지에서 Error Stack 정보를 출력하는 것은 위험하다.(해킹 할 수 있는 정보를 제공)   


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


### 검색
검색은 일반적으로 HTTP GET에서 Query String에 검색 조건을 정의하는 경우가 일반적인데, 이 경우 검색조건이 다른 Query String과 섞여 버릴 수 있다. 예를 들어 name=cho이고, region=seoul인 사용자를 검색하는 검색을 Query String만 사용하게 되면 다음과 같이 표현할 수 있다.    
/users?name=cho&region=seoul
그런데, 여기에 페이징 처리를 추가하게 되면   

/users?name=cho&region=seoul&offset=20&limit=10    

페이징 처리에 정의된 offset과 limit가 검색 조건인지 아니면 페이징 조건인지 분간이 안간다. 그래서, 쿼리 조건은 하나의 Query String으로 정의하는 것이 좋은데   

/user?q=name%3Dcho,region%3Dseoul&offset=20&limit=10   

이런식으로 검색 조건을 URLEncode를 써서 “q=name%3Dcho,region%3D=seoul” 처럼 (실제로는 q= name=cho,region=seoul )표현하고 Deleminator를 , 등을 사용하게 되면 검색 조건은 다른 Query 스트링과 분리된다.   
물론 이 검색 조건은 서버에 의해서 토큰 단위로 파싱되어야 한다.   

### 전역 검색과 리소스 검색   
<전체 검색>   
전체 리소스에 대한 검색을 요청하는 API의 경우에는 /search와 같은 전역 검색을 뜯하는 URL를 정의하여 사용하면 된다.   
예 > /search?q=name%3Dlee : 이름이 lee인 모든 리소스를 조회    

<리소스 검색>   
특정 리소스에 대한 검색을 원할 경우에는 지금까지 예제로 들어왔던 것과 동일하게 리소스를 명시하고 뒤에 검색 조건을 작성하면 된다.    
예 > /students?q=name%Dlee 이름이 lee인 학생들 조회 


### 단일 API URL

[설정]   
api.supermarket.com/food는 food.supermarket.com으로 라우팅.  
api.supermarket.com/drink는 drink.supermarket.com으로 라우팅.   

[장점]   
위와 같이 구성하면 API서버가 추가로 확장하더라도 유연하게 확장 및 운영이 가능하며 사용자는 단일 API를 변함없이 바라보기만 하면된다는 장점이 있다.   
추가적으로 단일 엔드포인트를 제공하면 부하 분산 및 로그를 통한 Audit(감사)를 쉽게 할 수 있다는 장점이 있다.   


### 프로토콜 레벨 암호화
HTTPS 사용(ssl을 이용한 http)   
HTTPS 는 Man in the middle attack에 취약하다.   
Man in the middle attack : 기본적인 메커니즘은 서버에서 보낸 인증서를 바꿔치기 해서 클라이언트로 보내는 방식     
api.apiserver.com/car 는 car.apiserver.com으로 라우팅 하도록 구현하면 된다.   
api.apiserver.com/car 는 car.apiserver.com으로 라우팅 하도록 구현하면 된다.   


### 리소스간에 관계를 표현
 - 우리가 API로 표현하고자 하는 리소스들은 서로 특정한 관계를 맺고 있다. 관계의 예를 들어보면 아래와 같다. 
 - devices/cleaner를 보면 devices와 cleaner라는 두가지 리소스가 표현되어 있다. 이 두 리소스는 상위 개념과 하위 개념이라는 관계를 가지고 있다고 말할 수 있다. cleaner는 devices에 속하는 개념이라고 보면 된다.  
 - 위에서 설명한 관계 이외에도 리소스간에는 다양한 관계가 존재할 수 있다. 그렇다면 james와 apple이라는 리소스로는 어떠한 관계를 표현할 수 있을까?james가 가지고 있는 apple, james가 좋아하는 apple 등..이 있을 수 있을 것이다.  이와 같은 관계를 REST API로 표현하는 방법을 알아보자. 

 2-1> 서브 리소스 표현 방법 
 - HTTP Get  /users/tom/cars : 사용자 tom이 소유하고 있는 car들 정보 조회

 2-2> 서브 리소스의 관계를 명시 하여 표현하는 방법 
 - HTTP GET /users/tom/likes/subjects : 사용자 tom이 좋아하는 과목 정보 조회 
 - 위와 같이 관계 "likes"를 URL에 명시적으로 표시해주면 된다.  

### HATEOS(Hypermedia as the engine of application state)
HATEOS는 하이퍼미디어의 특징을 활용하여 HTTP Response에 리소스에 대한 Link 정보를 함께 탑재하여 사용자에게 전달해주는 것이다. 이와 같은 기능은 페이징 처리시 전/후 페이징에 대한 정보를 Link와 함께 전달해 주거나 리소스에 대한 디테일한 링크를 표시하는 목적으로 사용할 수 있다. HATEOAS를 API에 적용하면 가독성이 증대되는 장점이 있지만 응답 메시지가 다른 리소스 URL에 의존성이 생겨 구현이 다소 까다로워지는 단점이 있으니 알아두자.  

### API 페이징 처리
사용자의 요청 정보에 대해 많은 양의 정보를 리스트 형태로 서버가 응답해야 하는 경우 페이징 처리가 필요하다. (한번에 많은 많은 내용을 처리하는 것은 서버의 성능과 네트워크 부하면에서도 비효율적이며 요즘같은 빅데이터를 다루는 경우에는 처리 자체가 불가능한 경우가 많이 있다.)

[Partial Response 디자인 알아보기]
 - Facebook : /terry/friends?fields=id,name   
 - Google : ?fields=title,media:group(media:thumnail)    

예를 들어 100번째 레코드부터 125번째 레코드까지 받는 API를 정의하면   

-Facebook API 스타일 : /record?offset=100&limit=25   
-Twitter API 스타일 : /record?page=5&rpp=25 (RPP는 Record per page로 페이지당 레코드수로 RPP=25이면 페이지 5는 100~125 레코드가 된다.)   
-LikedIn API 스타일 : /record?start=50&count=25   

apigee의 API가이드를 보면 좀더 직관적이라는 이유로 페이스북 스타일을 권장하고 있다.
record?offset=100&limit=25
리소스에 대한 응답 메세지에 대해서 굳이 모든 필드를 포함할 필요가 없는 케이스가 있다. 예를 들어 페이스북 FEED의 경우에는 사용자 ID, 이름, 글 내용, 날짜, 좋아요 카운트, 댓글, 사용자 사진등등 여러가지 정보를 갖는데, API를 요청하는 Client의 용도에 따라 선별적으로 몇가지 필드만이 필요한 경우가 있다. 필드를 제한하는 것은 전체 응답의 양을 줄여서 네트워크 대역폭(특히 모바일에서) 절약할 수 있고, 응답 메세지를 간소화하여 파싱등을 간략화할 수 있다.
그래서 몇몇 잘 디자인된, REST API의 경우 이러한 Partial Response 기능을 제공하는데, 주요 서비스들을 비교해보면 다음과 같다.   

5-1> 페이징 디자인

 - Facebook : /record?offset=100&limit=25 : 100번째 부터 25개 레코드 조회
 - LinkedIn : /record?start=50&count=25 : 50번째 부터 25개 레코드 조회 

  
 
 #### Partial response
- Linked in : /people:(id,first-name,last-name,industry)   
- Facebook : /terry/friends?fields=id,name   
- Google : ?fields=title,media:group(media:thumnail)   
Linked in 스타일의 경우 가독성은 높지만 :()로 구별하기 때문에, HTTP 프레임웍으로 파싱하기가 어렵다. 전체를 하나의 URL로 인식하고, :( 부분을 별도의 Parameter로 구별하지 않기 때문이다.   
Facebook과 Google은 비슷한 접근 방법을 사용하는데, 특히 Google의 스타일은 더 재미있는데, group(media:thumnail) 와 같이 JSON의 Sub-Object 개념을 지원한다.   
Partial Response는 Google 스타일을 이용하는 것을 권장한다.   

5-2> Partial Response 처리하기 
 - 리소스의 일부 정보만 조회 하고자 할 때 사용한다.    
 (사용자가 서버에 리소스에 대한 정보를 요청할 때 해당 리소스에 포함된 모든 정보를 요구하는 경우보다는 리소스에 해당하는 세부 몇 개의 정보만을 원하는 경우가 더 많다.) 
 - 예를 들면 user 리소스에 해당하는 정보는 id, pw, 이름, 주소, 전화번호 정보들이 있다고 해보자. 사용자는 user들의 정보 조회시 id와 이름 정보만 필요로 하는 경우가 있을 것이다. 이 때 user 사용자에게 user의 모든 정보를 제공하는 것보다 필요한 정보만 제공해준다면 전체 응답의 양을 대폭 줄일 수 있을 것이다. 

### naming convetion 

### naming convention 목적
나쁜 예	a = b * c ;	a / b / c 각 변수가 의미하는 바를 파악하기 힘듦
좋은 예	weekly_pay = hours_worked * hourly_pay_rate ;	변수명만 보고도 주급 계산을 위한 변수임을 알 수 있음

### 표기법 종류
- camel case
  lower camel case (upper camel case 와 분류할 수 있지만 대부분 camel case 는 lower camel case 를 가리킴)
  각 단어의 첫 문자를 대문자로 표기하지만, 전체에서의 첫 글자는 소문자로 표기 (단봉낙타 표기법)
  ex) camelCase, lowerCamelCase

  
- pascal case
  전체 이름의 첫 문자를 포함한 각 단어의 첫 문자를 대문자로 표시
  
- snake case
  각 단어의 사이를 '_'로 구분하여 표기
  ex) camel_case, snake_case
  
- Kebab case
  각 단어 사이를 '-'로 구분하여 표기
  ex) kebab-case
  
- hungarian notation
  이름앞에 변수의 타입을 접두어로 넣어주어 표기(ch-char, db-double, str-string, b-boolean)
  ex) bCamelCase strCamelCase

### http method override
일부 브라우저, 네트워크 프로시는 GET, POST 만 쓸 수 있다.    
그러므로 PUT(PATCH), DELETE 요청을 할 때는 서버에 힌트를 제공하는 방법을 제공해야 한다.   
POST /articles    
---payload--- _method=PUT&title=...&content=...    
 

POST /articles   
<b> X-HTTP-Method-Override=PUT <b/>   
---payload--- _method=PUT&title=...&content=...   
