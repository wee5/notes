# springmvc控制层设计模式

* 设计需求：接收参数，传出参数，跳转



## 接受参数的不同实现方式

* 传统servlet：通过ServletRequest取参数

* 形参：1.使用注释@RequestParam接收请求中的参数

  ​		2.使用注释@PathVariable接收显式参数

  ​		@RequestMapping("/index1/{id}/{name}")

* Model：1.表单对应Model属性名

  ​		2.get方式（地址后）传递参数对应Model属性名



## 传出参数的不同实现方式

* 传统servlet：通过ServletResponse传参数
* ModelAndView：Mv.addObject("键","值")



## 跳转的不同实现方式

* 返回类型String：字符串形式返回跳转地址

* ModelAndView：Mv.setViewName("地址")

## 补充：ModelAndView方法

* 构造方法：(String viewName, String attributeName, Object attributeValue)
* 构造方法：ModelAndView("redirect:/index.jsp")

* ModelAndView.addObject("键","值")
* ModelAndView.setViewName("地址")

