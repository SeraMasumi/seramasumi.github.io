---
layout: default
title: SQL总结
nav_order: 5
---



##基本操作

选取列

```sql
SELECT col1, col2 FROM table1
```
选取所有列
```sql
SELECT * FROM table1
```
选取不同的列 每列只出现一次
```sql
SELECT DISTINCT col1 from table1
```
有条件地选取列
```sql
SELECT col1 FROM table1 WHERE col operator value
SELECT * FROM Persons WHERE City=‘SJose’ # 字符串要加单引号 等于= 不等于<> 可加AND, OR
```
对结果集进行排序
```sql
SELECT Customer, Order FROM Orders ORDER BY Company DESC, Order ASC # 默认升序，降序用DESC
```
插入新row
```sql
INSERT INTO Persons VALUES ('Jingnong', 23)
```
插入新row，有些col有数据有些没有
```sql
INSERT INTO Persons (LastName, Age) VALUES ('Jingnong', 23)
```
更新一个row中的一些col
```sql
UPDATE Person SET FirstName='Wang', Age='12' WHERE LastName='Jingnong'
```
删除
```sql
DELETE FROM Person WHERE LastName='Jingnong'
```
在保留table的情况下删除所有row
```sql
DELETE * FROM table1
```
限制返回的条数
```sql
SELECT Customer FROM Orders LIMIT 1
```
用通配符搜索指定模式
```sql
SELECT col1 FROM table1 WHERE City LIKE 'N%' # 以N开头 '%'用来替代一个或多个字符，'_'用来代替一个字符
SELECT col1 FROM table1 WHERE City LIKE '%g' # 以g结尾
SELECT col1 FROM table1 WHERE City NOT LIKE '%ion%' # 不含ion
SELECT * FROM Persons WHERE City LIKE '[!ALN]%' # 不以A或L或N开头。[charList]中任意一个匹配 [!charList]中任意都不匹配
```
给WHERE匹配多个值用IN
```sql
SELECT * FROM table1 WHERE Name IN ('Jingnong', 'Nina')
```
选取列并制定别名Alias：AS
```sql
SELECT LastName AS Family, FirstName AS Name FROM Persons
```
给表别名
```sql
SELECT o.OrderId, p.Name FROM Orders AS o, Persons AS p WHERE o.OrderId = 3
```



##JOIN

JOIN 和 ON 搭配使用。

| JOIN       | Explain                                                      |
| ---------- | ------------------------------------------------------------ |
| JOIN       | 如果表中有至少一个匹配，则返回行 (即 INNER JOIN)             |
| LEFT JOIN  | 即使右表中没有匹配，也从左表返回所有的行 (即 LEFT OUTER JOIN) |
| RIGHT JOIN | 即使左表中没有匹配，也从右表返回所有的行 (即 RIGHT OUTER JOIN) |
| JOIN       | 只要其中一个表中存在匹配，就返回行                           |

列出所有人的订购
```sql
SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo
FROM Persons, Orders
WHERE Persons.Id_P = Orders.Id_P 
===
SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo
FROM Persons
INNER JOIN Orders
ON Persons.Id_P = Orders.Id_P
ORDER BY Persons.LastName
```
列出所有人，有订购的话也列出订购
```sql
SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo
FROM Persons
LEFT JOIN Orders
ON Persons.Id_P=Orders.Id_P
ORDER BY Persons.LastName
```
列出所有订购，有人的话也列出以及订购的人
```sql
SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo
FROM Persons
RIGHT JOIN Orders
ON Persons.Id_P=Orders.Id_P
ORDER BY Persons.LastName
```



## 合计函数 GROUP BY

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name

SELECT Customer,SUM(OrderPrice) FROM Orders
GROUP BY Customer # 对每个customer分别计算orderPrice
```

合计函数不能使用 WHERE，取而代之的是 HAVING

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name
HAVING aggregate_function(column_name) operator value

SELECT Customer,SUM(OrderPrice) FROM Orders
GROUP BY Customer
HAVING SUM(OrderPrice)<2000 # 显示sum小于2000的
```

和 WHERE 合用

```sql
SELECT Customer,SUM(OrderPrice) FROM Orders
WHERE Customer='Bush' OR Customer='Adams'
GROUP BY Customer
HAVING SUM(OrderPrice)>1500
```



