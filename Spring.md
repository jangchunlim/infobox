Constructor
===========
* controller   
URL과 실행 함수를 매핑   
비즈니스 로직이 있는 service를 호출하여 비즈니스 로직 처리   
반환할 템플릿을 정의 및 JSON 등으로 응답   

* service   
비즈니스 로직을 구현    
데이터 처리(모델)를 담당하는 repository에서 데이터를 가져와서 controller에 넘겨주거나, 비즈니스 로직을 처리   

* domain > entity   
DB 테이블과 매핑되는 객체(Entity)를 정의   
JPA에서는 Entity를 통해 데이터를 조작함   

* domain > repository   
데이터를 가져오거나 조작하는 함수를 정의   
Interface를 implements하여 미리 만들어진 함수를 사용할 수 있으며, 또한 직접 구현이 가능   

* dto   
controller와 service 간에 주고 받을 객체를 정의하며, 최종적으로는 view에 뿌려줄 객체   
Entity와 속성이 같을 수 있으나, 여러 service를 거쳐야 하는 경우 dto의 몸집은 더 커짐   
ex) AEntity에 a 속성, BEntity에 b속성이 있을 때, ZDto에 a,b 속성으로 정의될 수 있음   
entity와 dto를 분리한 이유는 Entity는 DB 테이블이 정의되어 있으므로, 데이터 전달 목적을 갖는 객체인 dto를 정의하는 것이 좋다고 합니다. ( 참고 )

* static   
css, js, img 등의 정적 자원들을 모아놓은 디렉터리입니다.   
* templates   
템플릿을 모아놓은 디렉터리입니다.   
Thymeleaf는 HTML을 사용합니다.   




Spring annotaion
================

### Annotation  
메타데이터(실제데이터가 아닌 데이터를 위한 데이터)라고 불린다.
컴파일 또는 런타임에 해석이 된다.
설정값들을 명시한다는 점에서 xml과 비슷하다.  
Annotation은 선언위에 존재해서 어떤 내용인지 쉽게 판단할 수 있다.

### @Component  
클래스 상단에 위치시키며 클래스 이름이 bean의 이름이 된다.

### @Configuration
@Configuration으로 정의된 클래스는 @Bean으로 정의된 메소드들을 포함하며,
@Component의 확정이라서 @Autowired로도 찾을 수 있다.

<pre>
<code>
@Component
public class TestBean
{
    ...
}

@Configuration
public class TestConfig
{
    @Bean
    public TestBean testBean()
    {
        return new TestBean();
    }
}
</pre>
</code>

### @Bean  
@Configuration으로 선언된 클래스 내에 있는 메소드를 정의할 때 사용한다.
이 메소드가 반환하는 객체가 bean이 되며 메소드 이름이 bean의 이름이 된다.
bean으로 선언할 때 bean의 이름을 바꾸고싶다면 name, autowire property 를 사용한다.
<pre><code>
@Configuration
public class TestConfig
{
    @Bean(name="anotherNameBean", autowire=Autowire.BY_NAME)
    public TestBean testBean()
    {
        return new TestBean();
    }
}
</pre></code>

### @Aotowired
bean을 자동으로 삽입해주며 생성자, 필드, 메소드에 적용이 가능하다.

### @Qualifier
같은 클래스의 bean이 2개 이상일 경우 스프링은 bean의 이름을 이용하여 의존성 주입을 하는데,
어떤 이름의 bean을 주입할 지 선언할때 사용한다.

<pre><code>
public class AutowiredTest
{
    @Autowired
    public void autowiredTestMethod(@Qualifier("testBean")TestBean abc)
    {
        ...
    }
}
</pre></code>

### @AllArgsConstructor
Bean 주입 방식과 관련이 있으며, 생성자로 Bean 객체를 받는 방식을 해결해주는 어노테이션이다.   
그래서 BoardService 객체를 주입 받을 때 @Autowired 같은 특별한 어노테이션을 부여하지 않는다.   
그 밖에, @NoArgsConstructor @RequiredArgsConstructor 어노테이션이 있다.    
해당 어노테이션을 붙이고 생성자를 만들지 않으면 null point error 가 

### @GetMapping/@PostMapping
URL을 매핑해주는 어노테이션이며, HTTP Method에 맞는 어노테이션을 작성한다.


### @RestController
@Controller + @ResponseBody  
@ResponseBody를 모든 메소드에서 적용한다.  
메소드의 반환 결과(문자열)를 JSON 형태로 반환한다.  


### @Controller 와 @RestController 차이
@Controller  
API와 view를 동시에 사용하는 경우에 사용  
대신 API 서비스로 사용하는 경우는 @ResponseBody를 사용하여 객체를 반환한다.  
view(화면) return이 주목적  
@RestController  
view가 필요없는 API만 지원하는 서비스에서 사용 (Spring 4.0.1부터 제공)  
@RequestMapping 메서드가 기본적으로 @ResponseBody 의미를 가정한다.  
data(json, xml 등) return이 주목적  
즉, @RestController = @Controller + @ResponseBody  


