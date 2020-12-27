### 什么是连接查询

实际开发中，大部分情况下都不是从单表中查询数据，都是从多张表中联合查询取出最终结果。

一个业务一般对应多张表，比如：学生和班级，起码两张表

如果学生和班级的信息存储到一张表中，有些信息在表中就会存在大量的重复，导致数据的冗余，占用过多的内存空间

### 连接查询的分类

根据语法出现的年代划分为：

​	sql92（一般老的DBA可能还在使用这种语法。DBA：Data Base Administrator，数据库管理员）

​	sql99（一种比较新的语法）

根据表的连接方式划分为：

​		内连接：又分等值连接、非等值连接和自连接

​		外连接：左外连接（左连接）和右外连接（右连接）

​		全连接（不用掌握，了解即可，很少用）

### 连接查询的笛卡尔积现象（笛卡尔乘积现象）

#### 找出每一个员工的部门名称，要求显示员工名和部门名

```sql
select ename,deptno from emp;
mysql> select ename,deptno from emp;--emp表
+--------+--------+
| ename  | deptno |
+--------+--------+
| SMITH  |     20 |
| ALLEN  |     30 |
| WARD   |     30 |
| JONES  |     20 |
| MARTIN |     30 |
| BLAKE  |     30 |
| CLARK  |     10 |
| SCOTT  |     20 |
| KING   |     10 |
| TURNER |     30 |
| ADAMS  |     20 |
| JAMES  |     30 |
| FORD   |     20 |
| MILLER |     10 |
+--------+--------+
14 rows in set (0.00 sec)
mysql> select dname,deptno from dept;--dept表
+------------+--------+
| dname      | deptno |
+------------+--------+
| ACCOUNTING |     10 |
| RESEARCH   |     20 |
| SALES      |     30 |
| OPERATIONS |     40 |
+------------+--------+
4 rows in set (0.00 sec)
--根据需求写出sql语句
select e.ename,d.dname from emp e,dept d;--ename和dname要联合一块显示，粘到一块，此时没有条件限制ename中的一个字段就得与dname中的所有字段都配对一起然后显示
mysql> select e.ename,d.dname from emp e,dept d;
+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | ACCOUNTING |
| SMITH  | RESEARCH   |
| SMITH  | SALES      |
| SMITH  | OPERATIONS |
| ALLEN  | ACCOUNTING |
| ALLEN  | RESEARCH   |
| ALLEN  | SALES      |
| ALLEN  | OPERATIONS |
| WARD   | ACCOUNTING |
| WARD   | RESEARCH   |
| WARD   | SALES      |
| WARD   | OPERATIONS |
| JONES  | ACCOUNTING |
| JONES  | RESEARCH   |
......................
56 rows in set (0.00 sec)
--如上面查询所示，当两张表进行连接查询的时候没有添加任何条件限制时，最终的查询结果条数是两张表记录条数的乘积
--这就是笛卡尔积现象
```

### 关于表的别名

```sql
select e.ename,d.dname from emp e,dept d;
--好处
	1、执行效率高
	2、可读性好，没有起别名时查找字段会到所有表中查询，效率低；有时候可能两张表中会有相同名称的字段，这样会让sql语句能判别
```

### 避免笛卡尔积现象：加条件过滤多余数据

避免了笛卡尔积现象，匹配次数仍然是56次，只是筛选出了有效的数据而已

```sql
--找出每一个员工的部门名称，要求显示员工名和部门名
select e.ename,d.dname from emp e,dept d where e.deptno=d.deptno;--56条四选一
mysql> select e.ename,d.dname from emp e,dept d where e.deptno=d.deptno;--sql92语法，不常用
+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | RESEARCH   |
| ALLEN  | SALES      |
| WARD   | SALES      |
| JONES  | RESEARCH   |
| MARTIN | SALES      |
| BLAKE  | SALES      |
| CLARK  | ACCOUNTING |
| SCOTT  | RESEARCH   |
| KING   | ACCOUNTING |
| TURNER | SALES      |
| ADAMS  | RESEARCH   |
| JAMES  | SALES      |
| FORD   | RESEARCH   |
| MILLER | ACCOUNTING |
+--------+------------+
14 rows in set (0.14 sec)
```

