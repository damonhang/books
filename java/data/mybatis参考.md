# mybatis参考

## mybatis

主要的类:

**SqlSessionFactoryBuilder** 用于创建SqlSessionFactory

**SqlSessionFactory** 用于获取SqlSession

**SqlSession** 非线程安全的，用于执行语句



### 核心配置configuration

- [properties](https://mybatis.org/mybatis-3/configuration.html#properties)  定义可以引用的属性

- [settings](https://mybatis.org/mybatis-3/configuration.html#settings) 

- [typeAliases](https://mybatis.org/mybatis-3/configuration.html#typeAliases) 类型别名只是Java类型的简称。 它仅与XML配置有关**可以直接指定一个package，同时有一些内置的java类alias**

- [typeHandlers](https://mybatis.org/mybatis-3/configuration.html#typeHandlers) 处理java类与sql类型之间的转换关系

- [objectFactory](https://mybatis.org/mybatis-3/configuration.html#objectFactory) 类型别名只是Java类型的简称。 它仅与XML配置有关

- [plugins](https://mybatis.org/mybatis-3/configuration.html#plugins) 插件，例如分页插件

- environments

  - environment
    - transactionManager
    - dataSource

- [databaseIdProvider](https://mybatis.org/mybatis-3/configuration.html#databaseIdProvider)

- [mappers](https://mybatis.org/mybatis-3/configuration.html#mappers)

  ```xml
  <mappers>
    <package name="org.mybatis.builder"/>
  </mappers>
  <bean id="sessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
      <property name="dataSource" ref="datasource"></property> 
      <property name="mapperLocations" value="classpath:com/fan/mapper/*.xml" /> 
       <property name="configLocation" value="classpath:mybatis/SqlMapConfig.xml"></property> -->
  </bean>
   <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
          <property name="basePackage" value="com.ken.wms.dao"/>
   </bean>
  ```

### mapper配置文件

Mapper XML文件只有几个一级的元素（按照应定义的顺序）：

cache –给定名称空间的缓存配置。

cache-ref –从另一个名称空间引用缓存配置。

resultMap –最复杂，功能最强大的元素，描述如何从数据库结果集中加载对象。

parameterMap –已弃用！ 老式的参数映射方式。 首选内联参数，将来可能会删除此元素。 此处未记录。

sql –可重用的SQL块，可由其他语句引用。

insert –映射的INSERT语句。

```xml
#{age,javaType=int,jdbcType=NUMERIC,typeHandler=MyTypeHandler}
```

update –映射的UPDATE语句。

delete –映射的DELETE语句。

select –映射的SELECT语句。

**mapUnderscoreToCamelCase** 可以设置驼峰与下划线之间的自动映射

### 缓存

本次缓存:

每次创建新会话时，MyBatis都会创建一个本地缓存并将其附加到会话。 在会话中执行的任何查询都将存储在本地缓存中，因此使用相同输入参数对相同查询的进一步执行将不会访问数据库。 更新，提交，回滚和关闭时，将清除本地缓存

默认情况下启用session缓存(本地缓存)，只存在于session存活期间，要启用全局二级缓存使用<cache/>，启用后默认有以下生效:

- 所有select都将缓存

- 所有插入更新都将刷新缓存

- 缓存使用LRU算法

- 不会基于时间表刷新

- 将存储1024个列表或对象引用

  **二级缓存是事务性的。 当事务提交或者回滚时对缓存进行更新,并且没有新增或修改语句使用flushCache=true 时**

### 动态sql

MyBatis使用基于OGNL的强大表达式



## mybatis-spring



## mybatis-spring-autoconfiguration