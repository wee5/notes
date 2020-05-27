# java：监听器4：spring事件



* spring中的事件，由spring事先编写成类，这些类可以由spring识别解析
* 或者说，spring事件是基于jdk事件封装的，说到底还是由jdk解析



## spring常见事件

* spring提供以下五种标准事件
  * 上下文**更新**事件（**ContextRefreshedEvent**）：该事件会在ApplicationContext被初始化或者更新时发布。也可以在调用ConfigurableApplicationContext 接口中的refresh()方法时被触发
  * 上下文**开始**事件（**ContextStartedEvent**）：当容器调用ConfigurableApplicationContext的Start()方法开始/重新开始容器时触发该事件
  * 上下文**停止**事件（**ContextStoppedEvent**）：当容器调用ConfigurableApplicationContext的Stop()方法停止容器时触发该事件
  * 上下文**关闭**事件（**ContextClosedEvent**）：当ApplicationContext被关闭时触发该事件。容器被关闭时，其管理的所有单例Bean都被销毁
  * **请求处理**事件（**RequestHandledEvent**）：在Web应用中，当一个http请求（request）结束触发该事件