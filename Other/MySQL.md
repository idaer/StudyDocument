# Mysql速通

## 查询

**SELECT** 语句用于从数据库中选取数据。

```sql
-- *可改为对应的列名多个列名用‘,’连接
SELECT * FROM table_name WHERE (condition)
```

### SELECT DISTINCT语句

在表中，一个列可能会包含多个重复值，有时您也许希望仅仅列出不同（distinct）的值。DISTINCT 关键词用于返回唯一不同的值。

```sql
SELECT DISTINCT column1, column2, ...
FROM table_name;
```

### SELECT分页/限制查询

SELECT TOP 子句用于规定要返回的记录的数目。SELECT TOP 子句对于拥有数千条记录的大型表来说，是非常有用的。

SELECT TOP写法(SQL Server / MS Access)

```sql
-- number为总数，precent表示将结果以百分比展示
SELECT TOP number|percent column_name(s)
FROM table_name;
```

ROWNUM写法(Oracle)

```sql
SELECT column_name(s)
FROM table_name
WHERE ROWNUM <= number;
```

LIMIT写法(MySQL)

```sql
SELECT column_name(s)
FROM table_name
LIMIT number;
```

### 连接

SQL join 用于把来自两个或多个表的行结合起来。下图展示了 LEFT JOIN、RIGHT JOIN、INNER JOIN、OUTER JOIN 相关的 7 种用法。

