# redis4：持久化



## RDB方式（快照）

* 执行过程
  * redis启动，读取rdb快照文件，将数据载入内存
  * 此时可以对数据库写入，删除等操作，**内存保持redis数据库最新状态**
  * 使用fork函数，子进程将内存中数据写入硬盘中的临时文件，**临时文件同内存保持redis数据库最新状态**
  * 子进程写完所有所有数据后，临时文件替换旧rdb文件，一次快照操作完成，**临时文件保持redis数据库在一次快照操作中的最后一次状态**



## AOF方式







* 参考：
  * https://blog.csdn.net/belvine/article/details/90509875
  * https://blog.csdn.net/weixin_33978016/article/details/88747443