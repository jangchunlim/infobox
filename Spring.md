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

Spring code Review
==================



terminal : ./mvnw package  
-> 빌드 후 jar로 패키징해준다.

아래와 같이 pom.xml에서 아무런 명시가 없다면 기본적으로 jar 파일로 패키징된다.
<img width="425" alt="스크린샷 2020-02-29 오후 9 05 50" src="https://user-images.githubusercontent.com/60742564/75607194-c08cc000-5b37-11ea-95e7-7a855582f52b.png">

