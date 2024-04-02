---
title: 数据库SQL语句用法总结
date: 2024-03-27
category: Jekyll
layout: post
---

# 表





# 索引





# 元组的增删改查





# 视图





# 权限






# 触发器







# 🔵 练习题

## 查询等练习

一、

学生Student（sno,sname,sbirthday, ssex）

课程Course（cno,cname,teacher）

选修SC（sno,cno,score）

请用一条Sql语句完成查询：







🔵 **查询存在两名及以上不及格成绩的课程名、最低成绩、最高成绩和平均成绩。**

```sql
Select course.cname, Min(sc.score), Max(sc.score), Avg(sc.score) 
From course,sc Where course.cno = sc.cno
Where sc.cno in (
    select cno from sc where score < 60 group by cno having count(*) > 1
)
Group By course.cname
```





**🔵 若“操作系统”成绩不为空，则更新每个同学的“数据库”课程成绩为“操作系统”课程成绩。**

```sql
# 题干中给出的是课程的cname，需要使用到course表。先简化问题：
# 假设操作系统课程的cno是OS，数据库课程的cno是DB
Update sc Set score = (
    Select score From sc sc1 
    Where sc1.sno = sc.sno And sc1.cno = 'OS'
) # sc查询每一条元组，通过子查询获取该元组OS课程成绩
Where cno = 'DB' # 更新的是数据库课程成绩
And Exists ( # 子查询确保OS成绩非空
    Select * From sc sc2 
	Where sc2.sno = sc.sno And cno = 'OS' And sc2.score is not null
)

# 参考答案：
Update sc Set score =
(Select score From sc sc1 
Where sc1.sno=sc.sno And sc1.cno in
(Select cno From course Where cname =’操作系统’))
Where cno in (Select cno From course Where cname =’数据库’) 
And Exists (Select * From sc sc2 
Where sc2.sno = sc.sno And sc2.score is not null And cno in
(Select cno From course Where cname =’操作系统’))
```





## 授权练习

关系模式如下：

职工（职工号，姓名，年龄，职务，工资，部门号）

部门（部门号，名称，经理名，地址，电话号）

请用SQL的GRANT 和REVOKE语句（加上视图机制）完成以下授权定义或存取控制功能。

**用户王明对两个表有SELECT 权力。**

```sql
GRANT SELECT ON 职工,部门
TO 王明


```

**每个职工只对自己的记录有SELECT 权力。**

```sql
CREATE VIEW 张三记录 AS
    SELECT *
    FROM 职工
    WHERE 职工.姓名=‘张三’
GRANT SELECT ON 张三记录 TO 张三

GRANT SELECT ON 职工
WHEN USER()=NAME
TO ALL;

```

