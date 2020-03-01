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

application : ctr + sifht + R

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

### ApplicationContext
IOC는 BeanFactory과 ApplicationContext를 사용한다.(주로 ApplicationContext를 사용한다.)
ApplicationContext는 BeanFactory를 상속받고 다른 인터페이스들을 상속받기 때문에 Spring 내에서 더 많은 일을 수행한다.
개발자가 직접 접근하는 일은 거의 없다.



