# redis6：密码



## 设置密码

* 配置文件设置密码

  * ```conf
    #requirepass foobared  (改成下面)
    requirepass 123456
    #若需要改配置生效，需要在命令启动时指定配置文件
    ```

  * 启动：**redis-server redis.windows.conf**（window**s**有**s**）

* 命令设置密码（**临时**）

  * config set requirepass 123456



## 连接

* 连接：redis-cli -a 123456
* 若存在密码，但连接时未指定密码，输入任何命令时就会提示：NOAUTH Authentication required.
  * 再输入auth 123456即可