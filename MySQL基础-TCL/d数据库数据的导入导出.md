### DBA命令

##### 将数据库当中的数据导出

```sql
--在windows的dos命令窗口里执行：
	mysqldump mysqltudy>F:\mysqlstudy.sql -uroot -prong195302
表示将mysqlstudy数据库中的数据导出（>表示导出）到F盘，生成为mysqlstudy.sql，后面是mysql的用户名和密码

	mysqldump mysqltudy emp>F:\mysqlstudy.sql -uroot -prong195302  --指定数据库中的指定表进行导出
```

##### 导入数据

```sql
--首先要创建一个对应的数据库，如
create database mysqlstudy;
--然后使用它
use mysqlstudy;
--然后用source语句导入数据
source F:\mysqlstudy.sql;  --这样就把原先mysqlstudy数据库中的所有表数据放进了新建的mysqlstudy数据库中
```

