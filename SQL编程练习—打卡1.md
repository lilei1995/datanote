# SQL编程练习—打卡1

### 数据分析的四种类型模式：

1.描述型：发生了什么？ 一个例子是每月的利润和损失账单

2.诊断型：为什么会发生？

3.预测型：可能发生什么？

4.指导型：我需要做什么？



#### 查找所有员工的last_name和first_name以及对应的dept_name

查找所有员工的last_name和first_name以及对应的dept_name，也包括暂时没有分配部门的员工

#####  CREATE TABLE `departments` (

 `dept_no` char(4) NOT NULL,
 `dept_name` varchar(40) NOT NULL,
 PRIMARY KEY (`dept_no`));

#####  CREATE TABLE `dept_emp` (

 `emp_no` int(11) NOT NULL,
 `dept_no` char(4) NOT NULL,
 `from_date` date NOT NULL,
 `to_date` date NOT NULL,
 PRIMARY KEY (`emp_no`,`dept_no`));

#####  CREATE TABLE `employees` (

 `emp_no` int(11) NOT NULL,
 `birth_date` date NOT NULL,
 `first_name` varchar(14) NOT NULL,
 `last_name` varchar(16) NOT NULL,
 `gender` char(1) NOT NULL,
 `hire_date` date NOT NULL,
 PRIMARY KEY (`emp_no`)); 



## 解题

1、第一次LEFT JOIN连接employees表与dept_emp表，得到所有员工的last_name和first_name以及对应的**dept_no**，也包括暂时没有分配部门的员工

  2、第二次LEFT   JOIN连接上表与departments表，即**连接dept_no与dept_name**，得到所有员工的last_name和first_name以及对应的**dept_name**，也包括暂时没有分配部门的员工

```
SELECT em.last_name, em.first_name, dp.dept_name
FROM (employees AS em LEFT JOIN dept_emp AS de ON em.emp_no = de.emp_no)
LEFT JOIN departments AS dp ON de.dept_no = dp.dept_no
```

3、第一次 left join 是把未分配部门的员工算进去了，但是只得到了部门号，没有部门名，所以第二次也要 left join 把含有部门名 departments 连接起来，否则在第二次连接时就选不上未分配部门的员工了。
你把第二个 left join 改为 inner join 后，看看报错信息就明白了。  

4、right join是以右边的表为主,如果左边表没有关联的数据,补null,同理left join是一样的



### 查找员工编号emp_no为10001其自入职以来的薪水salary涨幅值growth

CREATE TABLE `salaries` (
`emp_no` int(11) NOT NULL,
`salary` int(11) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`from_date`));

```
select (MAX(salary)-MIN(salary)) as growth 
from salaries where emp_no = "10001"
```



### 查找所有员工自入职以来的薪水涨幅情况，给出员工编号emp_no以及其对应的薪水涨幅growth，并按照growth进行升序

CREATE TABLE `employees` (
`emp_no` int(11) NOT NULL,
`birth_date` date NOT NULL,
`first_name` varchar(14) NOT NULL,
`last_name` varchar(16) NOT NULL,
`gender` char(1) NOT NULL,
`hire_date` date NOT NULL,
PRIMARY KEY (`emp_no`));
CREATE TABLE `salaries` (
`emp_no` int(11) NOT NULL,
`salary` int(11) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`from_date`));



本题思路是先分别用两次LEFT   JOIN左连接employees与salaries，建立两张表，分别存放员工当前工资（sCurrent）与员工入职时的工资（sStart），再用INNER   JOIN连接sCurrent与sStart，最后限定在同一员工下用当前工资减去入职工资。  

```

SELECT sCurrent.emp_no, (sCurrent.salary-sStart.salary) AS growth   
FROM (SELECT s.emp_no, s.salary FROM employees e LEFT JOIN salaries s 
ON e.emp_no = s.emp_no WHERE s.to_date = ``'9999-01-01'``) AS sCurrent
INNER JOIN (SELECT s.emp_no, s.salary FROM employees e LEFT JOIN salaries s ON e.emp_no = s.emp_no WHERE s.from_date = e.hire_date) AS sStart
ON sCurrent.emp_no = sStart.emp_no
ORDER BY growth
```















