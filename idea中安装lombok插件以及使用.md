# idea中安装lombok插件以及使用

## 1.安装插件

在idea中，FIle--->Settings--->Plugins--->Marketplace中搜索lombok，安装。

## 2.在pom.xml中引入依赖

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <scope>provided</scope>
</dependency>
```

## 3.常用注解

**@Data：**注解在类上，会为类的所有属性自动生成setter/getter、equals、canEqual、hashCode、toString方法，如为final属性，则不会为该属性生成setter方法。

**@Getter/@Setter：**注解在属性上，为属性自动生成getter、setter方法

```java
import lombok.AccessLevel;
import lombok.Getter;
import lombok.Setter;

public class GetterSetterExample {
  @Getter @Setter private int age = 10;
  @Setter(AccessLevel.PROTECTED) private String name;
  @Override public String toString() {
    return String.format("%s (age: %d)", name, age);
  }
}
```

**@NonNull：**注解在属性或构造器上，lombok会生成一个非空的声明，可用于校验参数，能帮助避免空指针

```java
import lombok.NonNull;

public class NonNullExample extends Something {
	private String name;
	public NonNullExample(@NonNull Person person) {
		super("Hello");
	//	if (person == null) {
	//		throw new NullPointerException("person");
	//	}
		this.name = person.getName();
	}
}
```

**@Cleanup：**注解在流或者连接上，自动调用close()方法

```java
import lombok.Cleanup;
import java.io.*;

public class CleanupExample {
	public static void main(String[] args) throws IOException {
        @Cleanup 
		InputStream in = new FileInputStream(args[0]);
        @Cleanup
		OutputStream out = new FileOutputStream(args[1]);
	}
}
```

**@ToString：**注解在类上，生成一个toString()方法，默认情况下，会输出类名、所有属性（会按照属性定义顺序），用逗号来分割。可以通过exclude来排除某一属性。

