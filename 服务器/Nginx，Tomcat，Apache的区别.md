# Nginx，Tomcat，Apache的区别

* Nginx和Tomcat区别：
  * Nginx常作**静态内容服务器**和**代理服务器**，直接外来请求转发给后面的应用服务器
  * Tomcat常作**应用服务器**
  * 严格讲，Nginx和Apache是**HTTP Server**。Tomcat是**Application Server**，是Server/JSO应用容器
* HTTP Server和Application Server区别
  * 客户端通过HTTP Server访问服务器上存储的资源（html，图片等），通过HTTP协议传输给客户端
  * Application Server往往运行在HTTP Server背后，执行应用，将动态内容转化为静态内容后，通过HTTP Server分发给客户端
  * Nginx只做请求分发，不做处理
* Nginx和Apache区别
  * Apache是同步多进程模型，一个连接对应一个进程
  * Nginx多个连接（万级别）对应一个进程
  * Apache超稳定，处理动态请求有优势
  * Nginx轻量级，抗并发，处理静态文件好
  * 建议使用前端Nginx抗并发，后端Apache集群，配合使用

# HTTP和FTP的区别

- HTTP：Hyper Text Transfer Protocol，超文本传输协议，面向网页
- FTP：File Transfer Protocol，文件传输协议，面向文件

