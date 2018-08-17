## [Building an Application with Spring Boot](https://spring.io/guides/gs/spring-boot/)
#### 스프링 부트로 애플리케이션 빌드하기
스프링 부트로 간단한 웹 애플리케이션을 만들고 유용한 서비스를 추가해본다.

```
spring boot version: 2.0.3.RELEASE
build: Gradle(2.X)
java version: 1.8
```
#### Spring Boot로 할 수 있는 것
Spring Boot는 응용 프로그램을 빠르게 만들 수있는 방법을 제공합니다. 클래스 패스와 구성된 bean을 살펴보고 합리적인 가정을 한 후 누락 된 부분에 대해 추가합니다. Spring Boot를 사용하면 비즈니스 기능에는 집중하고 인프라에 대한 집중도는 줄일 수 있습니다.

예 :

- 스프링 MVC가 거의 항상 필요한 몇 가지 특정 bean이 있으며, Spring Boot는 이들을 자동으로 추가합니다. 또한 Spring MVC 애플리케이션은 서블릿 컨테이너를 필요로하기 때문에 스프링 부트는 임베디드 Tomcat을 자동으로 구성합니다.
- Jetty를 사용한다면 당신은 Tomcat을 원하지 않고 대신 Jetty를 임베드해야합니다. Spring Boot가 이를 처리합니다.
- Thymeleaf를 사용하고 싶다면 애플리케이션 컨텍스트에 항상 추가되어야하는 빈이 있습니다. Spring Boot가 그들을 추가합니다.

다음은 Spring Boot가 제공하는 자동 구성의 몇 가지 예입니다. 동시에, 스프링 부트는 방해받지 않습니다. 예를 들어, Thymeleaf가 경로 상에 있다면, SpringBoot는 응용 프로그램 컨텍스트에 자동으로 `SpringTemplateEngine`을 추가합니다. 그러나 자신 만의 설정으로 자신 만의`SpringTemplateEngine`을 정의한다면 Spring Boot는 하나를 추가하지 않을 것입니다. 이렇게하면 약간의 노력으로 제어 할 수 있습니다.

참고 : Spring Boot는 코드를 생성하거나 파일을 편집하지 않습니다. 대신 응용 프로그램을 시작할 때 스프링 부트는 빈과 설정을 동적으로 연결하여 응용 프로그램 컨텍스트에 적용합니다.

#### 간단한 웹 애플리케이션 만들기
이제 간단한 웹 응용 프로그램 용 웹 컨트롤러를 만들 수 있습니다.

`src / main / java / hello / HelloController.java`

```java
package hello;

import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;

@RestController
public class HelloController {
    
    @RequestMapping("/")
    public String index() {
        return "Greetings from Spring Boot!";
    }
}
```

이 클래스는`@ RestController` 플래그가 붙어 있습니다. 즉, Spring MVC가 웹 요청을 처리 할 준비가되었음을 의미합니다. `@ RequestMapping`은`/`를`index ()`메소드에 매핑합니다. 브라우저에서 호출하거나 명령 줄에서 curl을 사용할 경우 이 메서드는 순수 텍스트를 반환합니다. 왜냐하면`@ RestController`는 뷰가 아닌 데이터를 리턴하는 웹 요청을 초래하는 두 개의 주석 인`@ Controller`와`@ ResponseBody`를 결합하기 때문입니다.

#### 응용 프로그램 클래스 만들기
여기에 컴포넌트가있는`Application` 클래스를 생성합니다 :

`src / main / java / hello / Application.java`

```java
package hello;

import java.util.Arrays;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;

@SpringBootApplication
public class Application {
    
    public static void main(String[] args) {
        ApplicationContext ctx = SpringApplication.run(Application.class, args);
        
        System.out.println("Let's inspect the beans provided by Spring Boot:");
        
        String[] beanNames = ctx.getBeanDefinitionNames();
        Arrays.sort(beanNames);
        for (String beanName : beanNames) {
            System.out.println(beanName);
        }
    }

}

```

`@SpringBootApplication`은 다음을 모두 추가하는 편리한 주석입니다.

* `@Configuration`은 클래스를 애플리케이션 컨텍스트의 Bean 정의 소스로 태그 지정합니다.

* `@EnableAutoConfiguration`은 Spring Boot에게 클래스 패스 설정, 다른 bean 및 다양한 속성 설정을 기반으로 bean 추가를 시작하도록 지시합니다.

* 일반적으로 `@EnableWebMvc`를 Spring MVC app에 추가 할 것이지만 Spring Boot는 classpath에서 `spring-webmvc`를 볼 때 자동으로 추가합니다. 이것은 응용 프로그램에 웹 응용 프로그램으로 플래그를 지정하고 `DispatcherServlet` 설정과 같은 주요 동작을 활성화합니다.

* `@ComponentScan`은 `hello` 패키지에서 다른 구성 요소, 구성 및 서비스를 찾아서 컨트롤러를 찾을 수 있도록 Spring에 지시합니다.

`main ()` 메소드는 Spring Boot의 `SpringApplication.run ()` 메소드를 사용하여 애플리케이션을 시작한다. 한 줄의 XML도 없으며 web.xml 파일도 필요 없습니다. 이 웹 응용 프로그램은 100 % 순수 자바이며 배관이나 인프라 구성에 대해 다룰 필요가 없습니다.

당신의 app에 의해 생성되었거나 Spring Boot 덕분에 자동적으로 추가 된 bean을 모두 가져와서 분류하고 인쇄합니다.

#### 단위 테스트 추가

추가 한 엔드 포인트에 대한 테스트를 추가하는 것이 좋으며 Spring Test는 이미이를위한 일부 기계를 제공하며 프로젝트에 포함하기 쉽습니다.

