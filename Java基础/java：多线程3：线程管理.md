# java：多线程3：线程管理



## 线程睡眠sleep

* 原因：线程执行的太快，或需要强制执行到下一个线程
* 方法：
  * **Thread.sleep(long millis)**在指定的毫秒数内让正在执行的线程休眠
  * **Thread.sleep(long millis，int nanos)**在指定的毫秒数加指定的纳秒数内让正在执行的线程休眠
* 指定线程休眠时间，实际时间可能会偏大；因为从就绪状态进入运行状态由系统控制，所花费的时间不能人为控制
* 描述：**线程休眠会进入阻塞状态**



## 线程让步yield

* 方法：**Thread.yield()**
* 描述：**线程让步会直接进入就绪状态，不会进入阻塞状态；可能刚好线程调度器马上又把它调度出来了**
* **sleep和yield的区别：**
  * sleep方法声明抛出InterruptedException，调用该方法需要捕获该异常。yield没有声明异常，也无需捕获
  * sleep方法暂停当前线程后，会进入阻塞状态，只有当睡眠时间到了，才会转入就绪状态。而yield方法调用后 ，是直接进入就绪状态。



## 线程合并join

* 原因：临时加入线程执行
* 方法：**Thread.join()**
* 描述：**当B线程执行到A线程的join()方法时，B线程会等待，等A线程执行完，B线程再执行**



## 停止线程

* 方法：**通过循环控制**（原stop()方法有缺陷，已停用）
* **特殊情况：**当线程处于了冻结状态，就不会读取到标记，也就不会结束。当没有指定方法让冻结的线程回复到运行状态时，我们需要对冻结状态进行清除，也就是强制让线程恢复到运行状态中来，这样可就可以操作标记让线程结束
* Thread类提供该方法： interrupt（）；（如果线程在调用Object类的wait（）、wait（long）、wait（long，int）方法，或者该类的join（）、join（long）、join（long、int）、sleep（long）或sleep（long、int）方法过程中受阻，则其中断状态将被清除，还将收到一个InterruptedException。）



## 优先级

* 描述：**每个线程都有优先级，优先级表示执行概率**
* Thread类提供三个优先级常量：
  * MAX_PRIORITY=10; NORM_PRIORITY=5; MINPRIORITY=1;