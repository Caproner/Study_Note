# SQL笔记

至少我得懂得怎么用数据库吧。。。

参考教程：runoob

基于mysql进行学习

## SQL入门

### 一个简单的示例

```mysql
use RUNOOB;
set names utf8;
SELECT * FROM Websites;
```

首先是前面两句。这两句是mysql用的。

+ `use RUNOOB`：选择RUNOOB数据库
+ `set names utf8`：设置使用的字符集为utf8

接着就是通用的sql语句：

+ `SELECT * FROM Websites`：拉取表Websites

#### 一些需要注意的地方

+ sql对大小写不敏感，也就是说`select`和`SELECT`是等效的（你想要的话甚至可以`sELecT`，但这个就属于道德问题的讨论范畴了）
+ 跟C++类似的，sql语句的结尾用分号表示

### SELECT 语句

select语句用于查询数据库

```sql
SELECT * FROM Websites
SELECT name,country FROM Websites;
```

### SELECT DISTINCT 语句

select distinct语句用于在查询的基础上**去重**

```sql
SELECT DISTINCT country FROM Websites;
```

### WHERE 子句

where接在select语句后面，用于添加筛选条件

```sql
SELECT * FROM Websites WHERE country='CN';
```

### AND & OR 运算符

and和or运算符用于对多个条件进行逻辑运算（与运算&或运算）

```sql
SELECT * FROM Websites
WHERE alexa > 15
AND (country='CN' OR country='USA');
```

### ORDER BY 关键字

order by关键字接在select语句后面，用于将结果进行升序排序

```sql
SELECT * FROM Websites
ORDER BY alexa;
```

当然也可以使用desc关键字进行降序排序

```sql
SELECT * FROM Websites
ORDER BY alexa DESC;
```

并且其排序可以支持多列排序

```sql

```

### INSERT INTO 语句

insert into语句用于向表中插入数据

```sql
INSERT INTO Websites (name, url, alexa, country)
VALUES ('百度','https://www.baidu.com/','4','CN');
```

### UPDATE 语句

update语句用于更新表中的数据

```sql
UPDATE Websites 
SET alexa='5000', country='USA' 
WHERE name='菜鸟教程';
```

注意一般情况下**一定要加WHERE子句**，否则UPDATE更新会覆盖表中的所有记录

除非你本来就想这么做

### DELETE 语句

delete语句用于删除表中的数据

```sql
DELETE FROM Websites
WHERE name='百度' AND country='CN';
```

如果需要清空表中的内容则使用如下语句

```mysql
# 两个语句是等效的
DELETE FROM Websites;
DELETE * FROM Websites;
```

## SQL命令查询

### SELECT LIMIT 语句

在select语句的基础上使用limit关键字可以限定只查看前几条记录

```mysql
# 查找表中前2条记录
SELECT * FROM Websites LIMIT 2;
```

### LIKE 操作符

like操作符可以用于进行近似匹配

```mysql
# 查找表中name以G开头的数据
SELECT * FROM Websites
WHERE name LIKE 'G%';
```

like操作符后面接匹配用字符串，其中就会涉及到通配符

#### 通配符

通配符表如下所示

| 通配符 | 描述               |
| ------ | ------------------ |
| `%`    | 匹配零个或多个字符 |
| `_`    | 匹配一个字符       |

#### 运用正则表达式

可以将like关键字替换为regexp或者rlike，那么之后接的字符串使用的便是正则表达式的匹配规则

```mysql
SELECT * FROM Websites
WHERE name REGEXP '^[^A-H]';
```

### IN 操作符

in操作符用于where子句中，用于其规定多个值（相较于等于号而言的，等于号只能规定一个值）

```mysql
SELECT * FROM Websites
WHERE name IN ('Google','菜鸟教程');
```

### BETWEEN 操作符

between操作符用于where子句中，用于界定值的范围

```mysql
# 选取alexa值在1到20的范围内的数据
SELECT * FROM Websites
WHERE alexa BETWEEN 1 AND 20;
```

### 别名

可以使用as操作符来指定别名

