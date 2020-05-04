# 日志1：intro



* 常见日志框架：
  * jul：java.util.logging；java原生日志框架，最简单
  * log4j：Apache的开源项目，最经典
  * logback：与log4j出同一人之手；性能最好
    * logback-core：是其他两个模块的基础模块
    * logback-classic：是log4j的改良版本，logback-classic完整实现SLF4J API使你可以很方便地更换成其它日记系统如Log4j或j.u.l
    * logback-access：访问模块与Servlet容器集成提供通过Http来访问日记的功能



## 日志门面

* **门面模式**（Facade Pattern），也称之为外观模式，其核心为：外部与一个子系统的通信必须通过一个统一的外观对象进行，使得子系统更易于使用
  * 对于应用程序来说，无论底层的日志框架如何变，都不需要有任何感知。只要门面服务做的足够好，随意换另外一个日志框架，应用程序不需要修改任意一行代码，就可以直接上线
  * ![avatar](图片引用\20181128151611259.png)
* 日志门面框架
  * **slf4j**：simple logging facade for java
  * **commons-logging**：jcl



## slf4j

* **静态绑定（中间包）**：引入适配层的jar包slf4j-log412.jar及底层日志框架实现log4j.jar。简单的说适配层做的事情就是把slf4j的api转化成log4j的api。通过这样的方式来屏蔽底层框架实现细节
  * ![avatar](图片引用\Snipaste_2020-05-01_19-44-01.png)
* **桥接**：**对某套API的伪实现；调用有类似功能的别的API，而不是直接去完成API所声明的功能。这样就完成了从“某套API”到“别的API”的转调；**有日志门面API转调到具体日志框架API的桥接器，也存在将日志框架API转调到日志门面API的桥接器；比如你的application中使用了slf4j，并绑定了logback；但是项目中引入了一个A.jar，A.jar使用的日志框架是log4j；你只需要引入log4j-over-slf4j.jar，并删除log4j.jar就可以实现slf4j对A.jar中log4j的接管
  * 分类
    * 门面桥接包：jcl-over-slf4j.jar
    * 实现桥接包：log4j-over-slf4j.jar；jul-to-slf4j.jar
  * 应用场景：
    * 主系统中引入子系统，两系统中门面相同，实现不同；引入实现桥接包，并删除子系统的实现包
    * 主系统中引入子系统，两系统中门面不同，实现相同；直接引入门面桥接包
  * ![avatar](图片引用\Snipaste_2020-05-02_10-03-47.png)



* 推荐：
  * https://blog.csdn.net/jpf254/article/details/80757041 日志-slf4j桥接原理分析
  * https://blog.csdn.net/s332755645/article/details/73992860 jcl-over-slf4j slf4j-log4j12等log工具作用