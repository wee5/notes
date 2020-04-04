# java：多线程2：创建线程



## 继承Thread类

* 定义类继承Thread

* 复写Thread类中的run方法

* 调用线程的start方法

  * 两步：启动线程；调用run方法

* public class ThreadDemo1 {

      public static void main(String[] args) {
          
          //创建两个线程
          ThreadDemo td = new ThreadDemo("zhangsan");
          ThreadDemo tt = new ThreadDemo("lisi");
          //执行多线程特有方法，如果使用td.run();也会执行，但会以单线程方式执行。
          td.start();
          tt.start();
          //主线程
          for (int i = 0; i < 5; i++) {
              System.out.println("main" + ":run" + i);
          }
      }
  }
  //继承Thread类
  class ThreadDemo extends Thread{
      
      //设置线程名称
      ThreadDemo(String name){
          super(name);
      }
      //重写run方法。
      public void run(){
          for(int i = 0; i < 5; i++){
          System.out.println(this.getName() + "：run" + i);　　//currentThread()  获取当前线程对象（静态）。  getName（） 获取线程名称。
          }
      }
  }



## 实现runable接口

* **接口应该由哪些打算通过某一线程执行其实例的类来实现，类必须定义一个成为run的无参方法**

* 定义类实现Runable接口

* 覆盖Runable接口中的run方法；将线程要运行的代码放在该run方法中

* 通过Thread类建立线程对象

* 将Runable接口的子类对象作为实际参数传递给Thread类的构造方法；自定义的run方法所属的对象是Runable接口的子类对象，所以要让线程执行指定对象的run方法就要先明确run方法所属的对象

* 调用Thread类的start方法开启线程，并调用Runable接口子类的run方法

* ```
  public class RunnableDemo {
      public static void main(String[] args) {
          RunTest rt = new RunTest();
          //建立线程对象
          Thread t1 = new Thread(rt);
          Thread t2 = new Thread(rt);
          //开启线程并调用run方法。
          t1.start();
          t2.start();
      }
  }
  
  //定义类实现Runnable接口
  class RunTest implements Runnable{
      private int tick = 10;
      //覆盖Runnable接口中的run方法,并将线程要运行的代码放在该run方法中。
      public void run(){
          while (true) {
              if(tick > 0){
                  System.out.println(Thread.currentThread().getName() + "..." + tick--);
              }
          }
      }
  }
  ```



## 通过Callable和Futrue创建线程

* 定义类实现Callable接口，并实现call方法，该方法将作为线程执行体，且具有返回值
* 创建Callable实现类的实例，使用FutrueTask类进行包装Callable对象，FutrueTask对象封装类Callable对象的call()方法的返回值
* 使用FutrueTask对象作为Thread对象启动新线程
* 调用FutrueTask对象的get()方法获取子线程执行结束后的返回值
* public class CallableFutrueTest {
      public static void main(String[] args) {
          CallableTest ct = new CallableTest();                        //创建对象
          FutureTask<Integer> ft = new FutureTask<Integer>(ct);        //使用FutureTask包装CallableTest对象
          for(int i = 0; i < 100; i++){
              //输出主线程
              System.out.println(Thread.currentThread().getName() + "主线程的i为：" + i);
              //当主线程执行第30次之后开启子线程
              if(i == 30){        
                  Thread td = new Thread(ft,"子线程");
                  td.start();
              }
          }
          //获取并输出子线程call()方法的返回值
          try {
              System.out.println("子线程的返回值为" + ft.get());
          } catch (InterruptedException e) {
              e.printStackTrace();
          } catch (ExecutionException e) {
              e.printStackTrace();
          }
      }
  }
  class CallableTest implements Callable<Integer>{
      //复写call() 方法，call()方法具有返回值
      public Integer call() throws Exception {
          int i = 0;
          for( ; i<100; i++){
              System.out.println(Thread.currentThread().getName() + "的变量值为：" + i);
          }
          return i;
      }
  }



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