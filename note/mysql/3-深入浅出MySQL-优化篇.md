### SQL 语句优化

#### SQL 问题

1. 通过show status命令查看各种SQL的执行频率

```mysql
show status com*	 	//各种数据库操作的数量
```

2. 定位执行效率较低的SQL语句
   - 通过慢查询日志定位
   - 通过show processlist查看当前MySQL进行的线程，包括线程状态，是否锁表等
3. 通过EXPLAIN分析低效SQL的执行计划
4. 通过show profile分析SQL

#### 索引问题

MYSQL支持一下索引：

- B-Tree索引，最常见的索引类型
- HASH索引，只有MEMORY引擎支持，Hash索引不适合范围索引，只适合 “=” 条件的
- R-Tree索引（空间索引），MyISAM特有，使用较少
- Full-text（全文索引）

B树中的B不代表二叉树，代表平衡树

<div>
    <image src="img/3.png"></image>
</div>



### 优化数据库对象



### 锁问题



### 优化MySQL Server



### 磁盘IO问题



### 应用优化



