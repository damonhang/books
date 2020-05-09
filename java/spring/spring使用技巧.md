# Spring使用技巧

## springboot

#### 配置文件加密:

```xml
<dependency>
    <groupId>com.github.ulisesbocchio</groupId>
    <artifactId>jasypt-spring-boot</artifactId>
    <version>2.0.0</version>
</dependency>
```

基于jasypt-spring-boot处理配置文件加密，基于`EnvironmentPostProcessor`预处理配置文件