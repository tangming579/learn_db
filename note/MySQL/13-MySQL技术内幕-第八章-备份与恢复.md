### 备份与恢复概述

按备份方法分类：

- （Hot Backup）热备
- （Cold Backup）冷备
- （Warm Backup）温备

按备份后文件内容分类：

- 逻辑备份
- 裸文件备份

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