# SQL

## 语法

大小写不敏感

### SELECT

- SELECT 列 FROM 表，*代表所有列

  eg：SELECT * FROM Persons

- SELECT **DISTINCT** 列 FROM 表

  **DISTINCT**会去除重复项

- SELECT 列 FROM 表 WHERE 列 运算符 值

  | 操作符  | 描述         |
  | ------- | ------------ |
  | =       |              |
  | <>或!=  | 不等         |
  | >       | 大于         |
  | <       | 小于         |
  | >=      | 大于等于     |
  | <=      | 小于等于     |
  | BETWEEN | 在范围内     |
  | LIKE    | 搜索某种模式 |

  eg：SELECT * FROM Persons WHERE City='Beijing'文本使用引号，数值不

- **AND**，**OR**，**NOT**可以实现逻辑运算
- **ORDER BY**实现排序，**DESC**降序，**ASC**升序



### INSERT

插入

- INSET INTO 表 VALUES （值，值）
- INSERT INTO 表 （列，列） VALUES （值，值）



### UPDATE

修改

- UPDATE 表 SET 列 = 新值 WHERE 列 = 旧

  eg：UPDATE Person SET Address = 'Zhongshan 23', City = 'Nanjing'




### DELETE

删除行

DELETE FROM 表 WHERE 列=值

DELETE FROM 表     或     DELETE * FROM 表可以删除所有行



### TOP

规定返回记录的数目

SELECT TOP 数目或百分比 列 FROM 表

eg：SELECT TOP 2 * FROM Persons

or：SELECT TOP 50 PERCENT * FROM Persons



### LIKE

用于在WHERE中搜索列中的指定模式

SELECT 列 FROM 表 WHERE 列 LIKE 模式

eg：SELECT * FROM Persons WHERE City LIKE 'N%' 会选择出所有N开头的城市，'%g'则是g结尾



### 通配符

- %一个或多个字符

- _一个字符

- [charlist]字符列中任何单一字符

- [^charlist] [!charlist] 不在字符列中的任一字符

  

### IN

可以在WHERE中规定多个值

SELECT 列 FROM 表 WHERE 列 IN（值，值，）

eg：SELECT * FROM Persons WHERE LastName IN （'Amams'，'Carter'）



### BETWEEN

选取介于两个值之间的数据，可以是数值文本或日期

SELECT 列 FROM 表 WHERE 列 BEWTEEN 值1 AND 值2

不同数据库对开闭区间的设置不同



### 别名

SELECT 列 FROM 表 AS 别名：表的别名

SELECT 列 AS 别名 FROM 表：列的别名



### JOIN

通过键连接数据表

eg：SELECT 列 FROM 表1 INNER JOIN 表2 ON 条件

- INNER JOIN 自然连接
- LEFT JOIN 左边不动，右边有的接上，没有的是NULL
- RIGHT JOIN
- FULL JOIN



### UNION

UNION用于合并两个或多个SELECT语句的结果，要求必须有相同数量的列、相似的数据类型，列的顺序必须相同

SELECT 列 FROM 表1

UNION

SELECT 列 FROM 表2

**注意：**UNION默认选取不同值，允许重复则使用UNION ALL



### SELECT INTO

SELECT INTO从一个表中选取数据，然后把数据插入另一个表，常用于创建表的备份

SELECT 列 INTO 新表 [IN 外部数据库] FROM 旧表

eg：SELECT * INTO Persons_backup FROM Persons

or：SELECT LastName,FirstName INTO Persons IN 'Backup.mdb' FROM Persons



### CREATE DB

CREATE DATABASE 数据库名



### CREATE TABLE

CREATE TABLE 表（列1 数据类型，列2，数据类型）

- integer（size） int（size）smallint（size）tinyint（size）：整数，括号内最大位数
- decimal（size，d） numeric（size，d）：浮点数，最大位数，小数最大位数
- char（size）：定长字符串
- varchar（size）：变长字符串
- date（yyyymmdd）：日期

eg：

CREATE TABLE Persons

（

ID_P int，

LastName varchar（255），

FirstName varchar（255），

Address varchar（255）

）



### 约束

创建表时可以在CREATE TABLE规定约束，创建之后在ALTER TABLE规定约束

#### NOT NULL

不接受NULL，如果不在该字段添加值则无法插入或更新记录。ID_P int NOT NULL

#### UNIQUE

唯一标识

PRIMARY KEY有自动定义的UNIQUE约束，每个表可以有多个UNIQUE但只能有一个PRIMARY KEY

CREATE TABLE Persons

（

ID_P int NOT NULL，

UNIQUE（ID_P）

）

命名UNIQUE约束或为多个列定义UNIQUE约束：

CREATE TABLE Persons

（

CONSTRAINT uc_PersonID UNIQUE （ID_P，LastName）

）

表已创建时：

ALTER TABLE 表 ADD UNIQUE （列）

命名或多列：ALTER TABLE 表 ADD CONSTRAINT 名 UNIQUE （列1，列2）

撤销约束：

ALTER TABLE 表 DROP INDEX 名

#### PRIMARY KEY

主键，唯一且不NULL

CREATE TABLE Persons

（

ID_P int NOT NULL

PRIMARY KEY （ID_P）

）

命名或多列：

CREATE TABLE Persons

（

CONSTRAINT 名 PRIMARY KEY （列1，列2）

）

表已创建：

ALTER TABLE 表 ADD PRIMARY KEY （列）

命名或多列：

ALTER TABLE 表 ADD CONSTRAINT 名 PARIMARY KEY （列1，列2）

如果使用ALTER TABLE添加主键，则必须在创建表时将主键声明为 NOT NULL

撤销约束

ALTER TABLE 表 DROP PRIMARY KEY

#### FOREIGN KEY

同PRIMARY KEY，但要在最后加上REFERENCES 被参照表（列）

eg：Persons表和Orders表，Orders表中的ID_P应

ID_P int FOREIGN KEY REFERENCES Persons（ID_P）

#### CHECK

限制列中值的范围

单列的CHECK只允许特定值

对表的CHECK会在特定列中对值进行限制

语法同PRIMARY KEY

#### DEFAULT

