### limit（重点，分页查询的基础）

limit是mysql特有的，其他数据库中没有，不通用。Oracle中的是rownum

limit是取结果集中的部分数据，这是其作用

##### 语法机制

```sql
limit starINdex,length
	--starIndex表示起始位置，从0开始，0表示第一条数据
	--length表示每次取几个数据
```

```sql
--取出工资前五名的员工（降序取前五个）
select ename,sal from emp order by sal desc limit 0,5;
select ename,sal from emp order by sal desc limit 5;
+-------+---------+
| ename | sal     |
+-------+---------+
| KING  | 5000.00 |
| FORD  | 3000.00 |
| SCOTT | 3000.00 |
| JONES | 2975.00 |
| BLAKE | 2850.00 |
+-------+---------+
5 rows in set (0.00 sec)
--就是说limit是sql语句最后执行的
select			4
	..
from			1
	..
join			1
	..
on				1
	..
where			2
	..
group by		3
	..
having			5
	..
order by		6
	..
limit			7
	..;

--找出工资排名在第四到第九的员工
select ename,sal from emp order by sal desc limit 3,6;
mysql> select ename,sal from emp order by sal desc limit 3,6;
+--------+---------+
| ename  | sal     |
+--------+---------+
| JONES  | 2975.00 |
| BLAKE  | 2850.00 |
| CLARK  | 2450.00 |
| ALLEN  | 1600.00 |
| TURNER | 1500.00 |
| MILLER | 1300.00 |
+--------+---------+
6 rows in set (0.00 sec)

```

### 2、通用的标准分页sql

每页显示3条记录：

第一页：0，3

第二页：3，3

第三页：6，3

第n页：（n-1）*3，3

所以每页显示pageSize条记录：

第pageNo页：（pageNo-1）*pageSize，pageSize

##### java代码

```java
{
    int pageNo = 2;//页码2
    int pageSize = 10;//每页显示10条记录
    limit (pageNo-1)*pageSize=10,pageSize=10
}
```

