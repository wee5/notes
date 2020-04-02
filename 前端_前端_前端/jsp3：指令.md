# jsp三：指令

* **作用：**用来设置整个jsp页面相关属性，如网页的编码方式和脚本语言
* **语法：**<%@ directive attribute="value" %>
* 三种指令：以**键值对**形式存在，逗号隔开![avatar](图片引入\QQ截图20200227132723.png)

## page指令

* Page指令为容器提供当前页面的使用说明。一个JSP页面可以包含多个page指令
* **语法：**<%@ attribute="value" %>
* **等价XML格式：**《jsp:directive.page attribute="value" /》
* **相关属性：**![avatar](图片引入\QQ截图20200227134540.png)

## include指令

* JSP可以通过include指令来包含其他文件。被包含的文件可以是**JSP文件、HTML文件或文本文件**。包含的文件就好像是该JSP文件的一部分，会被同时编译执行。
* **语法：**<%@ include file="文件相对 url 地址" %>
* **等价XML语法：**《jsp:directive.include file="文件相对 url 地址" /》

## taglib指令

* JSP API允许用户自定义标签，一个自定义标签库就是自定义标签的集合。

  Taglib指令引入一个自定义标签集合的定义，包括库路径、自定义标签。

* **语法：**<%@ taglib uri="uri" prefix="prefixOfTag" %>

* **等价XML语法：**《jsp:directive.taglib uri="uri" prefix="prefixOfTag" /》