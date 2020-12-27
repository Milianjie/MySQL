## 1、分组函数

其中的关键字：

count	计数

sum	求和

avg	平均值

max	最大值

min	最小值

### 记住：所有的分组函数都是对某一组数据进行操作的

```sql
需求：
1、找出工资总和
select sum(sal) from emp;
mysql> select sum(sal) from emp;
+----------+
| sum(sal) |
+----------+
| 29025.00 |
+----------+
1 row in set (0.17 sec)

2、找出最高工资
select max(sal) as '最高工资' from emp;
mysql> select max(sal) as '最高工资' from emp;
+--------------+
| 最高工资     |
+--------------+
|      5000.00 |
+--------------+
1 row in set (0.05 sec)

3、找出最低工资
select min(sal) as '最低工资'from emp;

4、找出平均工资
select avg(sal) as '平均工资'from emp;

5、找出所有员工数
select count(ename) as '总员工数'from emp;或者select count(*) from emp;
mysql> select count(ename) as '总员工数'from emp;select count(*) from emp;
+--------------+
| 总员工数     |
+--------------+
|           14 |
+--------------+
1 row in set (0.00 sec)

+----------+
| count(*) |
+----------+
|       14 |
+----------+
1 row in set (0.00 sec)
```

分组函数只有五个

分组函数也叫多行处理函数，特点就是：输入多行，输出一行

譬如求工资的平均数，在表中有多个sal字段的数据，即多行，通过多组函数将这些数据输入，输出成一个平均工资，只有一行。。。

分组函数自动忽略null

### 例如计数补贴数量comm

```sql
select count(comm) from emp;
mysql> select count(comm) from emp;
+-------------+
| count(comm) |
+-------------+
|           4 |
+-------------+
1 row in set (0.00 sec)
```

### 查询津贴补贴的和

```sql
不需要额外判断comm是否为null的过滤条件
即不需要这样写 select sum(comm) from emp where comm is null;
直接这样写 select sum(comm) from emp;
mysql> select sum(comm) from emp;
+-----------+
| sum(comm) |
+-----------+
|   2200.00 |
+-----------+
1 row in set (0.00 sec)
由于有null参与的运算其运算结果都为null，但上面语句却并不是null，说明null并没有参与运算，被忽略了
```

### 查询出高于平均工资的员工

```sql
先看下面这段sql语句：
select ename,sal from emp where sal>avg(sal);
根据执行顺序翻译为
-->从表emp中（from emp）
-->以sal>avg(sal)条件(where sal>avg(sal))
-->查询员工且输出ename和sal(select ename,sal)
（1）由于where的条件语句先select执行，所以条件中的avg(sal)并没有被定义，没有这个值，所以会报错：
ERROR 1111 (HY000): Invalid use of group function//无效的使用了group函数
结论：sql语法规则，分组函数不能使用在where子句充当条件
```

### count（*）和count（某个字段）

count（*）统计的是总记录条数。count（某个字段）统计的是该字段非null的记录条数

### 分组函数也可以组合起来使用

```sql
select count(sal),max(sal),min(sal),avg(sal),sum(sal) from emp;
select count(sal),max(sal),min(sal),avg(comm),sum(sal) from emp;

mysql> select count(sal),max(sal),min(sal),avg(sal),sum(sal) from emp;
+------------+----------+----------+-------------+----------+
| count(sal) | max(sal) | min(sal) | avg(sal)    | sum(sal) |
+------------+----------+----------+-------------+----------+
|         14 |  5000.00 |   800.00 | 2073.214286 | 29025.00 |
+------------+----------+----------+-------------+----------+
1 row in set (0.00 sec)

mysql> select count(sal),max(sal),min(sal),avg(comm),sum(sal) from emp;
+------------+----------+----------+------------+----------+
| count(sal) | max(sal) | min(sal) | avg(comm)  | sum(sal) |
+------------+----------+----------+------------+----------+
|         14 |  5000.00 |   800.00 | 550.000000 | 29025.00 |
+------------+----------+----------+------------+----------+
1 row in set (0.00 sec)
```



## 2、单行处理函数

特点就是输入一行输出一行

### 计算每个员工的年薪（工资sal+补贴comm）

```sql

规定：所有的数据库中有null参与的运算其运算结果都为null
select ename,(sal+comm)*12 as yearsal from emp;//该sql语句中由于comm字段的数据有null，所以没办法正常计算出所有员工的年薪
select ename,comm from emp;
mysql> select ename,comm from emp;
+--------+---------+
| ename  | comm    |
+--------+---------+
| SMITH  |    NULL |
| ALLEN  |  300.00 |
| WARD   |  500.00 |
| JONES  |    NULL |
| MARTIN | 1400.00 |
| BLAKE  |    NULL |
| CLARK  |    NULL |
| SCOTT  |    NULL |
| KING   |    NULL |
| TURNER |    0.00 |
| ADAMS  |    NULL |
| JAMES  |    NULL |
| FORD   |    NULL |
| MILLER |    NULL |
+--------+---------+
14 rows in set (0.00 sec)
此时需要在进行运算时判断comm字段的数据是否为null，如果为null，可以把它当成0处理
需要用到ifnull()处理函数，该处理函数是单行处理函数
select ename,ifnull(comm,0) from emp;
mysql> select ename,ifnull(comm,0) from emp;
+--------+----------------+
| ename  | ifnull(comm,0) |
+--------+----------------+
| SMITH  |           0.00 |
| ALLEN  |         300.00 |
| WARD   |         500.00 |
| JONES  |           0.00 |
| MARTIN |        1400.00 |
| BLAKE  |           0.00 |
| CLARK  |           0.00 |
| SCOTT  |           0.00 |
| KING   |           0.00 |
| TURNER |           0.00 |
| ADAMS  |           0.00 |
| JAMES  |           0.00 |
| FORD   |           0.00 |
| MILLER |           0.00 |
+--------+----------------+
14 rows in set (0.03 sec)

所以要查询出员工的年薪的sql语句为
select ename,(sal+ifnull(comm,0))*12 as yearsal from emp;
mysql> select ename,(sal+ifnull(comm,0))*12 as yearsal from emp;
+--------+----------+
| ename  | yearsal  |
+--------+----------+
| SMITH  |  9600.00 |
| ALLEN  | 22800.00 |
| WARD   | 21000.00 |
| JONES  | 35700.00 |
| MARTIN | 31800.00 |
| BLAKE  | 34200.00 |
| CLARK  | 29400.00 |
| SCOTT  | 36000.00 |
| KING   | 60000.00 |
| TURNER | 18000.00 |
| ADAMS  | 13200.00 |
| JAMES  | 11400.00 |
| FORD   | 36000.00 |
| MILLER | 15600.00 |
+--------+----------+
14 rows in set (0.00 sec)
```

