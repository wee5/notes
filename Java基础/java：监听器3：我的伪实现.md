# java：监听器3：我的伪实现



* 本节未完成

## 实现原则

* 开发者对于监听器的操作仅限于实现，继承，注解；**不应该在代码中出现监听器相关操作；让开发者更专注逻辑业务**

## 事件源

* ```java
  public class Man{
      private String name;
      
      public void walk(){
          System.out.print("man is walking...")
      }
      public void talk(Man man){
          System.out.print(this.name+"is talking with"+man.getName())
      }
      //省略get/setName方法和有参构造函数...
  }
  ```

## 事件

* 事件由spring或jdk预设；同时可以自定义事件

## 监听器

* 监听器由spring预设；规定了**事件发生时做出的动作**

## 监听器和事件的内部实现（我的猜想）

* 暂时不清楚

## 测试类

* ```java
  public class test{
      public static void main(String [] args){
          Man weezy=new Man("weezy");
          Man fit=new Man("fit");
          //以下两方法被监听
          weezy.walk();
          fit.talk(weezy);
      }
  }
  ```

* 