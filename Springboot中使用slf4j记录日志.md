# Springboot中使用slf4j记录日志

## 1.安装lombok插件，并引入依赖

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <scope>provided</scope>
</dependency>
```

## 2.在yml文件中配置日志级别

```yml
logging:
  level:
#日志级别
    root: info
  pattern:
  #日志输出格式
    console: '%d{yyyy-MM-dd HH:mm:ss} %-5level [%thread] %logger{15} - %msg%n'
  file:
  #日志文件输出路径
    path: D:\java\log
```

## 3.在需要日志的类上加入注解@Slf4j

```java
@Slf4j
@RestController
@Api(value = "user",description = "用户操作")
public class UserController
```

该注解相当于：

```java
private static Logger log = LoggerFactory.getLogger(UserController.class);
```

