# 控制层：注释：@ModelAttribute



## Model类

* 需求：后台要从控制层直接返回前端所需的数据
* Model和ModelMap用法差不多
* spring自动为Model创建实例，并作为controller的入参
* Model的生命周期只有一个http请求的处理过程，请求处理完后，Model就销毁了



## @ModelAttribute

* ```java
  //示例
  @ModelAttribute
  public User addUser() {
      User user = new User();
      user.setUsername("小芳");
      user.setPassword("1234");
      return user;
  }
  ```



### 方法上

* 在Model模型中添加属性值

* 被@ModelAttribute注释的方法会在此controller每个方法执行前被执行，因此对于一个controller映射多个URL的用法来说，要谨慎使用。

* 用法：

  * 不指定属性名称value，返回值为void

    * 无论请求什么接口，都会率先执行

    * ```java
      //示例：
      @ModelAttribute
      public void populateModel(ModelMap model) {
          model.addAttribute("attributeName", "123");
      }
      ```

  * 指定或不指定属性名称value，返回值为对象

    * 相当于model.addAttribute("user", user)，不指定时默认属性名为变量名

    * ```java
      //示例：
      @ModelAttribute //("aaa")
      public User addUser() {
          User user = new User();
          user.setUsername("小芳");
          user.setPassword("1234");
          return user;
      }
      ```

  * 指定属性名称，返回值为字符串

    * 相当于model.addAttribute("string1-key", "string1-value")

    * ```java
      示例：@ModelAttribute("string1-key")
      public String addString() {
          System.out.println("---addString---");
          return "string1-value";
      }
      ```

    * 



## @ModelAttribute和@RequestMapping同时注释一个方法

* 这时这个方法的返回值并不是表示一个视图名称，而是model属性的值

* 视图名称由RequestToViewNameTranslator根据请求"/helloWorld.do"转换为逻辑视图helloWorld。

* ```java
  //示例
  @Controller 
  public class HelloWorldController {
  
  	@RequestMapping(value = "/helloWorld.do") 
  	@ModelAttribute("attributeName") 
  	public String helloWorld() { 
      	return "hi"; 
      } 
  }
  ```



### 参数前

* 从 Model模型中取出属性值，对应到传参

  * ```java
    //示例：
    @RequestMapping(value = "helloWorld")
    public String helloWorld(@ModelAttribute("abc") User user,
        @ModelAttribute("attributeName") String aName,
        @ModelAttribute("string1-value") String svalue) {
        return "helloWorld";
    }
    ```

* 从Form表单或者URL参数中获取属性参数值，放到对应类型的参数中

  * 此时表单中的组件名和参数属性名称一致

  * 参数类必须有无参构造方法

  * @ModelAttribute可以不写

  * ```java
    //示例：
    @RequestMapping(value = "helloWorld2")
        public String helloWorld2(@ModelAttribute("user") User user) {
        System.out.println("---helloWorld---"+user.getUsername()+", "+user.getPassword());
        return "helloWorld";
    }
    ```

### 返回值前

* 放在方法的返回值之前，添加方法返回值到模型对象中，用于视图页面展示时使用
* @ModelAttribute  注解的返回值会覆盖 @RequestMapping  注解方法中的 @ModelAttribute  注解的同名命令对象



## @SessionAttribute

* 只能注释在类上
* 将Model中的name参数保存到了session中（如果Model中没有name参数，而session中存在一个name参数，那么SessionAttribute会讲这个参数塞进Model中）
* SessionAttribute有两个参数：
    * String[] value：要保存到session中的参数名称
    * Class[] typtes：要保存的参数的类型，和value中顺序要对应上
* 可以写做@SessionAttributes(types = {User.class,Dept.class},value={“attr1”,”attr2”})
* 原理理解：它的做法大概可以理解为将Model中的被注解的attrName属性保存在一个SessionAttributesHandler中，在每个RequestMapping的方法执行后，这个SessionAttributesHandler都会将它自己管理的“属性”从Model中写入到真正的HttpSession；同样，在每个RequestMapping的方法执行前，SessionAttributesHandler会将HttpSession中的被@SessionAttributes注解的属性写入到新的Model中。
* 如果想删除session中共享的参数，可以通过SessionStatus.setComplete()，这句只会删除通过@SessionAttribute保存到session中的参数



