## 条件查询

```sql
语法格式：
select
	字段1,字段2,...
from
	表名
where
	条件;
执行顺序：先from，然后where，最后select
```

### 1、从emp表中查询工资等于5000的员工

```
mysql> select ename from emp where sal=5000;
+-------+
| ename |
+-------+
| KING  |
+-------+
1 row in set (0.00 sec)
```

当然也可查询工资大于5000或小于5000的员工，或者不等于5000的员工

```sql
select ename from emp where sal>5000;
select ename from emp where sal<5000;
mysql> select ename from emp where sal<>5000;
+--------+
| ename  |
+--------+
| SMITH  |
| ALLEN  |
| WARD   |
| JONES  |
| MARTIN |
| BLAKE  |
| CLARK  |
| SCOTT  |
| TURNER |
| ADAMS  |
| JAMES  |
| FORD   |
| MILLER |
+--------+
13 rows in set (0.00 sec)
```

### 2、从emp表中查询SMITH的工资（注意的是员工名字的字段数据类型是字符串varchar）

```sql
mysql> select sal,ename from emp where ename = 'smith';
+--------+-------+
| sal    | ename |
+--------+-------+
| 800.00 | SMITH |
+--------+-------+
1 row in set (0.00 sec)
```

### 3、找出工资大于1000小于3000的员工，包含1000和3000

```sql
select ename,sal from emp where sal>=1000 and sal<=3000;
上面语句也可以这样写
select ename,sal from emp where sal between 1000 and 3000;//between...and...是全闭区间
（如果这样写select ename,sal from emp where sal between 3000 and 1000;
会翻译成select ename,sal from emp where sal>=3000 and sal<=1000;）这样的数据是不存在的
mysql>  select ename,sal from emp where sal between 1000 and 3000;
+--------+---------+
| ename  | sal     |
+--------+---------+
| ALLEN  | 1600.00 |
| WARD   | 1250.00 |
| JONES  | 2975.00 |
| MARTIN | 1250.00 |
| BLAKE  | 2850.00 |
| CLARK  | 2450.00 |
| SCOTT  | 3000.00 |
| TURNER | 1500.00 |
| ADAMS  | 1100.00 |
| FORD   | 3000.00 |
| MILLER | 1300.00 |
+--------+---------+
11 rows in set (0.00 sec)
```

### 4、between...and...还可以用在字符串上（需注意的是区间是左闭右开，而且取的是首字母）

```sql
mysql>  select ename,sal from emp where ename between 'A' and 'C';//由下可知不包含C
+-------+---------+
| ename | sal     |
+-------+---------+
| ALLEN | 1600.00 |
| BLAKE | 2850.00 |
| ADAMS | 1100.00 |
+-------+---------+
3 rows in set (0.00 sec)
```

### 5、找出哪些人津贴为null或者哪些人津贴不为null（comm）

```sql
emp表中津贴comm用null表示，在数据库中null表示没有值，什么都没有，为空。
其与comm = 0.00表示的不是一个概念，所以做条件时不能用=衡量。
必须使用is null 或者is not null去判断。
select ename,comm from emp where comm is not null;
select ename,comm from emp where comm is null;

mysql> select ename,comm from emp where comm is not null;
+--------+---------+
| ename  | comm    |
+--------+---------+
| ALLEN  |  300.00 |
| WARD   |  500.00 |
| MARTIN | 1400.00 |
| TURNER |    0.00 |
+--------+---------+
4 rows in set (0.00 sec)
```

#### 找出哪些人没有津贴（即comm为null或者为0.00）

```sql
select ename,comm from emp where comm is null or comm = 0;
mysql> select ename,comm from emp where comm is null or comm = 0;
+--------+------+
| ename  | comm |
+--------+------+
| SMITH  | NULL |
| JONES  | NULL |
| BLAKE  | NULL |
| CLARK  | NULL |
| SCOTT  | NULL |
| KING   | NULL |
| TURNER | 0.00 |
| ADAMS  | NULL |
| JAMES  | NULL |
| FORD   | NULL |
| MILLER | NULL |
+--------+------+
11 rows in set (0.00 sec)
```

