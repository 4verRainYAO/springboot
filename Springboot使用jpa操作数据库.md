# Springboot使用jpa操作数据库

## 1.在application中配置数据源

```yml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/beegotest?serverTimezone=UTC&characterEncoding=utf-8
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver
  jpa:
    hibernate:
      #根据实体类自动创建表，更新或者创建数据表结构
      ddl-auto: update
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

**@Data注解：**idea中的一个插件lombok，加上该注解，能简化代码，提高效率。注解在类上会自动生成setter/getter、equals、canEqual、hashCode、toString方法，如为final属性，则不会为该属性生成setter方法。关于idea中安装使用lombok，请参考：https://github.com/4verRainYAO/springboot/blob/master/idea%E4%B8%AD%E5%AE%89%E8%A3%85lombok%E6%8F%92%E4%BB%B6%E4%BB%A5%E5%8F%8A%E4%BD%BF%E7%94%A8.md

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

4.dao层中使用@Query注解的sql语句注意点

```java
public interface DepartmentDao extends JpaRepository<Department,Integer> {
    @Query("update Department d set d.empNum=:num where d.deptNo=:id ")
    @Modifying
    void updateNumber(@Param("num")Integer num,@Param("id")Integer id);
}
```

（1）使用实体类代替表名；

（2）使用实体的属性名代替字段名

（3）使用=：的形式代替参数

（4）=：后面的参数要跟**@Parm**注解中的一致

（5）update跟delete语句需要加上  **@Modifying**注解

（6）在Service上，需要加上**@Service**跟**@Transactional**

