# redis1：intro

* 来源redis菜鸟

## 介绍

* 高性能key-value数据库，支持持久化，可将内存中的数据保存在磁盘中
* 还提供String，list，set，zset，hash等数据结构存储
* 支持数据备份，即master-slave模式数据备份



## 优势

* 读写快

* 丰富的数据类型

* 原子性，所有操作非成功即失败

* 丰富的特性，支持publish/subscribe, 通知, key 过期等等特性




## 配置

* 查看配置：config get CONFIG_SETTING_NAME（*为所有配置）

* 修改配置：config set CONFIG_SETTING_NAME CONFIG_SETTING_VALUE

* 参数说明参考redis菜鸟




## 数据类型

* 五种数据类型：string，hash，list，set，zset（sorted set有序集合）

#### string

* 二进制安全，即该string可以包含任何类型，如jpg图片，序列化对象
* 一个键最大存储512MB

#### hash

* 是键值（key=>value）对集合
* field，value映射表，field是**string**类型，hash特别适合**存储对象**
* 每个hash可以存储 2^32 -1 键值对（40多亿）

#### list

* 简单的**字符串**列表，按照**插入顺序**排序，可添加一个元素在列表的**头部或尾部**
* 列表最多存储2^32-1元素

#### set

* set是**string的无序集合**
* 集合通过哈希表实现。添加，删除，查找的复杂度都是O(1)

#### zset(sorted set有序集合)

* zset和set一样是**string**类型元素的集合，且**不允许重复成员**
* 每个元素关联一个**double类型分数**，通过分数为集合中元素排序
* zset成员是唯一的，分数(score)可以重复

#### hyperloglog

* 用来做基数统计的算法
* 优点：输入元素数量或体积非常大时，计算基数所需要的空间总是固定的，而且很小
* hyperloglog根据输入元素计算基数，并不会存储元素本身；所以它不能像集合一样返回各个元素

#### 发布订阅（pub/sub）

* 一种消息通信模式：发送者pub发送消息，订阅者sub订阅消息
* redis客户端可以订阅任意数量的频道
* 新消息通过publish命令发送给频道时，消息会被发送给它的所有订阅者

#### 事务

* redis事务可以一次执行多个命令，并且带有这三个保证
  * 批量操作在发送exce命令前被放入队列缓存
  * 受到exce命令进入事务执行，事务中任意命令执行失败，其余命令依然被执行
  * 在事务过程中，其他客户端提交的命令请求不会插入到事务执行命令序列中
* 事务从开始到执行的三个阶段
  * 开始事务
  * 命令入队
  * 执行事务

#### 脚本

* Redis 脚本使用 Lua 解释器来执行脚本。 Redis 2.6 版本通过内嵌支持 Lua 环境

#### 连接

* Redis 连接命令主要是用于连接 redis 服务

#### 服务器

* Redis 服务器命令主要是用于管理 redis 服务



* 推荐：
  * https://blog.csdn.net/liqingtx/article/details/60330555
  * https://www.jianshu.com/p/a1038eed6d44 redis完整教程