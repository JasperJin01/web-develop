---
title: 接口请求响应与测试、常用插件等小工具
date: 2024-04-03
layout: post
---







# 工具&插件

代码补全插件 Codota

## lombok（插件）

- 根据成员变量生成get和set方法
- 根据成员变量生成类的构造函数
- 重写toString()和hashCode方法
- 引入日志框架logFactory，用来打印日志

这些事情lombok可以通过注解的方式帮我们自动生成。



要在pom.xml里面加上如下依赖，插件生效。

```xml
 <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
</dependency>
```

### @Data

在java类上使用@Data注解，将为我们在编译期自动生成

- 成员变量的get和set方法
- equals方法
- canEqual方法
- hashCode方法
- toString方法

### @Slf4j

将在编译期自动帮我们引入Logger日志常量，我们在代码中就直接使用log.info或log.debug打印日志即可。下图中红色代码就用Slf4j注解代替就可以了。

### @Builder

在Java类上使用Builder注解之后，我们可以使用如下代码为对象属性赋值

```java
LombokPOJO lombokPOJO = LombokPOJO.builder()
        .name("kobe")
        .age(39)
        .build();
```

### @AllArgsConstructor

AllArgsConstructor注解将为我们在编译期自动生成：全参构造函数。







# REST架构风格、RESTful服务

* REST 通过 URI 暴露资源时，会强调不要在 URI 中出现动词。例如 GET /api/dogs/{id}而不是GET /api/getDogs/{id}
* 用HTTP方法体现对资源的操作（动词）
    * GET ： 获取、读取资源
    * POST ： 添加资源
    * PUT ： 修改资源
    * DELETE ： 删除资源
* 通过HTTP状态码体现动作的结果,不要自定义
    * 200 OK 
    * 400 Bad Request 
    * 500 Internal Server Error
* 使用复数名词。例如/dogs 而不是 /dog

RESTful 服务是一种基于 REST（Representational State Transfer，表述性状态转移）架构风格的服务，它利用 HTTP 协议进行通信，通过 URI（统一资源标识符）来访问和操作资源，使用 HTTP 方法（如 GET、POST、PUT、DELETE）来表示对资源的不同操作。



# 常用注解开发RESTful接口

**在Model文件夹中新建Aritcle.java类和Reader.java类。**

- @Builder为我们提供了通过对象属性的链式赋值构建对象的方法，下文中代码会有详细介绍。
- @Data注解帮我们定义了一系列常用方法，如：getters、setters、hashcode、equals等

**新建ArticleController.java控制类文件。**控制类文件实现了四个方法（接口），分别进行的Article的增删改查。

## @RestController和@Controller标识控制类

* @RestController和@Controller：在控制器类上方加这个注解
    * 使用 @RestController 注解的类中的方法会自动将方法的返回值**转换为 JSON 或 XML 格式**，并作为 **HTTP 响应的内容**返回给客户端，而不是渲染视图。
    * @Controller进行HTML渲染

⁉️ 关于如何判断返回的格式是JSON还是XML，Spring会根据客户端请求的`Accept`头部信息来确定响应的内容格式。如果客户端请求中包含`Accept: application/json`头部信息，Spring会将响应内容转换为JSON格式；如果客户端请求中包含`Accept: application/xml`头部信息，Spring会将响应内容转换为XML格式。

## AjaxResponse类

**<font color = red>创建一个类AjaxResponse，用于统一规范接口响应的数据格式。</font>**

比如不创建AjaxResponse类时，ArticleController中的GET函数可能返回一个Article类。

那么这时将要返回的Article类进行抽象（Object类更通用），然后顺带**isok（请求是否处理成功）、code（请求响应状态码（200、400、500））、message（请求结果描述信息，给人看的）**信息，打包成一个AjaxResponse类返回。



## @RequestMapping（类级别或方法级别）

**使用 `@RequestMapping` 注解表示特定的处理方法将处理指定的 URL 路径（用value字段表示）。**

**同时声明请求的方法（GET, POST, PUT, DELETE）。请求方法结合RequestMapping可以简写。**

```java
@RequestMapping(value = "/articles", method = RequestMethod.POST)
@PostMapping("/articles")
```



## @PathVariable

方法参数的值来自于在请求的 URL 的路径，所以叫PathVariable。

```java
@GetMapping("/articles/{id}")
public AjaxResponse getArticle(@PathVariable("id") Long id) {
    // ...
}
```



## @RequestBody、@RequestParam和@RequestHeader（处理 HTTP 请求）

`@RequestBody`: 用于**将 HTTP 请求的请求体（Body）中的数据绑定到方法的参数上**。通常用于处理 POST、PUT 等请求中发送的 JSON 或 XML 格式的数据。例如：

```java
@PostMapping("/users")
public ResponseEntity<User> createUser(@RequestBody User user) {
    // 创建用户
    userService.createUser(user);
    return ResponseEntity.ok(user);
}
```





RequestParam挺好理解，就是在URL后面加?xxx=xxx做参数

```java
public AjaxResponse saveArticle(@RequestParam  String author,
                              @RequestParam  String title,
                              @RequestParam  String content,
                              @DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
                              @RequestParam  Date createTime){

log.info("saveArticle:" + createTime);
return AjaxResponse.success();
```



`@RequestHeader`: 用于**获取 HTTP 请求头中的值**，并将其绑定到方法的参数上。通常用于获取请求头中的某个特定信息，如用户代理、授权信息等。例如：

```java
@GetMapping("/users")
public ResponseEntity<User> getUser(@RequestHeader("User-Agent") String userAgent) {
    // 根据 User-Agent 查询用户
    User user = userService.getUserByUserAgent(userAgent);
    return ResponseEntity.ok(user);
}
```





# 测试

后端（接口）测试工具postman。模拟表单提交、json参数提交等。



前端的createTime是字符串，后端（Article类）的字符串是Date类型。需要在configuration中配置：

```
spring.jackson.date-format=yyyy-MM-dd HH:mm:ss
spring.jackson.time-zone=GMT+8
```





## 四种传参方式





POST方法传参，在body中



测试方法





# 其他

## ==Spring Boot集成第三方类库的步骤==

1. 通过maven引入springboot-XXXX-starter
2. 修改yaml或properties全局统一配置文件
3. 加入一个Java Config。这个属于个性化配置，如果使用通用配置，这一步不需要。



Artifact



==application.properties改成application.yml（pom.xml和这些东西是一个意思吗？是一个含义吗？）==



