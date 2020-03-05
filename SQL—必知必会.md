



# 检索数据



### 检索单个列

```
select prod_name from products;
```

sql结束要加；分号

SQL不分大小写

### 检索多个列

```
select prode_name , prod_id , prod_price from products;
```

### 检索所有列

```
select * from product;
```

### 检索不同的值

```
select distinct vend_id from product;
```

### 限制结果

使用TOP关键字来限制最多返回多少行,检索的是前五行

```
select top 5 pro_name from products;
```

DBms   DB2的表达方式

```
select prod_name from products  fetch first 5 rows only;
```

Oracle ，基于ROWNUM(行计算器)

```
select prod_name from products where rownum<=5;
```

limit语句限制，输出不超过5行数据

```
select prod_name from products limit 5；
```

指示Mysql等DBMS返回从第5行起的5行数据

```
select prod_name from products limit 5  offset 5；
```

### 使用注释

```
1，  # 这是一条注释
2，  /* XXXXXXXX */
3，  alalalla（代码）    -- 后面接注释
```

# 排序检索数据

### 排序数据

如果不排序，数据一般将以它在底层表中出现的顺序显示，有可能是数据最初添加到表中的顺序

**order  by**  按照字母排序

```
select prod_name from products  order by   prod_name  ;
```

注：order by语句必须为select 语句中的最后一条子句；

​        可用非检索的列进行排序数据



### 按多个列排序

按多个列排序，简单的指定列名，列名之间用逗号分开即可

在实际情况中，不仅有一个排序条件。

```
select prod_id ,prod_price , prod_name from products
order by prod_price , prod_id ;
```

再有相同的价格时才会对ID进行排序，如果价格是唯一的，不会按照名字排序

### 按照位置排序

order by 支持按相对列位置进行排序

```
select prod_id ,prod_price , prod_name from products order by 2,3;
```

### 指定排序方向

按照价格的降序排序

```
select prod_id ,prod_price , prod_name from products
order by prod_price desc;
```

多个列排序，不仅对价格进行，相同价格的按照名字排序

```
select prod_id ,prod_price , prod_name from products
order by prod_price desc， prod_name ;
```

DESC  （descending）对直接前面的列名进行排序，降序，

ASC  (ascending)  升序  （默认）

# 过滤数据

### 使用where子句

只搜索所需数据需要指定**搜索条件**，搜索条件也称为**过滤条件**；

```
select prod_name ，prod_price  from products where prod_price=3.45；
```

| 操作符     | 描述                   |
| :--------- | :--------------------- |
| =          | 等于                   |
| <>         | 不等于                 |
| >  （<）   | 大于  (小于)           |
| !=         | 不等于                 |
| >=  （<=） | 大于等于  （小于等于） |
| ！<  (!>)  | 不小于  （不大于）     |
| BETWEEN    | 在某个范围内           |
| LIKE       | 搜索某种模式           |
| is null    | 为null                 |

### 检查单个值

```
select prod_name ，prod_price  from products where prod_price <= 10 ；
```

### 不匹配检查

```
select vend_id ，prod_name  from products where vend_id <> 'DLL01' ；
```

### 范围值检查

```
select prod_name ，prod_price  from products where prod_price  betweent 5 and 10 ；
```

### 空值检查

```
null 无值 （no value） 它与字段包含0，空字符串或仅仅包含空格不同
```

```
select prod_name ，prod_price  from products where prod_price is null ；  --不能写=null
```



# 高级数据过滤

## 使用组合where子句

**操作符（operator）**

用来联结或改变where子句中的子句的关键字，也称为逻辑操作符，

### AND 操作符

```
select vend_id ，prod_name  from products where vend_id = 'DLL01' and prod_price <= 4;
```

### or操作符

OR操作符与and操作符作用相反，OR是指条件满足任意一个，而不是全部满足，主要满足了第一个条件，那么第二个将不在计算，

```
select vend_id ，prod_name  from products where vend_id = 'DLL01' or vend_id = 'BSS01';
```

### 求值序列

