# 考点聚集

## Mysql基础考点

* 事务的原理，特性，事务并发控制
* 常用的字段、含义和区别
* 常用数据库引擎之间区别

### 什么是事务

Transaction

* 事务是数据库并发控制的基本单位
* 事务可以看作是一系列SQL语句的集合
* 事务必须要么全部执行成功，要么全部执行失败（回滚）

转账操作是事务使用的一个常见场景

Transaction示例

```python
session.begin()
try:
    item1 = session.query(Item).get(1)
    item2 = session.query(Item).get(2)
    item1.foo = 'bar'
    item2.bar = 'foo'
    session.commit()  # commit提交事务  如果commit失败，就会去执行session的一个rollback操作 
except:
    session.rollback()
    raise
    
```

### 事务的ACID特性

**ACID是事务的四个基本特性**

牢记四个特性以及它们的含义

* 原子性（Atomicity）：一个事务中所有操作全部完成或失败
* 一致性（Consistency）：事务开始和结束之后数据完整性没有被破坏

* 隔离性（Isolation）：允许多个事务同时对数据库修改和读写（事务可以并发执行）
* 持久性（Durability）：事务结束之后，修改是永久的不会丢失

### 事务的并发控制可能产生哪些问题

如果不对事务进行并发控制，可能会产生四种异常情况

* 幻读（phantom read）：一个事务第二次查出现第一次没有的结果（说明别的事务已经插入了一些数据）
* 非重复读（nonrepeatable read）：一个事务重复读两次得到不同结果（比如说一个事务里面有两个查询语句，他查询语句是一样的，但是读了两次却得到了两个不同的结果。这说明读取操作的结果是不可重复的）
* 脏读（dirty read）：一个事务读取到另一个事务没有提交的修改
* 丢失修改（lost update）：并发写入造成其中一些修改丢失（线程安全）

### 四种事务隔离级别

为了解决并发控制异常，定义了4种事务隔离级别

* 读未提交（read uncommited）：别的事务可以读取到未提交改变
* 读已提交（read commited）：别的事务只能读取到已经提交的数据
* 可重复读（repeatable reaad）：同一个事务先后查询结果一样（Mysql InnoDB默认实现可重复读级别）
* 串行化（Serializable）：事务完全串行化的执行，隔离级别最高，执行效率最低

### 如何解决高并发场景下的插入重复

高并发场景下，写入数据库会有数据重复问题

* 使用数据库的唯一索引（就是不允许某些字段来去重复，如果重复了数据库就会抛一个异常出来）
* 使用队列异步写入
* 使用redis等实现分布式锁（这样的话就可以在插入的时候持有这个锁，在插入完成之后释放这个锁）

### 乐观锁和悲观锁

什么是乐观锁，什么是悲观锁

* 悲观锁是先获取锁再进行操作。一锁二查三更新 。当我们执行`select for update`的时候，别的事务暂时就没法来更新当前这个数据表了。
* 乐观锁先修改，更新的时候发现数据已经变了就回滚（check and set）。先假设我改的时候别人不会改，所以我先去修改，然后如果修改完成之后发现数据已经变了，去执行一个回滚操作。乐观锁一般通过版本号或者时间戳实现。
* 需要根据响应速度、冲突频率、重试代价来判断使用哪一种

### Mysql常用数据类型-字符串（文本）

| CHAR[Length]    | Length bytes                 | A fixed-length field from 0 to 255 characters long       |
| --------------- | ---------------------------- | -------------------------------------------------------- |
| VARCHAR[Length] | String length + 1 or 2 bytes | A variable-length field from 0 to 65,535 characters long |
| TINYTEXT        | String length + 1 bytes      | A string with a maximum length of 255 characters         |
| TEXT            | String length + 2 bytes      | A string with a maxmum length of 65,535 characters       |

CHAR --> 一般用来存储定长的字符串

VARCHAR --> 一般用来存储不定长的字符串

TEXT --> 来存储像新闻或文章这种较长的文本类型

### Mysql常用数据类型-数值

TINYINT[Length]

INT[Length]

BIGINT[Length]

FLOAT[Length, Decimals]

DOUBLE[Length, Decimals]

注：Length --> 指的是数据库显示的时候最小显示Length个字符宽

### Mysql常用数据类型-日期和时间

| DATE      | 3 bytes | In the format of YYYY-MM-DD                                  |
| --------- | ------- | ------------------------------------------------------------ |
| DATETIME  | 8 bytes | In the format of YYYY-MM-DD HH:MM:SS                         |
| TIMESTAMP | 4 bytes | In the format of YYYYMMDDHHMMSS; acceptable range starts in 1970 and ends in year 2038 |

### InnoDB vs MyISAM

两种引擎常见的区别

* MyISAM不支持事务，InnoDB支持事务
* MyISAM不支持外键，InnoDB支持外键
* MyISAM只支持表锁，InnoDB支持行锁和表锁