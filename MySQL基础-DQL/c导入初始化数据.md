### 导入初始化数据

第一步：登录MySql数据库管理系统

dos命令窗口：mysql -uroot -p+密码（为了登录时输入密码不显示密码，可以在-p后回车，再输入密码）

```
C:\Users\钟荣杰>mysql -uroot -p
Enter password: **********
```

第二步：查看有哪些数据库

登录数据库管理系统后输入命令：show databases；（注意这个是MySql专属的命令，不是SQL语句，SQL语句是在一个数据中进行操作的，现在还没到数据库中）

```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| blog               |
| chengyuan01        |
| day15              |
| learning           |
| mybatis            |
| mybatis_test       |
| mysql              |
| mytest             |
| performance_schema |
| person             |
| student            |
| sys                |
| t_user             |
| test               |
| test1              |
+--------------------+
16 rows in set (1.94 sec)
```

其中数据库 information_schema、 mysql 、performance_schema、test这四个是MySql自带的数据库

第三步：创建自己的数据库

create database mysqlstudy;（注意这个仍是MySql专属的命令，不是SQL语句，因为这是建数据库，不是建表）

```
mysql> create database mysqlstudy;
Query OK, 1 row affected (0.00 sec)
```

第四步：使用mysqlstudy数据库，并查看该数据库中有哪些表

use mysqlstudy; （现在还是在对数据库进行操作，选择要使用的数据库，所以是MySQL命令，不是SQL语句）

show tables;（MySQL命令，不是SQL语句）

```
mysql> use mysqlstudy;
Database changed
mysql> show tables;
Empty set (0.00 sec)

mysql>
```

第五步：初始化数据（用MySQL中的source命令执行sql脚本）

```
mysql> source F:\Git_Repositories\MySqlStudy\resources\mysqlstudy.sql
```

初始化完成后生成了三张表：

```
mysql> show tables;
+----------------------+
| Tables_in_mysqlstudy |
+----------------------+
| dept                 |
| emp                  |
| salgrade             |
+----------------------+
3 rows in set (0.00 sec)

```

第六步：删除数据库

drop database mysqlstudy;