### 1、什么是视图

站在不同的角度看到的数据（同一张表的数据，通过不同的角度去看待）

### 2、创建视图与删除视图

```sql
创建视图：
	create view myview1 as select ename,sal,deptno from emp3;
删除视图：
	drop view myview1;
--注意：只有DQL语句才能以视图对象的方式创建出来

--对视图的增删改查CRUD，会影响到原表数据。这样就不是直接操作原表

mysql> create view myview1 as select ename,sal,deptno from emp3;
Query OK, 0 rows affected (0.15 sec)

mysql> select * from myview1;
+--------+---------+--------+
| ename  | sal     | deptno |
+--------+---------+--------+
| SMITH  |  800.00 |     20 |
| ALLEN  | 1600.00 |     30 |
| WARD   | 1250.00 |     30 |
| JONES  | 2975.00 |     20 |
| MARTIN | 1250.00 |     30 |
| BLAKE  | 2850.00 |     30 |
| CLARK  | 2450.00 |     10 |
| SCOTT  | 3000.00 |     20 |
| KING   | 5000.00 |     10 |
| TURNER | 1500.00 |     30 |
| ADAMS  | 1100.00 |     20 |
| JAMES  |  950.00 |     30 |
| FORD   | 3000.00 |     20 |
| MILLER | 1300.00 |     10 |
+--------+---------+--------+
14 rows in set (0.00 sec)

mysql> update myview1 set ename='BBBB',sal=66666 where deptno=20;--对视图进行修改
Query OK, 5 rows affected (1.78 sec)
Rows matched: 5  Changed: 5  Warnings: 0

mysql> select * from myview1;--查看视图
+--------+----------+--------+
| ename  | sal      | deptno |
+--------+----------+--------+
| BBBB   | 66666.00 |     20 |
| ALLEN  |  1600.00 |     30 |
| WARD   |  1250.00 |     30 |
| BBBB   | 66666.00 |     20 |
| MARTIN |  1250.00 |     30 |
| BLAKE  |  2850.00 |     30 |
| CLARK  |  2450.00 |     10 |
| BBBB   | 66666.00 |     20 |
| KING   |  5000.00 |     10 |
| TURNER |  1500.00 |     30 |
| BBBB   | 66666.00 |     20 |
| JAMES  |   950.00 |     30 |
| BBBB   | 66666.00 |     20 |
| MILLER |  1300.00 |     10 |
+--------+----------+--------+
14 rows in set (0.00 sec)

mysql> select * from emp3;--查看原表emp3，原表数据被修改了
+-------+--------+-----------+------+------------+----------+---------+--------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL      | COMM    | DEPTNO |
+-------+--------+-----------+------+------------+----------+---------+--------+
|  7369 | BBBB   | CLERK     | 7902 | 1980-12-17 | 66666.00 |    NULL |     20 |
|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 |  1600.00 |  300.00 |     30 |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 |  1250.00 |  500.00 |     30 |
|  7566 | BBBB   | MANAGER   | 7839 | 1981-04-02 | 66666.00 |    NULL |     20 |
|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 |  1250.00 | 1400.00 |     30 |
|  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 |  2850.00 |    NULL |     30 |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 |  2450.00 |    NULL |     10 |
|  7788 | BBBB   | ANALYST   | 7566 | 1987-04-19 | 66666.00 |    NULL |     20 |
|  7839 | KING   | PRESIDENT | NULL | 1981-11-17 |  5000.00 |    NULL |     10 |
|  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 |  1500.00 |    0.00 |     30 |
|  7876 | BBBB   | CLERK     | 7788 | 1987-05-23 | 66666.00 |    NULL |     20 |
|  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |   950.00 |    NULL |     30 |
|  7902 | BBBB   | ANALYST   | 7566 | 1981-12-03 | 66666.00 |    NULL |     20 |
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 |  1300.00 |    NULL |     10 |
+-------+--------+-----------+------+------------+----------+---------+--------+
14 rows in set (0.00 sec)

--视图的作用：
通过以上可以看出视图就是取原表中的一部分，进行CRUD，视图中的字段名在创建视图时可以修改成其他名称，如下
create view myview1 as select ename a,sal b,deptno c from emp3;
这样可以保证程序员可以对表进行维护，且保证表的保密性，隐藏了表的实现细节
```

