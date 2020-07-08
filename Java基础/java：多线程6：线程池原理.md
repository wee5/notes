# java：多线程6：线程池原理

* **线程池出现原因：**一个线程的耗时包括创建线程，线程使用（就绪，运行，阻塞等），销毁线程；若线程使用时间很短，那专门创建一个线程去使用的效率就很低；于是需要线程复用，即线程池
* 可以看看这个，http://www.cnblogs.com/dolphin0520/p/3932921.html



## 框架结构

![avatar](图片引用\Snipaste_2020-06-20_10-46-44.png)

![avatar](图片引用\Snipaste_2020-06-20_10-58-35.png)

顶级接口：java.util.concurrent.Executor

实现接口：java.util.concurrent.ExecutorService（执行服务）包含服务的生命周期

实现接口：java.util.concurrent.ScheduleExecutorService（调度相关服务）

核心实现类：

​	java.util.concurrent.ThreadPoolExecutor（普通线程的实现类）

​	java.util.concurrent.ScheduledThreadPoolExecutor（调度核心实现类）