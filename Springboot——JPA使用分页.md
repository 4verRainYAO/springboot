# Springboot——JPA使用分页

## 1.两个类

### （1）Pageable接口：

**PageRequest**是Pageable的实现类，可以通过以下工厂方法创建：

```java
public static PageRequest of(int page,int size)

public static PageRequest of(int page,int size,Sort sort)

public static PageRequest of(int page,int size,Direction direction,String ... properties)
```

page代表当前第几页，从0开始，即0表示第一页。

size表示一页显示多少条记录。

### （2）Page类：

```java
int getTotalPages()    获取总的页数

long getTotalElements()   返回总数

List getContent()     返回某页的结果集
```

然后再JPA自带的接口findAll中将PageRequest参数传进去

```java
Page<T> findAll(Pageable var1)
```



## 2.代码实现

### （1）分页对象类设计

```java
@ApiModel(value = "分页返回对象")
@Data
public class PageResult<T> {
    @ApiModelProperty(value = "总页数", example = "1")
    private Integer pageTotal;
    @ApiModelProperty(value = "总条数", example = "1")
    private Long count;
    @ApiModelProperty(value = "当前页内容")
    private List<T> pageList;
}
```

### (2)Controller设计

```java
@GetMapping("/getEmployeesByPage")
@ApiModelProperty(value = "通过get方式所有员工",notes = "get请求")
public PageResult<Employee> getEmployeesByPage(@RequestParam("index") Integer pageIndex,@RequestParam("size") Integer size){
    PageRequest pageRequest = PageRequest.of(pageIndex,size);
    Page<Employee> page = employeeDao.findAll(pageRequest);
    PageResult<Employee> result = new PageResult<>();
    result.setPageTotal(page.getTotalPages());
    result.setCount(page.getTotalElements());
    result.setPageList(page.getContent());
    return  result;
}
```

得到总页数，总记录数，和分页的数据列表