```
select vend_id ，prod_name  from products where vend_id = 'DLL01' or vend_id = 'BSS01' 
and prod_price <= 4;
```

以上操作中，第二个条件，价格小于4没有被执行，因为，**SQL优先处理AND语句**，自定义组合，vend_id = 'BSS01' and prod_price <= 4;  输出的结果为  'BSS01'中 prod_price <= 4的结果和，'DLL01' 的全部产品

**解决方法是加（）**

```
select vend_id ，prod_name  from products where（ vend_id = 'DLL01' or vend_id = 'BSS01'） 
and prod_price <= 4;
```

### IN操作符

```
select vend_id ，prod_name  from products where  vend_id in ('DLL01','BSS01') 
order by pros_name;
```

IN操作符必须加（ ，）

完成与or相同的左右 

```
where vend_id = 'DLL01' or vend_id = 'BSS01'  order by pros_name;
```

## not操作符

否定后面跟的任何条件

```
select vend_id ，prod_name  from products where NOT vend_id = 'DLL01' ;
```

```
select vend_id ，prod_name  from products where NOT vend_id <> 'DLL01' ;
```

# 用通配符进行过滤

## like 操作符

**通配符：用来匹配值的一部分的特殊字符**

**搜索模式：有字面值，通配符或两者组合构成的搜索条件**

**谓词：操作符作为谓词的时候不是操作符，从技术上来说 LIKE是谓词而不是操作符**

**通配符搜索只能用于文本字段（字符串）**

### 百分号（%）通配符

%表示任何字符出现任意次数

```
select prod_name ,prod_id from products where prod_name like 'fish';
```

ACCESS通配符  如果使用的是microsoft accessa 需要使用的是* 而不是%

```
select prod_name ,prod_id from products where prod_name like '%fish%';  --'f%sh';
```

### 下划线（_）通配符

DB2不支持通配符（_）

microsoft accessa 需要使用的是？ 而不是_

_刚好匹配一个字符，不能多也不能少，like可以匹配多个字符不同

```
select prod_name ,prod_id from products where prod_name like '_ inch teddy bear';
```

### 方括号（[]）通配符

[]用来指定一个字符集，必须匹配指定位置的一个字符

# 创建计算字段

## 计算字段

字段：基本上与列（column）的意思相同，将此相互使用。

## 拼接字段

**拼接**：将值联结到一起（将一个值附加到另一个值上）构成单个值

操作符  ：  +  ， ||

```
select vend_name + '(' + vend_country  + ')' from vendors
order by vend_name;
```

```
select vend_name || '(' || vend_country || ')' from vendors
order by vend_name;
```

Mysql 和MariaDB 中使用的语句：

```
select concat ( vend_name , '(' , vend_country , ')') from vendors
order by vend_name;
```



  **rtrim()函数**     去掉值右边的所有空格，对各个列进行整理

```
select rtrim  (vend_name) + '(' + vend_country + ')'  from vendors
order by vend_name;
```

**ltrim()函数**   去掉值左边的所有空格

**trim()函数 **    去掉值字符串左右两边的所有空格

## 使用别名

AS 关键字赋予别名

```
select rtrim  (vend_name) + '(' + vend_country + ')'  as vend_name 
from vendors    order by vend_name;
```

## 执行算术计算

```
select prod_id,quantity, item_price , quantity * item_price AS expanded_price 
from orderitem   
where order_num =20008;
```

# 使用函数处理数据

## 函数

| 函数             | 语法                                                         |
| ---------------- | ------------------------------------------------------------ |
| 提取字符串的组成 | Access使用MID();DB2,Qracle,PostgreSQL,和SQLite使用的是 SUBSTR();MYsql 和SQL Server 使用SUBSTRING(); |
| 数据类型转换     | Access,Qracle使用多个函数，每种类型的转换有一个函数;DB2和PostgreSQL使用的是 CAST();     MariaDB，MYsql 和SQL Server 使用CONVERT(); |
| 取当前日期       | Access使用NOW();DB2,PostgreSQL使用current_date();MariaDB，MYsql 使用curdate(); Qracle使用sysdate; SQL Server 使用的是GETDATE;SQLite使用的是DATE（） |

