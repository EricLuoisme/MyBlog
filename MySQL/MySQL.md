# 基本命令

##### 基础命令行操作

| 命令                 | 说明                     |
| -------------------- | ------------------------ |
| show databases       | 展示所有数据库           |
| show tables          | 展示该数据库下所有表     |
| describe [TABLENAME] | 展示该表所有字段类型信息 |
| exit                 | 退出链接                 |
| --                   | 单行注释                 |
| /**/                 | 多行注释                 |

**MySQL关键字不区分大小写**



##### 基础DDL语句

```SQL 
CREATE [DATABASE/TABLE] (IF NOT EXIST) [NAME]
-- 可以使用 IF NOT EXIST 来避免报错
```

如果表名之类的是特殊字符，需要使用``来包括起来



# 数据库字段类型

> 数值类型

| 关键字                                    | 字节 |
| ----------------------------------------- | ---- |
| tinyint                                   | 1    |
| smallint                                  | 2    |
| mediumint                                 | 3    |
| int（使用int(3)相当于显示int的3位默认）   | 4    |
| bigint                                    | 8    |
| float                                     | 4    |
| double                                    | 8    |
| decimal(字符串形式的浮点数，金融计算使用) | 可变 |

> 字符串

| 关键字   | 字节     |
| -------- | -------- |
| char     | 0-255    |
| varchar  | 0-65535  |
| tinytext | 2^8 -1   |
| text     | 2^16 - 1 |

> 时间日期

| 关键字    | 格式                |
| --------- | ------------------- |
| date      | YYYY-MM-DD          |
| time      | hh:mm:ss            |
| datetime  | YYYY-MM-DD hh:mm:ss |
| timestamp | 时间戳              |
| year      | 年                  |

> null

**不要使用NULL进行运算，结果为NULL**



# 数据库字段属性

**Unsigned**

- 无符号整数
- 不能为负数



**zerofill**

- 0填充的
- 不足位数，从左到右进行填充



**auto_increment**

- 自增
- 通常设置为唯一主键，避暑为证书类型



**notNull**

- 不能为空



# 数据库Engine

| 特性       | MYISAM | INNODB                 |
| ---------- | ------ | ---------------------- |
| 事务支持   | 不支持 | 支持                   |
| 数据行锁定 | 不支持 | 支持                   |
| 外键约束   | 不支持 | 支持                   |
| 全文索引   | 不支持 | 支持                   |
| 表空间大小 | 较小   | 较大，约为MYISAM的两倍 |

INNODB：安全性高，能对事务进行处理，多表多用户操作



# 修改与删除表字段

修改：ALTER

```sql
-- 修改表名
ALTER TABLE [TableName] RENAME AS [newTabelName]

-- 添加字段
ALTER TABLE [TableName] ADD [newFieldName] [FieldType]

-- MODIFY能修改字段类型和约束
ALTER TABLE [TableName] MODIFY [fieldName] [newFieldType]

-- CHANGE用来字段重命名
ALTER TABLE [TableName] CHANGE [fieldName] [replacedFieldName] [fieldType]

-- 删除字段
ALTER TABLE [TableName] DROP [fieldName]
```



# 查询

## 模糊查询

一个'__'，代表一个位



## select完整语法

> ```sql
> SELECT [ALL | Distinct]
> {* | table.* | [table.field1[as alias1][,table.field2[as alias2]][,....]}
> FROM table_name[as table_alias]
> 	[left | right | inner join table_name2[as table_alias2]] --联合查询
>     [WHERE ...] -- 指定结果需满足的条件
>     [GROUP BY ...] -- 指定结果按照哪几个字段来分组
>     [HAVING ...] -- 过滤分组记录必须满足的条件
>     [ORDER BY ...] -- 查询记录按一个或多个条件排序
>     [LIMIT {[offset,]row_count | row_countOFFSET offset}]
>     -- 指定查询记录从哪条到哪条
> ```



## join连接

<img src="https://www.runoob.com/wp-content/uploads/2019/01/sql-join.png" alt="img" style="zoom:80%;" />



# 分页

##### 为什么要分页？

- 缓解数据库压力

```sql
SELECT s.'StudentNo', 'StudentName'
FROM student s
INNER JOIN result r
ON s.StudentNo = r.StudentNo
WHERE subjectName = 'testing'
ORDER BY StudentResult ASC
LIMIT 0,5
```

-- limit 0,5 -- 第一页，显示第0到第4个

-- limit 5,5 -- 第二页，显示第5到第9个

-- limit 10,5 -- 第三页，显示第10到第14个



# 事务

```sql
START TRANSACTION --标记一个事务的开始，从这之后的sql都在同一个事物内

INSERT XXX
INSERT XXX

-- 提交
COMMIT

-- 回滚
ROOLBACK

-- 事务结束
SET autocommit = 1 -- 开启自动提交

SAVEPOINT -- 设置一个事务的保存点
ROLLBACK TO SAVEPOINT -- 回滚到保存点
RELEASE SAVEPOINT --撤销保存点
```



一个转账的例子：

```sql
 SET autocommit = 0;
 START TRANSACTION
 
 UPDATE ...;
 UPDATE ...;
 
 COMMIT;
 ROLLBAK;
 
 SET autocommit = 1;
```



# 索引

> MySQL官方对索引的定义为：帮助高效获取数据的数据结构，提取句子主干，就可以得到索引的本质——索引是数据结构。

## 索引的分类

- 主键索引（PRIMARY KEY）
  - 唯一的标识，主键不可重复，只有一个列可以作为主键
- 唯一索引（UNIQUE KEY）
  - 避免重复的列出现，可重复
- 常规索引（KEY/INDEX）
- 全文索引（FullText）
  - MyISAM才有
  - 快速定位数据



## 索引原则

- 索引不是越多越好
- 不要对经常需要变动的数据加索引
- 小数据量的表不要加索引



### Btree索引

http://blog.codinglabs.org/articles/theory-of-mysql-index.html



















