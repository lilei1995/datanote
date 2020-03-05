

# sql 索引

## 建立索引

当数据库单表数据量非常大的时候，使用普通查询耗时非常多，默认会根据搜索条件全表扫描；添加索引后，查询时就会先去索引列表中一次定位到特定值的行数，大大减少遍历匹配的行数，所以能明显增加查询的速度。

添加索引的话，首先去索引列表中查询，而我们的索引列表是B类树的数据结构，查询的时间复杂度为O(log2N)，定位到特定值得行就会非常快，所以其查询速度就会非常快。
![1569330789303](C:\Users\83759\AppData\Roaming\Typora\typora-user-images\1569330789303.png)

#### 使用CREATE 语句创建索引

```
CREATE INDEX index_name ON table_name(column_name,column_name) include(score)
```

#### 普通索引

```
CREATE UNIQUE INDEX index_name ON table_name (column_name) ;
```

#### 非空索引

```
CREATE PRIMARY KEY INDEX index_name ON table_name (column_name) ;
```

#### 主键索引 

#### 使用ALTER TABLE语句创建索引

```
alter table table_name add index index_name (column_list) ;
alter table table_name add unique (column_list) ;
alter table table_name add primary key (column_list) ;
```

#### 删除索引

```
drop index index_name on table_name ;
alter table table_name drop index index_name ;
alter table table_name drop primary key ;
```



### SQL CREATE UNIQUE INDEX 语法

在表上创建一个唯一的索引。唯一的索引意味着两个行不能拥有相同的索引值。

```
CREATE UNIQUE INDEX index_name
ON table_name (column_name)
```

### B+树

先说下，**在MySQL文档里，实际上是把B+树索引写成了BTREE**，例如像下面这样的写法：

> ```
> CREATE TABLE t(
> aid int unsigned not null auto_increment,
> userid int unsigned not null default 0,
> username varchar(20) not null default ‘’,
> detail varchar(255) not null default ‘’,
> primary key(aid),
> unique key(uid) USING **BTREE**,
> key (username(12)) USING **BTREE** — *此处 uname 列只创建了最左12个字符长度的部分索引*
> )engine=InnoDB;
> ```
>
> 

一个经典的**B+树索引数据结构**见下图：

![img](https://images2015.cnblogs.com/blog/99941/201607/99941-20160706162343639-644872932.jpg)



**B+树是一个平衡的多叉树，从根节点到每个叶子节点的高度差值不超过1，而且同层级的节点间有指针相互链接。**

在B+树上的常规检索，从根节点到叶子节点的搜索效率基本相当，不会出现大幅波动，而且基于索引的顺序扫描时，也可以利用双向指针快速左右移动，效率非常高。

因此，B+树索引被广泛应用于数据库、文件系统等场景。顺便说一下，xfs文件系统比ext3/ext4效率高很多的原因之一就是，它的文件及目录索引结构全部采用B+树索引，而ext3/ext4的文件目录结构则采用Linked list, hashed B-tree、Extents/Bitmap等索引数据结构，因此在高I/O压力下，其IOPS能力不如xfs。

### 哈希算法



### 哈希算法与B+树的区别

简单地说，**哈希索引就是采用一定的哈希算法**，把键值换算成新的哈希值，检索时不需要类似B+树那样从根节点到叶子节点逐级查找，只需一次哈希算法即可立刻定位到相应的位置，速度非常快。

从上面的图来看，B+树索引和哈希索引的明显区别是：

- **如果是等值查询，那么哈希索引明显有绝对优势**，因为只需要经过一次算法即可找到相应的键值；当然了，这个前提是，键值都是唯一的。如果键值不是唯一的，就需要先找到该键所在位置，然后再根据链表往后扫描，直到找到相应的数据；
- 从示意图中也能看到，**如果是范围查询检索，这时候哈希索引就毫无用武之地了**，因为原先是有序的键值，经过哈希算法后，有可能变成不连续的了，就没办法再利用索引完成范围查询检索；
- 同理，**哈希索引也没办法利用索引完成排序**，以及like ‘xxx%’ 这样的部分模糊查询（这种部分模糊查询，其实本质上也是范围查询）；
- **哈希索引也不支持多列联合索引的最左匹配规则**；
- B+树索引的关键字检索效率比较平均，不像B树那样波动幅度大，**在有大量重复键值情况下，哈希索引的效率也是极低的，因为存在所谓的哈希碰撞问题**。