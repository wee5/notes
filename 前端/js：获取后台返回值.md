# js：获取后台返回值



* ```js
  1.后台传给前台的方法
  
  String rulemodeid=req.getParameter("rulemodeid");
  req.setAttribute("rulemodeid", rulemodeid);
  
  2.前台js的方法
  
  var rulemodeid= "${rulemodeid }";
  
  其中“”是必须加的，不然报defined错
  ```

* 