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

# MySQL索引背后的数据结构及算法原理

## 数据结构及算法基础

MySQL官方对索引的定义为：索引（Index）是帮助MySQL高效获取数据的数据结构。提取句子主干，就可以得到索引的本质：索引是数据结构。

查询是数据库最主要的功能之一，所以我们都希望查询的速度能尽可能的快，因此我们需要从查询算法上进行优化。最基本的就是顺序查询（linear search），但数据量大的时候将会很效率很差。而二分查找，二叉树查找性能更好。随之而来的问题是，这些查找方法都依赖于某种特定的数据结构（二分查找需要大小有序，二叉树查找需要依赖二叉树）。而数据本身的组织结构不可能全满足各种数据结构（例如，不能按照两列数据都从小到大排）。

所以，数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式**引用（指向）**数据，这样就可以在这些数据结构上实现高级查找算法。*这种数据结构，就是索引*。

![img](C:\MyBlog\MySQL\1.png)

图1展示了一种可能的索引方式。左边是数据表，一共有两列七条记录，最左边的是数据记录的物理地址（注意逻辑上相邻的记录在磁盘上也并不是一定物理相邻的）。为了加快Col2的查找，可以维护一个右边所示的二叉查找树，每个节点分别包含索引键值和一个指向对应数据记录物理地址的指针，这样就可以运用二叉查找在O(log2n)O(log2n)的复杂度内获取到相应数据。

但是实际的数据库几乎都是使用红黑树实现的。

### BTree

![img](C:\MyBlog\MySQL\2.png)

中间空白的地方存储的是指针，指向下一个节点。而每个非叶子节点，由n-1个key和n个指针组成，其中 d <= n <= 2d，d是这个BTree的度。而叶节点，至少包含一个key和两个指针，叶节点指针均为空。而一个节点从左到右排，都是从小到大。而中间的指针，指向的下面子树，就是这两个值之间的值。



### B+Tree

![img](C:\MyBlog\MySQL\3.png)

具体区别如图所示。



### 带顺序访问指针的B+Tree

![img](C:\MyBlog\MySQL\4.png)



## 为什么使用BTree

一般来说，索引本身也很大，不可能全部存储在内存中，因此索引往往以索引文件的形式存储的磁盘上。这样的话，索引查找过程中就要产生磁盘I/O消耗，相对于内存存取，I/O存取的消耗要高几个数量级，所以评价一个数据结构作为索引的优劣最重要的指标就是在查找过程中磁盘I/O操作次数的渐进复杂度。换句话说，索引的结构组织要尽量减少查找过程中磁盘I/O的存取次数。

而红黑树结构上深度明显比BTree深度大，逻辑上很近的节点可能物理上很远。但BTree刚好相反，由于磁盘的物理性质，读取的位置可以很近。也就是BTree的度可以很大，一次就可以加载很多。所以BTree比红黑树更适合。

## MySQL索引实现

### MyISAM实现

使用的就是B+Tree作为索引结构，使用的索引是“非聚集的”。

主索引：

<img src="C:\MyBlog\MySQL\8.png" alt="img" style="zoom: 80%;" />

副索引：

<img src="C:\MyBlog\MySQL\9.png" alt="img" style="zoom:80%;" />

### InnoDB实现

虽然InnoDB也使用B+Tree作为索引结构，但具体实现方式却与MyISAM截然不同，使用的是聚集索引。

第一个重大区别是InnoDB的数据文件本身就是索引文件。从上文知道，MyISAM索引文件和数据文件是分离的，索引文件仅保存数据记录的地址。而在InnoDB中，表数据文件本身就是按B+Tree组织的一个索引结构，这棵树的叶节点data域保存了完整的数据记录。这个索引的key是数据表的主键，因此InnoDB表数据文件本身就是主索引。

![img](C:\MyBlog\MySQL\10.png)

第二个与MyISAM索引的不同是InnoDB的辅助索引data域存储相应记录主键的值而不是地址。换句话说，InnoDB的所有辅助索引都引用主键作为data域。例如，为定义在Col3上的一个辅助索引：

![img](C:\MyBlog\MySQL\11.png)

这里以英文字符的ASCII码作为比较准则。聚集索引这种实现方式使得按主键的搜索十分高效，但是**辅助索引搜索需要检索两遍索引**：首先检索辅助索引获得主键，然后用主键到主索引中检索获得记录。

了解不同存储引擎的索引实现方式对于正确使用和优化索引都非常有帮助，例如知道了InnoDB的索引实现后，就很容易明白为什么**不建议使用过长的字段作为主键**，因为所有辅助索引都引用主索引，过长的主索引会令辅助索引变得过大。再例如，用非单调的字段作为主键在InnoDB中不是个好主意，因为InnoDB数据文件本身是一颗B+Tree，非单调的主键会造成在插入新记录时数据文件为了维持B+Tree的特性而频繁的分裂调整，十分低效，而使用**自增字段**作为主键则是一个很好的选择。

## 索引使用策略优化

使用InnoDB时，如果可以，一定要使用一个与业务无关的自增id作为主键，这样索引效率可以最大。























