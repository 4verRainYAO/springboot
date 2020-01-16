# springboot
关于springboot编写项目的一些总结以及一些关于springboot的面试问题

## 1.为什么要使用spring？

- 方便解耦，便于开发（Spring就是一个大工厂，可以将所有对象的创建和依赖关系维护都交给spring管理）
- spring支持aop编程（spring提供面向切面编程，可以很方便的实现对程序进行权限拦截和运行监控等功能）
- 声明式事务的支持（通过配置就完成对事务的支持，不需要手动编程）
- 方便程序的测试，spring 对junit4支持，可以通过注解方便的测试spring 程序
- 方便集成各种优秀的框架（）
- 降低javaEE API的使用难度（Spring 对javaEE开发中非常难用的一些API 例如JDBC,javaMail,远程调用等，都提供了封装，是这些API应用难度大大降低）

## 2.什么是AOP？

简单来说就是统一处理某一“切面”（类）的问题的编程思想，比如统一处理日志、异常等。

这种在运行时，动态地将代码切入到类的指定方法、指定位置上的编程思想就是面向切面的编程。

AOP是Spring提供的关键特性之一。AOP即面向切面编程，是OOP编程的有效补充。

使用AOP技术，可以将一些系统性相关的编程工作，独立提取出来，独立实现，然后通过切面切入进系统。

从而避免了在业务逻辑的代码中混入很多的系统相关的逻辑——比如权限管理，事物管理，日志记录等等。

这些系统性的编程工作都可以独立编码实现，然后通过AOP技术切入进系统即可。从而达到了 将不同的关注点分离出来的效果。

我的理解就是可以使用事务跟日志而不影响其他的业务逻辑？

## 3.什么是IOC？

ioc：Inversionof Control（中文：控制反转）是 spring 的核心，对于 spring 框架来说，就是由 spring 来负责控制对象的生命周期和对象间的关系。在Java开发中，IoC意味着将你设计好的类交给系统去控制，而不是在你的类内部控制。这称为控制反转。

简单来说，控制指的是当前对象对内部成员的控制权；控制反转指的是，这种控制权不由当前对象管理了，由其他（类,第三方容器）来管理。

通俗地讲：就是把原本你自己制造，使用的对象，现在交由别人制造，而通过构造函数，setter方法或方法（这里指使用这个对象的方法）参数的方式传给你，由你使用。

#### DI（Dependency Injection 依赖注入）

容器调用set方法或者构造器来建立对象之间的依赖关系

## 4.spring有哪些主要模块？

Spring有七大功能模块，分别是Spring Core，AOP，ORM，DAO，MVC，WEB，Context。

- **Spring Core**

  Core模块是Spring的核心类库，Spring的所有功能都依赖于该类库，Core主要实现IOC功能，Sprign的所有功能都是借助IOC实现的。

- **AOP**

  AOP模块是Spring的AOP库，提供了AOP（拦截器）机制，并提供常用的拦截器，供用户自定义和配置。

- **ORM**

  Spring 的ORM模块提供对常用的ORM框架的管理和辅助支持，Spring支持常用的Hibernate，ibtas，jdao等框架的支持，Spring本身并不对ORM进行实现，仅对常见的ORM框架进行封装，并对其进行管理

- **DAO模块**

  Spring 提供对JDBC的支持，对JDBC进行封装，允许JDBC使用Spring资源，并能统一管理JDBC事物，并不对JDBC进行实现。（执行sql语句）

- **WEB模块**

  WEB模块提供对常见框架如Struts1，WEBWORK（Struts 2），JSF的支持，Spring能够管理这些框架，将Spring的资源注入给框架，也能在这些框架的前后插入拦截器。

- **Context模块**

  Context模块提供框架式的Bean访问方式，其他程序可以通过Context访问Spring的Bean资源，相当于资源注入。

- **MVC模块**

  WEB MVC模块为Spring提供了一套轻量级的MVC实现，在Spring的开发中，我们既可以用Struts也可以用Spring自己的MVC框架，相对于Struts，Spring自己的MVC框架更加简洁和方便。

## 5.spring常用的注入方式有哪些？

- setter 属性注入
- 构造方法注入
- 注解方式注入

## 6.spring 中的 bean 是线程安全的吗？

spring 中的 bean 默认是单例模式，spring 框架并没有对单例 bean 进行多线程的封装处理。

实际上大部分时候 spring bean 无状态的（比如 dao 类），所有某种程度上来说 bean 也是安全的，但如果 bean 有状态的话（比如 view model 对象），那就要开发者自己去保证线程安全了，最简单的就是改变 bean 的作用域，把“singleton”变更为“prototype”，这样请求 bean 相当于 new Bean()了，所以就可以保证线程安全了。

- 有状态就是有数据存储功能。
- 无状态就是不会保存数据。

## 7.spring 自动装配 bean 有哪些方式？

- no：默认值，表示没有自动装配，应使用显式 bean 引用进行装配。
- byName：它根据 bean 的名称注入对象依赖项。
- byType：它根据类型注入对象依赖项。
- 构造函数：通过构造函数来注入依赖项，需要设置大量的参数。
- autodetect：容器首先通过构造函数使用 autowire 装配，如果不能，则通过 byType 自动装配。

## 8.spring事务实现方式有哪些？

- 声明式事务：声明式事务也有两种实现方式，基于 xml 配置文件的方式和注解方式（在类上添加 @Transaction 注解）。
- 编码方式：提供编码的形式管理和维护事务。

## 9.spring中用到的设计模式

单例模式：spring创建对象，IOC

代理模式：AOP

