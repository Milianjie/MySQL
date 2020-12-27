### 1、找出薪资大于1000并且部门编号为20或30的员工（and和or连用）

```mysql
select ename,sal,deptno from emp where sal>1000 and deptno=20 or deptno=30;
上面语句的后果不能满足查询要求，原因是语句被翻译为查询（薪资大于1000且部门编号等于20）或者（部门编号等于30的员工）
条件薪资大于1000是大前提，需要先实现，然后后面要的编号条件才实现
and 关键字的优先级高于 or，所以要用括号括起 or 关键字链接的条件
select ename,sal,deptno from emp where sal>1000 and (deptno=20 or deptno=30);
mysql> select ename,sal,deptno from emp where sal>1000 and (deptno=20 or deptno=30);
+--------+---------+--------+
| ename  | sal     | deptno |
+--------+---------+--------+
| ALLEN  | 1600.00 |     30 |
| WARD   | 1250.00 |     30 |
| JONES  | 2975.00 |     20 |
| MARTIN | 1250.00 |     30 |
| BLAKE  | 2850.00 |     30 |
| SCOTT  | 3000.00 |     20 |
| TURNER | 1500.00 |     30 |
| ADAMS  | 1100.00 |     20 |
| FORD   | 3000.00 |     20 |
+--------+---------+--------+
9 rows in set (0.00 sec)
```