**可移植（Portable）**  ：所编写的代码可以在多个系统上运行

SQL语句可以移植，SQL函数不可移植，

## 使用函数

### 文本处理函数

UPPER（）函数，将文本转换为大写

```
select vend_name , UPPER (vend_name) AS vend_name_upcase from vendors
order by vend_name;
```

- **`CONCAT(str1, str2, ...)`：拼接字符串**

  ```sql
  mysql> SELECT CONCAT('There', ' is', ' an', ' apple.');
  ```

- **`CONCAT_WS(separator, str1, str2, ...)`：使用指定分隔符连接字符串**

  ```sql
  mysql> SELECT CONCAT_WS('-', 'There', 'is', 'an', 'apple');
  ```

- **`LEFT(str, length)`：从左截取指定长度的子字符串**

  ```sql
  mysql> SELECT LEFT('ZHANG SAN', 5);
  ```

- **`RIGHT(str, length)`：从右截取指定长度的子字符串**

  ```sql
  mysql> SELECT RIGHT('ZHANG SAN', 3);
  ```

- **`SUBSTRING(str, index, length)`：从指定位置处开始截取指定长度的子字符串**

  ```csharp
   mysql> SELECT SUBSTRING('ABCDEFG', 3, 4);
  ```

- **`LENGTH(str)`：返回字符串的长度**

  ```sql
  mysql> SELECT LENGTH('ZHANG SAN');
  ```

​      Length（）函数举例：

​      检索 city表 中的所有 ID序号，Name值 以及 Name值的长度，检索的所有信息全部输出。

```
SELECT ID,`Name`,LENGTH(`Name`)
```

