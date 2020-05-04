# jsp二：语法

* **相似语法**
  * jsp语法：<% 代码片段 %>
  * jsp声明：<%! declaration %>
  * jsp表达式：<%= 表达式 %>

## 脚本程序

* 脚本程序可以包含任意量的**Java语句、变量、方法或表达式**，只要它们在脚本语言中是有效的
* **java语句：**<%代码片段%>
* **其等价的XML语句：**《jsp:scriptlet》代码片段《/jsp:scriptlet》
* **中文编码问题：**<%@ page language="java" contentType="text/html;  charset="UTF-8"  pageEncoding="UTF-8"%>

## jsp声明

* 一个声明语句可以声明一个或多个变量，方法，供后面的java代码使用。**先声明后使用**

* **jsp声明语法：**<%! declaration;  []  declaration; ]+ ...%>

* **其等价的XML语句：**《jsp:declaration》代码片段《/jspdeclaration》

* 演示：

  ```
  <%! int i = 0; %> 
  <%! int a, b, c; %> 
  <%! Circle a = new Circle(2.0); %> 
  ```

## jsp表达式

* 一个JSP表达式中包含的脚本语言表达式，先被**转化成String**，然后插入到表达式出现的地方
* 由于表达式的值会被转化成String，所以您可以在一个文本行中使用表达式而不用去管它是否是HTML标签
* 表达式元素中可以包含任何符合Java语言规范的表达式，但是**不能使用分号**来结束表达式
* **jsp表达式语法：**<%= 表达式 %>
* **其等价的XML语句：**《jsp：expression》表达式《/jsp：expression》

## jsp注释

* **作用：**为代码作注释以及将某段代码注释掉
* **jsp注释语法：**<%-- 注释 --%>
* 不同情况下使用注释的语法规则：![avatar](图片引入\1582722906035.png)

* **作用：**用来设置与整个jsp页面相关的属性
* **jsp指令语法：**<%@ derective attribute="value" %>
* **三种指令标签：**![avatar](图片引入\QQ截图20200227132723.png)

## jsp行为

* **作用：**JSP行为标签使用XML语法结构来控制servlet引擎。它能够动态插入一个文件，重用JavaBean组件，引导用户去另一个页面，为Java插件产生相关的HTML等等。
* **jsp行为语法：**<jsp:action_name attribute="value" /> （严格遵守XML标准）
* **罗列一些可用的jsp行为标签：**![avatar](图片引入\1582723291535.png)

## jsp隐含对象

* **jsp九大对象：**![avatar](图片引入\1582723363082.png)

## 控制流语句

* JSP提供对Java语言的全面支持。您可以在JSP程序中使用Java API甚至建立Java代码块，包括判断语句和循环语句等等

### 判断语句

* **if...else：**

  ```
  <% if (day == 1 | day == 7) { %>
        <p>今天是周末</p>
  <% } else { %>
        <p>今天不是周末</p>
  <% } %>
  ```

* **switch…case：**

  ```
  <% 
  switch(day) {
  case 0:
     out.println("星期天");
     break;
  case 1:
     out.println("星期一");
     break;
  case 2:
     out.println("星期二");
     break;
  case 3:
     out.println("星期三");
     break;
  case 4:
     out.println("星期四");
     break;
  case 5:
     out.println("星期五");
     break;
  default:
     out.println("星期六");
  }
  %>
  ```

### 循环语句

* **for：**

  ```
  <%for ( fontSize = 1; fontSize <= 3; fontSize++){ %>
     <font color="green" size="<%= fontSize %>">
      菜鸟教程
     </font><br />
  <%}%>
  ```

* **while：**

  ```
  <%while ( fontSize <= 3){ %>
     <font color="green" size="<%= fontSize %>">
      菜鸟教程
     </font><br />
  <%fontSize++;%>
  <%}%>
  ```

* **jsp运算符：**JSP支持所有Java逻辑和算术运算符![avatar](图片引入\1582724042440.png)

## jsp字面量

- 布尔值(boolean)：true 和 false;
- 整型(int)：与 Java 中的一样;
- 浮点型(float)：与 Java 中的一样;
- 字符串(string)：以单引号或双引号开始和结束;
- Null：null