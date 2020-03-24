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


@Autowired
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


