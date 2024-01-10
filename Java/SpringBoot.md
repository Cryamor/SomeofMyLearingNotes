## Before Start

> Spring Boot makes it easy to create stand-alone, production-grade Spring based Applications that you can "just run".

Spring Boot简化Spring应用的创建及部署，克服Spring的配置繁琐（XML配置）和依赖繁琐（坐标&版本）问题。

### Features

- 自动配置
- 起步依赖：POM，传递依赖
- 嵌入式服务器：内嵌Tomcat

### HelloWorld

1. Maven构建一个Module，在`pom.xml`中添加所需要继承的父工程和Web开发的起步依赖坐标：

    ```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.1.4</version>
    </parent>
    ```

    ```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
    ```

2. 在`src\java`中创建包和类`org.example.controller.HelloController`，添加一个`hello`方法：

    ```java
    @RestController
    public class HelloController {
        @RequestMapping("/hello")
        public String hello(){
            return "Hello Spring Boot!";
        }
    }
    ```

3. 创建引导类，即Spring Boot项目的入口：

    ```java
    @SpringBootApplication
    public class HelloApplication {
        public static void main(String[] args){
            SpringApplication.run(HelloApplication.class, args);
        }
    }
    ```

4. 运行，在控制台中看到日志：

    ```
      .   ____          _            __ _ _
     /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
    ( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
     \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
      '  |____| .__|_| |_|_| |_\__, | / / / /
     =========|_|==============|___/=/_/_/_/
     :: Spring Boot ::                (v3.1.4)
    
    2023-10-13T14:26:54.752+08:00  INFO 23532 --- [           main] org.example.HelloApplication             : Starting HelloApplication using Java 20.0.1 with PID 23532 (F:\IdeaProjects\SpringBootHello\target\classes started by ZMC in F:\IdeaProjects\SpringBootLearning)
    ...
    ```

    注意`Tomcat started on port(s): 8080 (http) with context path`一行，访问[localhost:8080/hello](http://localhost:8080/hello)就能看到“Hello Spring Boot!”

5. 可以使用[Spring Initializr](https://start.spring.io/)跳过配置步骤

### 配置

1. properties方式：在`src\resources\application.properties`中配置自动识别内容和自定义内容：

   ```properties
   server.port=8081
   server.address=127.0.0.1
   ```

2. yaml/yml方式：在`src\resources\application.yml`（或`.yaml`）中配置，冒号后必须要有空格：

   ```yaml
   server:
     port: 8081
     address: 127.0.0.1
   ```

3. 配置优先级：Properties > yml > yaml，低优先级的重复配置会被覆盖

4. yml获取配置，以获取person中的name为例，有三种方法：

   - ```java
     @Value("${person.name}")
     private String name;
     System.out.println(name);
     ```

   - ```java
     @Autowired
     private Environment env;
     System.out.println(env.getProperty("person.name"));
     ```

   - ```java
     @Component
     @ConfigurationProperties(prefix = "person")
     public class Person {
         private String name;
     }
     public class HelloController{
         @Autowired
         private Person person;
     	System.out.println(person.name);   
     }
     ```

5. profile配置：`spring.profiles.active=dev`意味着会读取`application-dev.properties`中的配置信息（yaml同理）；或者在同一个yaml中用`---`分隔，但新版本中要改为`spring.config.activate.on-profile`；此外，还可以在命令行参数中指定：

   ```yaml
   ---
   spring.config.on-profile: dev
   server:
     port: 8081
   ---
   spring.config.on-profile: pro
   server:
     port: 8083
   ---
   spring.config.activate.on-profile: dev
   ```

6. 内部配置加载顺序：

   1. `file:./config/`
   2. `file:./`：项目根目录
   3. `classpath:/config/`
   4. `classpath:./`：`classpath`（如`resources`）根目录

7. 外部配置加载顺序：见[官方文档](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.external-config)，配置文件优先级：jar外 > 内，`application-{profile}.properties` > `application.properties`

### 整合框架

#### Junit

Spring Boot整合Junit进行单元测试

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

测试类：以`UserService`和`UserServiceTest`为例，注意各注解：

```java
// src/main/java/org/example/UserService.java
@Service
public class UserService {
    public void add(){
        System.out.println("Add Done");
    }
}
```

```java
// src/test/java/org/example/test/UserServiceTest.java
@SpringBootTest
public class UserServiceTest {
    @Autowired
    private UserService userService;
    @Test
    public void testAdd(){
        userService.add();
    }
}
```

#### Redis

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

测试：

```java
@Autowired
private RedisTemplate redisTemplate; // 默认127.0.0.1:6379
@Test
public void testSet(){
    redisTemplate.boundValueOps("name").set("zhangsan");
}
@Test
public void testGet(){
    System.out.println(redisTemplate.boundValueOps("name").get());
}
```

#### Mybatis

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>3.0.2</version>
</dependency>
```

### 内置Web服务器

Spring Boot默认使用Tomcat作为内置服务器，可以在4种之间切换（Jetty、Netty、Undertow、Tomcat）

切换至Jetty：

`spring-boot-starter-web`排除`spring-boot-starter-tomcat`：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

引入Jetty依赖：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jetty</artifactId>
</dependency>
```

报错可能是因为`jakarta-servlet`版本问题，在properties加入以解决：

```xml
<jakarta-servlet.version>5.0.0</jakarta-servlet.version>
```

## AutoConfigure 自动配置原理

###  @Conditional注解

Q: Spring Boot是如何知道要创建哪个Bean的（有没有导入坐标）？

A: @Conditional

`@Conditional`注解可以用在任何类或方法上，只有所有指定的条件都满足时才会创建Bean

`@Conditional`源码：

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Conditional {
    Class<? extends Condition>[] value();
}
```

只有一个value参数，类型是Condition数组，Condition是一个接口：

```java
@FunctionalInterface
public interface Condition {
    boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata);
}
```

只有一个match方法用于判断条件是否成立，两个入参为条件上下文context（获取环境、IOC容器、classloader对象等），和用于获取被标注对象上所有注解信息的metadata

`@Conditional`派生出Class、Bean、Property、Resources、Web Application和SpEL Expression六大类，以Class为例，有`@ConditionalOnClass`和`@ConditionalOnMissingClass`：

#### @ConditionalOnClass

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Conditional({OnClassCondition.class})
public @interface ConditionalOnClass {
    Class<?>[] value() default {};

    String[] name() default {};
}
```

判断value值中的Class类是否都可以使用classloader加载到，如果能则符合条件装配

使用方式，以`WebClientAutoConfiguration`为例：

```java
@AutoConfiguration(
    after = {CodecsAutoConfiguration.class, ClientHttpConnectorAutoConfiguration.class}
)
@ConditionalOnClass({WebClient.class})
public class WebClientAutoConfiguration {
    ...
}
```

只有当`WebClient.class`类存在时，才会加载`WebClientAutoConfiguration`到Spring容器

#### @ConditionalOnMissingClass

判断环境中没有对应Class才初始化Bean

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Conditional({OnClassCondition.class})
public @interface ConditionalOnMissingClass {
    String[] value() default {};
}
```

### @Enable*和@Import注解

Enable开头的注解原理是使用`@Import`导入一些配置类，实现Bean的**动态加载**

`@ComponentScan`注解扫描当前引导类所在包及其子包

`@Import`注解加载的类会被Spring创建并放入IOC容器，有四种方法：

1. 导入Bean
2. 导入配置类
3. 导入`ImportSelector`的实现类
4. 导入`ImportBeanDefinitionRegistrar`的实现类

#### @EnableAutoConfiguration详解

`@EnableAutoConfiguration`使用`@Import`加载配置类

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
    String ENABLED_OVERRIDE_PROPERTY = "spring.boot.enableautoconfiguration";

    Class<?>[] exclude() default {};

    String[] excludeName() default {};
}
```

采用的是导入`ImportSelector`的实现类的方式，`AutoConfigurationImportSelector`中有`selectImports`：

```java
public String[] selectImports(AnnotationMetadata annotationMetadata) {
    if (!this.isEnabled(annotationMetadata)) {
        return NO_IMPORTS;
    } else {
        AutoConfigurationEntry autoConfigurationEntry = this.getAutoConfigurationEntry(annotationMetadata);
        return StringUtils.toStringArray(autoConfigurationEntry.getConfigurations());
    }
}
```

`getAutoConfigurationEntry`方法中调用`getCandidateConfigurations`方法，可以在断言中看到配置文件位置：`META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`（在Spring Boot3之前是文件`META-INF/spring/spring.factories`），该文件中定义了大量配置类，在Spring Boot启动时会自动加载这些配置类，初始化Bean（不是所有Bean都被初始化，配置类中使用`@Conditional`加载满足条件的Bean）

### 自定义starter

参考源码中的实现

自定义一个redis-starter，先创建starter和configuration两个模块，starter引入configuration的依赖

在configuration模块中创建AutoConfiguration类，提供Jedis的Bean：

```java
@Configuration
@EnableConfigurationProperties(RedisProperties.class)
@ConditionalOnClass(Jedis.class)
public class RedisAutoConfiguration {
    @Bean
    @ConditionalOnMissingBean(name = "jedis")
    public Jedis jedis(RedisProperties redisProperties){
        return new Jedis(redisProperties.getHost(), redisProperties.getPort());
    }
}
```

在resources中创建配置文件`META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`，加入自动装配类的路径：

```
com.example.myredis.config.RedisAutoConfiguration
```

此时在测试模块中已经能通过自定义的starter引入Jedis并且能正常使用

查看`org.springframework.boot.autoconfigure.data.redis.RedisAutoConfiguration`，实际的实现与上述基本相同

## 监听机制

Spring Boot监听机制是对Java事件监听机制的封装

Java事件监听机制定义以下角色：

1. 事件Event，继承自`java.util.EventObject`
2. 事件源Source，任意对象Object
3. 监听器Listener，实现`java.util.EventListener`接口的对象

Spring Boot监听机制涉及`ApplicationContextInitializer`、`ApplicationRunner`、`CommandLineRunner`、`SpringApplicationRunListener`四个接口，将其全部实现后运行会发现只有`ApplicationRunner`和`CommandLineRunner`执行，另外两者需要在`META-INF/spring.factories`中进行配置（接口路径=实现路径）：

```
org.springframework.boot.SpringApplicationRunListener=com.example.springbootlistener.listener.MySpringApplicationRunListener
org.springframework.context.ApplicationContextInitializer=com.example.springbootlistener.listener.MyApplicationContextInitializer
```

## 启动流程分析

1. 构造（初始化）`SpringApplication`：

   1. 配置source：构造函数入参`resourceLoader`和`primarySources`，后者判断非空后创建`LinkedHashSet`存储

   2. 配置是否是Web环境：

      ```java
      this.webApplicationType = WebApplicationType.deduceFromClasspath();   
      ```

   3. 创建初始化构造器和应用监听器，获取工厂对象：

      ```java
      this.setInitializers(this.getSpringFactoriesInstances(ApplicationContextInitializer.class));       this.setListeners(this.getSpringFactoriesInstances(ApplicationListener.class));
      ```

   4. 配置应用的主方法所在类：

      ```java
      this.mainApplicationClass = this.deduceMainApplicationClass();
      ```

2. 执行`run`方法：

   1. 启动计时：

      ```java
      long startTime = System.nanoTime();
      ...
      Duration timeTakenToStartup = Duration.ofNanos(System.nanoTime() - startTime);
      ```

   2. 启用监听，`starting`调用所有Listener的`starting`方法：

      ```java
      SpringApplicationRunListeners listeners = this.getRunListeners(args);
      listeners.starting(bootstrapContext, this.mainApplicationClass);
      ```

   3. 准备环境：

      ```java
      ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
      ConfigurableEnvironment environment = this.prepareEnvironment(listeners, bootstrapContext, applicationArguments);
      ```

   4. 打印Banner（Spring图标，可以用`banner.txt`自己改）：

      ```java
      Banner printedBanner = this.printBanner(environment);
      ```

   5. 准备context：

      ```java
      context = this.createApplicationContext();
      context.setApplicationStartup(this.applicationStartup);
      this.prepareContext(bootstrapContext, context, environment, listeners, applicationArguments, printedBanner);
      ```

   6. 刷新，创建Bean：

      ```java
      this.refreshContext(context);
      ```

   7. 结束计时，启动结束。

## Actuator 监控

导入依赖：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

访问http://localhost:8080/actuator，得到`json`：

```json
{
    "_links": {
        "self": {
            "href": "http://localhost:8080/actuator",
            "templated": false
        },
        "health": {
            "href": "http://localhost:8080/actuator/health",
            "templated": false
        },
        "health-path": {
            "href": "http://localhost:8080/actuator/health/{*path}",
            "templated": true
        }
    }
}
```

在配置文件中配置显示详细健康信息：

```properties
management.endpoint.health.show-details=always
```

能在[localhost:8080/actuator/health](http://localhost:8080/actuator/health)看到完整健康信息，导入第三方库后可以监控第三方库的健康情况

显示所有endpoints：

```properties
management.endpoints.web.exposure.include=*
```

如http://localhost:8080/actuator/beans中能看到所有Bean的情况

## 项目部署

### jar包

在Maven/项目名/Lifecycle下选择package，完成jar包打包

控制台中输入

```bash
java -jar 项目名.jar
```

即可运行

### war包

在`pom.xml`中加入

```xml
<packaging>war</packaging>
```

为了能使其被Tomcat识别，需要修改启动类：

```java
@SpringBootApplication
public class SpringBootDeployApplication extends SpringBootServletInitializer {

    public static void main(String[] args) {
        SpringApplication.run(SpringBootDeployApplication.class, args);
    }

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        return builder.sources(SpringBootDeployApplication.class);
    }
}
```

打包，同jar

注意在Tomcat运行后虚拟路径会发生变化，如`/user -> /springboot/user`
