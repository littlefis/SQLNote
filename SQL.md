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

INTERSECT交，EXCEPT差



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

创建默认值

CREATE TABLE Persons

（

City varchar（255） DEFAULT 'Sandnes'

）

表已创建：

ALTER TABLE Persons ALTER City SET DEFAULT 'Sandnes'

撤销：

ALTER TABLE Persons ALTER City DROP DEFAULT

### INDEX

创建索引，可以在不读取整个表的情况下，更快地查找数据。用户无法看到索引。更新索引需要时间，所以仅在常被搜索到的列或表上创建索引

**创建：**CREATE INDEX 索引名 ON 表名 （列名，）

**唯一索引：**CREATE UNIQUE INDEX 索引名 ON 表名 （列名） 唯一索引意味着两个行不能有相同索引值



### DROP

#### DROP INDEX

删除索引

ALTER TABLE 表名 DROP INDEX 索引名

#### DROP TABLE

删除表（包括表的结构、属性、索引）

DROP TABLE 表名

#### DROP DATABASE

删除数据库

DROP DATABASE 数据库名

#### 删数据不删表

TRUNCATE TABLE 表名



### ALTER TABLE

用于在已有的表中添加修改删除列

**添加：**ALTER TABLE 表 ADD 列 数据类型

**删除：**ALTER TABLE 表 DROP COLUMN 列

**修改数据类型：**ALTER TABLE 表 ALTER COLUMN 列 数据类型



### AUTO INCREMENT

自动创建主键字段值

CREATE TABLE Persons

（

ID_P NOT NULL AUTO_INCREMENT

PRIMARY KEY （ID_P）

默认开始值为1，每次递增1，修改起始值：

ALTER TABLE Persons AUTO_INCREMENT=100



### VIEW

视图是基于SQL语句集的可视化的表，包含行和列

CREATE VIEW 视图名 AS SELECT 列 FROM 表 WHERE 条件

eg：CREATE VIEW [Current] AS SELECT id,name FROM items WHERE condition

更新时重写一次同名VIEW

撤销：DROP VIEW 视图名

### 日期

#### 函数

NOW（）当前日期和时间

CURDATE（）当前日期

CURTIME（）当前时间

DATE（）提取日期部分

EXTRACT（）返回日期/时间的单独部分

DATE_ADD（）给日期添加指定的时间间隔

DATE_SUB（）减

DATEDIFF（）日期差

DATE_FORMAT（）用不同格式显示日期/时间

#### 格式

DATE：YYYY-MM-DD

DATETIME：YYYY-MM-DD HH:MM:SS

TIMESTAMP：YYYY-MM-DD HH:MM:SS

YEAR：YYYY或YY

### NULL

测试NULL值使用 IS NULL 和 IS NOT NULL

IFNULL（列，替换值）

## 数据类型

### Text类型

| 数据类型         | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| CHAR(size)       | 保存固定长度的字符串（可包含字母、数字以及特殊字符）。在括号中指定字符串的长度。最多 255 个字符。 |
| VARCHAR(size)    | 保存可变长度的字符串（可包含字母、数字以及特殊字符）。在括号中指定字符串的最大长度。最多 255 个字符。注释：如果值的长度大于 255，则被转换为 TEXT 类型。 |
| TINYTEXT         | 存放最大长度为 255 个字符的字符串。                          |
| TEXT             | 存放最大长度为 65,535 个字符的字符串。                       |
| BLOB             | 用于 BLOBs (Binary Large OBjects)。存放最多 65,535 字节的数据。 |
| MEDIUMTEXT       | 存放最大长度为 16,777,215 个字符的字符串。                   |
| MEDIUMBLOB       | 用于 BLOBs (Binary Large OBjects)。存放最多 16,777,215 字节的数据。 |
| LONGTEXT         | 存放最大长度为 4,294,967,295 个字符的字符串。                |
| LONGBLOB         | 用于 BLOBs (Binary Large OBjects)。存放最多 4,294,967,295 字节的数据。 |
| ENUM(x,y,z,etc.) | 允许你输入可能值的列表。可以在 ENUM 列表中列出最大 65535 个值。如果列表中不存在插入的值，则插入空值。注释：这些值是按照你输入的顺序存储的。可以按照此格式输入可能的值：ENUM('X','Y','Z') |
| SET              | 与 ENUM 类似，SET 最多只能包含 64 个列表项，不过 SET 可存储一个以上的值。 |

### Number类型

| 数据类型        | 描述                                                         |
| --------------- | ------------------------------------------------------------ |
| TINYINT(size)   | -128 到 127 常规。0 到 255 无符号*。在括号中规定最大位数。   |
| SMALLINT(size)  | -32768 到 32767 常规。0 到 65535 无符号*。在括号中规定最大位数。 |
| MEDIUMINT(size) | -8388608 到 8388607 普通。0 to 16777215 无符号*。在括号中规定最大位数。 |
| INT(size)       | -2147483648 到 2147483647 常规。0 到 4294967295 无符号*。在括号中规定最大位数。 |
| BIGINT(size)    | -9223372036854775808 到 9223372036854775807 常规。0 到 18446744073709551615 无符号*。在括号中规定最大位数。 |
| FLOAT(size,d)   | 带有浮动小数点的小数字。在括号中规定最大位数。在 d 参数中规定小数点右侧的最大位数。 |
| DOUBLE(size,d)  | 带有浮动小数点的大数字。在括号中规定最大位数。在 d 参数中规定小数点右侧的最大位数。 |
| DECIMAL(size,d) | 作为字符串存储的 DOUBLE 类型，允许固定的小数点。             |

