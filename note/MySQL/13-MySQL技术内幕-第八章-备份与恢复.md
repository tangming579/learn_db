### 备份与恢复概述

按备份方法分类：

- （Hot Backup）热备
- （Cold Backup）冷备
- （Warm Backup）温备

按备份后文件内容分类：

- 逻辑备份
- 裸文件备份

### 快照备份



### 复制

#### 复制的工作原理

复制（replication）是MySQL数据库提供的一种高可用高性能解决方案，主要分为以下三步：

- 主数据库（master）把数据更改记录到二进制日志中（binlog）
- 从数据库（slave）把主数据库的二进制日志复制到自己的中继日志中（relay log）
- 从服务器重做中继日志中的日志，把更改应用到自己的数据库上

<div>
    <image src="img\5.png" height=300></image>
</div>
#### 快照 + 复制的备份架构

复制的主要功能：

- 数据分布
- 读取负载平衡，如通过LVS
- 数据库备份
- 高可用性和故障转移

为了避免在主服务器上执行了误操作导致从服务器也跟着运行了误操作，比较好的方式是对从服务器上的数据库所在分区做快照。当发生主服务器上的误操作时，只需要将从服务器上的快照进行恢复，再根据二进制日志进行 point-in-time 的恢复即可。

<div>
    <image src="img/6.png"></image>
</div>
#### 配置步骤

##### master 机器的配置

1. 启用二进制日志。
2. 为当前节点设置一个全局唯一的server_id。
3. 创建有复制权限的用户账号 REPLIACTION SLAVE ,REPLIATION CLIENT。

```
[mysqld]
log-bin=/var/log/mysql/mysql-bin
server-id=1
```

```
mysql> CREATE USER 'repl'@'%' 
mysql> GRANT REPLICATION SLAVE ON *.*  TO  'repl'@'%'  identified by 
 'QFedu123!';
mysql> 
```

##### slave 机器的配置

1. 启动中继日志。
2. 为当前节点设置一个全局唯一的server_id。
3. 使用有复制权限的用户账号连接至主节点，并启动复制线程。

```
relay-log=relay-log
relay-log-index=relay-log.index
bind-address = 0.0.0.0
server-id = 106
```

#### 注意点

从节点要设置某些限定使得它不能进行写操作,才能保证复制当中的数据一致。

#####  限制从服务器为只读
在从服务器上设置：
read_only = ON,但是此限制对拥有SUPER权限的用户均无效。
阻止所有用户：
mysq>FLUSH TABLES WITH READ LOCK;

##### 如何保证主从复制时的事物安全

主节点：

sync_binlog=1： Mysql开启bin-log日志使用bin-log时，默认情况下，并不是每次执行写入就与硬盘同步，这样在服务器崩溃时，就可能导致bin-log最后的语句丢失。可以通过这个参数来调节

```
innodb_flush_logs_at_trx_commit=ON 刷写日志
innodb_support_xa=ON (分布式事务：基于它来做两段式提交功能)
sync_master_info=1：让从服务器中的 master_info 及时更新。
```

从节点：

```
skip_slave_start =ON (跳过自动启动，使用手动启动。)
relay_log也会在内从中先缓存，然后在同步到relay_log中去，可以使用下面参数使其立即同步。
sync_relay_log =1 ，默认为10000，即每10000次sync_relay_log事件会刷新到磁盘。为0则表示不刷新，交由OS的cache控制。
sync_relay_log_info=1每间隔多少事务刷新relay-log.info，如果是table（innodb）设置无效，每个事务都会更新
```

