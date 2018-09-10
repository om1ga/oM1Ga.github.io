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