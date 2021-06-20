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

#### 2.4创建数据库：

**查询现有数据库：**`show databases;`

**创建数据库:**`create database <dbname>`

**使用（切换）数据库：**`use <dbname>`

#### 2.5创建表：

**查看当前数据库中的表：**`show tables;`

**创建表：**`create table <table_name> (字段1 , 字段2 , ...);`

```
create table pet (name varchar(20), owner varchar(20), species varchar(20), sex char(1), birth date, death date);
```

**查询表结构：**`desc <table_name>` 或 `show create table <table_name>`。

```
mysql> desc pet;
+---------+-------------+------+-----+---------+-------+
| Field   | Type        | Null | Key | Default | Extra |
+---------+-------------+------+-----+---------+-------+
| name    | varchar(20) | YES  |     | NULL    |       |
| owner   | varchar(20) | YES  |     | NULL    |       |
| species | varchar(20) | YES  |     | NULL    |       |
| sex     | char(1)     | YES  |     | NULL    |       |
| birth   | date        | YES  |     | NULL    |       |
| death   | date        | YES  |     | NULL    |       |
+---------+-------------+------+-----+---------+-------+
6 rows in set (0.00 sec)
```

#### 2.6表中添加数据：

数据：

创建一个.txt文件, 每个字段用tab隔开，没有值的字段使用\N

```
Fluffy	Harold	cat	f	1993-02-04
Claws	Gwen	cat	m	1994-03-17
Buffy	Harold	dog	f	1989-05-13
Fang	Benny	dog	m	1990-08-27
Bowser	Diane	dog	m	1979-08-31	1995-07-29
Chirpy	Gwen	bird	f	1998-09-11
Whistler	Gwen	bird	\N	1997-12-09	\n
Slim	Benny	snake	m	1996-04-29
```

**方法一：使用insert语句：**

`insert into <table_name> values(字段1 , 字段2 , ...);`

```
insert into pet values('ff','hh','cat','f','1993-02-04','1999-02-04');
Query OK, 1 row affected (0.37 sec)
```

**方法二：加载文件：**

`load data local infile <filepath> into table <table_name>;`

将.txt文件导入表中。

```
mysql> load data local infile 'pet.txt' into table pet;
ERROR 3948 (42000): Loading local data is disabled; this must be enabled on both the client and server sides
```

报错：禁止加载本地数据;这必须在客户端和服务器端都启用。

**解决方法：**

输入命令：`show variables like 'local_infile';`

```
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| local_infile  | OFF   |
+---------------+-------+
```

然后开启：`set global local_infile=on;`

启动方法是: `mysql --local-infile -u <username> -p`

```
mysql> load data local infile 'pet.txt' into table pet;
Query OK, 8 rows affected, 7 warnings (0.15 sec)
Records: 8  Deleted: 0  Skipped: 0  Warnings: 7
```

#### 2.7数据检索：

**查询：**`select * from <table_name>;`

​			`selcet <value> from <table_name> where 限定`

**删除：**`delete from <table_name>;`

​			`delete from <table_name> where 限定`

**更新：**`update <table_name> set 字段名称=更新值 where 限定`

```
update pet set birth='2020-01-01' where name='Bowser';
mysql> select * from pet;
+----------+--------+---------+------+------------+------------+
| name     | owner  | species | sex  | birth      | death      |
+----------+--------+---------+------+------------+------------+
| Fluffy   | Harold | cat     | f    | 1993-02-04 | NULL       |
| Claws    | Gwen   | cat     | m    | 1994-03-17 | NULL       |
| Buffy    | Harold | dog     | f    | 1989-05-13 | NULL       |
| Fang     | Benny  | dog     | m    | 1990-08-27 | NULL       |
| Bowser   | Diane  | dog     | m    | 2020-01-01 | 1995-07-29 |
| Chirpy   | Gwen   | bird    | f    | 1998-09-11 | NULL       |
| Whistler | Gwen   | bird    | NULL | 1997-12-09 | 0000-00-00 |
| Slim     | Benny  | snake   | m    | 1996-04-29 | NULL       |
+----------+--------+---------+------+------------+------------+
8 rows in set (0.00 sec)
```

**多条件查询：**

**AND:** `select * from <table_name> where key1 and key2`

同时满足限定key1和key2的才被查询。

```
mysql> select * from pet where species='dog' and sex='f';
+-------+--------+---------+------+------------+-------+
| name  | owner  | species | sex  | birth      | death |
+-------+--------+---------+------+------------+-------+
| Buffy | Harold | dog     | f    | 1989-05-13 | NULL  |
+-------+--------+---------+------+------------+-------+
1 row in set (0.00 sec)
```

**OR:** `select * from <table_name> where key1 or key2`

满足key1或key2即可。

**查询特定的列：**

`select <列名>,<列名> from <table_name>;`

```
mysql> select owner, birth from pet;
+--------+------------+
| owner  | birth      |
+--------+------------+
| Harold | 1993-02-04 |
| Gwen   | 1994-03-17 |
| Harold | 1989-05-13 |
| Benny  | 1990-08-27 |
| Diane  | 2020-01-01 |
| Gwen   | 1998-09-11 |
| Gwen   | 1997-12-09 |
| Benny  | 1996-04-29 |
+--------+------------+
8 rows in set (0.00 sec)
```

