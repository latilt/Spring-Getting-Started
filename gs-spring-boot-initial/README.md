## [Building an Application with Spring Boot](https://spring.io/guides/gs/spring-boot/)
#### ������ ��Ʈ�� ���ø����̼� �����ϱ�
������ ��Ʈ�� ������ �� ���ø����̼��� ����� ������ ���񽺸� �߰��غ���.

```
spring boot version: 2.0.3.RELEASE
build: Gradle(2.X)
java version: 1.8
```
#### Spring Boot�� �� �� �ִ� ��
Spring Boot�� ���� ���α׷��� ������ ���� ���ִ� ����� �����մϴ�. Ŭ���� �н��� ������ bean�� ���캸�� �ո����� ������ �� �� ���� �� �κп� ���� �߰��մϴ�. Spring Boot�� ����ϸ� ����Ͻ� ��ɿ��� �����ϰ� ������ ���� ���ߵ��� ���� �� �ֽ��ϴ�.

�� :

- ������ MVC�� ���� �׻� �ʿ��� �� ���� Ư�� bean�� ������, Spring Boot�� �̵��� �ڵ����� �߰��մϴ�. ���� Spring MVC ���ø����̼��� ���� �����̳ʸ� �ʿ���ϱ� ������ ������ ��Ʈ�� �Ӻ���� Tomcat�� �ڵ����� �����մϴ�.
- Jetty�� ����Ѵٸ� ����� Tomcat�� ������ �ʰ� ��� Jetty�� �Ӻ����ؾ��մϴ�. Spring Boot�� �̸� ó���մϴ�.
- Thymeleaf�� ����ϰ� �ʹٸ� ���ø����̼� ���ؽ�Ʈ�� �׻� �߰��Ǿ���ϴ� ���� �ֽ��ϴ�. Spring Boot�� �׵��� �߰��մϴ�.

������ Spring Boot�� �����ϴ� �ڵ� ������ �� ���� ���Դϴ�. ���ÿ�, ������ ��Ʈ�� ���ع��� �ʽ��ϴ�. ���� ���, Thymeleaf�� ��� �� �ִٸ�, SpringBoot�� ���� ���α׷� ���ؽ�Ʈ�� �ڵ����� `SpringTemplateEngine`�� �߰��մϴ�. �׷��� �ڽ� ���� �������� �ڽ� ����`SpringTemplateEngine`�� �����Ѵٸ� Spring Boot�� �ϳ��� �߰����� ���� ���Դϴ�. �̷����ϸ� �ణ�� ������� ���� �� �� �ֽ��ϴ�.

���� : Spring Boot�� �ڵ带 �����ϰų� ������ �������� �ʽ��ϴ�. ��� ���� ���α׷��� ������ �� ������ ��Ʈ�� ��� ������ �������� �����Ͽ� ���� ���α׷� ���ؽ�Ʈ�� �����մϴ�.

#### ������ �� ���ø����̼� �����
���� ������ �� ���� ���α׷� �� �� ��Ʈ�ѷ��� ���� �� �ֽ��ϴ�.

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

�� Ŭ������`@ RestController` �÷��װ� �پ� �ֽ��ϴ�. ��, Spring MVC�� �� ��û�� ó�� �� �غ񰡵Ǿ����� �ǹ��մϴ�. `@ RequestMapping`��`/`��`index ()`�޼ҵ忡 �����մϴ�. ���������� ȣ���ϰų� ��� �ٿ��� curl�� ����� ��� �� �޼���� ���� �ؽ�Ʈ�� ��ȯ�մϴ�. �ֳ��ϸ�`@ RestController`�� �䰡 �ƴ� �����͸� �����ϴ� �� ��û�� �ʷ��ϴ� �� ���� �ּ� ��`@ Controller`��`@ ResponseBody`�� �����ϱ� �����Դϴ�.

#### ���� ���α׷� Ŭ���� �����
���⿡ ������Ʈ���ִ�`Application` Ŭ������ �����մϴ� :

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

`@SpringBootApplication`�� ������ ��� �߰��ϴ� ���� �ּ��Դϴ�.

* `@Configuration`�� Ŭ������ ���ø����̼� ���ؽ�Ʈ�� Bean ���� �ҽ��� �±� �����մϴ�.

* `@EnableAutoConfiguration`�� Spring Boot���� Ŭ���� �н� ����, �ٸ� bean �� �پ��� �Ӽ� ������ ������� bean �߰��� �����ϵ��� �����մϴ�.

* �Ϲ������� `@EnableWebMvc`�� Spring MVC app�� �߰� �� �������� Spring Boot�� classpath���� `spring-webmvc`�� �� �� �ڵ����� �߰��մϴ�. �̰��� ���� ���α׷��� �� ���� ���α׷����� �÷��׸� �����ϰ� `DispatcherServlet` ������ ���� �ֿ� ������ Ȱ��ȭ�մϴ�.

* `@ComponentScan`�� `hello` ��Ű������ �ٸ� ���� ���, ���� �� ���񽺸� ã�Ƽ� ��Ʈ�ѷ��� ã�� �� �ֵ��� Spring�� �����մϴ�.

