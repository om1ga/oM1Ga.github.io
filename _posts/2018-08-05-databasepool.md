---
layout: post
title:  "数据库连接池"
categories: java database
tags:  java database 
author: oM1Ga
---

* content
{:toc}

**使用连接池有哪些好处？**

百度百科： 数据库连接是一种关键的、有限的、昂贵的资源，这一点在多用户的网页应用程序中体现得尤为突出。对数据库连接的管理能显著影响到整个应用程序的伸缩性和健壮性，影响到程序的性能指标。数据库连接池正是针对这个问题提出来的。

- **减少连接创建时间**

虽然与其它数据库相比 GBase 提供了较为快速连接功能，但是创建新的 JDBC 连接仍会招致网络和 JDBC 驱动的开销。如果这类连接是“循环”使用的，使用该方式这些花销就可避免。

- **简化的编程模式**

当使用连接池时，每一个单独的线程能够像创建了一个自己的 JDBC 连接一样操作，允许用户直接使用JDBC编程技术。

- **受控的资源使用**

如果用户不使用连接池，而是每当线程需要时创建一个新的连接，那么用户的应用程序的资源使用会产生非常大的浪费并且可能会导致高负载下的异常发生。

**简单来说：数据库连接池负责分配,管理和释放数据库连接,它允许应用程序重复使用一个现有的数据库连接,而不是重新建立一个。**




目前常用的Java开源连接池有两种
- DBCP 数据库连接池
- C3P0 数据库连接池

## DBCP

DBCP(DataBase Connection Pool) 是 Apache 软件基金组织下的开源连接池实现。可能是使用最多的开源连接池，原因大概是因为配置方便，而且很多开源和tomcat应用例子都是使用的这个连接池吧。
这个连接池可以设置最大和最小连接，连接等待时间等，基本功能都有。这个连接池的配置参见附件压缩包中的:dbcp.xml
使用评价：在具体项目应用中，发现此连接池的持续运行的稳定性还是可以，不过速度稍慢，在大并发量的压力下稳定性
有所下降，此外不提供连接池监控

## C3P0

CP3P0是另外一个开源的连接池，在业界也是比较有名的，这个连接池可以设置最大和最小连接，连接等待时间等，基本功能都有。
这个连接池的配置参见附件压缩包中的:c3p0.xml

## 连接池和数据源的关系

连接池：
连接池是由容器（比如Tomcat）提供的，用来管理池中的连接对象。

连接池自动分配连接对象并对闲置的连接进行回收。

连接池中的连接对象是由数据源（DataSource）创建的。

连接池（Connection Pool）用来管理连接（Connection）对象。

数据源：

数据源（DataSource）用来连接数据库，创建连接（Connection）对象。

 java.sql.DataSource接口负责建立与数据库的连接

 由Tomcat提供，将连接保存在连接池中。

`DataSource`有两种实现方式

### 1. 直连数据库方式

当调用DataSource.getConnection()时,其实它调用的是DriverManager.getConnection(url, user, password)来获取一个Connection,Connection使用完后被close,断开与数据库的连接,我们称这总方式是直连数据库,因为每次都需要重新建立与数据库之间的连接,而并没有把之前的Connection保留供下次使用.

``` java
String url = "jdbc:mysql://localhost:3306/student";
String name = "root";//将要连接数据库的账户
String password = "root";//将要连接数据库的密码
Connection connection = DriverManager.getConnection(url,name,password);
```

### 2. 池化连接方式

可以说这种方式就是使用了连接池技术.DataSource内部封装了一个连接池,当你获取DataSource的时候,它已经敲敲的与数据库建立了多个Connection,并将这些Connection放入了连接池,此时调用DataSource.getConnection()它从连接池里取一个Connection返回,Connection使用完后被close,但这个close并不是真正的与数据库断开连接,而是告诉连接池"我"已经被使用完,"你"可以把我分配给其它"人"使用了.就这样连接池里的Connection被循环利用,避免了每次获取Connection时重新去连接数据库.(`org.apache.tomcat.jdbc.pool.DataSource`)也提供了DBCP连接池的相关接口.

通过BasicDataSource对象可以配置JDBC连接池的DriverName，URL，User和Password。

```java
public static void dbpoolInit() {
    dataSource = new BasicDataSource();
    dataSource.setDriverClassName(DRIVER_NAME);
    dataSource.setUrl(DB_URL);
    dataSource.setUsername(USER);
    dataSource.setPassword(PASSWORD);
}
```

 设置完这些属性之后，可以通过`getConnection()`方法，获取一个连接。

 ``` java
 public static void dbPoolTest() {
    try {
        ct = dataSource.getConnection();
    } catch (SQLException e) {
        e.printStackTrace();
    }
}
```
上面获取的数据源实例，调用`close()`方法的逻辑由关闭连接，清理环境变成了将该连接归还给连接池中。也可以不做关闭操作，因为该操作已经由连接池接管。建议不关闭。


## JDBC

以下内容来自百度百科：
JDBC（Java DataBase Connectivity,java数据库连接）是一种用于执行SQL语句的Java API，可以为多种关系数据库提供统一访问，它由一组用Java语言编写的类和接口组成。
Java数据库连接体系结构是用于Java应用程序连接数据库的标准方法。JDBC对Java程序员而言是API，对实现与数据库连接的服务提供商而言是接口模型。作为API，JDBC为程序开发提供标准的接口，并为数据库厂商及第三方中间件厂商实现与数据库的连接提供了标准方法。JDBC使用已有的SQL标准并支持与其它数据库连接标准，如ODBC之间的桥接。JDBC实现了所有这些面向标准的目标并且具有简单、严格类型定义且高性能实现的接口。
简而言之就是JAVA程序连接(操作)数据库的一种方式。