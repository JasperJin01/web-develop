---
title: 华中科技大学考研复试 数据库笔记
date: 2024-03-18
category: Jekyll
layout: post
---



# 第一章 数据库概述

1. DB（数据库）、DBMS（数据库管理系统）、DBS（数据库系统）三者之间的关系：**DBS包括DB和DBMS**。
    * 数据库系统（DBS）是由数据库（DB）、数据库管理系统（DBMS）、应用程序和数据库管理员（Database Administrator，DBA）组成。![截屏2024-03-29 11.07.40](/Users/jmjin/Library/Application Support/typora-user-images/截屏2024-03-29 11.07.40.png)
2. 概念词：实体entitiy，**联系**（一对一/一对多/多对多，两个实体之间联系/实体自己的联系/多个实体联系），关系（就是一张表），元组，属性，**码key**（唯一标识，不含多余属性），数据模型，二级映像与数据的独立性
    * 候选码 candidate key：主属性组，可以唯一地标识一个元组
    * 主码 primary key：从多个候选码中选出一个座位主码
    * 主属性：候选码的属性称为主属性；非主属性：不包含在任何候选码中的属性
3. **B+索引**属于内模式，**关系（数据表）、触发器**属于模式，**视图**属于外模式/子模式。
4. 关系代数的五个基本操作：投影（**去重**），选择，并，差，笛卡尔积。关系代数中的除法R ÷ S**用于找出R中所有与S中所有元组都有关系的元组**。
    1. 数据独立性（体现的是应用程序与使用数据的独立，总之当数据库如何改变，不影响应用程序，应用程序不变）分类：
    2. 逻辑（数据）独立性——改变模式，或改变模式/子模式映像，内模式不变，应用程序不变
    3. 物理（数据）独立性——改变内模式，应用程序不变

5. unique（唯一可为NULL）
6. 自然连接 ⋈，左外连接（就是把左边关系R中要舍弃的元组保留）
7. 聚集索引
8. **完整性约束**：
    1. 实体完整性：**主属性**不能去空值
    2. 参照完整性：确保表中的外键值必须在另一个表的主键中存在，或者是NULL（如果允许的话）
    3. 用户自定义完整性：UNIQUE、NOT NULL、CHECK
9. **数据库范式**：
    * 1NF：一个正确的数据表就满足1NF。反例：一个列包含子列，就不是1NF。
    * 2NF：每一个非主属性完全依赖于码。例如：AB是码，但是有B->C，则关系不满足2NF。
    * 3NF：不存在非主属性对码的传递依赖。
    * BCNF：
    * 4NF：
