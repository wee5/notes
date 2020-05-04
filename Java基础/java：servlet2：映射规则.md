# java：servlet2：映射规则



## 前言：**部署多个servlet**

* 在java路径下编写的servlet（需要**继承xxxServlet接口**），在部署时会自动部署到webapp路径下

* 可以同时编写多个servlet，再对其进行**注册和映射**

  * ![avatar](图片引用\Snipaste_2020-04-26_15-15-27.png)

  * ```java
    public class HovServlet extends HttpServlet {
        @Override
        protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            this.doPost(req, resp);
        }
    
        @Override
        protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            ServletOutputStream outputStream = resp.getOutputStream();
            outputStream.write("hi hov! its your servlet!".getBytes());
        }
    }
    
    public class WeeServlet extends GenericServlet {
        @Override
        public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
            ServletOutputStream outputStream = res.getOutputStream();
            outputStream.write("hello wee! its your servlet!".getBytes());
        }
    }
    ```



## **web.xml**

#### 映射规则

* 映射方式

  * 具体映射路径：如"myservlet"
  * 通配符映射：（两种固定格式）
    * *.扩展名（根路径下的某扩展名的请求进行处理）
    * /开头，*结尾（对根路径或其下路径的请求进行处理）

* **servlet映射规则是解决请求由哪个servlet处理，而不是为每一个请求做映射**

  * 对于如下的一些映射关系：

    - Servlet1映射到`/abc/*`；
    - Servlet2映射到`/*`；
    - Servlet3映射到`/abc`；
    - Servlet4映射到`*.do`。

    有如下问题：

    - 当请求URL为“`/abc/a.html`”，“`/abc/*`”和“`/*`”都匹配，哪个Servlet响应？——Servlet引擎将调用Servlet1；
    - 当请求URL为“`/abc`”时，“`/abc/*`”、“`/*`”和“`/abc`”都匹配，哪个Servlet响应？——Servlet引擎将调用Servlet3；
    - 当请求URL为“`/abc/a.do`”时，“`/abc/*`”、“`/*`”和“`*.do`”都匹配，哪个Servlet响应？——Servlet引擎将调用Servlet1；
    - 当请求URL为“`/a.do`”时，“`/*`”和“`*.do`”都匹配，哪个Servlet响应？——Servlet引擎将调用Servlet2；
    - 当请求URL为“`/xxx/yyy/a.do`”时，“`/*`”和“`*.do`”都匹配，哪个Servlet响应？——Servlet引擎将调用Servlet2。

* 总结：**匹配的原则就是"谁长得更像就找谁"，“\*.do”——这种\*在前面的时候优先级最低**

* 自动装载：web应用启动时，自动转载和实例化Servlet对象，及调用Servlet对象的init方法

  * 必须为正整数，数字越小，Servlet越优先创建
  * 可为Web应用写一个InitServlet，这个Servlet配置为启动时装载，为整个Web应用创建必要的数据库表和数据

* **缺省servlet**

  - 当匹配不到映射的url时，请求将交给缺省Servlet处理；即缺省Servlet用于处理所有其他Servlet都不处理的访问请求
  - **当访问Tomcat服务器中的某个静态HTML文件和图片时（不包含jsp），实际上是在访问这个缺省Servlet(服务器中的html文件数据的读取由缺省Servlet完成**

* ```xml
  <servlet>
      <servlet-name>WeeServlet</servlet-name>	<!-- 注册tomcat -->
      <servlet-class>com.company.servlet.WeeServlet</servlet-class>
      <load-on-startup>1</load-on-startup>	<!--自动装载;web应用启动时，自动转载和实例化Servlet对象，及调用Servlet对象的init方法-->
  </servlet>
  <servlet-mapping>	<!-- 映射url路径 -->
      <servlet-name>WeeServlet</servlet-name>
      <url-pattern>/wee</url-pattern>
  </servlet-mapping>
  
  <servlet>
      <servlet-name>HovServlet</servlet-name>
      <servlet-class>com.company.servlet.HovServlet</servlet-class>
  </servlet>
  <servlet-mapping>
      <servlet-name>HovServlet</servlet-name>
      <url-pattern>/hov</url-pattern>
  </servlet-mapping>
  ```



## **继承xxxServlet的意义**

* 若不继承，直接当继承了的用，在注册时会报错
  * ![avatar](图片引用\Snipaste_2020-04-26_14-28-18.png)
  * 报错：该类不是javax.servlet.Servlet类型，即没有继承xxxServlet接口
* 意义：**体现面向接口编程，自己编写的类不会在servlet相关包中存在引用，只会对自己包中的类由引用，所以利用多态，对接口类实现扩展，即类型不变，方法体自定义**

