### 表类型（存储引擎）的选择

和大多数数据库不同，MySQL 中有一个存储引擎的概念，针对不同的存储需求可以选择不同的存储引擎

MySQL支持的存储引擎，InnoDB和BDB提供事务安全表，其他存储引擎都是非事务安全的。5.5之后，默认的存储引擎是InnoDB。

<div>
    <image src=".\img\1.png"></image>
</div>

其中常用的引擎为：MyISAM、InnoDB、MEMORY和MERGE。

#### MyISAM

MyISAM不支持事务，也不支持外键，其优势是访问速度快，对事物完整性没有要求或以Select、Insert操作为主的应用可以使用该存储引擎。