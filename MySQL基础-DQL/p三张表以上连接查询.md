### 三张表连接查询

```sql
--找出每个员工的部门名称及工资等级,这里所有员工都可以用内连接查询出来
--找出每个员工的部门名称及工资等级以及上级领导，这里连接自身表作为领导表要用外连接，另两张用nei
select
	e.ename,d.dname,g.grade
from
	emp e
join
	dept d
on
	e.deptno=d.deptno 
join
	salgrade g
on
	e.sal between g.losal and g.hisal;
+--------+------------+-------+
| ename  | dname      | grade |
+--------+------------+-------+
| SMITH  | RESEARCH   |     1 |
| ADAMS  | RESEARCH   |     1 |
| JAMES  | SALES      |     1 |
| MILLER | ACCOUNTING |     2 |
| WARD   | SALES      |     2 |
| MARTIN | SALES      |     2 |
| ALLEN  | SALES      |     3 |
| TURNER | SALES      |     3 |
| CLARK  | ACCOUNTING |     4 |
| JONES  | RESEARCH   |     4 |
| SCOTT  | RESEARCH   |     4 |
| FORD   | RESEARCH   |     4 |
| BLAKE  | SALES      |     4 |
| KING   | ACCOUNTING |     5 |
+--------+------------+-------+
14 rows in set (0.16 sec)

--解释
...
	A
(inner或left outer)join
	B
on
	...
(inner或left outer)join
	C
on
	...
...
--上面表示A表与B表连接后在与C表连接查询，或者说AB表连接的结果表与C表再连接查询

```

