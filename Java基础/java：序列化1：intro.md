# java：序列化1：intro



## 概念

* **序列化：对象序列化是指将Java对象（动态的状态，如变量、函数）转换为字节流的过程，可以将其保存到磁盘文件中或通过网络发送到任何其他程序**
* **反序列化：**从字节流重构出Java对象的过程
* 序列化得到的字节流是与平台无关的，在一个平台上序列化的对象可以在不同的平台上反序列化



## 作用

* **对象持久化**
  * 我们知道，对象随着程序的运行而被创建，然后在不可达时被回收，生命周期是短暂的。但是如果我们想长久地把对象的内容保存起来怎么办呢？把它转化为字节序列保存在存储介质上即可。那就需要序列化
* **网络传输对象**
  * 我们知道，两个进程之间通信时，传递的音频、视频等信息是以二进制序列形式来传输的。那么，对象也可以吗？可以，通过序列化把主机A进程上的对象序列化为二进制序列，传输到主机B上的进程从序列中重构出该对象。这在RMI中应用广泛，RMI的结果可以是一个对象
* **进程间传递对象**



## 如何序列化

* 默认序列化方式

  * 定义类时实现Serializable接口即可，这个Serializable接口是一个空接口，没有需要实现的方法
  * 作用是标记该类的对象可以被序列化，启用其序列化功能。通过调用 ObjectOutputStream和ObjectInputStream的方法来即可对该对象进行序列化和反序列化

* 实现**Serializable接口**

  * 定义类时，实现Serializable接口，并在类中重写两个序列化与反序列化接口，在其中定义对象序列化与反序列化动作

  * ```java
    private void writeObject(java.io.ObjectOutputStream out)
         throws IOException
    
    private void readObject(java.io.ObjectInputStream in)
         throws IOException, ClassNotFoundException;
    ```

* 实现**Externalnalizable接口**

  * 在类中实现readExternal(ObjectInput in)和writeExternal(ObjectOutput out)方法，在方法中定义类对象自定义的序列化和反序列化操作
  * 这样通过对象输出流和对象输入流的输入输出方法序列化和反序列化对象时会自动调用类中定义的**readExternal(ObjectInput in)和writeExternal(ObjectOutput out)**方法

#### 序列化与反序列化API与使用过程

* 还没有记录完，参考下面





* 参考：https://www.cnblogs.com/ygj0930/p/10857597.html