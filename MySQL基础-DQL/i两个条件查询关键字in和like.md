

## 1、条件查询关键字  in 与 not in

### （1）in 等同于 or 

#### 例如找出工作岗位为MANAGER和SALESMAN的员工

```sql
下面两句sql语句效果是相同的，效率也是一样的
select ename,job from emp where job='MANAGER' or job='SALESMAN';
select ename,job from emp where job in('MANAGER','SALESMAN');
mysql> select ename,job from emp where job in('MANAGER','SALESMAN');
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
7 rows in set (0.11 sec)
```

### （2）not in ：表示不在这几个值当中（当然也有not is）

```SQL
mysql> select ename,job from emp where sal not in(1000,5000);
+--------+----------+
| ename  | job      |
+--------+----------+
| SMITH  | CLERK    |
| ALLEN  | SALESMAN |
| WARD   | SALESMAN |
| JONES  | MANAGER  |
| MARTIN | SALESMAN |
| BLAKE  | MANAGER  |
| CLARK  | MANAGER  |
| SCOTT  | ANALYST  |
| TURNER | SALESMAN |
| ADAMS  | CLERK    |
| JAMES  | CLERK    |
| FORD   | ANALYST  |
| MILLER | CLERK    |
+--------+----------+
13 rows in set (0.00 sec)
```

## 2、模糊查询 like

模糊查询当中需要掌握两个特殊字符：'%' 和 '_'

%表示代替任意多个字符，_ 表示代替任意一个字符

需求：找出员工名字中含有字母O的

```sql
select ename from emp where ename like '%O%';
//%O%表示名字中任意位置的O字母都行，因为%可以随意替代多个名字中O字母左右的多个非O字母
mysql> select ename from emp where ename like '%O%';
+-------+
| ename |
+-------+
| JONES |
| SCOTT |
| FORD  |
+-------+
3 rows in set (0.11 sec)
```

需求：找出员工名字中第二个字母为A的

```sql
select ename from emp where ename like '_A%';//第二个字母为A，那么未知名字中A前面就不能用任意多个%了，只能代替一个，所以前面用_代替
mysql> select ename from emp where ename like '_A%';
+--------+
| ename  |
+--------+
| WARD   |
| MARTIN |
| JAMES  |
+--------+
3 rows in set (0.00 sec)
```

需求：找出员工名字中最后一个字母为T的

```sql
select ename from emp where ename like '%T';//最后一个字母为T，那么未知名字中T后面就不需要特殊字符代替了，所以前面用%代替所有
mysql> select ename from emp where ename like '%T';
+-------+
| ename |
+-------+
| SCOTT |
+-------+
1 row in set (0.00 sec)
```

```sql
可以知道%和_是有特殊意义的字符，想要得到原来的模样就像java中用到转义字符\
所以想要查询名字中有下划线的员工就必须用\_表示下划线
select ename from emp where ename like '%\_%';
```

