# Springboot使用jpa操作数据库

## 1.在application中配置数据源

```yml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/beegotest?serverTimezone=UTC&characterEncoding=utf-8
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver
```

## 2.在pom.xml中引入mysql驱动

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>//这行意思是在运行和测试时才使用，编译时不需要
</dependency>
```

## 3.在pom.xml中引入jpa相关依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

## 4.编写实体类

**@Data注解：**idea中的一个插件lombok，加上该注解，能简化代码，提高效率。注解在类上会自动生成setter/getter、equals、canEqual、hashCode、toString方法，如为final属性，则不会为该属性生成setter方法。关于idea中安装lombok，请参考：

**@Entity注解：**JPA注解到类上表示是一个实体类，必须跟@Id注解一起使用，否则  No identifier specified for entity。name属性，表示其所对应的数据库中的表名。

**@Table：**JPA注解，当实体类与其映射的数据库表名不一致时，使用name属性简历映射，与@Entity注解一起使用

**@Column：**将实体类中的属性映射到数据库表中的字段

```java
@Data
@Entity
@Table(name = "user")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private int id;
    @Column(name = "username")
    private String username;
    @Column(name = "password")
    private String password;
}
```

## 5.编写数据访问层接口

1.接口需要继承JpaRepository类

2.可以使用JpaRepository自带的一些接口进行CRUD操作

3.可以使用@Query自定义sql语句进行CRUD操作

```java
public interface UserDao extends JpaRepository<UserEntity, Integer> {
    UserEntity findUserByName(String name);
    List<UserEntity> findByNameIn(List<String> nameList);
    UserEntity findUserById(int id);
    @Query("update UserEntity u set u.failureTimes = 0")
    @Modifying
    void updateUserFailureTimes();
    @Query("update UserEntity u set u.failureTimes = 0 where u.name='admin'")
    @Modifying
    void updateAdminFailureTimes();
    List<UserEntity> findByFailureTimesGreaterThanEqualAndParentId(Integer failureTimes, Integer parentId);
    List<UserEntity> findByParentId(Integer parentId);
    @Query("select id from UserEntity u where u.parentId=:parentId")
    List<Integer> findIdByParentId(@Param("parentId") Integer parentId);
}
```