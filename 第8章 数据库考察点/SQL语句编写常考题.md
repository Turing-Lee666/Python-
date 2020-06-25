# SQL语句编写常考题

## 考点聚集

### SQL语句以考察各种常用连接为重点

* 内连接(INNER JOIN)：两个表都存在匹配时，才会返回匹配行
* 外连接(LEFT/RIGHT JOIN)：返回一个表的行，即使另一个没有匹配
* 全连接(FULL JOIN)：只要某一个表存在匹配就返回

## 内连接

### INNER JOIN

* 将左表和右表能够关联起来的数据连接后返回

* 类似于求两个表的“交集”

* ```mysql
  select * from A inner join B on a.id=b.id;  
  ```

## 示例表

用A, B这两个表作为示例

A:

| id   | val  |
| ---- | ---- |
| 1    | ab   |
| 2    | a    |

B:

| id   | val  |
| ---- | ---- |
| 1    | ab   |
| 3    | b    |

先来编写内连接(inner join)

内连接(INNER JOIN)：两个表都存在匹配时，才会返回匹配行

```mysql
select A.id as a_id, B.id as b_id, A.val as a_val, B.val as b_val from A inner join B on A.id=B.id;
```

结果：

| a_id | b_id | a_val | b_val |
| ---- | ---- | ----- | ----- |
| 1    | 1    | ab    | ab    |

## 外连接

### 外连接包含左连接和右连接

* 左连接返回左表中所有记录，即使右表中没有匹配的记录
* 右连接返回右表中所有记录，即使左表中没有匹配的记录
* 没有匹配的字段会设置成NULL

Mysql中使用left join和right join实现

```mysql
select A.id as a_id, B.id as b_id, A.val as a_val, B.val as b_val from A left join B on A.id=B.id;
```

| a_id | b_id | a_val | b_val |
| ---- | ---- | ----- | ----- |
| 1    | 1    | ab    | ab    |
| 2    | NULL | a     | NULL  |

