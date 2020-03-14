# XML1:intro

* **作用：**传输和存储数据；显示数据
* **什么是xml：**可扩展标记语言；很像html的标记语言；设计宗旨是传输数据，而不是显示数据；xml标签没有被预定义，需要自行定义；具有自我描述性；

## 和HTML的区别

* xml不是html的替代，目的不同：
  * xml用来传输和存储数据，焦点是数据内容，旨在传输信息；
  * html用来显示数据，焦点是数据外观，旨在显示数据；

## xml不会做任何事情

* xml被设计用来结构化，存储以及传输信息；
* 示例：（Jani写给Tove的便签）
  * <note>
    <to>Tove</to>
    <from>Jani</from>
    <heading>Reminder</heading>
    <body>Don't forget me this weekend!</body>
    </note>
  * 这条便签具有自我描述性，没有预定义的标签，它仅仅是包装在xml标签中的纯粹的信息，需要编写软件或程序才能传送，接收和显示这个文档；

