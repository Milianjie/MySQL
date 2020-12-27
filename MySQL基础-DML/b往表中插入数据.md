### 1、insert语句插入数据

##### 语法格式：

```sql
	insert into 表名(字段名1,字段名2,字段名3,...) values (值1,值2,值3,...);
--要求是字段的数量要和值的数量一致，并且数据类型要对应相同。
这里在t_student表中插入一条数据
insert into
t_student(no,name,gender,classno,birth) 
values 
(01,'zhouhuimin','1','dayi52ban','1997-02-14');
--因为dos命令窗口中字符编码方式是GBK，而数据库中是UTF-8，上面语句有中文值以GBK方式插入数据库，字符集不统一会出现乱码，所以插入的数据先不用中文
mysql> select * from t_student;
+------+------------+--------+-----------+------------+
| no   | name       | gender | classno   | birth      |
+------+------------+--------+-----------+------------+
|    1 | zhouhuimin | 1      | dayi52ban | 1997-02-14 |
+------+------------+--------+-----------+------------+
1 row in set (0.00 sec)

--这样也行,只要前后字段和值类型对应就行
insert into
t_student(name,gender,classno,birth,no) 
values 
('zhongrongjie','0','dayi52ban','1997-02-15',2);
mysql> select * from t_student;
+------+--------------+--------+-----------+------------+
| no   | name         | gender | classno   | birth      |
+------+--------------+--------+-----------+------------+
|    1 | zhouhuimin   | 1      | dayi52ban | 1997-02-14 |
|    2 | zhongrongjie | 0      | dayi52ban | 1997-02-15 |
+------+--------------+--------+-----------+------------+
2 rows in set (0.00 sec)

insert into t_student(name) values ('piaochulong');--因为建表中时允许所有字段可为null，且default默认值为null，这条语句只赋值name字段，那么其他字段默认赋值为null
mysql> select * from t_student;
+------+--------------+--------+-----------+------------+
| no   | name         | gender | classno   | birth      |
+------+--------------+--------+-----------+------------+
|    1 | zhouhuimin   | 1      | dayi52ban | 1997-02-14 |
|    2 | zhongrongjie | 0      | dayi52ban | 1997-02-15 |
| NULL | piaochulong  | NULL   | NULL      | NULL       |
+------+--------------+--------+-----------+------------+
3 rows in set (0.00 sec)
--要想修改上面这条有null数据的字段的记录，就得用update语句，不能用insert语句
```

### 2、删除表

```sql
	drop table if exists t_student; --当这个表存在时删除表
mysql> drop table if exists t_student;
Query OK, 0 rows affected (0.24 sec)

mysql> show tables;
+----------------------+
| Tables_in_mysqlstudy |
+----------------------+
| dept                 |
| emp                  |
| salgrade             |
+----------------------+
3 rows in set (0.00 sec)

--再建表，这次给字段gender赋默认值default为1
create table t_student(
    	no bigint,
        name varchar(255),
        gender char(1) default 1,
        classno varchar(255),
        birth char(10)
    );
mysql> desc t_student;
+---------+--------------+------+-----+---------+-------+
| Field   | Type         | Null | Key | Default | Extra |
+---------+--------------+------+-----+---------+-------+
| no      | bigint(20)   | YES  |     | NULL    |       |
| name    | varchar(255) | YES  |     | NULL    |       |
| gender  | char(1)      | YES  |     | 1       |       |
| classno | varchar(255) | YES  |     | NULL    |       |
| birth   | char(10)     | YES  |     | NULL    |       |
+---------+--------------+------+-----+---------+-------+
5 rows in set (0.00 sec)
--当我们插入数据，只插入某个字段时如name时，gender字段默认赋1，其他赋值null
```

### 3、insert语句的其他写法

```sql
1、省略字段书写，只写后面的values，但这样只针对对所有字段赋值插入数据
insert into t_student values (01,'zhouhuimin','1','dayi52ban','1997-02-14');
insert into t_student values (01,'zhouhuimin','1','dayi52ban');--这样会报错

mysql> insert into t_student values (01,'zhouhuimin','1','dayi52ban','1997-02-14');
Query OK, 1 row affected (0.13 sec)

mysql> select * from t_student;
+------+------------+--------+-----------+------------+
| no   | name       | gender | classno   | birth      |
+------+------------+--------+-----------+------------+
|    1 | zhouhuimin | 1      | dayi52ban | 1997-02-14 |
+------+------------+--------+-----------+------------+
1 row in set (0.00 sec)

2、一次插入多行数据
insert into 
t_student (no,name,gender,classno,birth) 
values 
(01,'zhouhuimin','1','dayi52ban','1997-02-14'),
(02,'zhongrongjie','0','dayi52ban','1997-02-15'),
(03,'piaochulong','1','dayi52ban','1997-02-13'),
(04,'piaozhiyan','1','dayi52ban','1997-02-12')
;
mysql> select * from t_student;
+------+--------------+--------+-----------+------------+
| no   | name         | gender | classno   | birth      |
+------+--------------+--------+-----------+------------+
|    1 | zhouhuimin   | 1      | dayi52ban | 1997-02-14 |
|    2 | zhongrongjie | 0      | dayi52ban | 1997-02-15 |
|    3 | piaochulong  | 1      | dayi52ban | 1997-02-13 |
|    4 | piaozhiyan   | 1      | dayi52ban | 1997-02-12 |
+------+--------------+--------+-----------+------------+
4 rows in set (0.00 sec)

--刚开始一直报下面的错误，最后才发现自己把字段调换了，与数据类型匹配不上
mysql> insert into
    -> t_student (name,gender,classno,birth,no)--这里no放后面了
    -> values
    -> (01,'zhouhuimin','1','dayi52ban','1997-02-14'),
    -> (02,'zhongjie','1','dayi52ban','1997-02-15'),
    -> (03,'piaochulong','1','dayi52ban','1997-02-13'),
    -> (04,'piaozhiyan','1','dayi52ban','1997-02-12')
    -> ;
ERROR 1406 (22001): Data too long for column 'gender' at row 1
```

