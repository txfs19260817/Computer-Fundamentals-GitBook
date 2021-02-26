---
description: 索引的数据结构是什么？比其他的结构好在哪里？
---

# 索引的数据结构是什么？

## B+ Tree

### 定义

![](../../.gitbook/assets/image%20%2845%29.png)

以 M-way B+tree 为例，它的特点总结如下：

* 每个Inner Node最多存储 M 个 key，有 M+1 个 children；
* B+ Tree 每个叶子节点的深度都一样；
* 除了根节点，所有其它节点中至少处于半满状态，即 $$M/2-1  \leq \#keys \leq M-1$$ ；
* 一个节点中的 key 从左到右非递减排列，key之间有指向子节点的指针，子节点的值介于两个key值之间；
* 若每个Inner Node中包含 k 个 keys，则它必有 k+1 个子节点；
* B+ Tree 的叶子节点通过双向链表串联，从而为 sequential access 提供高效支持

### 操作

* 查找：首先在根节点进行查找，找到一个 key 所在的指针，然后递归地在指针所指向的节点进行查找。直到查找到叶子节点，然后在叶子节点上进行查找，找出 key 所对应的 data。
* 插入：有空间则直接插入，否则需要将叶子节点分裂成两个节点，同时在父节点上新增 entry，若父节点也空间不足，则递归地分裂，直到根节点为止。
* 删除：如果删除节点后不为半满状态，则先尝试从兄弟节点拆借 entries，如果失败，则将其与相应的兄弟节点合并。如果合并发生了，则可能需要递归地删除父节点上的entry。

### 比较

#### 二叉搜索树/平衡二叉树/红黑树

1. 树高更低：因为B+ Tree的扇出系数大，因此树高一般在2~4层，也就是说只需做2~4的磁盘I/O即可完成查询。二叉树的出度为 2，因此树的高度会更高。
2. 二叉搜索树不保证平衡，有可能退化为链表。平衡二叉树/红黑树进行插入、更新和删除操作时，有左旋和右旋的操作，维护平衡的开销对磁盘来说很大。
3. 磁盘访问原理：一次 I/O 就能完全载入一个节点。预读特性，相邻的节点也能够被预先载入。

#### 哈希表

1. 等值查找时，哈希索引只需一次就可以找到；
2. 范围查找时，因为B+ Tree的索引连续且有序，哈希做不到；
3. like 模糊查询同范围查找类似；
4. 有大量重复键值情况下，哈希碰撞问题会使得其效率下降。

### 聚簇索引/非聚簇索引

![](../../.gitbook/assets/image%20%2838%29.png)

聚簇索引的叶子节点内容包括：

* key：主键；
* 事务ID
* 回滚指针
* 行数据

![](../../.gitbook/assets/image%20%2847%29.png)

非聚簇索引的叶子节点内容是key和对应的主键。二级索引是非聚簇索引。在使用二级索引查找时，先在非聚簇索引上找出key对应的主键，再去聚簇索引用该主键找出数据。

## Reference

1. [https://zhenghe.gitbook.io/open-courses/cmu-15-445-645-database-systems/tree-indexes](https://zhenghe.gitbook.io/open-courses/cmu-15-445-645-database-systems/tree-indexes)
2. [http://www.cyc2018.xyz/数据库/MySQL.html\#索引优化](http://www.cyc2018.xyz/数据库/MySQL.html#索引优化)
3. [https://blog.csdn.net/qq\_43255017/article/details/109428962](https://blog.csdn.net/qq_43255017/article/details/109428962)
4. [https://www.cnblogs.com/zengkefu/p/5647279.html](https://www.cnblogs.com/zengkefu/p/5647279.html)