- **LOWER(str)`：将字符串转换为小写格式**

  ```sql
  mysql> SELECT LOWER('ZHANG SAN');
  ```

- **`UPPER(str)`：将字符串转换为大写格式**

  ```sql
  mysql> SELECT UPPER('zhang san');
  ```

- **`ELT(index, str1, str2, ...)`：返回字符串序列中指定位置的字符串**

  ```sql
  mysql> SELECT ELT(3, 'A', 'B', 'C', 'D');
  ```

- **`FIELD(str, str1, str2, ...)`：返回指定字符串str在字符串序列中的位置，找不到返回0**

  ```sql
  mysql> SELECT FIELD('C', 'A', 'B', 'C', 'D');
  ```

- **`FIND_IN_SET(str, stringList)`：返回指定字符串str在由“`,`”分割的字符串序列中的位置**

  ```sql
  mysql> SELECT FIND_IN_SET('C', 'A,B,C,D');
  ```

- **`FORMAT(X, D)`：按照指定的小数位数D将数值X转化为字符串**

  ```sql
  mysql> SELECT FORMAT(123.45678, 2);
  ```

- **`INSERT(str, index, length, newString)`：从指定位置开始用新字符串替换原字符串中指定长度的字符，index超出原字符串范围时返回原字符串**

  ```sql
  mysql> SELECT INSERT('ABCDEF', 3, 3, '123');
  ```

- **`LOCATE(substr, str)`：返回子字符串在指定字符串中第一次出现的位置**

  **`LOCATE(substr, str, index)`：从指定字符串的指定位置处开始查找子字符串出现的位置**

  **找不到返回0**

  ```sql
  mysql> SELECT LOCATE('a', 'banana');
  mysql> SELECT LOCATE('a', 'banana', 3);
  ```

- **`LPAD(str, length, padStr)`：在字符串左侧用padStr将原字符串填充至指定长度，当指定长度小于原字符串长度时，截断原字符串**

  ```sql
  mysql> SELECT LPAD('HELLO', 8, '!')
  ```

- **`RPAD(str, length, padStr)`：在字符串右侧用padStr将原字符串填充至指定长度，当指定长度小于原字符串长度时，截断原字符串**

  ```sql
  mysql> SELECT RPAD('HELLO', 8, '!');
  ```

- **`REPEAT(str, times)`：按照指定次数重复字符串**

  ```sql
  mysql> SELECT REPEAT('LA', 3);
  ```

- **`REPLACE(str, from_str, to_str)`：将字符串中的所有匹配的字符串替换为新字符串**

  ```sql
  mysql> SELECT REPLACE('LALALA', 'L', 'B');
  ```

- **Soundex（）：返回串的SOUNDEX值（注意：SOUNDEX是一个将任何文本串转换为描述其语音表示的字母数字模式的算法，）**

  Soundex（）函数举例：****
  city表 的 Name列 有一个名字为 ' Hern.Sang ' ，但是如果这是输入错误的值，此名字的正确值实际上是 ' Hern.Song ' ，使用Soundex（）函数进行搜索，匹配所有发音类似 ' Hern.Song ' 的名字：

  ```
  SELECT ID,`Name`
  FROM city
  WHERE SOUNDEX(`Name`) = SOUNDEX('Hern Song');
  ```

- **`REVERSE(str)`：将字符串逆序输出**

  ```sql
  mysql> SELECT REVERSE('ABC');
  ```

### 




### 常用日期和时间处理函数

MySQL没有DATEPART()函数，但是可以用YEAR（）函数来提取年份：

```
SELECT order-num FROM ORDERS WHERE YEAR(order_date)=2004
```

Oracle也没有该函数，可用如下：

```
SELECT order-num FROM ORDERS WHERE to_number(to_char(order_date,'YY'))=2004;
```

to_char（）函数用来提取日期的成分，再用to_number（）用来将提取出的转化成数值

- `NOW()`：返回当前的时间和日期

  ```sql
  mysql> SELECT NOW();
  ```

- `CURDATE()`：返回当前的日期

  ```sql
  mysql> SELECT CURDATE();
  ```

- `CURTIME()`：返回当前时间

  ```sql
  mysql> SELECT CURTIME();
  ```

- `DATE(dateAndTime)`：提取日期时间表达式中的日期部分

  ```sql
  mysql> SELECT DATE(NOW());
  ```

- `DAY()`：返回日期时间表达式中的天数部分

  ```sql
  mysql> SELECT DAY(NOW());
  ```

- `YEAR()`：返回日期时间表达式中的年部分

  ```sql
  mysql> SELECT YEAR(NOW());
  ```

- `EXTRACT(unit FROM date)`：按照指定的时间单位从日期时间表达式中提取年、月、日、时间等部分

  ```sql
  mysql> SELECT EXTRACT(YEAR FROM NOW());
  mysql> SELECT EXTRACT(WEEK FROM NOW());
  mysql> SELECT EXTRACT(HOUR_MICROSECOND FROM NOW());
  ```

  时间单位的值可参考[Temporal Intervals](https://links.jianshu.com/go?to=https%3A%2F%2Fdev.mysql.com%2Fdoc%2Frefman%2F8.0%2Fen%2Fexpressions.html%23temporal-intervals)文档

- `DATE_FORMAT(date, format)`：按照指定格式显示时间日期

  可选的格式可参考[Date and time functions](https://links.jianshu.com/go?to=https%3A%2F%2Fdev.mysql.com%2Fdoc%2Frefman%2F8.0%2Fen%2Fdate-and-time-functions.html%23function_date-format)

  ```sql
  mysql> SELECT DATE_FORMAT(NOW(), '%W %M %Y');
  ```

- `DATE_ADD(date, INTERVAL exp unit)`、`DATE_SUB(date, INTERVAL exp unit)`：日期和时间的加减操作。返回值是否包含时间取决于给定的时间日期的表达式和时间单位。

  ```sql
  mysql> SELECT DATE_ADD('2018-03-25', INTERVAL 1 DAY);
  mysql> SELECT DATE_SUB('2018-03-25 19:26:47', INTERVAL 1 HOUR);
  ```

- `DATEDIFF(date1, date2)`：返回两个日期的差值，会忽略表达式中的时间，仅对日期进行运算

  ```sql
  mysql> SELECT DATEDIFF('2018-4-30', NOW());
  ```

- `ADDDATE(date, INTERVAL exp unit)`：等同于`DATE_ADD()`

  `ADDDATE(date, days)`：在给定的日期上加上给定的天数

  ```sql
  mysql> SELECT ADDDATE(NOW(), 31);
  ```

- `ADDTIME(time1, time2)`：将两个时间表达式相加

  ```sql
  mysql> SELECT ADDTIME('10:33:24', '22:23:22');
  ```

### **数值处理函数**

数值处理函数仅处理数值数据。

在主要DBMS的函数中，数值函数是最一致、最统一的函数。

![这里写图片描述](https://img-blog.csdn.net/20180412002357281?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQ0NjU5MzQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

# 汇总数据

## 聚集函数

聚集函数：对某些运行的函数，计算并返回一个值。

**1、求个数/记录数/项目数等：count()**

count（*）是对表中行的数目进行计数，不管表列中包含的是空值（NULL）还是非空值

使用count(column)对特定列中具有值的行进行计数，忽略NULL值

```

