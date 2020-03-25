# Redis：精简



## 启动

* 安装完毕后
* redis路径下打开cmd，输入redis-server.exe redis.windows.conf(redis-server.exe为默认的)
* redis路径下另开cmd，输入redis-cli.exe -h 127.0.0.1 -p 6379



## 命令 COMMAND KEY_NAME (EXTRA)

### 启动命令

* redis路径下cmd，输入redis-cli连接本地redis服务
* 输入ping，返回pong，检测redis服务是否启动
* 远程连接redis服务，输入redis-cli -h host -p port -a password
* 避免中文乱码： --raw
* 补充：在redis路径下才能使用redis命令，连接redis服务的命令为redis-cli -h host -p port -a password，本地连接简化为redis-cli

### 键命令

* 键：set key value; del key
* String：set key value; get key
* Hash：hset key field value; hget key field; hdel key field [filed2]; hsetnx key field value
* List：rpush key index [index2]; rpushx key value; lpop key; lrem key count value; lindex key index; lrange key start stop;
* Set：sadd key member1 [member2]; smembers key; srandmember key [count]; srem key member1 [member2]
* zset：zadd key score1 member1 [score2 member2]; zrange key start stop [withscores]; zrangebylex key min max [limit offset count]; zrangebyscore key min max [withscores]; [limit] zrank key member; zscore key member; zrem key member [member ...]
* HyperLogLog：pfadd key element [element ...]; pfcount key [key ...]; pfmerge destkey sourcekey [sourcekey ...]
* 发布订阅：
  * 打开redis服务
  * 连接redis服务（不区分远程和本地），作为发布者
  * 另外开启cmd窗口，连接redis服务（不区分远程和本地），作为订阅者
  * 订阅者输入命令subscribe redischat，连接redischat频道
  * 发布者输入命令publish redischat massage，订阅者会自动打印massage
* 事务：multi/ exec/ discard/ watch key [key ...]/ unwatch
* 脚本：
* 连接：
* 服务器：