![SQL连接](https://www.runoob.com/wp-content/uploads/2019/01/sql-join.png)

连接会将所指定的数据项保留(无论是否匹配连接条件)，将未指定的数据项抛弃(为匹配到就设置为空)；上图白色的可以为空。红色的无论是否匹配都会保留

```sql
-- LEFT JOIN、RIGHT JOIN、INNER JOIN、OUTER JOIN
SELECT column1, column2, ...
FROM table1
INNER JOIN ...|JOIN table2 ON condition;
```

### UNION操作符

UNION 操作符用于合并两个或多个 SELECT 语句的结果集。请注意，UNION 内部的每个 SELECT 语句必须拥*有相同数量的列*。列也必须拥有*相似的数据类型*。同时，每个 SELECT 语句中的*列的顺序必须相同*。

```sql
-- UNION 不允许出现出现重复的数据项(table1和table2中相同发的数据项只能出现一次)
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;

-- UNION 允许出现出现重复的数据项
SELECT column_name(s) FROM table1
UNION ALL
SELECT column_name(s) FROM table2;
```

### ORDER BY关键字

ORDER BY 关键字用于对结果集按照**一个列或者多个列**进行排序。ORDER BY 关键字默认按照升序对记录进行排序。如果需要按照降序对记录进行排序，您可以使用 DESC 关键字。

```sql
-- 多列排序是在前面的排序属性优先级更高
SELECT column1, column2, ...
FROM table_name
ORDER BY column1, column2, ... ASC|DESC;
```

## 添加

NSERT INTO 语句用于向表中插入新记录。

```sql
-- 第一种形式无需指定要插入数据的列名，只需提供被插入的值即可：
INSERT INTO table_name
VALUES (value1,value2,value3,...);
-- 第二种形式需要指定列名及被插入的值：
INSERT INTO table_name (column1,column2,column3,...)
VALUES (value1,value2,value3,...);
```

### SELECT INTO/INSERT INTO ... SELECT

通过 SQL，您可以从一个表复制信息到另一个表。SELECT INTO 语句从一个表复制数据，然后把数据插入到另一个新表中。

注意：MySQL 数据库不支持 SELECT ... INTO 语句，但支持 INSERT INTO ... SELECT 。

```sql
-- MySQL不支持这种方式
SELECT *|column_name(s)
INTO newtable [IN externaldb]
FROM table1
WHERE condition(s);

-- MySQL支持INSERT INTO ... SELECT
INSERT INTO table2
(column_name(s))
SELECT column_name(s)
FROM table1;
```

## 更改

**UPDATE** 语句用于更新表中已存在的记录。

```sql
-- 将满足WHERE条件的行修改
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

## 删除

DELETE 语句用于删除表中的行。

```sql
DELETE FROM table_name
WHERE condition;
```

## 条件限制语句

WHERE 子句用于提取那些满足指定条件的记录。

|运算符|描述|
|--|--|
|=|等于|
|<>/!=|不等于|
|>|大于|
|<|小于|
|>=|大于等于|
|<=|小于等于|
|BETWEEN|在某个范围内|
|LIKE|匹配某种模式|
|IN|匹配某些值|

### LIKE与通配符

LIKE 操作符用于在 WHERE 子句中搜索列中的指定模式。通配符与 SQL LIKE 操作符一起使用。SQL 通配符用于搜索表中的数据。

```sql
-- 通配符在pattern中使用
SELECT column1, column2, ...
FROM table_name
WHERE column LIKE pattern;
```

通配符有一下几种

|通配符|描述|
|--|--|
|%|匹配任意数量的任意字符|
|_|匹配任意一个字符|
|[charlist]|匹配charlist内的字符（正则表达式）|
|[^charlist]或[!charlist]|匹配不属于charlist内的字符（正则表达式）|

使用正则表达式及相关通配符，MySQL 中使用 `REGEXP` 或 `NOT REGEXP` 运算符 (或 `RLIKE` 和 `NOT RLIKE`) 来操作正则表达式。
下面的 SQL 语句选取 name 以 "G"、"F" 或 "s" 开始的所有网站：

```sql
SELECT * FROM Websites
WHERE name REGEXP '^[GFs]';
```

### IN操作符

IN 操作符允许您在 WHERE 子句中规定多个值。

```sql
-- IN 后括号内满足的值都会被检索
SELECT column1, column2, ...
FROM table_name
WHERE column IN (value1, value2, ...);
```

### BETWEEN操作符

BETWEEN 操作符选取介于两个值之间的数据范围内的值。这些值可以是**数值、文本或者日期**。

```sql
-- 在value1 到 value2 的值都会被检索
SELECT column1, column2, ...
FROM table_name
WHERE column BETWEEN value1 AND value2;
-- 可以再通过NOT BETWEEN检测范围外的
SELECT column1, column2, ...
FROM table_name
WHERE column NOT BETWEEN value1 AND value2;
```

## 建表

CREATE TABLE 语句用于创建数据库中的表。表由行和列组成，每个表都必须有个表名。

```sql
CREATE TABLE table_name
(
column_name1 data_type(size),
column_name2 data_type(size),
column_name3 data_type(size),
....
);
```

### 约束

SQL 约束用于规定表中的数据规则。

如果存在违反约束的数据行为，行为会被约束终止。

约束可以在创建表时规定（通过 CREATE TABLE 语句），或者在表创建之后规定（通过 ALTER TABLE 语句）。

**SQL CREATE TABLE + CONSTRAINT** 语法

```sql
-- 在对应的列名后直接加入约束关键词
CREATE TABLE table_name
(
column_name1 data_type(size) constraint_name,
column_name2 data_type(size) constraint_name,
column_name3 data_type(size) constraint_name,
....
);
```

|约束|作用|
|--|--|
|NOT NULL|该列属性不能为空|
|UNIQUE|该列属性唯一（不能重复）|
|PRIMARY KEY|为该表主键（包含了NOT NULL和UNIQUE）|
|FOREIGN KEY|一个表中的外键 （FOREIGN KEY） 指向另一个表中的 主键(唯一约束的键)|
|CHECK|约束用于限制列中的值的范围|
|DEFAULT|DEFAULT 约束用于向列中插入默认值|

#### CHECK 约束

**注意：** 未命名约束撤销是非常麻烦的（在查询中）

```sql
-- 约束一列
--MySQL
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CHECK (P_Id>0)
)
-- SQL Server / Oracle / MS Access
CREATE TABLE Persons
(
P_Id int NOT NULL CHECK (P_Id>0),
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255)
)
-- 多行约束
--如需命名 CHECK 约束，并定义多个列的 CHECK 约束
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CONSTRAINT chk_Person CHECK (P_Id>0 AND City='Sandnes')
)
```

## 其他操作

### 别名

通过使用 SQL，可以为**表名称或列名称**指定别名。基本上，创建别名是为了让列名称的可读性更强。（在现在某些框架环境下也有其他作用）

```sql
-- 列的 SQL 别名语法
SELECT column_name AS alias_name
FROM table_name;
-- 表的 SQL 别名语法
SELECT column_name(s)
FROM table_name AS alias_name;

-- 在某些时候 AS 也可以被省略
-- 列
SELECT column_name  alias_name
FROM table_name;
-- 表
SELECT column_name(s)
FROM table_name alias_name;
```

### SQL函数

~~用到的时候再查吧~~
