---
description: 尊贵的关系型数据库
---

# Mysql

## 安装

[Mac for MySQL 5.7 安装教程](https://www.cnblogs.com/kimbo/p/8724595.html)

差点意思不展开说了~

## varchar\(255\) 与 varchar\(256\)

varchar需要一个额外的字段来计算具体字符串需要多少空间，255刚好是一个字节，超过255就得两个字节了，所以varchar\(255\)比varchar\(256\)能省一个字节

## MySQL server has gone away

* 检查数据库的连接时间 wait\_timeout
* 增加日志文件大小 innodb\_log\_file\_size
* 增大传递的包大小max\_allowed\_packet（在这里摔倒过）

  > show variables like '%max\_allowed\_packet%' 
  >
  > set global max\_allowed\_packet = 100\*1024\*1024

## 字符串与整型作为主键

* 字符串主键在插入过程，由于key不规律，页没满就发生分裂，页数特别多；而整型是顺序插入，页利用率高
* 字符串相对于整型需要的空间更多，在加载索引时，同样大小的页，索引范围比整型小

[MySQL 字符串主键和整型主键分析](https://www.cnblogs.com/zhoujinyi/archive/2012/09/21/2697522.html)

## 事务与锁

* 事务具有ACID（原子性、一致性、隔离性和持久性），锁是用于解决隔离性的一种机制
* 事务的隔离级别通过锁机制实现，事务具有不同的隔离级别，锁具有不同的粒度
* 开启事务就自动加锁

### 事务隔离级别

#### 1.Read Uncommitted（读取未提交数据）

所有事务可以看到其他事务未提交的内容。

事务读出的信息往往是过程性的，无用的，称之为脏读

#### 2.Read Committed（读取提交内容）

一个事务只能看到已提交事务做出的改变，未提交的事务的修改情况不可见。

事务A在查询一条记录后，另一个事务B迅速的改变该记录的值，这时事务A再读取记录时，会得到不同的值，即在同一个事务周期内，同一条记录存在两个值，称之为不可重复读。

除mysql外的数据库，多是这个隔离级别

#### 3.Repeatable Read（可重复读）

事务会锁定其访问的所有行，避免其他事务对其修改（悲观锁），从而保证在一个事务周期内，可重复读数据。

但是可能存在，事务A修改所有记录的a列为一个特定值x，这时事务B插入了一个新的记录a列为y（不在锁定行内，与可重复读不冲突），最终事务A结束后，发现库中a列并没有全为x，称之为幻读。

mysql innoDB是这个隔离级别。

#### 4.Serializable（可串行化）

事务排序就安全了

### 机制加锁

#### 按类型

* 读锁：共享锁

  > select ... lock in share mode

* 写锁：排他锁

  > select ... for update

#### 按粒度

* 表锁：由数据库服务器实现
* 行锁：由存储引擎实现

### 存储引擎

innoDB

### 多版本并发控制（MVCC）

[Mysql中MVCC的使用及原理详解](https://blog.csdn.net/w2064004678/article/details/83012387)











