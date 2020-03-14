# jsp一：INTRO（java serve pages）

* **动态生成html，xml**，或其他格式文档的web网页技术标准；
* 同php，asp，asp.net相似，运行在**服务端的语言**；
* jsp开发的web应用可以**跨平台使用**，linux， windows都可以运行；

## jsp处理过程

* JSP的执行过程
  * 客户端发出请求。
  * Web容器将JSP转译成Servlet源代码。
  * Web容器将产生的源代码进行编译。
  * Web容器加载编译后的代码并执行。
  * 把执行结果响应至客户端。
* 过程介绍
  - 客户端发出请求，请求为JSP，web容器就会找出相应的servlet进行处理。
  - 将servlet转成字节码文件。
  - 将字节码文件加载到web容器里。
  - 这时会在web容器里建立实例。
  - 进行初始化。
  - 通过service接受请求。
  - 然后web容器会自动产生两个对象servlet和service最后进行销毁。

## jsp的生命周期

* **编译：**
  * servlet容器编译servlet源文件，生成servlet类
* **初始化：**
  * 加载与jsp对应的servlet类，创建其实例，并调用它的初始化方法
* **执行：**
  * 调用与jsp对应的servlet实例的服务方法
* **销毁：**
  * 调用与jsp对应的servlet实例的销毁方法，然后销毁servlet实例

* **详细过程：**
  * **编译：**当浏览器请求JSP页面时，JSP引擎会首先去检查是否需要编译这个文件。如果这个文件没有被编译过，或者在上次编译后被更改过，则编译这个JSP文件。
    * 解析jsp文件
    * 将jsp文件转为servlet
    * 编译servlet
  * **初始化：**
    * 容器载入JSP文件后，它会在为请求提供任何服务前调用jspInit()方法；如果您需要执行自定义的JSP初始化任务，复写jspInit()方法就行了
  * **执行：**
    * 这一阶段描述了jsp生命周期中一切与请求相关的交互行为，直至被销毁；当jsp网页完成初始化后，jsp引擎将会调用jspServlet()方法；jspService()方法需要一个HttpServletRequest对象和一个HttpServletResponse对象作为它的参数
  * **jsp清理：**
    * jsp生命周期的销毁阶段描述了当一个sp网页从容器中被移除时所发生的一切；jspDestroy()方法在jsp中等价于servlet中的销毁方法，当需要执行任何清理工作时复写jspDestroy()方法，比如是方法数据库连接或者关闭文件夹等；



