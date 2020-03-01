## springboot参考

## 核心

 `@SpringBootApplication`, 作为 `@EnableAutoConfiguration`和 `@ComponentScan`的组合 

#### 基本使用

@import和@importSource导入其他配置类

##### 自动配置

如果定义了相关bean，则不会自动配置

排除自动配置：

```java
@EnableAutoConfiguration(exclude={DataSourceAutoConfiguration.class})
```

 或者spring.autoconfigure.exclude属性

**自动配置根据是否包含对应jar**

#### application

在ApplicationContext启动之前的事件可以通过`SpringApplication.addListeners(…)`来注册

监听器添加方式:

```java
1.在MATA-INF/spring.factory中添加 org.springframework.context.ApplicationListener=com.example.project.MyListener
2.在SpringApplication.addListeners SpringApplicationBuilder.listeners
```

主要的application类型:`AnnotationConfigServletWebServerApplicationContext`

使用`ApplicationArguments`来获取启动参数项

ApplicationRunner` or `CommandLineRunner 可以在Springapplication启动后执行指定代码

#### 配置

[配置官方参考](https://docs.spring.io/spring-boot/docs/2.1.12.BUILD-SNAPSHOT/reference/html/boot-features-external-config.html)

[主要的配置项](https://docs.spring.io/spring-boot/docs/2.1.14.BUILD-SNAPSHOT/reference/html/common-application-properties.html)

获取配置的主要三种方式:

- @Value注解
- `Environment`抽象
- @ConfigurationProperties 注解

可以通过@ConfigurationProperties(prefix="my")读取配置信息 需要使用**EnableConfigurationProperties**

可以通过下列方式传递参数:

- application.properties 该文件可以放在CURRENT_DIR、CURRENT_DIR/config、CLASSPATH、CLASSPATH/config 下，可以通过spring.config.name制定文件

- ```shell
  java -Dspring.application.json='{"name":"test"}' -jar myapp.jar
  ```

- application-{profile}.properties 默认profile为default

产生随机配置:`my.secret=${random.value}`

可以使用env中的变量作为插值`app.description=${app.name}`

spring.config.name可以指定配置地址

##### 单文件多profile

```yaml
server:
  port: 8000
---
spring:
  profiles: default
  security:
    user:
      password: weak
```

#### profile

指定profile方式

- 命令行--spring.profiles.active=dev,hsqldb
- property文件 spring.profiles.active=dev,hsqldb
- 环境变量 export SPRING_PROFILES_ACTIVE=dev

#### 日志

可以在classpath下配置`logging.config`文件进行日志配置，并加载对应的配置文件 例如:`logback.xml`

```properties
logging.level.root=warn
logging.level.org.springframework.web=debug
logging.level.org.hibernate=error
logging.group.tomcat=org.apache.catalina, org.apache.coyote, org.apache.tomcat
logging.level.tomcat=TRACE
```

#### 国际化

messages.properties配置文件

```properties
spring.messages.basename=messages,config.i18n.messages
spring.messages.fallback-to-system-locale=false
```

#### JSON

默认采用Jackson 并自动配置`ObjectMapper`

#### WEB

- 使用`HttpMessageConverter`进行request和response的转化，可以自自定义Converter
- 使用@JsonComponent 自定义json转化器
- 默认包含FreeMarker等模板引擎
- 可以通过`ResponseEntityExceptionHandler`进行通用异常处理
- 可以在error目录下添加5XX对应的错误页面或者模板引擎

##### 静态资源

默认放在/static或者/public` or `/resources` or `/META-INF/resources下

src/main/webapp只在打成war的时候起作用

```properties
spring.mvc.static-path-pattern=/resources/**
spring.resources.static-locations
```

##### 内嵌容器

- 使用ServletComponentScan扫描 WebServlet WebFilter等

- 可以通过WebServerFactoryCustomizer编程自定义化容器

#### 数据源

`spring-boot-starter-jdbc` 和 `spring-boot-starter-data-jpa` 默认使用`HikariCP`连接池，如果有自定义的`DataSource`那么自动配置不会进行 查看[`DataSourceProperties`](https://github.com/spring-projects/spring-boot/tree/2.1.x/spring-boot-project/spring-boot-autoconfigure/src/main/java/org/springframework/boot/autoconfigure/jdbc/DataSourceProperties.java) 支持的配置

JdbcTemplate` and `NamedParameterJdbcTemplate会自动配置

#### 缓存

@`EnableCaching`开启缓存支持

使用[`RestTemplate`](https://docs.spring.io/spring-boot/docs/2.1.12.BUILD-SNAPSHOT/reference/html/boot-features-resttemplate.html)和[`WebClient`](https://docs.spring.io/spring-boot/docs/2.1.12.BUILD-SNAPSHOT/reference/html/boot-features-webclient.html)调用web服务[`WebServiceTemplate`](https://docs.spring.io/spring-boot/docs/2.1.12.BUILD-SNAPSHOT/reference/html/boot-features-webservices.html#boot-features-webservices-template)

#### 自动配置

依托实现@`Configuration`进行

在项目中需要定义META-INF/spring.factories，并指明EnableAutoConfiguration`格式如下`:

```properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.mycorp.libx.autoconfigure.LibXAutoConfiguration,\
com.mycorp.libx.autoconfigure.LibXWebAutoConfiguration
```

> 自动配置只能以这种方式加载。 确保在特定的程序包空间中定义它们，并且决不要将它们作为组件扫描的目标。 此外，自动配置类不应启用组件扫描以查找其他组件。 应该使用特定的@Imports代替

##### 自定义

1. 命名示例:acme-spring-boot-starter
2. 分为`autoconfigure`和`starter`模块
3. 自动配置模块包含开始使用该库所需的所有内容
4. 使用注释处理器来收集元数据文件（META-INF / spring-autoconfigure-metadata.properties）中自动配置的条件。 如果存在该文件，它将用于急切过滤不匹配的自动配置
5. stater模块依赖`autoconfigure`模块。

#### 生产

[`spring-boot-actuator`](https://github.com/spring-projects/spring-boot/tree/2.1.x/spring-boot-project/spring-boot-actuator) 模块提供生产就绪功能



### 工具

springboot cli

开发者工具spring-boot-devtools