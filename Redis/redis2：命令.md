# redis2：命令

- redis客户端基本语法：$ redis-cli

## 开启本地服务和连接redis

- 命令操作
- - 开启本地：redis路径下打开cmd，输入redis-server.exe redis.windows.conf
  - 连接本地redis：redis路径下另开cmd，redis-cli.exe
  - 连接远程redis：输入redis-cli.exe -h 127.0.0.1 -p 6379 -a “password“
- 图形界面操作
- - 开启本地：双击redis-server.exe
  - 打开redis客户端：双击redis-cli，再通过命令连接服务



## 键命令

- 语法：COMMAND KEY_NAME



## HyperLogLog

- 用于做基数统计的算法
- 不返回输入的各个元素
- 基数：数据集中不重复的数据的个数，基数估计就是在误差可接受的范围内，快速计算基数
- 命令：
  - pfadd KEY ELEMENT：添加指定元素到HyperLogLog中
  - pfcount KEY：返回给定 HyperLogLog 的基数估算值
  - pfmerge DESTKEY SOURCEKEY：将多个 HyperLogLog 合并为一个 HyperLogLog

## 常用命令

| 数据类型        | 增                                                           | 查                                                           | 删                                                           |
| --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **string**      | set key value                                                | get key                                                      | del key                                                      |
|                 | mset key1 value1 key2 value2                                 | mget key1 key2                                               |                                                              |
| **hash**        | hset key field value（单个设值）                             | hget key field                                               | hdel key field                                               |
|                 | hmset key field1 value1 field2 value2（多个设值）            | hgetall key（取所有）                                        | hdel key field1 field2                                       |
|                 | hsetnx key field value(无值则设)                             |                                                              |                                                              |
| **list**        | lpush/rpush key value1 value2                                | lindex key index（索引查询）                                 | rpop/lpop key（移除列表最后一个元素，返回移除元素）          |
| **set**         | sadd key member1 member2（集合添加成员）                     | smembers key（返回所有成员）                                 | spop key（移除并返回一个随机成员）                           |
|                 |                                                              | srandmember key [count]（返回一个或多个随机数）              | srem key member1 member2（移除一个或多个成员）               |
| **zset**        | zadd key score1 member1 scored2 member2（添加成员或更新分数） | zrange key start stop [withscores]（返回分数之间的成员）     |                                                              |
|                 |                                                              | zrevrank key member（返回指定成员，分数降序排序）            |                                                              |
|                 |                                                              | zscore key member（返回成员分数）                            |                                                              |
| **hyperloglog** | pfadd key element1 element2（添加元素）                      | pfcount key key1 key2（返回给定log的基数估值）               | pfmerge destkey sourcekey1 sourcekey2（合并log）             |
| **发布订阅**    | psubscribe pattern1 pattern2（订阅一个或多个符合给定模式的频道） | pubsub subcommand [argument [argument]]（查看订阅与发布系统状态） | [PUNSUBSCRIBE [pattern [pattern ...\]]（退订所有给定模式的资源） |
|                 | [SUBSCRIBE channel [channel ...\]]（订阅给定一个或多个频道的信息） |                                                              | [UNSUBSCRIBE [channel [channel ...\]]]（退订给定的频道）     |
|                 | [PUBLISH channel message]（将消息发送到指定的频道）          |                                                              |                                                              |
| **事务**        | multi（标记事务开始）                                        | exce（执行所有事务块内的命令）                               | discard（取消事务，放弃执行事务块内的所有命令）              |
|                 | watch key1 key2（监视一个或多个key；事务执行之前，这些key被其他命令改动，事务会被打断） |                                                              | unwatch（取消watch命令对所有key的监视）                      |
|                 |                                                              |                                                              |                                                              |

* hash实例

  * ```
    127.0.0.1:6379>  HMSET runoobkey name "redis tutorial" description "redis basic commands for caching" likes 20 visitors 23000
    OK
    127.0.0.1:6379>  HGETALL runoobkey
    1) "name"
    2) "redis tutorial"
    3) "description"
    4) "redis basic commands for caching"
    5) "likes"
    6) "20"
    7) "visitors"
    8) "23000"
    ```