### @Autowired
org.springframework.beans.factory.annotation.Autowired
Type에 따라 알아서 Bean을 주입한다.
필드, 생성자, 입력 파라미터가 여러 개인 메소드(@Qualifier는 메소드의 파라미터)에 적용 가능
Type을 먼저 확인한 후 못 찾으면 Name에 따라 주입한다.
Name으로 강제하는 방법: @Qualifier을 같이 명시
예시

TIP) Bean을 주입받는 방식 (3가지)
@Autowired
setter
생성자 (@AllArgsConstructor 사용) -> 권장방식
https://gmlwjd9405.github.io/2018/12/02/spring-annotation-types.html

### PathVariable("no") Long no   
유동적으로 변하는 Path variable 처리하는 방법
<pre><code>
@DeleteMapping("/post/{no}")
    public String delete(@PathVariable("no") Long no) {
        boardService.deletePost(no);

        return "redirect:/";
    }
</pre></code>


How to run Spring project
=========================


### package
terminal : ./mvnw package  
-> 빌드 후 jar로 패키징해준다.

아래와 같이 pom.xml에서 아무런 명시가 없다면 기본적으로 jar 파일로 패키징된다.
<img width="425" alt="스크린샷 2020-02-29 오후 9 05 50" src="https://user-images.githubusercontent.com/60742564/75607194-c08cc000-5b37-11ea-95e7-7a855582f52b.png">