**去重：**

在想要去重的列名前加`distinct`即可。

```
mysql> select distinct owner from pet;
+--------+
| owner  |
+--------+
| Harold |
| Gwen   |
| Benny  |
| Diane  |
+--------+
4 rows in set (0.00 sec)
```

**排序：**

根据某个字段进行排序使用关键字`order by`默认是升序

```
select name,birth from pet order by birth;
+----------+------------+
| name     | birth      |
+----------+------------+
| Buffy    | 1989-05-13 |
| Fang     | 1990-08-27 |
| Fluffy   | 1993-02-04 |
| Claws    | 1994-03-17 |
| Slim     | 1996-04-29 |
| Whistler | 1997-12-09 |
| Chirpy   | 1998-09-11 |
| Bowser   | 2020-01-01 |
+----------+------------+
8 rows in set (0.35 sec)
```

升序：`desc`

降序：`asc`

```
select name,birth from pet order by birth asc;
+----------+------------+
| name     | birth      |
+----------+------------+
| Buffy    | 1989-05-13 |
| Fang     | 1990-08-27 |
| Fluffy   | 1993-02-04 |
| Claws    | 1994-03-17 |
| Slim     | 1996-04-29 |
| Whistler | 1997-12-09 |
| Chirpy   | 1998-09-11 |
| Bowser   | 2020-01-01 |
+----------+------------+
8 rows in set (0.00 sec)
```

多列排序：

```
select name,species,birth from pet order by species asc, birth desc;
+----------+---------+------------+
| name     | species | birth      |
+----------+---------+------------+
| Chirpy   | bird    | 1998-09-11 |
| Whistler | bird    | 1997-12-09 |
| Claws    | cat     | 1994-03-17 |
| Fluffy   | cat     | 1993-02-04 |
| Bowser   | dog     | 2020-01-01 |
| Fang     | dog     | 1990-08-27 |
| Buffy    | dog     | 1989-05-13 |
| Slim     | snake   | 1996-04-29 |
+----------+---------+------------+
8 rows in set (0.00 sec)
```

首先按照species的首字母顺序排序，再按照birth排序，故1996出现在最后的位置。

**日期计算：**

查询当前日期：

```
select curdate();
+------------+
| curdate()  |
+------------+
| 2021-06-20 |
+------------+
1 row in set (0.34 sec)
```

获取年：`year()`参数是日期。

```
select year(curdate());
+-----------------+
| year(curdate()) |
+-----------------+
|            2021 |
+-----------------+
1 row in set (0.00 sec)
```

```
select year("2018-1-5");
+------------------+
| year("2018-1-5") |
+------------------+
|             2018 |
+------------------+
1 row in set (0.00 sec)
```

获取月：`month()`参数是日期。

```
select month(curdate());
+------------------+
| month(curdate()) |
+------------------+
|                6 |
+------------------+
1 row in set (0.00 sec)
```

获取日：`day()`参数是日期。

```
select day(curdate());
+----------------+
| day(curdate()) |
+----------------+
|             20 |
+----------------+
1 row in set (0.00 sec)
```

计算岁数：`timestampdiff()`

```
select name,birth,curdate(),timestampdiff(year,birth,curdate()) as age from pet;
+----------+------------+------------+------+
| name     | birth      | curdate()  | age  |
+----------+------------+------------+------+
| Fluffy   | 1993-02-04 | 2021-06-20 |   28 |
| Claws    | 1994-03-17 | 2021-06-20 |   27 |
| Buffy    | 1989-05-13 | 2021-06-20 |   32 |
| Fang     | 1990-08-27 | 2021-06-20 |   30 |
| Bowser   | 2020-01-01 | 2021-06-20 |    1 |
| Chirpy   | 1998-09-11 | 2021-06-20 |   22 |
| Whistler | 1997-12-09 | 2021-06-20 |   23 |
| Slim     | 1996-04-29 | 2021-06-20 |   25 |
+----------+------------+------------+------+
8 rows in set (0.00 sec)
```

**null和not null值：**

对一些字段类型进行检查，判断某些字段是否为NULL，或者non-null。

```
select name,birth from pet where death is not null;
+----------+------------+
| name     | birth      |
+----------+------------+
| Bowser   | 2020-01-01 |
| Whistler | 1997-12-09 |
+----------+------------+
2 rows in set (0.00 sec)
```



### 开放数据库到局域网

MySQL 版本：8

选择数据库

```
use mysql;
```

查看当前用户信息

```
mysql> select host,user from user;
+-----------+------------------+
| host      | user             |
+-----------+------------------+
| localhost | mysql.infoschema |
| localhost | mysql.session    |
| localhost | mysql.sys        |
| localhost | root             |
+-----------+------------------+
4 rows in set (0.00 sec)
```

应该更改host名称，使得任意IP可以访问数据库

```
update user set host = '%' where user = 'root';
```

刷新权限

```
flush privileges;
```

**客机连接数据据库**

```
mysql -h （主机IP地址） -P 3306 -u （主机用户名） -p（主机密码）
```

-p后面没有空格

