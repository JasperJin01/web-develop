---
title: 数据库相关
date: 2024-04-05
layout: post
---



# 🔴 安装MySQL

`/Users/jmjin/opt/anaconda3/bin/mysql` 并不是 MySQL 服务器，而是 MySQL 客户端工具。在终端输入mysql后会定位到这个mysql，是错误的。所以`mysql.server start`要把前面的mysql路径补全才能启动。

具体来说，server完整路径是在bin目录下的，brew下载的路径在`/opt/homebrew/Cellar/`。例如我的启动路径如下：

```bash
/opt/homebrew/Cellar/mysql/8.3.0_1/bin/mysql.server start
```

数据库的数据和安装文件是分开装的，所以卸载并重装数据库后，数据库中的文件（包括之前的密码）是被保留的。,



**设置数据库和关系**

```sql
CREATE TABLE `article` (
	`id` INT(11) NOT NULL AUTO_INCREMENT,
	`author` VARCHAR(32) NOT NULL,
	`title` VARCHAR(32) NOT NULL,
	`content` VARCHAR(512) NOT NULL,
	`create_time` DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
	PRIMARY KEY (`id`)
)
COMMENT='文章'
ENGINE=InnoDB;
```



在pom.xml文件中引入驱动：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```

在application.yml文件中加入配置：

```yml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/webtestdb?useUnicode=true&characterEncoding=utf-8&useSSL=false
    username: root
    password: 20011013
```







# SpringJDBC

## @Repository 标识持久层

@Repository 用于标识一个类是数据访问层（Repository 层）的组件。通常用于 DAO（Data Access Object）类或者其他与数据库交互的类上。



## @Resource 依赖注入

当 Spring 扫描到带有 `@Resource` 注解的类时，它会创建该类的实例，并将其纳入到 Spring 的容器中，以便其他组件可以通过**依赖注入**来使用该类。

> 依赖注入（Dependency Injection，简称 DI）是一种设计模式，它是指不通过new一个对象的实例，而是通过函数调用传参来传入（注入）一个对象。



## JdbcTemplate

SpringJDBC为我们提供了jdbc的模板类JdbcTemplate。

`JdbcTemplate` 封装了 JDBC 的底层细节，无需手动处理数据库连接、Statement 和 ResultSet 等资源，**提供了一组方法来执行 SQL 查询、更新等操作**。

使用JdbcTemplate需要在pom.xml（Maven）中引入下面依赖：

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
</dependency>
或者
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```



## BeanPropertyRowMapper类

**BeanPropertyRowMapper的作用（映射）**：将查询结果中的每行数据映射到指定 Java 类的属性中。

例如，如果数据库查询结果中有一个列名为 `create_time`，而 Java 类中有一个属性名为 `createTime`，`BeanPropertyRowMapper` 就会自动将查询结果中的 `create_time` 列映射到 Java 类的 `createTime` 属性中。



## Controller、Service、Repository分三层

Controller控制器做接口，接收HTTP请求并相应。

服务层处在Controller和Repository之间，Controller调用Controller，执行操作。**涉及到事务的处理**。

持久层/Repository层/DAO层都是差不多的，指的是和数据打交道的层。



**Service层的实现：**

在service层中一般要有接口，然后实现接口。

@Service标识持久层







## @Transactional





## JpaRepository接口

