# 事务
---

####事务ACID
- 原子性（atomicity）
一个事务必须被视为一个不可分割的整体，要不全部执行，要不全部回滚
- 一致性（consistency）
数据库总是从一个一致性状态转换到另一个一致性状态。
- 隔离性（isolation）
通常一个事务所做的修改在最总提交之前对其他事务不可见。
- 持久性（durability）
一旦事务提交，数据将永久性保存在数据库中

---

####事务隔离级别

- 未提交读-READ UNCOMMITTED ： 事务中对数据的修改即使未提交对其他事务也是可见的。可能出现脏读

- 提交读-READ COMMITTED ： 一个事务开始时只能看见已提交的事务所做的修改。不可重复读

- 可重复度-REPEATABLE READ ： 解决了脏读问题，可重复读中保证了同一个事务中多次读取同样的记录结果是一致的。但是还是无法解决两外的问题--幻读。所谓幻读， 指的是当某个事务在读取某个范围内的记录时，另外一个事务又在该范围内插入了新的记录，当之前的事务再次读取该范围 的记录时，会产生幻行（Phantom Row）。InnoDB和XtraDB存储引擎通过多版本并发控制（MVCC，Multiversion Concurrency Control） 解决了幻读的问题。

- 可串行化-SERIALIZABLE ： 强制事务串行执行解决幻读问题

| 隔离级别 | 脏读可能性 | 不可重复读可能性 | 幻读可能性 | 加锁读 |
| :-- | :-- | :-- | :-- | :-- |
|READ UNCOMMITTE|yes|yes|yes|no|
|READ COMMITTED|no|yes|yes|no|
|REPEATABLE READ|no|no|yes|no|
| SERIALIZABLE |no|no|no|yes|

---