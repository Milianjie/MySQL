### group by：

按照某个字段或某些字段进行分组

### having：

对分组之后的数据进行二次过滤

### 案例：找出每个工作岗位的最高工资

```sql
select max(sal),job from emp group by job; //先执行from emp，然后执行group by job，最后执行select max(sal)
mysql> select max(sal) from emp group by job;
+----------+
| max(sal) |
+----------+
|  3000.00 |
|  1300.00 |
|  2975.00 |
|  5000.00 |
|  1600.00 |
+----------+
5 rows in set (0.44 sec)
--看不到工作岗位，加个工作岗位
mysql> select max(sal),job from emp group by job;
+----------+-----------+
| max(sal) | job       |
+----------+-----------+
|  3000.00 | ANALYST   |
|  1300.00 | CLERK     |
|  2975.00 | MANAGER   |
|  5000.00 | PRESIDENT |
|  1600.00 | SALESMAN  |
+----------+-----------+
5 rows in set (0.00 sec)
----分组函数一般都与group by联合使用，并且任何一个分组函数（count、max、min、avg、sum）都是在group by语句执行结束之后才会执行，如果一条sql语句没有group by，那么整张表的数据就自成一组数据。而且group by语句是在where语句之后执行，所以分组函数不能用作where的条件，执行顺序如下：
select			5
...
from			1
...
where			2
...
group by		3
...
having			4
...
order by		6
...
所以为了查询高于平均工资的员工不能用下面语句
select ename,sal from emp where sal>avg(sal);
只能先查出平均工资，再把具体值放进去
select avg(sal) from emp;
select ename,sal from emp where sal>2073.214286;
或者这样用sql语句的嵌套，其中where中的是子查询语句
select ename,sal from emp where (select avg(sal) from emp);
```

```sql
select ename,max(sal),job from emp group by job;
--上面语句在Oracle数据库中执行报错，但在Mysql数据库中可以执行，但执行的结果没有任何意义
--结论：当一条sql语句中有group by的话，select后面只能跟分组函数和参与分组的字段
```

### 多个字段联合起来分组：如找出每个部门不同岗位的最高薪资

```sql
select deptno,job,sal from emp;
mysql> select deptno,job,sal from emp;
+--------+-----------+---------+
| deptno | job       | sal     |
+--------+-----------+---------+
|     20 | CLERK     |  800.00 |
|     30 | SALESMAN  | 1600.00 |
|     30 | SALESMAN  | 1250.00 |
|     20 | MANAGER   | 2975.00 |
|     30 | SALESMAN  | 1250.00 |
|     30 | MANAGER   | 2850.00 |
|     10 | MANAGER   | 2450.00 |
|     20 | ANALYST   | 3000.00 |
|     10 | PRESIDENT | 5000.00 |
|     30 | SALESMAN  | 1500.00 |
|     20 | CLERK     | 1100.00 |
|     30 | CLERK     |  950.00 |
|     20 | ANALYST   | 3000.00 |
|     10 | CLERK     | 1300.00 |
+--------+-----------+---------+
14 rows in set (0.00 sec)
--每个部门都有不同的岗位，同一岗位人员也有多个，要分组的就是同一部门的某一岗位分一组然后找出最高薪资
select deptno,job,max(sal) from emp group by deptno,job;
mysql> select deptno,job,max(sal) from emp group by deptno,job;
+--------+-----------+----------+
| deptno | job       | max(sal) |
+--------+-----------+----------+
|     10 | CLERK     |  1300.00 |--这里就是将10部门的CLERK岗位zuogao的员工分为一组找出最高薪资
|     10 | MANAGER   |  2450.00 |
|     10 | PRESIDENT |  5000.00 |
|     20 | ANALYST   |  3000.00 |
|     20 | CLERK     |  1100.00 |
|     20 | MANAGER   |  2975.00 |
|     30 | CLERK     |   950.00 |
|     30 | MANAGER   |  2850.00 |
|     30 | SALESMAN  |  1600.00 |
+--------+-----------+----------+
9 rows in set (0.11 sec)
```

### 要求：找出每个部门的最高薪资，并且只显示薪资大于2900的数据

```sql
--第一步：找出每个部门的最高薪资
mysql> select max(sal),deptno from emp group by deptno;
+----------+--------+
| max(sal) | deptno |
+----------+--------+
|  5000.00 |     10 |
|  3000.00 |     20 |
|  2850.00 |     30 |
+----------+--------+
3 rows in set (0.00 sec)
--第二步：找出薪资大于2900的
mysql> select max(sal),deptno from emp group by deptno having max(sal)>2900;
+----------+--------+
| max(sal) | deptno |
+----------+--------+
|  5000.00 |     10 |
|  3000.00 |     20 |
+----------+--------+
2 rows in set (0.32 sec)--这种方式效率较低

--以下方法先利用where找出薪资大于2900的，是后面分组数据减少，效率较高。结论：能用where解决的尽量不用having
mysql> select max(sal),deptno from emp where sal>2900 group by deptno;
+----------+--------+
| max(sal) | deptno |
+----------+--------+
|  5000.00 |     10 |
|  3000.00 |     20 |
+----------+--------+
2 rows in set (0.13 sec)

--有些情况是没法用where只能用having
--例如找出每个部门的平均薪资，只显示薪资大于2000的数据
mysql> select avg(sal),deptno from emp group by deptno having avg(sal)>2000;
+-------------+--------+
| avg(sal)    | deptno |
+-------------+--------+
| 2916.666667 |     10 |
| 2175.000000 |     20 |
+-------------+--------+
2 rows in set (0.13 sec)
```

