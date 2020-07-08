# java：监听器6：jdk监听器实现



通过注解方式实现的jdk原生监听器



## 前言

本文不包含自定义的事件

jdk中包含的web监听器，对事件和事件源的编码是预先编写好的，所以只需要自己编写监听器的响应动作

即只需要操作监听器即可，无需操作事件和事件源



## 启动类

```java
@SpringBootApplication
@ServletComponentScan//用注解实现的监听器，需要此注解，后面不赘述
public class ListenerTestApplication {

    public static void main(String[] args) {
        SpringApplication.run(ListenerTestApplication.class, args);
    }

}
```



## ServletContextListener监听器

```java
@WebListener
public class CustomServletContextListener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        //解析参数sce（与响应行为无关，仅仅展示ServletContextEvent的相关用法）
        ServletContext servletContext = sce.getServletContext();//获取事件源，和下面方法获得对象一致
        Object source = sce.getSource();//获取事件源
        System.out.println(servletContext.equals(source));//结果是true

        //开启定时任务（响应行为）
        /*new CustomTimeTask().startTimeTask();*/

    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println("ServletContext销毁了");
    }
}
```



## HttpSessionListener监听器

```java
@WebListener
public class CustomHttpSessionListener implements HttpSessionListener {
    @Override
    public void sessionCreated(HttpSessionEvent arg0) {
        //获得Session对象的方法arg0.getSession()
        String id = arg0.getSession().getId();
        System.out.println("session创建"+id);

    }
    @Override
    public void sessionDestroyed(HttpSessionEvent arg0) {
        System.out.println("session销毁");

    }
}
```



## ServletRequestListener监听器

```java
@WebListener
public class CustomServletRequestListener implements ServletRequestListener {

    @Override
    public void requestInitialized(ServletRequestEvent sre) {
        System.out.println("监听到ServletRequest创建");
    }

    @Override
    public void requestDestroyed(ServletRequestEvent sre) {
        System.out.println("监听到ServletRequest销毁");
    }

}
```



## ServletContextAttributeListener监听器

```java
/*
* ServletContextAttributeListener:Servlet上下文属性监听器
* */
@WebListener
public class CustomServletContextAttributeListener implements ServletContextAttributeListener {
    @Override
    public void attributeAdded(ServletContextAttributeEvent scae) {
        System.out.println("监听到ServletContext属性添加");
        scae.getName();//获得域中被添加属性的name
        scae.getValue();//获得域中被添加属性的value
    }

    @Override
    public void attributeRemoved(ServletContextAttributeEvent scae) {
        System.out.println("监听到ServletContext属性移除");
        scae.getName();//获得域中被移除属性的name
        scae.getValue();//获得域中被移除属性（被移除前）的value
    }

    @Override
    public void attributeReplaced(ServletContextAttributeEvent scae) {
        System.out.println("监听到ServletContext属性修改");
        scae.getName();//获得域中被修改属性的name
        scae.getValue();//获得域中被修改属性（被修改前）的value
    }
}
```



## HttpSessionAttributeListener监听器

```java
@WebListener
public class CustomHttpSessionAttributeListener implements HttpSessionAttributeListener {
    @Override
    public void attributeAdded(HttpSessionBindingEvent se) {

    }

    @Override
    public void attributeRemoved(HttpSessionBindingEvent se) {

    }

    @Override
    public void attributeReplaced(HttpSessionBindingEvent se) {

    }
}
```



## HttpSessionBindingListener监听器

```java
public class User implements HttpSessionBindingListener {
    private String name;
    private int age;

    @Override
    public void valueBound(HttpSessionBindingEvent event) {
        System.out.println("监听到对象User被绑定到Session中");
    }

    @Override
    public void valueUnbound(HttpSessionBindingEvent event) {
        System.out.println("监听到对象User在Session中被移除");
    }

    /* 省略构造函数和get/set方法 */
}
```



## ServletRequestAttributeListener监听器

```java
@WebListener
public class CustomServletRequestAttributeListener implements ServletRequestAttributeListener {
    @Override
    public void attributeAdded(ServletRequestAttributeEvent srae) {

    }

    @Override
    public void attributeRemoved(ServletRequestAttributeEvent srae) {

    }

    @Override
    public void attributeReplaced(ServletRequestAttributeEvent srae) {

    }
}
```



## HttpSessionActivationListener监听器

```java
@WebListener
public class UserSerializable implements HttpSessionActivationListener, Serializable {
    private String name;
    private int age;

    @Override
    public void sessionWillPassivate(HttpSessionEvent se) {
        System.out.println("监听到UserSerializable对象被钝化");
    }

    @Override
    public void sessionDidActivate(HttpSessionEvent se) {
        System.out.println("监听到UserSerializable对象被活化");
    }

    /* 省略构造函数和get/set方法 */
}
```

