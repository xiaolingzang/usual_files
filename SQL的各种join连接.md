# SQL的各种join连接

最常见的 JOIN 类型：**SQL INNER JOIN**（简单的 JOIN）、**SQL LEFT JOIN**、**SQL  RIGHT JOIN**、**SQL FULL JOIN**，其中前一种是内连接，后三种是外链接。

假设我们有两张表，Table A是左边的表，Table B是右边的表

|id | name | id|address|
| :--------|:-------| :--------|:------|
| 1|Google|1|美国|
| 2|淘宝|5|中国|
| 3|微博|3|中国|
| 4|Facebook|6|美国|

## INNER JOIN

内连接是最常见的一种连接，只连接匹配的行,INNER JOIN与JOIN是相同
### INNER JOIN用法
```
select column_name(s)
from table1
INNER JOIN table2
ON
table1.column_name=table2.column_name
```

<div align="center">
 ![](https://i.imgur.com/iRm7Wma.png)
</div>

```
select * from Table A inner join Table B
on Table A.id=Table B.id
```
执行上述语句，得到：  

| id | name | address |
|:--------|:-------| :--------|
|1|Google|美国|
|2|微博|中国|

## LEFT JOIN

LEFT JOIN返回**左表的全部行和右表满足ON条件的行**，如果左表的行在右表中没有匹配，那么这一行右表中对应数据用NULL代替，在某些数据库中，LEFT JOIN 称为LEFT OUTER JOIN。

### LEFT JOIN 语法
```
select column_name(s)
from table 1
LEFT JOIN table 2
ON table 1.column_name=table 2.column_name
```
<div align="center">
![](https://i.imgur.com/GWmB3S2.png)
</div>

```
select * from Table A left join Table B
on Table A.id=Table B.id
```
执行以上SQL输出结果如下：  

| id | name |address|
|:--------|:-------| :--------|
|1|Google|美国|
|2|淘宝|null
|3|微博|中国|
|4|Facebook|null|

## RIGHT JOIN
RIGHT JOIN返回右表的全部行和左表满足ON条件的行，如果右表的行在左表中没有匹配，那么这一行左表中对应数据用NULL代替。在某些数据库中，RIGHT JOIN 称为RIGHT OUTER JOIN。

### RIGHT JOIN语法
```
select column_name(s)
from table 1
RIGHT JOIN table 2
ON table 1.column_name=table 2.column_name
```
<div align="center">
![](https://i.imgur.com/qKVvUxV.png)
</div>

执行以上SQL输出结果如下：

| id | name |address|
|:--------|:-------| :--------|
|1|Google|美国|
|2|null|中国|
|3|微博|中国|
|4|null|美国| 

## FULL OUTER JOIN
FULL JOIN 会从左表 和右表 那里返回所有的行。如果其中一个表的数据行在另一个表中没有匹配的行，那么对面的数据用NULL代替.

### FULL OUTER JOIN语法
```
select column_name(s)
from table 1
FULL OUTER JOIN table 2
ON table 1.column_name=table 2.column_name
```

 <div align="center">
![](https://i.imgur.com/KMknOPR.png)
</div>

```
select * from Table A full outer join Table B
on Table A.id=Table B.id
```
执行以上SQL输出结果如下：

| id | name |address|
|:--------|:-------| :--------|
|1|Google|美国|
|2|淘宝|null
|3|微博|中国|
|4|Facebook|null|
|5|null|中国|
|6|null|美国| 