`main ()` �޼ҵ�� Spring Boot�� `SpringApplication.run ()` �޼ҵ带 ����Ͽ� ���ø����̼��� �����Ѵ�. �� ���� XML�� ������ web.xml ���ϵ� �ʿ� �����ϴ�. �� �� ���� ���α׷��� 100 % ���� �ڹ��̸� ����̳� ������ ������ ���� �ٷ� �ʿ䰡 �����ϴ�.

����� app�� ���� �����Ǿ��ų� Spring Boot ���п� �ڵ������� �߰� �� bean�� ��� �����ͼ� �з��ϰ� �μ��մϴ�.

#### ���� �׽�Ʈ �߰�

�߰� �� ���� ����Ʈ�� ���� �׽�Ʈ�� �߰��ϴ� ���� ������ Spring Test�� �̹��̸����� �Ϻ� ��踦 �����ϸ� ������Ʈ�� �����ϱ� �����ϴ�.

���� ������ ���Ӽ� ��Ͽ� ������ �߰��Ͻʽÿ�.

```groovy
testCompile("org.springframework.boot:spring-boot-starter-test")
```

Maven�� ����ϴ� ��� ���Ӽ� ��Ͽ� ������ �߰��Ͻʽÿ�.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

���� ���� ����Ʈ�� ���� ���� ��û �� ������ �����ϴ� ������ ���� �׽�Ʈ�� �ۼ��Ͻʽÿ�.

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

`MockMvc`�� Spring Test���� �����Ǹ� ���� ���� Ŭ���� ��Ʈ�� ���� HTTP ��û��`DispatcherServlet`���� ������ ����� ���� ��û�� �� ���ְ��մϴ�. `@ AutoConfigureMockMvc`�� �Բ�`@ SpringBootTest`�� ����Ͽ�`MockMvc` �ν��Ͻ��� �����Ͻʽÿ�. `@ SpringBootTest`�� ����Ͽ� �츮�� ��ü ���ø����̼� ���ؽ�Ʈ�� ����� ������ ��û�ϰ� �ֽ��ϴ�. �� �ٸ� �����`@ WebMvcTest`�� ����Ͽ� ���ؽ�Ʈ�� �� ���̾� �� �����ϵ��� Spring Boot�� ��û�ϴ� ���Դϴ�. ������ ��Ʈ�� �ڵ����� ���ø����̼��� ���� ���ø����̼� Ŭ������ ã�´�. ������ ���� �ٸ� ���� ������� �������̵��ϰų� ������ ���� ���ִ�.

HTTP ��û ����Ŭ�� �����Ӹ� �ƴ϶� Spring Boot�� ����Ͽ� �ſ� ������ ��ü ���� ���� �׽�Ʈ�� �ۼ��� �� �ֽ��ϴ�. ���� ��� ���� ���� �׽�Ʈ ��� �Ʒ� �׽�Ʈ�� �� �� �ֽ��ϴ�.

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

�Ӻ���� ������`webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT`�� ���� ������ ��Ʈ���� ���۵ǰ� ���� ��Ʈ�� ��Ÿ�ӿ�`@ LocalServerPort`�� �߰ߵ˴ϴ�.

#### ���� ��� ���� �߰�
����Ͻ� �� ����Ʈ�� �����ϴ� ��� ���� ���񽺸� �߰��ؾ� �� �� �ֽ��ϴ�. Spring Boot�� health, audits, beans ��� ���� [actuator ���](http://docs.spring.io/spring-boot/docs/{spring_boot_version}/reference/htmlsingle/#production-ready)�� �����մϴ�.

���� ������ ���Ӽ� ��Ͽ� ������ �߰��Ͻʽÿ�.

```groovy
compile("org.springframework.boot:spring-boot-starter-actuator")
```

Maven�� ����ϴ� ��� ���Ӽ� ��Ͽ� ������ �߰��Ͻʽÿ�.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

�׷� ���� ���� �ٽ� �����Ͻʽÿ�.

���ø����̼ǿ� ���ο� RESTful ���� ����Ʈ ��Ʈ�� �߰� �� ���� �� �� �ֽ��ϴ�. Spring Boot�� �����ϴ� ���� �����Դϴ�.

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
���� : ����`/actuator/shutdown` ���� ����Ʈ�� ������ �⺻������ JMX�� ���ؼ��� �� �� �ֽ��ϴ�. HTTP endpoint�� �߰��Ϸ���
`application.properties` ���Ͽ� `management.endpoints.shutdown.enabled = true`�� �����Ͻʽÿ�.
```

���� ���¸� ���� Ȯ���� �� �ֽ��ϴ�.

```
$ curl localhost:8080/actuator/health
{"status":"UP"}
```

#### Spring Boot's starters ���캸��
[Spring Boot's "starters"](https://docs.spring.io/spring-boot/docs/2.0.3.RELEASE/reference/htmlsingle/#using-boot-starter)

[source code](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-project/spring-boot-starters)
