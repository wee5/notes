# html

* 超文本标记语言（Hyper Text Markup Language）是一种用于创建网页的标准标记语言；html运行在浏览器上，由浏览器来解析；

## 通用标签

* 声明html版本
  * HTML5：<!DOCTYPE html>
  * HTML 4.01：<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
* **<html>** 元素是 HTML 页面的根元素
* **<head>** 元素包含了文档的元（meta）数据，如 **<meta charset="utf-8">** 定义网页编码格式为 **utf-8**。
* **<body>** 元素包含了可见的页面内容
* </h1>~</h6>标题，前后自动换行；搜索引擎使用标题为您的网页的结构和内容编制索引
* </hr> 标签在 HTML 页面中创建水平线
* </p>段落（块级元素）；</b>换行；<br 折行
* <!-- 注释 -->
* 

## **通用属性**

* 键值存在；name="value"
* class：定义一个或多个类名
* id：唯一的id
* style：规定行内样式
* title：秒速额外信息，作为工作条使用

## 常用标签

### 《head》

* 包含了所有的头部标签元素
* 《meta》：提供meta标记，描述 关键词，作者， 字符集
  - 标签提供了元数据.元数据也不显示在页面上，但会被浏览器解析；
* 《title》：定义该文档标题
  * 浏览器工具栏的标题；收藏夹中的标题；搜索引擎结果页面的标题；
* 《base》：定义页面所有链接默认的链接目标地址
* 《link》：定义了文档与外部资源之间的关系，常用于链接到样式表
* 《style》：定义样式文件引用地址，也可以直接添加样式来渲染

## 页面规定字符集

* HTML 4.01： <meta http-equiv="content-type" content="text/html; charset=UTF-8">
* HTML5： <meta charset="UTF-8">











补充：

* 文本格式化标签

* 超链接：点击这些内容来跳转到新的文档或者当前文档中的某个部分，*链接文本"* 不必一定是文本。图片或其他 HTML 元素都可以成为链接。使用 target 属性，你可以定义被链接的文档在何处显示HTML 链接- id 属性

  id属性可用于创建在一个HTML文档书签标记。

  **提示:** 书签是不以任何特殊的方式显示，在HTML文档中是不显示的，所以对于读者来说是隐藏的。