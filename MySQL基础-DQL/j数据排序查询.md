### 1、按工资排序查询员工（有升序和降序）

默认情况下，其排序方式是升序

如果想要指定排序方式，可以在要排序的字段名加关键字：asc（ascend，表示升序）和desc（descend，表示降序）

```sql
select ename,sal from emp order by sal;//默认升序输出
select ename,sal from emp order by sal asc;//指定升序
select ename,sal from emp order by sal desc;//指定降序
mysql> select ename,sal from emp order by sal;
+--------+---------+
| ename  | sal     |
+--------+---------+
| SMITH  |  800.00 |
| JAMES  |  950.00 |
| ADAMS  | 1100.00 |
| WARD   | 1250.00 |
| MARTIN | 1250.00 |
| MILLER | 1300.00 |
| TURNER | 1500.00 |
| ALLEN  | 1600.00 |
| CLARK  | 2450.00 |
| BLAKE  | 2850.00 |
| JONES  | 2975.00 |
| SCOTT  | 3000.00 |
| FORD   | 3000.00 |
| KING   | 5000.00 |
+--------+---------+
14 rows in set (0.00 sec)
mysql> select ename,sal from emp order by sal desc;
+--------+---------+
| ename  | sal     |
+--------+---------+
| KING   | 5000.00 |
| SCOTT  | 3000.00 |
| FORD   | 3000.00 |
| JONES  | 2975.00 |
| BLAKE  | 2850.00 |
| CLARK  | 2450.00 |
| ALLEN  | 1600.00 |
| TURNER | 1500.00 |
| MILLER | 1300.00 |
| WARD   | 1250.00 |
| MARTIN | 1250.00 |
| ADAMS  | 1100.00 |
| JAMES  |  950.00 |
| SMITH  |  800.00 |
+--------+---------+
14 rows in set (0.00 sec)
```

### 2、按工资降序查询员工，如果工资一样按名字字母升序排序

需要注意的是：多个条件进行数据排序时，排序条件越靠前的的字段越能起到到主导作用，只有当前面的字段无法完成排序的时候，才会启用后面的字段。

```sql
select ename,sal from emp order by sal desc,ename asc;
mysql> select ename,sal from emp order by sal desc,ename asc;
+--------+---------+
| ename  | sal     |
+--------+---------+
| KING   | 5000.00 |
| FORD   | 3000.00 |
| SCOTT  | 3000.00 |
| JONES  | 2975.00 |
| BLAKE  | 2850.00 |
| CLARK  | 2450.00 |
| ALLEN  | 1600.00 |
| TURNER | 1500.00 |
| MILLER | 1300.00 |
| MARTIN | 1250.00 |
| WARD   | 1250.00 |
| ADAMS  | 1100.00 |
| JAMES  |  950.00 |
| SMITH  |  800.00 |
+--------+---------+
14 rows in set (0.00 sec)
```

### 3、找出工作岗位是SALESMAN的员工，并且按照薪资的降序排列

```sql
select ename,sal,job from emp where job='salesman' order by sal desc;
mysql> select ename,sal,job from emp where job='salesman' order by sal desc;
+--------+---------+----------+
| ename  | sal     | job      |
+--------+---------+----------+
| ALLEN  | 1600.00 | SALESMAN |
| TURNER | 1500.00 | SALESMAN |
| WARD   | 1250.00 | SALESMAN |
| MARTIN | 1250.00 | SALESMAN |
+--------+---------+----------+
4 rows in set (0.00 sec)
执行顺序：
select 
	ename,sal,job 		 3
from 
	emp 				1
where 
	job='salesman' 		 2
order by 
	sal desc;			4
从表emp中根据条件查询到所需字段，然后根据排序条件排序
所以 order by 是最后执行的，例如
select ename,sal as salary from emp order by salary;
上面的语句可以执行，说明salary有效，在排序前先执行查询select中的语句 select ename,sal as salary
```

