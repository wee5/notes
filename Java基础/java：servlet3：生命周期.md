# java：servlet3：生命周期



* 加载和实例化
* 初始化
* 请求处理
* 服务终止
* Servlet是一个供其他Java程序（Servlet引擎）调用的Java类，它不能独立运行，它的运行完全由**Servlet引擎**来控制和调度。针对客户端的多次Servlet请求，通常情况下，服务器只会创建一个Servlet实例对象，也就是说Servlet实例对象一旦创建，它就会驻留在内存中，为后续的其它请求服务，直至Web容器退出，Servlet实例对象才会销毁
* 客户端的多次Servlet请求，通常情况下，服务器只会创建一个Servlet实例对象
* 当Web服务器停止后或者Web应用从服务器里删除时，destroy()方法就会被执行