### run jar
terminal : java -jar 'build 후 jar파일이 생성된 위치의 상위폴더'/*.jar  
(해당 폴더에 jar file 은 하나 밖에 존재하지 않는다.)

application : ctr + R
application(test) : ctr + sifht + R

------------------------------------------------------------------------------------------------
Process Flow
============

### pet-clinic  

web 으로 호출되었을 때

1. DispatcherServlet, RequestMappingHandler 를 통해 기본 컨트롤러를 찾아간다.(owner contrlloer)  
<img width="1283" alt="스크린샷 2020-03-01 오후 12 28 12" src="https://user-images.githubusercontent.com/60742564/75618949-5831f300-5bb8-11ea-9b64-a999dc27a69d.png">

2. 컨트롤러에서 @getMapping 어노테이션을 통해 해당 도메인에 맞는 부분을 찾아간다.  
<img width="736" alt="스크린샷 2020-03-01 오후 1 44 36" src="https://user-images.githubusercontent.com/60742564/75619724-ee6b1680-5bc2-11ea-92ef-ead4d2832890.png">

3. 해당 로직을 수행한 후 정의된 view 를 return 한다.  
<img width="1127" alt="스크린샷 2020-03-01 오후 1 48 29" src="https://user-images.githubusercontent.com/60742564/75619769-74875d00-5bc3-11ea-8a1d-842752830ae5.png">





------------------------------------------------------------------------------------------------
Content
=======

### IOC & DI
빈을 만들어준다.  
만들어진 빈을 엮어준다.  
빈을 저장하는 컨테이너를 제공한다.  
모든 클래스를 빈으로 등록하지는 않는다.(주로 Entity)  
Bean등록을 직접 할 수 있다.  
<img width="564" alt="스크린샷 2020-03-01 오후 2 07 29" src="https://user-images.githubusercontent.com/60742564/75619958-2c1d6e80-5bc6-11ea-9a2f-182f3e9cc89a.png">  
사용할 의존성을 직접만든다.  
의존성 주입은 IOC컨테이너에 존재하는 bean들끼리만 가능하다.  
보통은 생성자를 통해서 받은 매개변수를 통해 직접 주입한다.  
이부분은 싱글톤 패턴의 구사 방식과 비슷한 듯 하다.    
<img width="723" alt="스크린샷 2020-03-01 오후 1 55 05" src="https://user-images.githubusercontent.com/60742564/75619848-65ed7580-5bc4-11ea-8995-9f45258adfae.png">  
외부에서 해당 클래스 주입(매개변수)할 수 있도록 한다.
생성자단계에서 반드시 명시해주지 않으면 에러가 발생한다.(인스턴스 생성 시 자체적으로 가지는 변수에 대해 주입이 필요하다.)  

IOC 를 통해서 주입된 값과 직접적으로 ApplicationContext에 접근하여 받은 주소값은 값다.  
<img width="825" alt="스크린샷 2020-03-01 오후 2 53 21" src="https://user-images.githubusercontent.com/60742564/75620453-8e796d80-5bcc-11ea-915b-65bf6f5edea1.png">


### ApplicationContext
IOC는 BeanFactory과 ApplicationContext를 사용한다.(주로 ApplicationContext를 사용한다.)  
ApplicationContext는 BeanFactory를 상속받고 다른 인터페이스들을 상속받기 때문에 Spring 내에서 더 많은 일을 수행한다.  
개발자가 직접 접근하는 일은 거의 없다.  


### Bean
IOC 컨테이너가 관리하는 객체를 Bean이라 한다.(ApplicationContext가 알고있는 객체)  
Bean에 등록하기 하기 위한 방법은 세가지가 있다
첫번째는 @Component 를 모두 스캔하여 해당 클래스의 인스턴스를 Bean으로 등록한다.(반드시 '@Componenet'라고 명시하지 않아도 된다.
두번째는 @bean (직접)
세번째는 특정 클래스를 상속받는다.

@SpringBootApplication아래에 있는 @ComponenetSacn 를 통해 @Component들을 모두 찾는다.

@Autowired를 통해서도 Bean으로 등록된다.

### DI
생성자를 통해 의존성을 주입하지않고 @Autowired를 통해서 주입이 가능하다.  
선언에서 final 이 빠지고 생성자를 제외하고 실행 = @Autowired  
IOC 컨테이너에서 주입한다.
<img width="825" alt="스크린샷 2020-03-01 오후 2 53 21" src="https://user-images.githubusercontent.com/60742564/75621503-87f0f300-5bd8-11ea-8954-5a09104131e0.png">

Setter에만 @Autowired 를 사용해서 구현도 가능하다.  
<img width="399" alt="스크린샷 2020-03-01 오후 4 40 14" src="https://user-images.githubusercontent.com/60742564/75621728-6b09ef00-5bdb-11ea-9187-290b85a3986e.png">


@RestController 이노테이션을 이용한 예제  
<img width="623" alt="스크린샷 2020-03-02 오후 3 36 31" src="https://user-images.githubusercontent.com/60742564/75651543-14181e80-5c9c-11ea-9547-526957b808c7.png">



---------------------------------------------------------------------------------------
properties
==========
### spring.jpa.show-sql=true   
* sql문을 log로 남겨준다.
### server.port=8090   
* port를 지정한다.   
### spring.jpa.hibernate.ddl-auto=create-drop   
* 로딩 시 create 하고 죽기 전 drop 하도록 한다.   
### spring.jpa.properties.hibernate.format_sql=true   
* sql 포맷 형태를 맞춘다.   

### entity 작성 시 @Id 의 출처는 javax.persistence.Id 이다.

<pre>
<code>
import org.springframework.data.annotation.Id;    (X)
import javax.persistence.Id;                      (O)
<code/>
<pre/>

----------------------------------------------------------------------------------------
tip
===

<img width="663" alt="스크린샷 2020-03-27 오후 7 06 02" src="https://user-images.githubusercontent.com/60742564/77745127-192f7a00-705e-11ea-8675-e573e74a8208.png">

* --stacktrace, --debug   
위 명령어를 command-line option 에 넣으면 컴파일 후 자세한 정보를 얻을 수 있다.

---------------------------------------------------------------------------------------
error
=====

dependencies omitted : 버전이 충돌될 때 나타난다.


----------------------------------------------------------------------------------------
junit test
==========
어노테이션을 제공한다.(@Test @Before @After)   
@Test 메서드가 호출 될 때마다 새로운 인스턴스를 생성하여 독립적인 테스트를 한다.
없는 경우 src/ 밑에 test 폴더 생성한 후 모듈 설정을 해준다.

----------------------------------------------------------------------------------------
redirect, forward
================

redirect = URL 변화 O, 객체 재사용 X, 시스템(seesion, DB)에 변화가 생기는 요청(로그인, 회원가입, 글쓰기)
forward = URL 변화 X, 객체 재사용 O, 시스템(seesion, DB)에 변화가 생기지 않는 단순조회(리스트보기, 검색)

model 객체를 통해 view에 데이터를 전달
================================
<pre><code>
@Controller
@AllArgsConstructor
public class BoardController {
    private BoardService boardService;

    /* 게시글 목록 */
    @GetMapping("/")
    public String list(Model model) {
        List<BoardDto> boardList = boardService.getBoardlist();

        model.addAttribute("boardList", boardList);
        return "board/list.html";
    }



    ...

}
</pre></code>

PutMapping 메소드의 return 값을 통해 특정 변수를 포함시켜야할 때 
======================================================
String.format 을 사용한다.   
C언어에서 printf를 하는 방식과 같다.
ex) return String.format("redirect:/questions/%d",id);

update 시 post, post를 이요한다.
=============================

<img width="546" alt="스크린샷 2020-03-30 오후 2 28 15" src="https://user-images.githubusercontent.com/60742564/77878224-d0acd200-7292-11ea-9376-f0d30224d8f4.png">



<img width="664" alt="스크린샷 2020-03-30 오후 2 28 37" src="https://user-images.githubusercontent.com/60742564/77878253-e0c4b180-7292-11ea-9800-7568dc284bde.png">


