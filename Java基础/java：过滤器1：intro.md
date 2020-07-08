# java：过滤器1：intro



## 生命周期

Filter的创建
 　　Filter的创建和销毁由web服务器负责。 web应用程序启动时，web服务器将创建Filter的实例对象，并调用其init方法，完成对象的初始化功能，从而为后续的用户请求作好拦截的准备工作，filter对象只会创建一次，init方法也只会执行一次。通过init方法的参数，可获得代表当前filter配置信息的FilterConfig对象

初始化 init

​	完成对象的初始化功能，从而为后续的用户请求作好拦截的准备工作，init方法也只会执行一次。通过init方法的参数，可获得代表当前filter配置信息的FilterConfig对象

doFilter

​	这个方法完成实际的过滤操作，当客户请求访问于过滤器关联的URL时，Servlet容器将先调用过滤器的doFilter方法。FilterChain参数用于访问后续过滤器

Filter的销毁 destroy
 　　web容器调用destroy方法销毁Filter。destroy方法在Filter的生命周期中仅执行一次。在destroy方法中，可以释放过滤器使用的资源