select count(cust_email)  AS num_cust from Company -- 有电子邮箱是个数 不包括空值
select count(*)  AS num_cust from Company --包括空值
```

**2、求某一列平均数 :avg()**

求vend_id = 'DL101'的平均值

```
select AVG(prod_price) AS avg_price  from products
where vend_id = 'DL101'
```

AVG只能用来确定特定数值列的平均值，如需要获取更多列的平均值，需要使用多个AVG（）函数

AVG（）自动忽略了列值为null的行

**3、求总和，总分等：sum() --必须为数字列****

```
select sum(score) as sum_score from Scores where year - 2005;
```

**4、求最大值，最高分，最高工资等：max()**

max()要求指定列名 

MAX（）忽略列值为NULL的行

```
select max(prod_price)  as max_price from Scores
```

使用文本数据时，返回的是该列排序后的最后一行

**5、求最小值，最低分，最低工资等：max()**

min()要求指定列名 

min())忽略列值为NULL的行

```
select min(prod_price)  as min_price from Scores
```

## 聚集不同值

```
select AVG (distinct prod_price) AS avg_price  from products
where vend_id = 'DL101'
```

排除掉相同价格的值，

all参数，是默认的

distinct 不能用于count(*)   可以使用于count()

distinct 必须使用列名，不能用于计算或者表达式

**TOP参数 和TOP PERCENT    限制查询**

## 使用组合几何函数

多个聚合函数 ,最好重新更新列名

```
select count(*) AS num_items,
max(prod_price)  as max_price
min(prod_price)  as min_price
AVG (distinct prod_price) AS avg_price
from products;
```

# 分组函数

## 数据分组

```
select vend_id , count(*) as num_prod  from products
group by vend_id;
```

group by 分组  对每个组进行聚合计算

即统计每个相同的vend_id 有多少种产品

将NULL分成一个组统计输出

GROUP By 子句必须在where 子句之后  order by 子句之前

## 过滤分组

having 可以代替 where 子句，唯一的区别是 WHERE 过滤行，having 过滤分组

```
select cust_id , count(*) as orders  from Orders
group by cust_id having count(*) >= 2;
```

找到下过两次订单的客户ID

where 在分组前进行过滤，having在数据分组后进行过滤，

```
select vend_id , count(*) as num_prods  from products
where prod_price >= 4
group by cust_id   having count(*) >= 2;
```

where子句中不能包含聚类函数，但是having中可以包含，

```
select vend_id , count(*) as num_prods  from products
group by cust_id   having count(*) >= 2;
```

使用having 时应该结合GROUP  BY 子句，而where子句用于标准的行级，

## 分组和排序

group  by  和order  by  

| ORDER BY         | GROUP BY                                                   |
| ---------------- | ---------------------------------------------------------- |
| 对产生的输出排序 | 对行分组，但输出可能不是分组的顺序                         |
| 任意列都可以使用 | 只可能使用选择列或者表达式列，而且必须使用每个选择列表达式 |
| 不一定需要       | 如果与聚集函数一起使用列（表达式），则必须使用             |

如果在使用group by子句的时候，应该也给出order by子句，这是保证数据正确排序的唯一方法，千万不要仅仅依赖GROUP by  排序数据

```
select order_num ,count(*) as items
from orderttem 
group by order_num  having count(*)>= 3;
```

按照订购数目排序输出，需要添加group by子句

```
select order_num ,count(*) as items
from orderttem 
group by order_num  having count(*)>= 3
order by items, order_num ;
```

## ，select 子句顺序

select   ,  from ,  where  , group  by   ,  having  ,  order  by  

## 子查询

嵌套在其他查询中的查询

```
select cust_name ,cust_contact  from customers
where cust_id in ('1004','1007')
```

将where 换成子查询，而不是硬编码这些顾客ID

```
select cust_name ,cust_contact  from customers
where cust_id in  （

