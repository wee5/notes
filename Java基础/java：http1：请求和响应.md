# java：Http1：请求和响应



参考：https://blog.csdn.net/qq_41331645/article/details/81186377

## 请求对象HttpServletRequest

**请求行**包括：传输方式（get或post） 请求的地址（url） 协议的版本

**请求头**中只有：只有键值对形式存在的参数

**请求实体**：post方式值是在请求实体中，get方式表单中的值是拼接在url后

HttpServletRequest的本质上就是HTTP协议的请求所封装的

### 方法

解决请求中的乱码问题:

```java
    request.setCharacterEncoding	//(当前项目的编码集)
```

获取请求行中的内容

```java
    Request.getMethod();   //获取请求方式
    Request.getRequestURI(); //获取地址栏中？之前端口之后
    Request.getRequestURL(); //获取？之前所有，返回StringBuffer
    Request.getScheme(); //获取协议
    Request.getContextPath(); //获取根目录
    Request.getQueryString(); //获取？之后
```

获取请求头中的内容：

```java
    Request.getHeader(键);  //键是不区分大小写的
```

获取网络信息：

```java
    request.getRemoteAddr();   //获取客户端ip地址 
    Request.getRemotePort();   //获取客户端的端口
    Request.getLocalAddr();   //获取服务器IP地址
    Request.getLocalPort();   //获取服务器端口号
```

获取表单参数：

​	首先获取单键单值：

```java
    	Request.getParameter(键);    //返回String,需要注意的是如果没有键返回的是null,有键而没有值返回空字符串；
```

​	获取同键不同值（主要针对复选框）：

```java
		request.getParameterValues(键);    //返回一个String[]
```

获取有键的集合：

```java
    Enumeration enum = Request.getParameterNames()   //返回的是Enumeration,也就是一个容器
    While(enum.hasMoreElements()){
        enum.nextElement();
    }
```



## 响应对象HttpServletResponse

**响应行**：包含协议版本，状态码，还有状态描述

**响应头**：也是键值对的形式存在的

**响应实体**：字符串或者是流信息

### 方法

设置响应的编码集:

```java
    Response.setHeader("content-type","text/html;charset=服务器编码集"）	//编码设置放在首位
```

设置响应头：

```java
     response.setHeader(key,value)   //忽略大小写
```

设置刷新，refresh

​	可以单独写秒数，代表的是多少秒后刷新本页面

​	也可以写秒数；url="地址"  代表的是多少秒后刷新并跳转到指定地址

设置响应字体：

​	输出字符串：

```java
		Response.getWriter()	//得到的是PrintWirter
```

​	输出流信息：

```java
 		Response.getOutputStream()	//返回的是ServletOutputStream,可以直接用OutputStream来接，剩下的就全是普通的流信息了
```



## 请求方法

**GET**：参数会拼接在url后，长度有限制

**POST**：参数会封装在请求中，长度无限制