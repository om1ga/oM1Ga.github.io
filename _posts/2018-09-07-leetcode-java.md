---
layout: post
title:  "leetcode collections"
categories: collection
tags:   Algorithm Java 
---

* content
{:toc}


## 2018-9-10

### 判断一个非负整数是否能拆成两个整数的平方和

首先是比较容易想到的，判断开方的结果是否为整数，并稍微了解了下在Java中判断double类型的数值是否为整数的方法，主要有两种：

- 向下取整的结果和本身的差值在一个精度范围以内  
``` java
    double eps = 1e-10;  // 精度范围  
    return obj-Math.floor(obj) < eps; 
```
- 强制类型转换成int，并和double相比是否相等，若相等则为整数。
``` java
    double a;
    return ((int) a == a);
```





下面是完整的代码

``` java
    public boolean judgeSquareSum(int c) {
        int a = 0;
        double b=0.0;
        while(Math.pow(a, 2)<=c){//以此计算1、2、3、4的平方
            double temp = c - Math.pow(a, 2);//求得另一个加数
            b = Math.sqrt(temp);//开2次方
            if((int)b == b){//判断开方结果是否为整数
                return true;
            }
            a++;
        }
        return false;
    }
```

其次是**效率较高**的解法

``` java
public boolean judgeSquareSum(int c) {
    if(c == 0) return true;//0直接返回成功
        int i = 0;//最小数
        int j = (int)Math.sqrt(c);//最大数
        while(i<=j) {//最小数和最大数相遇
            if(i*i+j*j == c) return true;
            else if(i*i+j*j>c) {//大于结果，大数左移动
                j--;
            }else {
                i++;//否则小数右移
            }
        }
        return false;
    }
```

