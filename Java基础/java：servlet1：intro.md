# java：servlet1：intro



* Servlet是SUN公司提供的一门用于**开发动态WEB资源的技术**;SUN公司在其API中提供了一个Servlet接口，用户若想开发一个动态WEB资源(即开发一个Java程序向浏览器输出数据)，需要完成以下2个步骤：
  - 编写一个Java类，实现Servlet接口；
  - 把开发好的Java类部署到WEB服务器中

* ```xml
  <!-- Servlet3.1之后版本 -->
  <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
  </dependency>
  <!-- Servlet3.1之前版本 -->
  <dependency>
      <groupId>org.mortbay.jetty</groupId>
      <artifactId>servlet-api</artifactId>
      <version>3.0.20100224</version>
  </dependency>
  ```



# Servlet的运行过程

- **Servlet程序是由Web服务器调用**，Web服务器收到客户端的Servlet访问请求后：
  - ①Web服务器首先检查是否已经装载并创建了该Servlet的实例对象。如果是，则直接执行第④步，否则，执行第②步；
  - ②装载并创建该Servlet的一个实例对象；
  - ③调用Servlet实例对象的init()方法；
  - ④创建一个用于封装HTTP请求消息的HttpServletRequest对象和一个代表HTTP响应消息的HttpServletResponse对象，然后调用Servlet的service()方法并将请求和响应对象作为参数传递进去；
  - ⑤Web应用程序被停止或重新启动之前，Servlet引擎将卸载Servlet，并在卸载之前调用Servlet的destroy()方法
- 图解
  - ![avatar](图片引用\20190417153309.png)
  - 注意：上图并没画出destory()方法。destory()方法会在Web容器移除Servlet时执行，客户机第一次访问服务器时，服务器会创建Servlet实例对象，它就永远驻留在内存里面了，等待客户机第二次访问，这时有一个用户访问完Servlet之后，此Servlet对象并不会被摧毁，destory()方法也就不会被执行。



















* 参考：https://www.cnblogs.com/rolandlee/p/10756573.html