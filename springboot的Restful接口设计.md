# springboot的Restful接口设计

## 1.Restful风格

GET用于查询

POST用于创建

PUT用于更新

DELETE用于删除

## 2.获取参数的几种常见注解

**@PathVariable：**使用url/{param}这种形式传参，用到该注解，一般在GET，DELETE，PUT方法会使用到，可以获取URL后所跟的参数。

```java
@GetMapping("/update/{id}")
public ResultJson<Department> updateDepartment(@PathVariable("id") int id)
```

**@RequestParam：**使用该注解来获取多个参数，在PUT和POST中常用。

```java
@PostMapping("/getUserById")
public ResultJson<User> Login(@RequestParam("id") int id)
```

**@RequestBody：**用于接受application/Json这种请求类型参数，后端需要封装一个对象

```json
{
	"username":"tom",
	"password":"123456"
}  
```

```java
 @PostMapping("/login")
 public ResultJson<User> Login(@RequestBody User user)
```

## 3.GET请求

通过url传参。

（1）/url?id=1

```java
public ResultJson<User> Login(@RequestParam("id") int id)
```

（2）/url/{id}

```java
public ResultJson<User> Login(@PathVariable("id") int id)
```

## 4.POST请求

（1）application/x-www-form-urlencoded数据格式

前端请求：

![image-20200116105550422](C:\Users\Ayao\AppData\Roaming\Typora\typora-user-images\image-20200116105550422.png)

后端接收：

```java
public ResultJson<Employee> addEmployee(String name,Double salary,Integer depno)
```

这种方式必须参数名跟 key一致。或者使用@RequestParm注解：

```java
public ResultJson<Employee> addEmployee(
	@RequestParm("name")String n,
	@RequestParm("salary")Double s,
	@RequestParm("depno")Integer d)
```

（2）application/json数据格式

前端请求：

![image-20200116110546214](C:\Users\Ayao\AppData\Roaming\Typora\typora-user-images\image-20200116110546214.png)

后端接收：使用**@RequestBody**注解到一个对象上：

```java
public ResultJson<Employee> addEmployee(@RequestBody Employee e){
        Employee employee = new Employee();
        employee.setName(e.getName());
        employee.setSalary(e.getSalary());
        employee.setDepno(e.getDepno());
        employeeDao.save(employee);
 }
```

或者使用**@RequestBody**注解到Map上：

```java
public ResultJson<Employee> addEmployee(@RequestBody Map<String,Object map){
        Employee employee = new Employee();
        employee.setName(map.get("name"));
        employee.setSalary(map.get("salary"));
        employee.setDepno(map.get("depno"));
        employeeDao.save(employee);
 }
```

## 5.PUT请求

用于更新数据，可以使用@RequestBody注解

```java
@PutMapping("/updateEmployee")
@ApiModelProperty(value = "更新员工信息",notes = "put请求")
public ResultJson<Employee> updateEmployee(@RequestBody Employee e)
```

## 6.DELETE请求

用于删除数据

根据id删除，可以将id通过url传送过来

```java
@DeleteMapping("/deleteEmployee")
@ApiModelProperty(value = "删除员工信息",notes = "delete请求")
public ResultJson<Employee> deleteEmployee(@RequestParam Integer id)
```