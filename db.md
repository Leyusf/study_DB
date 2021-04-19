# 数据库：

### 1.介绍：

#### 1.1数据库介绍：

是一个文件系统，通过SQL语言操作。

按照数据结构来组织、存储和管理数据的仓库。

#### 1.2数据库分类：

分为关系型、非关系型。

##### 关系型：

既需要保存数据也需要维护数据之间的关系可以使用关系型数据库。

E-R：实体关系图。

实体：类似于java中的对象。 （E-R图中矩形表示）

属性：成为实体的数据。 （E-R图中椭圆表示）

关系：实体和实体之间的关系。 （E-R中菱形表示）

### 2.MySQL:

#### 2.1登录MySQL：

`mysql -u <username> -p <pwd>`

```
C:\WINDOWS\system32>mysql -u root -p
Enter password: ******
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.23 MySQL Community Server - GPL

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
```

#### 2.2退出：

`qiut`即可退出。

#### 2.3输入查询：

**MySQL不区分大小写**

**以分号为一句，可以一行多句**

**SQL写了一半又不想执行可以在句尾写上\c：**

```
mysql> select sin(pi()/4)\c
mysql>
```

**查询MySQL版本号及当前时间：**

`select version(), current_date;`

查询结果：

```
+-----------+--------------+
| version() | current_date |
+-----------+--------------+
| 8.0.23    | 2021-04-19   |
+-----------+--------------+
1 row in set (0.00 sec)
```