##其他操作

合并多个SELECT的结果(表cols结构必须相同)(会自动去重)

```sql
SELECT column_name(s) FROM table_name1
UNION
SELECT column_name(s) FROM table_name2
```
不去重 UNION ALL
```sql
SELECT E_Name FROM Employees_China
UNION ALL
SELECT E_Name FROM Employees_USA
```
拷贝某些列到另一个表
```sql
SELECT LastName,FirstName
INTO Persons_backup
FROM Persons
```
创建DB
```sql
CREATE DATABASE database_name
```
创建table
```sql
CREATE TABLE Persons
(
    Id_P int,
    LastName varchar(255),
    FirstName varchar(255)
)
```
**创建table时的约束**：(直接加在变量后面) (Id_P int NOT NULL,)

NOT NULL # 不接受NULL

UNIQUE # Primary Key有自动定义的UNIQUE约束。

PRIMARY KEY # 主键必须唯一，不含NULL，每个表必须有且只有一个主键。

FOREIGN KEY # 指向另一表中的PRIMARY KEY

CHECK # 限制值范围 (CHECK (Id_P > 0))

DEFAULT # 默认值 City varchar(255) DEFAULT 'LA'

AUTO_INCREMENT # 每插入一次自增1



给某一列建立索引以便更快的查询(Index用户是看不到的)

```sql
CREATE INDEX PersonIndex
ON Person (LastName) 
```
DROP：删除索引，表和数据库

```sql
DROP TABLE table1
DROP DATABASE db1
```


ALTER TABLE：在已有的表中添加、修改或删除列
```sql
ALTER TABLE Persons
ADD Birthday date

ALTER TABLE Person
DROP COLUMN Birthday

ALTER TABLE Persons
ALTER COLUMN Birthday year # 把Birthday的数据类型改成year
```

**处理时间和日期(MySQL)**
DATE - 格式 YYYY-MM-DD
DATETIME - 格式: YYYY-MM-DD HH:MM:SS
TIMESTAMP - 格式: YYYY-MM-DD HH:MM:SS
YEAR - 格式 YYYY 或 YY

比较日期(用字符串就行)
```sql
SELECT * FROM Orders WHERE OrderDate='2008-12-26'
```
Built-in日期函数
NOW()   # 返回当前的日期和时间
CURDATE()   # 返回当前的日期
CURTIME()   # 返回当前的时间
DATE()  # 提取日期或日期/时间表达式的日期部分
EXTRACT()   # 返回日期/时间按的单独部分
DATE_ADD()  # 给日期添加指定的时间间隔
DATE_SUB()  # 从日期减去指定的时间间隔
DATEDIFF()  # 返回两个日期之间的天数
DATE_FORMAT()   # 用不同的格式显示日期/时间


**NULL**
是遗漏的未知数据，默认的是可以存放NULL值。不能用=，<>这种比较运算符来判断NULL。
IS NULL
IS NOT NULL
```sql
SELECT LastName,FirstName,Address FROM Persons
WHERE Address IS NOT NULL
```



## SQL函数

```sql
SELECT function(col1) FROM table1
AVG()
SELECT AVG(OrderPrice) AS OrderAverage FROM Orders # 获得平均值
SELECT Customer FROM Orders WHERE OrderPrice > (SELECT AVG(OrderPrice) FROM Orders) # 获得比avg高的
COUNT()
SELECT COUNT(col1) FROM table1
SELECT COUNT(*) FROM table1
SELECT COUNT(DISTINCT col1) FROM table1
MAX() MIN()
SELECT MAX(OrderPrice) AS LargestOrderPrice FROM Orders
SUM()
SELECT SUM(OrderPrice) AS OrderTotal FROM Orders
```
转换为大写 UCASE()，小写 LCASE()
```sql
SELECT UCASE(LastName) as LastName, FirstName FROM Persons
```

substring函数 MID() # 1基的
```sql
SELECT MID(City,1,3) as SmallCity FROM Persons
```

stringLength函数 LEN()
```sql
SELECT LEN(City) as LengthOfCity FROM Persons
```

四舍五入 ROUND()
```sql
SELECT ROUND(column_name,decimals) FROM table_name
SELECT ProductName, ROUND(UnitPrice,0) as UnitPrice FROM Products # 四舍五入到整数
```


## 例题