可添加UNSIGNED

### Date类型

| 数据类型    | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| DATE()      | 日期。格式：YYYY-MM-DD注释：支持的范围是从 '1000-01-01' 到 '9999-12-31' |
| DATETIME()  | *日期和时间的组合。格式：YYYY-MM-DD HH:MM:SS注释：支持的范围是从 '1000-01-01 00:00:00' 到 '9999-12-31 23:59:59' |
| TIMESTAMP() | *时间戳。TIMESTAMP 值使用 Unix 纪元('1970-01-01 00:00:00' UTC) 至今的描述来存储。格式：YYYY-MM-DD HH:MM:SS注释：支持的范围是从 '1970-01-01 00:00:01' UTC 到 '2038-01-09 03:14:07' UTC |
| TIME()      | 时间。格式：HH:MM:SS 注释：支持的范围是从 '-838:59:59' 到 '838:59:59' |
| YEAR()      | 2 位或 4 位格式的年。注释：4 位格式所允许的值：1901 到 2155。2 位格式所允许的值：70 到 69，表示从 1970 到 2069。 |

在 INSERT 或 UPDATE 查询中，TIMESTAMP 自动把自身设置为当前的日期和时间。TIMESTAMP 也接受不同的格式，比如 YYYYMMDDHHMMSS、YYMMDDHHMMSS、YYYYMMDD 或 YYMMDD。



## 函数

**语法：**SELECT function（列） FROM 表

- 合计函数面向一系列值，返回单个值，AVG MIN MAX SUM COUNT

### GROUP BY

按哪列进行组合

SELECT Customer，SUM（Price） FROM Orders

GROUP BY Customer

### HAVING

WHERE不能与合计函数一起使用



SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name
HAVING aggregate_function(column_name) operator value



eg：

SELECT Customer，SUM（Price） FROM Orders

GROUP BY Customer

HAVING SUM（Price）<2000

## 总结

| 语句                                                    | 语法                                                         |
| ------------------------------------------------------- | ------------------------------------------------------------ |
| AND / OR                                                | SELECT column_name(s) FROM table_name WHERE condition AND\|OR condition |
| ALTER TABLE (add column)                                | ALTER TABLE table_name  ADD column_name datatype             |
| ALTER TABLE (drop column)                               | ALTER TABLE table_name  DROP COLUMN column_name              |
| AS (alias for column)                                   | SELECT column_name AS column_alias FROM table_name           |
| AS (alias for table)                                    | SELECT column_name FROM table_name  AS table_alias           |
| BETWEEN                                                 | SELECT column_name(s) FROM table_name WHERE column_name BETWEEN value1 AND value2 |
| CREATE DATABASE                                         | CREATE DATABASE database_name                                |
| CREATE INDEX                                            | CREATE INDEX index_name ON table_name (column_name)          |
| CREATE TABLE                                            | CREATE TABLE table_name ( column_name1 data_type, column_name2 data_type, ....... ) |
| CREATE UNIQUE INDEX                                     | CREATE UNIQUE INDEX index_name ON table_name (column_name)   |
| CREATE VIEW                                             | CREATE VIEW view_name AS SELECT column_name(s) FROM table_name WHERE condition |
| DELETE FROM                                             | DELETE FROM table_name  (**Note:** Deletes the entire table!!)*or*DELETE FROM table_name WHERE condition |
| DROP DATABASE                                           | DROP DATABASE database_name                                  |
| DROP INDEX                                              | DROP INDEX table_name.index_name                             |
| DROP TABLE                                              | DROP TABLE table_name                                        |
| GROUP BY                                                | SELECT column_name1,SUM(column_name2) FROM table_name GROUP BY column_name1 |
| HAVING                                                  | SELECT column_name1,SUM(column_name2) FROM table_name GROUP BY column_name1 HAVING SUM(column_name2) condition value |
| IN                                                      | SELECT column_name(s) FROM table_name WHERE column_name IN (value1,value2,..) |
| INSERT INTO                                             | INSERT INTO table_name VALUES (value1, value2,....)*or*INSERT INTO table_name (column_name1, column_name2,...) VALUES (value1, value2,....) |
| LIKE                                                    | SELECT column_name(s) FROM table_name WHERE column_name LIKE pattern |
| ORDER BY                                                | SELECT column_name(s) FROM table_name ORDER BY column_name [ASC\|DESC] |
| SELECT                                                  | SELECT column_name(s) FROM table_name                        |
| SELECT *                                                | SELECT * FROM table_name                                     |
| SELECT DISTINCT                                         | SELECT DISTINCT column_name(s) FROM table_name               |
| SELECT INTO (used to create backup copies of tables)    | SELECT * INTO new_table_name FROM original_table_name*or*SELECT column_name(s) INTO new_table_name FROM original_table_name |
| TRUNCATE TABLE (deletes only the data inside the table) | TRUNCATE TABLE table_name                                    |
| UPDATE                                                  | UPDATE table_name SET column_name=new_value [, column_name=new_value] WHERE column_name=some_value |
| WHERE                                                   | SELECT column_name(s) FROM table_name WHERE condition        |