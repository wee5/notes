# redis命令

* redis客户端基本语法：$ redis-cl

## 开启本地服务和连接redis

* 命令操作

* * 开启本地：redis路径下打开cmd，输入redis-server.exe redis.windows.conf
  * 连接本地redis：redis路径下另开cmd，redis-cli.exe
  * 连接远程redis：输入redis-cli.exe -h 127.0.0.1 -p 6379 -a “password“

* 图形界面操作

* * 开启本地：双击redis-server.exe
  * 打开redis客户端：双击redis-cli，再通过命令连接服务

  ————————————————

## 键命令

* 语法：COMMAND KEY_NAME



## HyperLogLog

* 用于做基数统计的算法
* 不返回输入的各个元素
* 基数：数据集中不重复的数据的个数，基数估计就是在误差可接受的范围内，快速计算基数
* 命令：
* * pfadd KEY ELEMENT：添加指定元素到HyperLogLog中
  * pfcount KEY：返回给定 HyperLogLog 的基数估算值
  * pfmerge DESTKEY SOURCEKEY：将多个 HyperLogLog 合并为一个 HyperLogLog