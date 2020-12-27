### 关于查询结果的去重

```sql
select distinct job from emp;
mysql> select distinct job from emp;
+-----------+
| job       |
+-----------+
| CLERK     |
| SALESMAN  |
| MANAGER   |
| ANALYST   |
| PRESIDENT |
+-----------+
5 rows in set (0.17 sec)
--下面的语句是错误的
select ename，distinct job from emp;
--distinct只能放在所有字段的最前面，如下
mysql> select distinct job,deptno from emp;--此处表示的是去除job和deptno组合字段的重复记录，针对的是distinct后面的所有字段，并非一个
+-----------+--------+
| job       | deptno |
+-----------+--------+
| CLERK     |     20 |
| SALESMAN  |     30 |
| MANAGER   |     20 |
| MANAGER   |     30 |
| MANAGER   |     10 |
| ANALYST   |     20 |
| PRESIDENT |     10 |
| CLERK     |     30 |
| CLERK     |     10 |
+-----------+--------+
9 rows in set (0.00 sec)
```

### 统计岗位的数量

```sql
select count(distinct job) from emp;
mysql> select count(distinct job) from emp;
+---------------------+
| count(distinct job) |
+---------------------+
|                   5 |
+---------------------+
1 row in set (0.25 sec)
```

