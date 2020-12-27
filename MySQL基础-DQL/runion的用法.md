### union（可以将查询结果集相加）

```sql
--找出工作岗位是SALESMAN和MANAGER的员工
第一种：select ename,job from emp where job='SALESMAN' or job='MANAGER';
第二种：select ename,job from emp where job in('SALESMAN','MANAGER');
+--------+----------+
| ename  | job      |
+--------+----------+
| ALLEN  | SALESMAN |
| WARD   | SALESMAN |
| JONES  | MANAGER  |
| MARTIN | SALESMAN |
| BLAKE  | MANAGER  |
| CLARK  | MANAGER  |
| TURNER | SALESMAN |
+--------+----------+
7 rows in set (0.00 sec)
第三种：
select ename,job from emp where job='SALESMAN'
union
select ename,job from emp where job='MANAGER';
+--------+----------+
| ename  | job      |
+--------+----------+
| ALLEN  | SALESMAN |
| WARD   | SALESMAN |
| MARTIN | SALESMAN |
| TURNER | SALESMAN |
| JONES  | MANAGER  |
| BLAKE  | MANAGER  |
| CLARK  | MANAGER  |
+--------+----------+

--两张不相干的表拼接在一块
select ename from emp
union
select dname from dept;
+------------+
| ename      |--表中列名取决于第一条查询语句，并且拼接的查询语句列数要一致
+------------+
| SMITH      |
| ALLEN      |
| WARD       |
| JONES      |
| MARTIN     |
| BLAKE      |
| CLARK      |
| SCOTT      |
| KING       |
| TURNER     |
| ADAMS      |
| JAMES      |
| FORD       |
| MILLER     |
| ACCOUNTING |
| RESEARCH   |
| SALES      |
| OPERATIONS |
+------------+
18 rows in set (0.00 sec)
--下面或报错
select ename,job from emp
union
select dname from dept;
```

