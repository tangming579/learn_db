### 表类型（存储引擎）的选择

和大多数数据库不同，MySQL 中有一个存储引擎的概念，针对不同的存储需求可以选择不同的存储引擎

MySQL支持的存储引擎，InnoDB和BDB提供事务安全表，其他存储引擎都是非事务安全的。5.5之后，默认的存储引擎是InnoDB。

<div>
    <image src=".\img\1.png"></image>
</div>

其中常用的引擎为：MyISAM、InnoDB、MEMORY和MERGE。

#### MyISAM

MyISAM不支持事务，也不支持外键，其优势是访问速度快，对事物完整性没有要求或以Select、Insert操作为主的应用可以使用该存储引擎。

#### InnoDB

InnoDB存储引擎提供了具有提交、回滚和崩溃恢复能力的事务安全，但是对比MyISAMy，InnoDB写的效率要差一些，并且会占用更多的存储空间保留数据和索引。

特定如下：

- 自动增长列
- 外键约束
- 存储方式：
  - 共享表空间存储
  - 多表空间存储

#### MEMORY

MEMORY使用存在于内存中的内容来创建表，每个MEMORY表实际只对应一个磁盘文件。MEMORY表的访问速度非常快，因为它的数据是存放在内存中的。

### 选中合适的类型

#### char与varchar

char是固定长度的，处理速度比varchar要好，缺点是浪费空间。但随着版本升级，varchar性能越来越好，建议使用varchar

#### text与blob

存储少量字符串时，一般选择char和varchar，而较大文本时，通常使用text和blob。区别是blob能存储二进制数据，而text只能保存字符数据。

text和blob执行大量的删除操作时会引发一些性能问题，建议OPTIMIZE TABLE进行碎片整理

### 索引的设计和使用

MyISAM 和InnoDB 存储引擎的表默认创建的都是BTREE 索引。

默认情况下，MEMORY 存储引擎使用HASH 索引，但也支持BTREE 索引

#### 设计原则

- 最适合做索引的列在where中，而不是select中
- 索引的列基数越大，索引的效果越好
- 使用短索引。如果对字符串进行索引，应该指定一个前缀长度
- 利用最左前缀
- 不要过度索引

#### BTREE和Hash索引

HASH 索引有一些重要的特征

- 只用于使用=或<=>操作符的等式比较。
- 优化器不能使用HASH 索引来加速ORDER BY 操作。
-  只能使用整个关键字来搜索一行。

### 事务控制和锁定语句

MySQL支持对MyISAM和MEMORY存储引擎的表进行表级锁定，对BDB存储引擎表进行页级锁定，对InnoDB进行行级锁定。

#### LOCK TABLE和UNLOCK TABLE

LOCK TABLES可以锁定用于当前线程的表。如果表被其他线程锁定，则当前线程会等待，知道获取所有锁定为止。

UNLOCK TABLES可以释放当前线程获得的任何锁定。

#### 事务控制

MySQL通过SET AUTOCOMMIT、START TRANSACTION、COMMIT、ROLLBACK支持本地事务。

在事务中可以定义SAVEPOINT，指定回滚事务的一部分，但不能指定提交事务的一部分。

#### 分布式事务的使用

分布式事务目前只支持InnoDB

### SQL中的安全问题

#### SQL注入

