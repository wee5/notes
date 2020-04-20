# springboot web开发3：thymeleaf



## 模板引擎

* Jsp，Velocity，Freemarker，Thymeleaf
  * ![avatar](H:\markdown笔记\SpringBoot框架\图片引用\15839491-e1448ac7b37f36e6.png)

* springboot推荐thymeleaf，语法简单，功能强大



## 引入thymeleaf

* ```xml
  <properties>
      <!-- 切换thymeleaf版本 -->
      <thymeleaf.version>3.0.2.RELEASE</thymeleaf.version>
      <!-- 布局功能的支持程序，thymeleaf3主程序 layout2以上版本 -->
      <thymeleaf.layout-dialect.version>2.1.1</thymeleaf.layout-dialect.version>
  </properties>
  <!-- 引入thymeleaf -->
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-thymeleaf</artifactId>
  </dependency>
  ```



## 使用和语法

* ```java
  @ConfigurationProperties(prefix = "spring.thymeleaf")
  public class ThymeleafProperties {
      private static final Charset DEFAULT_ENCODING;
      public static final String DEFAULT_PREFIX = "classpath:/templates/";
      public static final String DEFAULT_SUFFIX = ".html";
      <!-- Thymeleaf自动渲染，classpath:/templates/路径下的，以.html为后缀的文件 -->
  ```

### 使用

* 导入thymeleaf的命名空间

  * ```xml
    <!-- 导入后有语法提示 -->
    <html lang="en" xmlns:th="http:/www.thymeleaf.org">
    ```

* 使用thymeleaf语法

  * ```xml
    <!DOCTYPE html>
    <html lang="en" xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="utf-8">
        <title>Title</title>
    </head>
    <body>
        <h1>成功</h1>
        <!-- th:text 将div内的文本内容设置为 -->
        <div th:text="${hello}">这是显示欢迎信息</div>
    </body>
    </html>
    ```

### 语法规则

* **th**：指定任意原生属性的值
  * ![avatar](图片引用\Snipaste_2020-04-19_11-41-45.png)

* 表达式

  * ```properties
    Simple expressions:（表达式语法）
        Variable Expressions: ${...}：获取变量值：OGNL
        	1.获取对象属性，调用方法
        	2.#ctx : the context object.
            #vars: the context variables.
            #locale : the context locale.
            #request : (only in Web Contexts) the HttpServletRequest object.
            #response : (only in Web Contexts) the HttpServletResponse object.
            #session : (only in Web Contexts) the HttpSession object.
            #servletContext : (only in Web Contexts) the ServletContext object.
            3.内置工具对象
            #execInfo : information about the template being processed.
            #messages : methods for obtaining externalized messages inside variables expressions, in the same way as they
            #uris : methods for escaping parts of URLs/URIs
            #conversions : methods for executing the configured conversion service (if any).
            #dates : methods for java.util.Date objects: formatting, component extraction, etc.
            #calendars : analogous to #dates , but for java.util.Calendar objects.
            #numbers : methods for formatting numeric objects.
            #strings : methods for String objects: contains, startsWith, prepending/appending, etc.
            #objects : methods for objects in general.
            #bools : methods for boolean evaluation.
            #arrays : methods for arrays.
            #lists : methods for lists.
            #sets : methods for sets.
            #maps : methods for maps.
            #aggregates : methods for creating aggregates on arrays or collections.
            #ids : methods for dealing with id attributes that might be repeated (for example, as a result of an iteration).
        Selection Variable Expressions: *{...}：选择表达式，和${}功能一样
        	补充：配合th:Object="${session.user}使用
        	例：<div th:object="${session.user}">
            <p>Name: <span th:text="*{firstName}">Sebastian</span>.</p>
            <p>Surname: <span th:text="*{lastName}">Pepper</span>.</p>
            <p>Nationality: <span th:text="*{nationality}">Saturn</span>.</p>
            </div>
        Message Expressions: #{...}：获取国际化内容
        Link URL Expressions: @{...}：定义URL：
        	例：@{/order/process(execId=${execId},execType='FAST')}
        Fragment Expressions: ~{...}：片段引用表达式
        	例：<div th:insert="~{commons :: main}">...</div>
        	
    Literals（字面量）
        Text literals: 'one text' , 'Another one!' ,…
        Number literals: 0 , 34 , 3.0 , 12.3 ,…
        Boolean literals: true , false
        Null literal: null
        Literal tokens: one , sometext , main ,…
        
    Text operations:（文本操作）
        String concatenation: +
        Literal substitutions: |The name is ${name}|
        
    Arithmetic operations:（数学运算）
        Binary operators: + , - , * , / , %
        Minus sign (unary operator): -
        
    Boolean operations:（布尔运算）
        Binary operators: and , or
        Boolean negation (unary operator): ! , not
        
    Comparisons and equality:（比较运算）
        Comparators: > , < , >= , <= ( gt , lt , ge , le )
        Equality operators: == , != ( eq , ne )
        
    Conditional operators:（条件运算）
        If-then: (if) ? (then)
        If-then-else: (if) ? (then) : (else)：三元运算
        Default: (value) ?: (defaultvalue)
        
    Special tokens:
        No-Operation: _
    ```

  * 