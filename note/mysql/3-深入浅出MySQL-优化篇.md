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

