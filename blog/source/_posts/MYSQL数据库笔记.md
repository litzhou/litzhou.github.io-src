---
title: MySQL 数据库使用笔记
date: 2017-12-25 12:40:23
categories:
    - DB
tags:
    - MySQL
---
常见错误解决方案：
1、mysql服务启动不了，进程意外终止 1067
   错误显示：<font color="red">can not connect to mysql server on local hosts(1061)</font>
   解决方法：原来是我傻逼把原来的MySQL数据库给删掉了
2、服务已经启动，但是输入密码时 进不去
错误显示：<font color="red">ERROR 1045 <28000>:Access denied for user'root'@'locahost'<using password:YES></font>
解决方法：http://blog.sina.com.cn/s/blog_759a5a7c01017dj0.html
3、默认端口号：3306

<!--more-->

MySQL语句规范：
关键字和函数名称全部大写；
数据库名称、表的名称、字段的名称全部小写；
SQL语句必须以分好结尾。
加中括号表示可以省略

显示当前版本；
```
    mysql>SELECT VERSION();
```
显示当前时间；
``` 
mysql>SELECT NOW();
```
显示当前用户；
``` 
mysql>SELECT USER();
```

修改原始密码：
打开命令提示符界面, 执行命令: mysqladmin -u root -p password 新密码
执行后提示输入旧密码完成密码修改, 当旧密码为空时直接按回车键确认即可。


开始：
//创建数据库：
``` 
MySQL>CREATE DATABASE (IF NOT EXISTS) case;
``` 
//显示已经存在的数据库；
``` 
MySQL>SH0W DATABASES;
```
//重命名数据库名称
先关闭数据库，然后找到文件夹所在目录，更改文件夹名称。
//显示某个数据库；
``` 
MySQL>SHOW CREATE DATABASE case;
```

//更改数据库编码为utf8;
``` 
MySQL>ALTER DATABASE case CHATACTER SET=utf8;
```
//删除数据库：
``` 
MySQL>DROP DATABASE case;
```

修改和删除

修改默认值：
```
ALTER TABLE TB_NAME ALTER 字段名 SET DEFAULT 默认值;
ALTER TABLE TB_NAME ALTER 字段名 DROP DEFAULT ;
```
修改表名
``` 
ALTER TABLE 表名 RENAME TO 新名;
```
修改字段名
``` 
ALTER TABLE 表名 CHANGE 旧字段 新字段 新字段数据类型
```
修改字段数据类型
``` 
ALTER 表名 MODIFY 属性名 数据类型
```
增加字段
``` 
ALTER TABLE 表名 ADD 字段1 字段1的条件 ［FIRST | AFTER 字段2］;
```
删除字段
``` 
ALTER TABLE 表名 DROP 字段;
```
修改字段的排列位置:
``` 
ALTER TABLE 表名 MODIFY 字段1 字段1数据类型 FIRST｜AFTER 字段2;
```
更改表的存储引擎:
``` 
ALTER TABLE 表名 ENGINE＝引擎名
```
添加主键约束：
``` 
ALTER TABLE 表名 ADD PRIMARY KEY (外键名)
```
删除外键约束:
``` 
ALTER TABLE 表名 DROP FOREIGN KEY (外键别名)
```
修改数据表的名称：
``` 
ALTER  TABLE table_name RENAME TO new_table_name
```
删除记录：
``` 
delete from users where id=1；
```
修改记录：
``` 
update 表名 set 字段=新值 where 条件;
update users set id=1 whers sex=1;
```

表字段的修改：
增加字段
``` 
ALTER table tb_name ADD column_name 属性 位置;   //增加一个字段，默认为空
alter table user add COLUMN new2 VARCHAR(20) NOT NULL;  //增加一个字段，默认不能为空
```
删除字段
``` 
alter table user DROP COLUMN new2; 　//删除一个字段
alter table user DROP column1,column2;  //删除多列
```
修改一个字段
``` 
alter table user MODIFY new1 VARCHAR(10); 　//修改一个字段的类型
alter table user CHANGE new1 new4 int;　　//修改一个字段的名称，此时一定要重新指定该字段的类型
```

第二章：
1、数据类型：
整型：
>TINYINT -2^7->2^7-1
SMALLINT -2^15->2^15-1
MEDIUMINT -2^23->2^23-1
INT -2^31->2^31-1
BIGINT -2^63->2^63-1

浮点型：
>FLOAT[(M,D)] m代表总位数，d代表小数点后位数
DOUBLE[(M,D)]

时间日期型：(了解)
>YEAR 1个字节
TIME 3
DATE 3
DATETIME 8
TIMESTAMP 4