10. 求**闭包**
11. 求**最小函数依赖集（最小覆盖）**过程/**极小化过程**：
       1. 对于所有形如A->BC拆分成A->B, A->C
       2. 对于所有形如DE->F查看是否有D->E（或者E->D），有则去掉DE->F中多余的E（或者D） 
       3. 对于所有依赖（例如X->Y），假设将其去掉，看剩余的函数依赖是否可以由X决定Y，如果可以则将X->Y去掉。
12. 🔴 **模式分解**：
        1. **具有无损连接性的模式分解**：R与分解后R1...Rn自然连接的结果相等
        1. **保持函数依赖的模式分解**
13. 数据库设计各阶段的典型步骤：画ER图（概念结构设计），数据库外模式（逻辑结构设计），生成DBMS系统支持的数据模型（逻辑结构设计），建立索引（物理结构设计）
14. **聚簇索引和非聚簇索引**：
      * **聚簇索引**：将数据存储与索引放到了一块，找到索引也就找到了数据。非聚簇索引：将数据存储于索引分开结构，索引结构的叶子节点指向了数据的对应行。
      * 一个基本表最多只能建立一个聚簇索引。**经常更新的列不宜建立聚簇索引**。
15. 连接操作的实现：
      1. 排序合并法
      2. 嵌套循环法
      3. 索引连接/Hash连接

16. 数据库设计相关概念
       * 数据抽象（分类 ismember of，聚集is part of，概括is subset of）
       * 🟠冲突的种类：属性冲突、命名冲突、结构冲突==每种的例子==
       * 🟠数据冗余，消除冗余的方法
17. 锁的复习：① 锁的种类（S、X、意向锁，粒度树，🔴相容矩阵 ）
      * **相容矩阵：IS和IS, S, IX, SIX相容，IX和IX相容，S和S相容**
18. 自主存取控制DAC（C2），强存取控制MAC（B1）。
      1. 










# SQL查询练习

### 除法，涉及到相关嵌套子查询

教学数据库有3个关系：


* 学生信息关系 student(<u>sno</u>，sname，age，sex)
* 学生选课关系 sc(<u>sno，cno</u>，score)
* 学校课程关系 course(<u>cno</u>，cname)

**(1) 检索所学课程包含了C002课程的学生学号**

**思路**: 查找所有选择了C002课程的学生学号。这个问题相对简单，可以直接通过查询选课关系`sc`表，筛选出`cno`为C002的记录即可。

```sql
SELECT DISTINCT sno FROM sc WHERE cno = 'C002';
```



**(2) 求至少选择了C001和C003两门课程的学生学号**

- **方法一**（使用NOT EXISTS）: 在sc表中排除那些没有选C001或C003的学生。
- **方法二**（使用自连接）: 对选课表`sc`自连接，条件为学号相同，一次筛选`cno`为C001，一次为C003，然后选出同时满足两个条件的学生学号。

```sql
SELECT DISTINCT sno FROM sc A 
WHERE NOT EXISTS (
    SELECT * 
    FROM course B 
    WHERE cno IN ('C001', 'C003') 
    AND NOT EXISTS (
        SELECT * 
        FROM sc C 
        WHERE A.sno = C.sno AND B.cno = C.cno
    )
);
```

* 外层查询(SELECT DISTINCT sno FROM sc A): 选择所有学生的学号，目的是从这个全集中筛选符合条件的学生。
* 第一个NOT EXISTS: 对于每个学生（由外层查询的A.sno指定），我们检查是否存在他们没有选的课程C001或C003。
    * 第二个NOT EXISTS: 对于每门课（由上一层的B.cno指定），我们检查是否存在没有这门课的记录。
        * SELECT * FROM sc C WHERE A.sno = C.sno AND B.cno = C.cno: 确定学生A是否选了课程B。如果这个查询找不到记录（意味着学生A没选课程B），那么第二个NOT EXISTS的条件成立，表明学生A至少缺少一门必选的课程，因此他们不应该被包括在结果中。

```sql
SELECT s1.sno
FROM (SELECT * FROM sc WHERE cno = 'C001') AS s1
JOIN (SELECT * FROM sc WHERE cno = 'C003') AS s2
ON s1.sno = s2.sno;
```

* FROM (SELECT * FROM sc WHERE cno = 'C001') AS s1: 创建一个临时表s1，包含所有选了C001的记录。
* JOIN (SELECT * FROM sc WHERE cno = 'C003') AS s2: 将s1与另一个临时表s2进行连接，s2包含所有选了C003的记录。
* ON s1.sno = s2.sno: 连接条件是两个临时表中的学生编号（sno）相同。这样的连接确保了我们只会得到同时选了这两门课的学生。

**(3) 求至少学习了学生S003所学课程的学生学号**

**思路**: 先找出S003学生所选的所有课程，然后找出所有选了这些课程的其他学生，确保这些学生至少选了S003学生所选的所有课程。

```sql
SELECT DISTINCT A.sno 
FROM sc AS A 
WHERE NOT EXISTS (
    SELECT * 
    FROM sc AS B 
    WHERE B.sno = 'S003' 
    AND NOT EXISTS (
        SELECT * 
        FROM sc AS C 
        WHERE A.sno = C.sno AND B.cno = C.cno
    )
);
```

1. **外层查询**: 选择所有学生的学号（`sno`），确保学号是唯一的。
2. **第一层嵌套NOT EXISTS**: 对于每个学生，检查是否存在S003所选的课程，这些课程没有被当前考虑的学生选过。
3. **第二层嵌套NOT EXISTS**: 对于S003选过的每门课程，检查当前考虑的学生是否也选了这门课。

**(4) 求选择了全部课程的学生的学号**

**思路**: 对于每门课程，检查是否所有学生都至少选了这门课程。这可以通过双重`NOT EXISTS`实现：对于每门课（外层`NOT EXISTS`），找不到一个学生（内层`NOT EXISTS`）没有选这门课。

```sql
SELECT DISTINCT A.sno 
FROM sc AS A 
WHERE NOT EXISTS (
    SELECT * 
    FROM course AS B 
    WHERE NOT EXISTS (
        SELECT * 
        FROM sc AS C 
        WHERE A.sno = C.sno AND B.cno = C.cno
    )
);

```

1. **外层查询**: 选出所有学生的学号。
2. **第一层嵌套NOT EXISTS**: 检查是否存在课程，这门课程没有被当前考虑的学生选过。
3. **第二层嵌套NOT EXISTS**: 对于每门课程，确认当前学生是否选了这门课。

**(5) 求选择了全部课程的学生的学号和姓名**

**思路**: 类似于上一题，但需要在学生信息表`student`和选课关系表`sc`之间进行连接操作，以获取满足条件的学生的学号和姓名。

```sql
SELECT DISTINCT A.sno, S.sname 
FROM student AS S 
JOIN sc AS A ON S.sno = A.sno 
WHERE NOT EXISTS (
    SELECT * 
    FROM course AS B 
    WHERE NOT EXISTS (
        SELECT * 
        FROM sc AS C 
        WHERE A.sno = C.sno AND B.cno = C.cno
    )
);
```

1. **JOIN操作**: 首先，通过JOIN操作连接`student`表和`sc`表，这样我们可以同时获取学生的学号和姓名。
2. **外层查询**: 选择所有学生的学号和姓名。
3. **第一层嵌套NOT EXISTS**: 检查是否存在课程，这门课程没有被当前考虑的学生选过。
4. **第二层嵌套NOT EXISTS**: 确认当前学生是否选了每门课。
