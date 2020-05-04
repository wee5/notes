# XML5：查看

* 主流浏览器均能查看一个正确的xml
  * 文档显示为代码颜色化的根以及子元素
  * 左侧加减号可以展开或收起元素结构
  * 查看源文件（无加减号），右键->查看源代码
* xml**直白地**显示数据
  * 在没有任何有关如何显示数据的信息的情况下，大多数的浏览器都会仅仅把 XML 文档显示为源代码
  * 标签由xml作者发明，浏览器无法确定像 <table> 这样一个标签究竟描述一个 HTML 表格还是一个餐桌

## css显示xml

* 在第二行引入css：<?xml-stylesheet type="text/css" href="cd_catalog.css"?>
* 样式选择器直接选中xml标签名
* 这种方法不常用

## XSLT显示xml

* XSLT（eXtensible Stylesheet Language Transformations）是首选的xml样式表语言，远比css完善
* XSLT 是在浏览器显示 XML 文件之前，由**浏览器**先把它转换为 HTML
* 在**服务器**上通过XSLT转换xml：可以减少不同浏览器可能产生不同结果



