---
layout: post
title:  "leetcode collections"
categories: collection
tags:  sql 
---

* content
{:toc}


## 2018-9-7

### 超过5名学生的课

有一个courses 表 ，有: student (学生) 和 class (课程)。
请列出所有超过或等于5名学生的课。
Note:
学生在每个课中不应被重复计算。





又臭又长的解答：
``` sql
select class from (select student,class from courseS group by student,class)/*去除重复*/ T GROUP BY  class HAVING COUNT(class) >= 5 /* 按class分组，返回出现次数大于五次的class*/

```
主流答案：
``` sql
select class from courses GROUP BY class having COUNT(DISTINCT student) >=5 ; /*原来count里面可以使用DISTINCT 长知识了*/
```


### 查询连续出现的数

如表logs有如下数据
+----+-----+
| Id | Num |
+----+-----+
| 1  |  1  |
| 2  |  1  |
| 3  |  1  |
| 4  |  2  |
| 5  |  1  |
| 6  |  2  |
| 7  |  2  |
+----+-----+
返回连续出现3次的num 1
可以使用id连接来实现

``` sql
select num fro logs a, logs b, logs c where a.id = b.id-1 and b.id = c.id-1
```

MySQL中，`datediff(a, b)`可以计算**日期a**和**日期b**相差的天数`(a-b)`;


### 查询部门工资最高的员工

Employee 表包含所有员工信息，每个员工有其对应的 Id, salary 和 department Id。

+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
+----+-------+--------+--------------+
Department 表包含公司所有部门的信息。

+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
编写一个 SQL 查询，找出每个部门工资最高的员工。例如，根据上述给定的表格，Max 在 IT 部门有最高工资，Henry 在 Sales 部门有最高工资。

+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| Sales      | Henry    | 80000  |
+------------+----------+--------+


又臭又长的解答：

``` sql
/*给每个分数排上名，筛选出各个department下排名第一的员工*/
SELECT 
    d.name AS Department,
    tmp.name AS Employee,
    tmp.salary AS Salary
FROM
    Department d
        LEFT JOIN
    (SELECT 
            t.salary,
            t.name,
            t.departmentid,
            (select count(distinct(tt.salary)) from employee tt where 
            tt.salary >= t.salary and t.departmentid = tt.departmentid) rank
            
    FROM
        employee t
    GROUP BY departmentid,name order by salary desc) tmp ON d.id = tmp.departmentid
WHERE
    tmp.rank = 1;

```

主流答案：
``` sql
select d.Name as Department,e.Name as Employee,e.Salary
from Department d,Employee e
where e.DepartmentId=d.Id and e.Salary=(Select max(Salary) from Employee where DepartmentId=d.Id)
```