```mysql
# 于是打印出来的表中就不会出现name和country，而是出现n和c
SELECT name AS n, country AS c
FROM Websites;
```

别名还可以用在查询语句本身中，用于缩短语句

```mysql
SELECT w.name, w.url, a.count, a.date 
FROM Websites AS w, access_log AS a 
WHERE a.site_id=w.id and w.name="菜鸟教程";
```

### 连接

在查询的过程中有可能会需要将两个表通过某个属性对应连接起来进行查询

这个时候就需要使用到连接

连接即为，将两个表的每行数据以某种规则（通常是a.xxx = b.xxx这种）匹配连接形成新的表（但只是临时的）

#### 内连接（INNER JOIN）

内连接生成的表中的数据需要全部都有匹配才会有。

假设有如下表格（Websites）：

```
+----+--------------+---------------------------+-------+---------+
| id | name         | url                       | alexa | country |
+----+--------------+---------------------------+-------+---------+
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝          | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程      | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博          | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
| 7  | stackoverflow | http://stackoverflow.com/ |   0 | IND     |
+----+---------------+---------------------------+-------+---------+
```

和另外一个表格（access_log）：

```
+-----+---------+-------+------------+
| aid | site_id | count | date       |
+-----+---------+-------+------------+
|   1 |       1 |    45 | 2016-05-10 |
|   2 |       3 |   100 | 2016-05-13 |
|   3 |       1 |   230 | 2016-05-14 |
|   4 |       2 |    10 | 2016-05-14 |
|   5 |       5 |   205 | 2016-05-14 |
|   6 |       4 |    13 | 2016-05-15 |
|   7 |       3 |   220 | 2016-05-15 |
|   8 |       5 |   545 | 2016-05-16 |
|   9 |       3 |   201 | 2016-05-17 |
+-----+---------+-------+------------+
```

那么对其使用如下语句：

```mysql
SELECT Websites.name, access_log.count, access_log.date
FROM Websites
INNER JOIN access_log
ON Websites.id=access_log.site_id
ORDER BY access_log.count;
```

得到的结果为：

```
+---------------+--------+-------------+
| name          | count  | date        |
+---------------+--------+-------------+
| 淘宝           |     10 | 2016-05-14  |
| 微博           |     13 | 2016-05-15  |
| Google        |     45 | 2016-05-10  |
| 菜鸟教程        |    100 | 2016-05-13  |
| 菜鸟教程        |    201 | 2016-05-17  |
| Facebook      |    205 | 2016-05-14  |
| 菜鸟教程        |    220 | 2016-05-15  |
| Google        |    230 | 2016-05-14  |
| Facebook      |    545 | 2016-05-16  |
+---------------+--------+-------------+
```

#### 左外连接（LEFT JOIN）

左外连接则是在内连接的基础上，左边的表中的记录如果跟右边的表没有任何匹配，也需要单独弄一行出来（跟右边相关的数据置为NULL）

```mysql
SELECT Websites.name, access_log.count, access_log.date
FROM Websites
LEFT JOIN access_log
ON Websites.id=access_log.site_id
ORDER BY access_log.count DESC;
```

#### 右外链接（RIGHT JOIN）

跟左外连接相反地，右边的表中的记录如果跟左边的表没有任何匹配，也需要单独弄一行出来（跟左边相关的数据置为NULL）

```mysql
SELECT Websites.name, access_log.count, access_log.date
FROM access_log
RIGHT JOIN Websites
ON access_log.site_id=Websites.id
ORDER BY access_log.count DESC;
```

### UNION 操作符

union操作符用于将两个select的结果合并

不同于连接的合并，union的合并是**将两个表的行叠在一起**，也就是说合并前的两个表的**每一列都必须有相似的数据类型（并且列数相同）**

其中如果直接使用union的话会**默认去掉重复的行**，如果想保留重复的行可以使用union all

```mysql
SELECT country FROM Websites
UNION
SELECT country FROM apps
ORDER BY country;
```

```mysql
SELECT country FROM Websites
UNION ALL
SELECT country FROM apps
ORDER BY country;
```



## SQL函数查询