빌드 파일의 종속성 목록에 다음을 추가하십시오.

```groovy
testCompile("org.springframework.boot:spring-boot-starter-test")
```

Maven을 사용하는 경우 종속성 목록에 다음을 추가하십시오.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

이제 엔드 포인트를 통해 서블릿 요청 및 응답을 조롱하는 간단한 단위 테스트를 작성하십시오.

`src / test / java / hello / HelloControllerTest.java`

```java
package hello;

import static org.hamcrest.Matchers.equalTo;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.http.MediaType;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.request.MockMvcRequestBuilders;

@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
public class HelloControllerTest {

    @Autowired
    private MockMvc mvc;

    @Test
    public void getHello() throws Exception {
        mvc.perform(MockMvcRequestBuilders.get("/").accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andExpect(content().string(equalTo("Greetings from Spring Boot!")));
    }
}
```

`MockMvc`는 Spring Test에서 제공되며 편리한 빌더 클래스 세트를 통해 HTTP 요청을`DispatcherServlet`으로 보내고 결과에 대한 요청을 할 수있게합니다. `@ AutoConfigureMockMvc`와 함께`@ SpringBootTest`를 사용하여`MockMvc` 인스턴스를 삽입하십시오. `@ SpringBootTest`를 사용하여 우리는 전체 애플리케이션 컨텍스트가 만들어 지도록 요청하고 있습니다. 또 다른 방법은`@ WebMvcTest`를 사용하여 컨텍스트의 웹 레이어 만 생성하도록 Spring Boot에 요청하는 것입니다. 스프링 부트는 자동으로 애플리케이션의 메인 애플리케이션 클래스를 찾는다. 하지만 뭔가 다른 것을 만들려면 오버라이드하거나 범위를 좁힐 수있다.

HTTP 요청 사이클을 보낼뿐만 아니라 Spring Boot를 사용하여 매우 간단한 전체 스택 통합 테스트를 작성할 수 있습니다. 예를 들어 위의 모의 테스트 대신 아래 테스트를 할 수 있습니다.

`src / test / java / hello / HelloControllerIT.java`

```java
package hello;

import static org.hamcrest.Matchers.*;
import static org.junit.Assert.*;

import java.net.URL;

import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.web.client.TestRestTemplate;
import org.springframework.boot.web.server.LocalServerPort;
import org.springframework.http.ResponseEntity;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class HelloControllerIT {

    @LocalServerPort
    private int port;

    private URL base;

    @Autowired
    private TestRestTemplate template;

    @Before
    public void setUp() throws Exception {
        this.base = new URL("http://localhost:" + port + "/");
    }

    @Test
    public void getHello() throws Exception {
        ResponseEntity<String> response = template.getForEntity(base.toString(),
                String.class);
        assertThat(response.getBody(), equalTo("Greetings from Spring Boot!"));
    }
}
```

임베디드 서버는`webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT`에 의해 임의의 포트에서 시작되고 실제 포트는 런타임에`@ LocalServerPort`로 발견됩니다.

#### 생산 등급 서비스 추가
비즈니스 웹 사이트를 구축하는 경우 관리 서비스를 추가해야 할 수 있습니다. Spring Boot는 health, audits, beans 등과 같은 [actuator 모듈](http://docs.spring.io/spring-boot/docs/{spring_boot_version}/reference/htmlsingle/#production-ready)을 제공합니다.

빌드 파일의 종속성 목록에 다음을 추가하십시오.

```groovy
compile("org.springframework.boot:spring-boot-starter-actuator")
```

Maven을 사용하는 경우 종속성 목록에 다음을 추가하십시오.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

그런 다음 앱을 다시 시작하십시오.

애플리케이션에 새로운 RESTful 엔드 포인트 세트가 추가 된 것을 볼 수 있습니다. Spring Boot가 제공하는 관리 서비스입니다.

```
2018-03-17 15:42:20.088  ... : Mapped "{[/error],produces=[text/html]}" onto public org.s...
2018-03-17 15:42:20.089  ... : Mapped "{[/error]}" onto public org.springframework.http.R...
2018-03-17 15:42:20.121  ... : Mapped URL path [/webjars/**] onto handler of type [class ...
2018-03-17 15:42:20.121  ... : Mapped URL path [/**] onto handler of type [class org.spri...
2018-03-17 15:42:20.157  ... : Mapped URL path [/**/favicon.ico] onto handler of type [cl...
2018-03-17 15:42:20.488  ... : Mapped "{[/actuator/health],methods=[GET],produces=[application/vnd...
2018-03-17 15:42:20.490  ... : Mapped "{[/actuator/info],methods=[GET],produces=[application/vnd.s...
2018-03-17 15:42:20.491  ... : Mapped "{[/actuator],methods=[GET],produces=[application/vnd.spring...
```

[actuator/health](http://localhost:8080/actuator/health), [actuator/info](http://localhost:8080/actuator/info), [actuator](http://localhost:8080/actuator).

```
참고 : 또한`/actuator/shutdown` 엔드 포인트가 있지만 기본적으로 JMX를 통해서만 볼 수 있습니다. HTTP endpoint를 추가하려면
`application.properties` 파일에 `management.endpoints.shutdown.enabled = true`를 설정하십시오.
```

앱의 상태를 쉽게 확인할 수 있습니다.

```
$ curl localhost:8080/actuator/health
{"status":"UP"}
```

#### Spring Boot's starters 살펴보기
[Spring Boot's "starters"](https://docs.spring.io/spring-boot/docs/2.0.3.RELEASE/reference/htmlsingle/#using-boot-starter)

[source code](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-project/spring-boot-starters)
