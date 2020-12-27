### 1、修改数据：update

##### 语法格式：

```sql
	update 表名 set 字段名=值1,字段名=值2,... where 条件;
	--如果不加where条件的话会将该字段的所有记录数据更改
--将dept表中的部门10的LOC改为SHANGHAI,名称改为HUAMANRESOURCE
	update dept set LOC='SHANGHAI',DNAME='HUAMANRESOURCE' where deptno=10;
```

### 2、删除数据：delete

##### 语法：

```sql
	delete from 表名 where 条件;
	--没有where条件会删除所有数据，delete语句只是把表中的值删了，原本每条记录的各个存储空间还在，就是格还留着，并没有将其释放，就算删完了所有记录，插入数据的时候也并不是从第0条开始插入，为了回滚用
	
--删除emp1表中的10部门记录（deptno=10的各条记录）
delete from emp1 where deptno=10;
mysql> delete from emp1 where deptno=10;
Query OK, 3 rows affected (0.20 sec)

mysql> select * from emp1;
+-------+--------+----------+------+------------+---------+---------+--------+
| EMPNO | ENAME  | JOB      | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO |
+-------+--------+----------+------+------------+---------+---------+--------+
|  7369 | SMITH  | CLERK    | 7902 | 1980-12-17 |  800.00 |    NULL |     20 |
|  7499 | ALLEN  | SALESMAN | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
|  7521 | WARD   | SALESMAN | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 |
|  7566 | JONES  | MANAGER  | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 |
|  7654 | MARTIN | SALESMAN | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 |
|  7698 | BLAKE  | MANAGER  | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 |
|  7788 | SCOTT  | ANALYST  | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 |
|  7844 | TURNER | SALESMAN | 7698 | 1981-09-08 | 1500.00 |    0.00 |     30 |
|  7876 | ADAMS  | CLERK    | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 |
|  7900 | JAMES  | CLERK    | 7698 | 1981-12-03 |  950.00 |    NULL |     30 |
|  7902 | FORD   | ANALYST  | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 |
+-------+--------+----------+------+------------+---------+---------+--------+
11 rows in set (0.00 sec)

--删除emp1表中的所有的记录条数据
mysql> delete from emp1;
Query OK, 11 rows affected (0.12 sec)

mysql> select * from emp1;
Empty set (0.00 sec)
```

### 3、删除大表（彻底删除，只留下表头，没法回滚）

##### 这种方式相当于从表头开始截断表，这是非常慎重的操作

```sql
truncate table emp1;
```

##### 对于表结构的修改，使用工具进行修改navicate，实际开发中表设计好了之后很少再有修改，修改表结构 就说明对之前的设计进行否定，即使需要修改也能利用工具进行。修改表的结构语句不会出现在java代码当中。																		出现再java代码中的sql语句是：insert、delete、update、select，这些都是对表中数据的操作，术语：CRUD操作						Create（增） Retrieve（检索） Update（） Delete（）