select cust_id   from  orders   where  order_num  in 

(select  order_num  from  orderitems  where  prod_id = 'RGAN01')）；
```

**使用select   count(*) 对表中的行进行计算**

```
select count(*)  as  orders
from  orders 
where cust_id = '1001';
```

使用子查询

```
select cust_name ,cust_state,(
select count(*)  from orders   where order.cust_id = customers.cust_id)  as  orders
from customers  
order by  cust_name;

```

# 联结表

## 关系表

关系表的设计就是把信息分解成多个表，一类数据一个表，各表通过某些共同的值互相关联。

设计良好的数据库或者应用程序称为可伸缩性。即能够适应不断增加的工作量而不失败，

联结是一种机制，用来在一条select语句中关联表,因此称为联结

## 创建联结

创建联结即指定要联结的所有表格以及关联她们的方式即可

```
select vend_name,pros_name,prod_price
from vendor ,producys
where vendors.vend_id = products.vend_id
```

where子句作为过滤条件，只包含那些匹配给定条件（这里是指联结条件）的行，

**笛卡儿积**：由没有联结条件的表返回的结果为笛卡尔积。检索出的行的数目将是第一个表中的行数乘以第二个表中的行数。

## 内联结

等值联结，基于两个表之间的相等测试，也成为**内联结**

```
select vend_name， prod_name , prod_price
from vendors inner join products
on vendors.vend_id= products.vend_id;
```

## 联结多个表

```
select vend_name， prod_name , prod_price,quantity
from vendors as v ,products  as p ,orderitems as o
where  v.vend_id= p.vend_id
and o.prod_id = p.prod_id
and order_num = 2007;
```

## 自联结

```
select c1.cust_id ,c1.cust_name ,c1.cust_contact
from customers as c1,customers  as c2
where c1.cust_name = c2.cust_name
and c2.cust_contact = 'jim jones'
```

## 外联结

许多联结将一个表中的行与另一个表中的行相关联，但有时候需要包含没有关联行的那些行。

```
select vend_name， prod_name , prod_price
from vendors left outer join products
on vendors.vend_id= products.vend_id;
```

包含许多没有订单顾客在内的所有顾客，

```
select vend_name， prod_name , prod_price
from vendors right outer join products
on vendors.vend_id= products.vend_id;
```

左外联结和右外联结，她们之间的唯一区别是所关联的表的顺序，。

# 组合查询

## 使用union

```
select cust_name ,cust_contact，cust_email  from  customers
where cust_state in ('il','in','mi')
union
select cust_name ,cust_contact，cust_email  from  customers
where cust_name = 'funs'
```

```
select cust_name ,cust_contact，cust_email  from  customers
where cust_state in ('il','in','mi')
OR cust_name = 'funs'
```

这不是两个条件，而是均要满足

UNION查询自动去除了重复的行，

如果想返回全部的匹配行，可以使用UNION ALL

# 插入数据

## inster 插入

```
inster into Customers
values('1','2','3','null','null');
```

整行插入，按照次序填充，没有就写null

插入部分行，插入前三行，后面就默认值，可以为NULL

```
inster into Customers  （cust_id ,cust_2,cust_3）
values('1','2','3');
```

## 插入检索出的数据

```
insert  into customers(*....(对应列名))
select *...(对应列名) from  custnew
```

## 从一个表复制到另一个表

```
select *  into custcopy from customers
```

