# Java规范

## log standard

如果log对象非自动生成(lombok),所有势力对象的名字都采用log

### 明确各个级别日志的含义
#### TRACE
通常，跟踪级别日志记录仅应用于跟踪程序中的执行流程以及标记程序中的特定位置(即确保已到达),应该只跟踪方法的关键节点，过多的trace跟踪记录可能会阻碍别人和自己
#### DEBUG
使用debug向自己或者其他开发人员report信息，记录方法的参数返回值等
#### INFO
使用info向主要人员和用户report信息
记录关键参数及方法调用的关键信息
#### WARN/ERROR
使用warn进行非严重错误信息记录(即不会影响逻辑错误或者程序异常)，使用error进行业务逻辑或者程序错误或可控/非可控异常的记录
### 注意事项
1. 使用slf4j进行日志的记录
2. 避免副作用，避免因日志记录引起的异常，或者增加对数据库的查询等
### SKILL
* **所有日志可以加上方法名作为前缀**
* **使用StringBuffers和toString()或toPrint()进行日志信息的string转换**
* 使用SLF4J的 **pattern substitution mechanism**替换字符串的累加，如果不使用该机制, **SEVERE**级别以下的日志则需要日志守卫，实例：
``` java
if(LOG.isDebugEnabled()){
     LOG.debug("This is " + "a best practice " + "example");
}
```
这样做是为了防止多余的字符串累加，减少内存的占用
* 重写对象的toString方法，避免使用Json.toString
* 只记录集合中的关键信息`log.debug("Returning user ids: {}", collect(users, "id"));`
 [Commons Beanutils](http://commons.apache.org/beanutils) 
* 避免对正常程序造成影响
* 调整日志记录的格式
参考:[打印日志的10个建议](https://blog.csdn.net/qq_38941937/article/details/82284942)
## response
采用**jsend**规范，可以参考[Jsend simple json api](https://github.com/omniti-labs/jsend)

参考资料:[Java Logging Standards and Guidelines - Knowledge Wiki](https://wiki.base22.com/btg/java-logging-standards-and-guidelines-2361.html)
[Java Logging Best Practices: How to Get More Out of Your Log Data](https://stackify.com/java-logging-best-practices/)
 [https://www.javacodegeeks.com/2011/01/10-tips-proper-application-logging.html](https://www.javacodegeeks.com/2011/01/10-tips-proper-application-logging.html) 
[Jsend simple json api](https://github.com/omniti-labs/jsend)
[JSON:API — A specification for building APIs in JSON](https://jsonapi.org/)
[ODATA 复杂json规范](http://docs.oasis-open.org/odata/odata-json-format/v4.0/errata02/os/odata-json-format-v4.0-errata02-os-complete.html#_Toc403940655)
[SWAGGER](https://github.com/swagger-api/swagger-core/wiki)
[RAML The simplest way to design APIs](https://raml.org/)
[HAL The Hypertext Application Language](http://stateless.co/hal_specification.html) 
[Developer’s Guide for REST Web Services](https://identify.pitneybowes.com/docs/identify/v1/en/rest/index.html)

## 命名规范

### 对象

### 业务类

**handler**：一般用于回调处理,当某些情况发生时触发的操作

**service** 通用服务类

**processor** 

**Provider**

**Converter** 对象转换

**Factory** 

**Container** 容器

**Context** 上下文

**Coordinator** 

**Writer**

**Reader**

**Protocol**

**editor**

**base**

**designer**

**helper** 辅助类层，一般是一些功能辅助，如SqlHelper封装数据库连接操作提供数据库操作对象，ConfigHelper帮助创建配置信息用于模块初始化构建

**manager**

**node**

**option**

**element**

**info**

**type**

**attribute**

**utils**层：工具类层，通用的、与业务无关的，可以独立出来，可供其他项目使用；

**tools**层：与某些业务有关，通用性只限于某几个业务类之间

**Delegator**

**Dispatchor**

**Registration**

#### 数据类

**VO** 值对象

**DTO** 数据传输对象

**Form** 接收页面表单的数据

**Model** Jpa中的持久化对象

**command**

### 方法

on方法 触发事件

finish方法 完成时触发事件

### 参考

https://stackoverflow.com/questions/1866794/naming-classes-how-to-avoid-calling-everything-a-whatevermanager

