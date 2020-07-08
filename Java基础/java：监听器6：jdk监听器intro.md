# java：监听器6：jdk监听器intro



## 上图

![avatar](图片引用\301700508104329.jpg)





## 监听器分类

### 按监听对象划分

ServletContext监听：监听application内置对象的创建和销毁

​	当web容器开启时，执行contextInitialized方法；当容器关闭或重启时，执行contextDestroyed方法

HttpSession监听：对应监控session内置对象的创建和销毁

​	当打开一个新的页面时，开启一个session会话，执行sessionCreated方法；当页面关闭session过期时，或者容器关闭销毁时，执行sessionDestroyed方法

ServletRequest监听：对应监控request内置对象的创建和销毁

​	当访问某个页面时，出发一个request请求，执行requestInitialized方法；当页面关闭时，执行requestDestroyed方法



### 按监听事件划分

事件的创建和销毁

监听属性的增删改

监听对象状态



## 其他

### Session数据的活化

由于session中保存大量访问网站相关的重要信息，因此过多的session数据就会服务器性能的下降，占用过多的内存。因此类似数据库对象的持久化，web容器也会把不常使用的session数据持久化到本地文件或者数据中。这些都是有web容器自己完成，不需要用户设定。

不用的session数据序列化到本地文件中的过程，就是钝化；

当再次访问需要到该session的内容时，就会读取本地文件，再次放入内存中，这个过程就是活化