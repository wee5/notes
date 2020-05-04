# java：servlet4：线程安全



## 前言

* **考虑线程安全时，看是否操作同一个对象，资源等**



# 当Servlet存在线程安全问题时

* ```java
  package cn.liayun;
  
  import java.io.IOException;
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  
  //存在线程安全问题的代码
  @WebServlet("/ServletSample")
  public class ServletSample extends HttpServlet {
  	
  	private int i = 0;
  
  	protected void doGet(HttpServletRequest request, HttpServletResponse response)
  			throws ServletException, IOException {
  		i++;
  
  		try {
  			Thread.sleep(1000 * 10);
  		} catch (InterruptedException e) {
  			// TODO Auto-generated catch block
  			e.printStackTrace();
  		}
  		
  		response.getOutputStream().write((i + "").getBytes());
  	}
  
  	protected void doPost(HttpServletRequest request, HttpServletResponse response)
  			throws ServletException, IOException {
  		doGet(request, response);
  	}
  
  }
  ```

* 把i定义成全局变量，当多个线程并发访问变量i时，就会存在线程安全问题了。线程安全问题只存在多个线程并发操作同一个资源的情况下，所以在编写Servlet的时候，如果并发访问某一个资源(变量，集合等)，就会存在线程安全问题，那么该如何解决这个问题呢？可使用同步代码块

* ```java
  package cn.liayun;
  
  import java.io.IOException;
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  
  //改善后的代码
  @WebServlet("/ServletSample")
  public class ServletSample extends HttpServlet {
  	
  	private int i = 0;//共享资源
  
  	protected void doGet(HttpServletRequest request, HttpServletResponse response)
  			throws ServletException, IOException {
  		i++;
  
  		synchronized (this) {
  			try {
  				Thread.sleep(1000 * 10);
  			} catch (InterruptedException e) {
  				// TODO Auto-generated catch block
  				e.printStackTrace();
  			}
  		}
  		
  		response.getOutputStream().write((i + "").getBytes());
  	}
  
  	protected void doPost(HttpServletRequest request, HttpServletResponse response)
  			throws ServletException, IOException {
  		doGet(request, response);
  	}
  
  }
  ```

* 加了synchronized后，并发访问i时就不存在线程安全问题了，为什么加了synchronized后就没有线程安全问题了呢？原因：假如现在有一个线程访问Servlet对象，那么它就先拿到了Servlet对象的那把锁，等到它执行完之后才会把锁还给Servlet对象，由于是它先拿到了Servlet对象的那把锁，所以当有别的线程来访问这个Servlet对象时，由于锁已经被之前的线程拿走了，后面的线程只能排队等候了。
  以上这种做法是给Servlet对象加了一把锁，保证任何时候都只有一个线程在访问该Servlet对象里面的资源，这样就不存在线程安全问题了。这种做法虽然解决了线程安全问题，但是编写Servlet却万万不能用这种方式处理线程安全问题，假如有9999个人同时访问这个Servlet，那么这9999个人必须按先后顺序排队轮流访问。
  针对Servlet的线程安全问题，SUN公司是提供有解决方案的：让Servlet去实现一个SingleThreadModel接口，如果某个Servlet实现了SingleThreadModel接口，那么Servlet引擎将以单线程模式来调用其service方法。查看Sevlet的API可以看到，SingleThreadModel接口中没有定义任何方法和常量，在Java中，把没有定义任何方法和常量的接口称之为标记接口，经常看到的一个最典型的标记接口就是"Serializable"，这个接口也是没有定义任何方法和常量的，标记接口在Java中有什么用呢？主要作用就是给某个对象打上一个标志，告诉JVM，这个对象可以做什么，比如实现了"Serializable"接口的类的对象就可以被序列化，还有一个"Cloneable"接口，这个也是一个标记接口，在默认情况下，Java中的对象是不允许被克隆的，就像现实生活中的人一样，不允许克隆，但是只要实现了"Cloneable"接口，那么对象就可以被克隆了。SingleThreadModel接口中没有定义任何方法，只要在Servlet类的定义中增加实现SingleThreadModel接口的声明即可。
  对于实现了SingleThreadModel接口的Servlet，Servlet引擎仍然支持对该Servlet的多线程并发访问，其采用的方式是产生多个Servlet实例对象，并发的每个线程分别调用一个独立的Servlet实例对象。实现SingleThreadModel接口并不能真正解决Servlet的线程安全问题，因为Servlet引擎会创建多个Servlet实例对象，而真正意义上解决多线程安全问题是指一个Servlet实例对象被多个线程同时调用的问题。事实上，在Servlet API 2.4中，已经将SingleThreadModel标记为Deprecated（过时的）

* ![avatar](图片引用\20190417154750.png)

* 以上代码还要注意异常的处理，代码`Thread.sleep(1000*4)`;只能try不能抛，因为子类在覆盖父类的方法时，不能抛出比父类更多的异常；并且catch之后，后台记录异常的同时并给用户一个友好提示，因为用户访问的是一个网页