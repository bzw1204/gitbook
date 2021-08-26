---
description: 记录事务超时处理
---

# Mysql事物锁等待超时 Lock wait timeout exceeded; try restarting transaction

工作中同事遇到此异常，查找解决问题时，收集整理形成此篇文章。

问题场景 问题出现环境： 1、在同一事务内先后对同一条数据进行插入和更新操作； 2、多台服务器操作同一数据库； 3、瞬时出现高并发现象；

不断的有一下异常抛出，异常信息：

```text
org.springframework.dao.CannotAcquireLockException: 
### Error updating database.  Cause: java.sql.SQLException: Lock wait timeout exceeded; try restarting transaction
### The error may involve com.*.dao.mapper.PhoneFlowMapper.updateByPrimaryKeySelective-Inline
### The error occurred while setting parameters
### SQL:-----后面为SQL语句及堆栈信息--------
```

原因分析 在高并发的情况下，Spring事物造成数据库死锁，后续操作超时抛出异常。 Mysql数据库采用InnoDB模式，默认参数:innodb\_lock\_wait\_timeout设置锁等待的时间是50s，一旦数据库锁超过这个时间就会报错。

解决方案 

1、通过下面语句查找到为提交事务的数据，kill掉此线程即可。

`select * from information_schema.innodb_trx` 

2、增加锁等待时间，即增大下面配置项参数值，单位为秒（s）

`innodb_lock_wait_timeout=500` 

3、优化存储过程,事务避免过长时间的等待。

参考信息 1、锁等待超时。是当前事务在等待其它事务释放锁资源造成的。可以找出锁资源竞争的表和语句，优化SQL，创建索引等。如果还是不行，可以适当减少并发线程数。 2、事务在等待给某个表加锁时超时，估计是表正被另的进程锁住一直没有释放。 可以用 SHOW INNODB STATUS/G; 看一下锁的情况。 3、搜索解决之道，在管理节点的\[ndbd default\]区加： TransactionDeadLockDetectionTimeOut=10000（设置 为10秒）默认是1200（1.2秒） 4、InnoDB会自动的检测死锁进行回滚，或者终止死锁的情况。

InnoDB automatically detects transaction deadlocks and rolls back a transaction or transactions to break the deadlock. InnoDB tries to pick small transactions to roll back, where the size of a transaction is determined by the number of rows inserted, updated, or deleted.

如果参数innodb\_table\_locks=1并且autocommit=0时，InnoDB会留意表的死锁，和MySQL层面的行级锁。另外，InnoDB不会检测MySQL的Lock Tables命令和其他存储引擎死锁。你应该设置innodb\_lock\_wait\_timeout来解决这种情况。 innodb\_lock\_wait\_timeout是Innodb放弃行级锁的超时时间。

参考文章：[http://www.51testing.com/html/16/390216-838016.html](http://www.51testing.com/html/16/390216-838016.html)

深入研究 由于此项目采用Spring+mybatis框架，事物控制采用“org.springframework.jdbc.datasource.DataSourceTransactionManager”类进行处理。此处还需进行进一步调研Spring实现的机制

