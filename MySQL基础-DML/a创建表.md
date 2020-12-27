### 1、建表的语法格式

```mysql
	cerat table 表明(
    	字段名1 数据类型,
        字段名1 数据类型,
        字段名1 数据类型,
        ...
    );
```

### 2、关于MySQL中字段的数据类型，下面是常见的

```sql
int			整数型（java中的int）
bigint		长整型（java中的long）
float		浮点型（java中的float和double）
char		定长字符串（java中的String）
varchar		可变长字符串（java中的StringBuilder/StringBuffer）
date		日期类型（java中的java.sql.Date类型）
BLOB		二进制大对象（存储图片、视频等流媒体信息）Binary Large OBject（java中的Object）
CLOB		字符大对象（存储较大文本，例如存储4G的字符串）Character Large OBject（java中的Object）
...
```

### 3、char和vachar怎么选择

```
在一张表中，有一个字段name，
	若其类型定义为char(6)，是定长的并且指定6个长度，我们插入数据例如 jack，此时底层直接分配6个空间来存储jack。无论插入数据怎样，上来就给你分配6个空间，数据不够给你补上，超过了就报错。
	而如果name定义的是varchar(6),我们要插入jack数据，底层会判断数据的长度，若数据没有超过指定长度就会根据数据长度分配空间，由于有了底层的判断，效率就会比char低。
	实际开发中，如果确定某个数据长度不会发生改变时，是定长的如：性别、生日等采用char
	当一个字段的数据长度不确定，如：简介、姓名等用varchar
```

### 4、BLOB和CLOB的使用

比如说存一部电影，有id（int），有电影名（varchar），有上映时间（char/date），宣传画（BLOB），剧情简介（CLOB）

一般情况下，表中是不存储音频、视频以及大内存的图片等媒体文件，存的是这些文件在云盘或硬盘的内存地址，因为数据库资源是非常珍贵的。

```
表名在数据库当中建议以：t_或者tbl_开头
```

### 5、创建一个学生表

```sql
--学生信息包括：
	学号、姓名、性别、班级编号、生日
	学号：	bigint
	姓名：	varchar(255)
	性别:	 char(255)
	班级编号:	varchar(255)
	生日:	char(10)
	create table t_student(
    	no bigint,
        name varchar(255),
        gender char(1),
        classno varchar(255),
        birth char(10)
    );
mysql> show tables;
+----------------------+
| Tables_in_mysqlstudy |
+----------------------+
| dept                 |
| emp                  |
| salgrade             |
| t_student            |
+----------------------+
4 rows in set (0.11 sec)
mysql> desc t_student;
+---------+--------------+------+-----+---------+-------+
| Field   | Type         | Null | Key | Default | Extra |
+---------+--------------+------+-----+---------+-------+
| no      | bigint(20)   | YES  |     | NULL    |       |
| name    | varchar(255) | YES  |     | NULL    |       |
| gender  | char(1)      | YES  |     | NULL    |       |
| classno | varchar(255) | YES  |     | NULL    |       |
| birth   | char(10)     | YES  |     | NULL    |       |
+---------+--------------+------+-----+---------+-------+
5 rows in set (0.11 sec)
```

