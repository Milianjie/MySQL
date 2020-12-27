### 1、存储引擎

```sql
--完整的建表语句
CREATE TABLE `t_user` (
  `id` int(11) NOT NULL,
  `username` varchar(255) DEFAULT NULL,
  `password` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8

--注意：MySQL当中，标识符是可以用飘号``括起来的，但这步通用，最好别用
从上面语句可以看出，MySQL默认使用的存储引擎是InnoDB方式，默认采用字符集是UTF-8
```

##### 存储引擎这个名字只在MySQL中有，Oracle中也有对应的机制，但不叫这个名字，只是叫表存储方式

##### MySQL有很多存储引擎，不同的存储引擎就是不同的表的存储方式，它们都有各自的优缺点，要合适的选择存储引擎

```sql
--查看当前MySQL版本支持的存储引擎
show engines \G;
mysql> show engines \G;		--9个
*************************** 1. row ***************************
      Engine: InnoDB
     Support: DEFAULT
     Comment: Supports transactions, row-level locking, and foreign keys
Transactions: YES
          XA: YES
  Savepoints: YES
*************************** 2. row ***************************
      Engine: MRG_MYISAM
     Support: YES
     Comment: Collection of identical MyISAM tables
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 3. row ***************************
      Engine: MEMORY
     Support: YES
     Comment: Hash based, stored in memory, useful for temporary tables
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 4. row ***************************
      Engine: BLACKHOLE
     Support: YES
     Comment: /dev/null storage engine (anything you write to it disappears)
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 5. row ***************************
      Engine: MyISAM
     Support: YES
     Comment: MyISAM storage engine
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 6. row ***************************
      Engine: CSV
     Support: YES
     Comment: CSV storage engine
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 7. row ***************************
      Engine: ARCHIVE
     Support: YES
     Comment: Archive storage engine
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 8. row ***************************
      Engine: PERFORMANCE_SCHEMA
     Support: YES
     Comment: Performance Schema
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 9. row ***************************
      Engine: FEDERATED
     Support: NO
     Comment: Federated MySQL storage engine
Transactions: NULL
          XA: NULL
  Savepoints: NULL
9 rows in set (1.70 sec)
```

### 2、MyISAM存储引擎

```sql
 Engine: MyISAM
     Support: YES
     Comment: MyISAM storage engine
Transactions: NO					--这里是NO说明不支持事务
          XA: NO
  Savepoints: NO
  --MyISAM是MySQL最常用的存储引擎，但它不是默认存储引擎
  这种存储方式采用三个文件来存储一张表：
  	表名.frm文件(存储表格式或者说表结构的文件，frm表form格式)
  	表名.MYD文件(存储表中数据的文件，D表data)
  	表名.MYI文件(存储表中索引的文件，I表Index)
  优点：可被压缩节省空间，可以转换为只读表，提高检索效率，不可以修改
```

### 3、InnoDB存储引擎

```sql
*************************** 1. row ***************************
      Engine: InnoDB
     Support: DEFAULT
     Comment: Supports transactions, row-level locking, and foreign keys
Transactions: YES			--支持事务，这是优点
          XA: YES
  Savepoints: YES
  --这种存储引擎，数据的安全得到保障
  优点：支持事务、行级锁、外键等
  其存储方式：
  表的结构存储在表名.frm文件中
  数据存储在tablespace的空间中（逻辑概念），无法被压缩，也无法被转换成只读
  在MySQL服务器奔溃后提供自动恢复机制
```

### 4、MEMORY存储引擎

```sql
*************************** 3. row ***************************
      Engine: MEMORY
     Support: YES
     Comment: Hash based, stored in memory, useful for temporary tables
Transactions: NO
          XA: NO
  Savepoints: NO
  --缺点：
  不支持事务，数据容易丢失。因为表中所有的数据和索引都是存储在内存当中
  --优点：查询速度最快
  以前叫做HEPA引擎
```

