# xml3：语法

* 声明：<? version="1.0" encoding="utf-8">（可选）
* 根元素：<root>必须包含根元素，是其他元素的父元素
* 关闭标签
* 对大小写敏感：同一对标签的开始结束标签大小写必须相同
* 正确嵌套
* 属性值加引号：也可以用单引号；若属性值中包含双引号，则用单引号括住双引号
* 实体引用：xml中，只有字符“<”和“&”是非法的，大于号是合法的；
  * ![](H:\笔记—markdown注\前端_前端_前端\图片引入\QQ截图20200228221643.png)
* 注释：<!--  注释  -->
* 保留空格：xml不会像html将连续空格合并为一个
* 以LF存储换行

## 元素

- 指从且包括开始标签到且包括结束标签的部分；包含属性，文本，其他元素，或前面所有；

### 元素命名规则

- 可以包含字母，数字及其他字符
- 不能以数字或者标点符号开始
- 不能以字母“xml”，“XML","Xml"等开始
- 不能包含空格
- 需要用符号作为分隔或连接时，最好使用"_"(下划线)
- 补充：元素是可扩展的（略）

## 属性

* 提供有关元素的额外信息

## 元素和属性

* 属性不嫩更包含**多个值**（元素可以）
* 属性不能包含**树结构**（元素可以）
* 属性不容易**扩展**

### 元数据和数据

* **数据：**xml用来存储传输的数据；应当存储为**元素**；
* **元数据：**和数据有关的数据；应当存储为**属性**；

### xml存储信息的三种方式

* <note date="10/01/2008">
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder</heading>
  <body>Don't forget me this weekend!</body>
  </note>
* <note>
  <date>10/01/2008</date>
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder</heading>
  <body>Don't forget me this weekend!</body>
  </note>
* <note>
  <date>
  <day>10</day>
  <month>01</month>
  <year>2008</year>
  </date>
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder</heading>
  <body>Don't forget me this weekend!</body>
  </note>
* 推荐第三种；日期作为数据，应当存储为元素；利用元素的树结构，将**数据(日期)**变成**数据(年月日)**