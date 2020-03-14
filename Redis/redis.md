# redis简介

* 来源redis菜鸟

## 介绍

* 高性能key-value数据库，支持持久化，可将内存中的数据保存在磁盘中
* 还提供list，set，zset，hash等数据结构存储
* 支持数据备份，即master-slave模式数据备份

## 优势

* 读写快

* 丰富的数据类型

* 原子性，所有操作非成功即失败

* 丰富的特性，支持publish/subscribe, 通知, key 过期等等特性

  ——————————



## 配置

* 查看配置：config get CONFIG_SETTING_NAME（*为所有配置）

* 修改配置：config set CONFIG_SETTING_NAME CONFIG_SETTING_VALUE

* 参数说明参考redis菜鸟

  ———————————



## 数据类型

* 五种数据类型：string，hash，list，set，zset（sorted set有序集合）

### string

* 二进制安全，即该string可以包含任何类型，如jpg图片，序列化对象
* 一个键最大存储512MB

### hash

* 是键值（key=>value）对集合
* field，value映射表，field是string类型，hash特别适合存储对象
* 每个hash可以存储 2^32 -1 键值对（40多亿）

### list

* 简单的字符串列表，按照插入顺序，可添加一个元素在列表的头部或尾部
* 列表最多存储2^32-1元素

### set

* set是string的无序集合
* 集合通过哈希表实现。添加，删除，查找的复杂度都是O(1)

### zset(sorted set有序集合)

* zset和set一样是string类型元素的集合，且不允许重复成员
* 每个元素关联一个double类型分数，通过分数为集合中元素排序
* zset成员是唯一的，分数(score)可以重复



