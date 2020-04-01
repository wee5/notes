# java：多线程2：创建线程



## 继承Thread类

* 定义类继承Thread
* 复写Thread类中的run方法
* 调用线程的start方法
  * 两步：启动线程；调用run方法



## 实现runable接口

* **接口应该由哪些打算通过某一线程执行其实例的类来实现，类必须定义一个成为run的无参方法**
* 定义类实现Runable接口
* 覆盖Runable接口中的run方法；将线程要运行的代码放在该run方法中
* 通过Thread类建立线程对象
* 将Runable接口的子类对象作为实际参数传递给Thread类的构造方法；自定义的run方法所属的对象是Runable接口的子类对象，所以要让线程执行指定对象的run方法就要先明确run方法所属的对象
* 调用Thread类的start方法开启线程，并调用Runable接口子类的run方法



## 通过Callable和Futrue创建线程

* 定义类实现Callable接口，并实现call方法，该方法将作为线程执行体，且具有返回值
* 创建Callable实现类的实例，使用FutrueTask类进行包装Callable对象，FutrueTask对象封装类Callable对象的call()方法的返回值
* 使用FutrueTask对象作为Thread对象启动新线程
* 调用FutrueTask对象的get()方法获取子线程执行结束后的返回值



## 三种方法比较

* **继承Thread**：
  * 优势：编写简单，可直接用this.getname（）获取当前线程，不必使用Thread.currentThread（）方法
  * 劣势：继承类Thread类，无法继承其他类
* **实现Runable**：
  * 优势：避免单继承的局限，多个线程可以共享一个target对象，适合多线程处理同一份资源的情形（**多线程优势：进程不可以共享数据，线程可以**）
  * 劣势：编写复杂，访问线程必须使用Thread.currentThread（）方法、无返回值
* **实现Callable**：
  * 优势：有返回值、避免了单继承的局限性、多个线程可以共享一个target对象，非常适合多线程处理同一份资源的情形
  * 劣势：比较复杂、访问线程必须使用Thread.currentThread（）方法



参考：https://www.cnblogs.com/yjboke/p/8919090.html