字符型：
>CHAR(M) 0<=M<=255
VARCHAR(M)
TINYTEXT
TEXT
MEDIUMTEXT
LONGTEXT
ENUM('value1,'value2'...)
SET('value1','value2',,,)

2、数据表的操作
打开数据库：
``` 
USE test(数据库名称）
```
 
创建数据表：
``` 
CREATE TABLE (IF NOT EXISTS) table_name<数据表名字>(column_name<根据项目大小确定的列名>data_type<数据类型>,..)
```
``` 
USE TEST;
CREATE TABLE tb1(
username VARCHAR(20),
age TINYINT UNSIGNED,<unsigned意思是不要负数>
salary FLOAT(8,2) UNSIGNED <float(8,2)的意思是总共有8位数，其中小数点后有2位)>
);
```


查看数据表列表
``` 
SHOW TABLES FROM test；
```

查看数据表结构
``` 
SHOW COLUMNS FROM TB1;
```

插入记录INSERT
``` 
INSERT [INTO] tb1_name [(col_name,，，)] VALUES(val,,,)
INSERT TB1 (username ,salary) VALUES ('tom',26,919.3);
```
插入的也可以是算式比如：33-2 或者函数式：MD5('342')
也可以一次插入多条记录，记录间用，分开就行。


记录查找SELECT
``` 
SELECT * FROM TB1_NAME
```


3、空值与非空

NULL 字段值可以为空  NOT NULL 字段禁止为空

创建表，设定某些量空与非空
``` 
>CREATE TABLE TB2(
>username VARCHAR(20) NOT NULL,
>age TINYINT UNSIGNED NULL,
>);
>INSERT TB2 VALUES(NULL,20);
```
<将报错说username不可为空>


4、自动编号（AUTO_INCREMENT)不能用char类型

自动编号，且需与主键组合使用
默认情况下，起始值为1，每次的增量为1；

5、主键约束(PRIMARY KEY)

每张数据表只存在一个主键
主键保证记录的唯一性
主键自动为not null

多个字段联合主键：
PRIMARY KEY(username，age);
``` 
>CREATE TABLE TB2(
>id SMALLINT UNSIGNED  PRIMARY KEY AUTO_INCREMENT,
```

AUTO_INCREMENT(自动递增)必须和PRIMARY KRY一起使用，而PRIMARY KEY则不一定要和AUTO_INCREMENT一起使用
``` 
>username VARCHAR(20) NOT NULL,
>);
>SHOW COLUMNS FROM TB3;
```
6、唯一约束(UNIQUE KEY)
唯一约束
唯一约束可以保证记录的唯一性
唯一约束的字段可以为NULL
每张数据表可以存在多个唯一约束
与主键的区别：一张数据表只能有一个主键，而UNIQUE KEY可以有多个可以NULL
``` 
>CREATE TABLE TB4(
>id SMALLINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,<自动编号字段>
>username VARCHAR(20) NOT NULL UNIQUE KEY,
>age TINYINT UNSIGNED
>);
```


插入记录：INSERT TB4(username，age) VALUES('TOM',23);
当再次写入同样的记录时，将提示错误，因为username用了unique约束。可想而知这个约束在数据表里可以有多个。

7、默认约束(DEFAULT)


默认值
当插入记录时，如果没有明确为字段赋值，则自动赋予默认值。
``` 
>CREATE TABLE TB5(
>id SAMLLINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
>username VARCHAR(20) NOT NULL UNIQUE KEY,
>sex ENUM('1','2','3') DEFAULT '3'
>);

```
验证：
``` 
INSERT TB5(username)VALUES('TOM');
```
将发现自动给sex赋值3了。

8、外键约束

要求：
表与表之间的链接
父表和子表必须使用相同的储存引擎（InnoDB)，而禁止使用临时表：
外键列和参照列必须具有相识的数据类型。其中数字的长度或是否有符号位必须相同；而字符     的长度则可以不同
外键列和参照列必须创建索引。如果外键列不存在索引的话，MySQL将自动创建索引。

``` 
CREATE TABLE provinces(
id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
pname VARCHAR(20) NOT NULL
);
```

查看数据表的引擎：
``` 
show create table provinces;
```
``` 
CREATE TABLE users(
id SMALLINT UNSIGNED PRIMARU KEY AUTO_INCREMENT,
username VARCHAR(10) NOT NULL,
//添加省份的话可以不用添加字段，只要添加关系表省份的编号就行
pid SMALLINT UNSIGNED, 
FOREIGN KEY(pid)REFERENCES provinces(id)
);
```

查看索引：
``` 
SHOW INDEXES FROM provinces\G;
SHOW INDEXES FROM users\G;
```

外键约束的参数：

CASCADE：从父表删除或更新且自动删除或更新字表中匹配的行
SET NULL：从父表删除或更新行，并设置子表中的外键列为NULL，如果使用该选项，必须保证 子表列没有指定NOT NULL
TESTRICT(约束、限制)：拒绝对父表的删除或更新操作
NO ACTION：标准的SQL的关键字，在MySQL中RESTRICT相同。


为自动编号的字段赋值

可以书写成default 或者null
创建表：
``` 
CREATE TABLE users(
id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
username VARCHAR(20) NOT NULL,
password VARCHAR(32) NOT NULL DEFAULT 123,
age TINYINT UNSIGNED NOT NULL,
sex BOOLEAN
);
```
插入记录：
``` 
INSERT users VALUES(NULL,'Jack',159357,20,1);

```