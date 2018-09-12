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