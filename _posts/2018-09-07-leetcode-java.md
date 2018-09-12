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

### 完美数  

完美数指的是一个数等于他所有的因子之和（不含自身），如`28`所有的因子有1、2、4、7、14，有`28=1+2+4+7+14`。给一个正整数n判断n是否为完美数。
n的范围为`[0,100,000,000]`,由于这个范围内只有五个完美数可以直接返回
以下是厚颜无耻的解法

``` java
bool checkPerfectNumber(int num) {
    return num==6 || num==28 || num==496 || num==8128 || num==33550336;
}
```

言归正传，由于1肯定是因子，可以提前加上，那么我们找其他因子的范围是`[2, sqrt(n)]`，如果使用i来遍历n，那么循环的条件是`i*i<n`，使用`sum`来累积各个`i`(可被n整除)下`i`和`n/i`的和,若`sum`已经大于`n`了则可以直接返回`false`，若`i*i = n`,则`i`被计算了两次，需要减去一个`i`。

``` java
 bool checkPerfectNumber(int num) {
        if (num == 1) return false;
        int sum = 1;
        for (int i = 2; i * i <= num; ++i) {
            if (num % i == 0) sum += (i + num / i);
            if (i * i == num) sum -= i;
            if (sum > num) return false;
        }
        return sum == num;
    }
```