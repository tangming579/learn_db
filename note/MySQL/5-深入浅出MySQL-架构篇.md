### MySQL复制

复制是指将主数据库的DDL和DML操作通过二进制日志传到复制服务器（也叫从库）。然后在复制服务器上对这些日志重新执行。使得从库和主库的数据保持同步。

#### 复制的优点

- 如果主库故障，可以快速切换到从库提供服务
- 可以在从库上执行查询操作，降低主库的访问压力
- 在从库上执行备份，避免备份期间影响主库的服务

MySQL的复制流程是异步的，实时性要求较高的数据仍需要从主库中获取

#### 复制原理

1. MySQL主库在数据提交时把数据变更作为事件记录在二进制日志文件Binlog中。

2. 主库推送二进制日志文件中的事件到从库中继日志 Relay Log，从库根据 Relay log重做数据变更操作，通过逻辑复制达到主库从库的数据一致

   <div>
       <image src="img/2.png"></image>
   </div>

#### 复制的3种常见架构

一主多从架构、多级复制架构、双主复制架构

### MySQL Cluster



### 高可用架构

