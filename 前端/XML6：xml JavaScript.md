# XML6：xml JavaScript

## XMLHttpRequest对象（js对象）

* **作用：**用于在后台与服务器交换数据
  * 在不重新加载页面的情况下更新网页（ajax）
  * 在页面已加载后从服务器请求数据
  * 在页面加载后从服务器接收数据
  * 在后台向服务器发送数据

## xml Parser（解析器）

* xml解析器把xml文档转换为xml Dom对象（可通过js操作的对象）

* **解析xml文档**（把 XML 文档解析到 XML DOM 对象中）

  * if (window.XMLHttpRequest)
    {// code for IE7+, Firefox, Chrome, Opera, Safari
    xmlhttp=new XMLHttpRequest();
    }
    else
    {// code for IE6, IE5
    xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
    }
    xmlhttp.open("GET","books.xml",false);
    xmlhttp.send();
    xmlDoc=xmlhttp.responseXML;

* **解析xml字符串**（把 XML 字符串解析到 XML DOM 对象中）

  *   txt="<bookstore><book>";
    txt=txt+"<title>Everyday Italian</title>";
    txt=txt+"<author>Giada De Laurentiis</author>";
    txt=txt+"<year>2005</year>";
    txt=txt+"</book></bookstore>";

    if (window.DOMParser)
    {
    parser=new DOMParser();
    xmlDoc=parser.parseFromString(txt,"text/xml");
    }
    else // Internet Explorer
    {
    xmlDoc=new ActiveXObject("Microsoft.XMLDOM");
    xmlDoc.async=false;
    xmlDoc.loadXML(txt); 
    }  

  * 补充：Internet Explorer 使用 loadXML() 方法来解析 XML 字符串，而其他浏览器使用 DOMParser 对象

### 跨域访问

* 出于安全方面的原因，现代的浏览器不允许跨域的访问。

  这意味着，网页以及它试图加载的 XML 文件，都必须位于相同的服务器上

## xml DOM对象

* **DOM**（Document Object Model 文档对象模型）定义了访问和操作文档的标准方法

* xml DOM把xml文档作为**树结构；文本，属性，元素都被认为是节点



