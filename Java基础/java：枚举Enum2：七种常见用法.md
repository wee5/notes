# java：枚举Enum2：七种常见用法



## 常量

* 在JDK1.5 之前，我们定义常量都是： public static fianl.... 。现在可以把相关的常量分组到一个枚举类型里，而且枚举提供了比常量更多的方法

* ```java
  public enum Color { 
  	RED, GREEN, BLANK, YELLOW 
  }
  ```



## switch

* ```java
  enum Signal {  
      GREEN, YELLOW, RED  
  }  
  public class TrafficLight {  
      Signal color = Signal.RED;  
      public void change() {  
          switch (color) {  
          case RED:  
              color = Signal.GREEN;  
              break;  
          case YELLOW:  
              color = Signal.RED;  
              break;  
          case GREEN:  
              color = Signal.YELLOW;  
              break;  
          }  
      }
  }
  ```



## 向枚举中添加新方法

* ```java
  public enum Color {  
      RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4);  
      // 成员变量  
      private String name;  
      private int index;  
      // 构造方法  
      private Color(String name, int index) {  
          this.name = name;  
          this.index = index;  
      }  
      // 普通方法  
      public static String getName(int index) {  
          for (Color c : Color.values()) {  
              if (c.getIndex() == index) {  
                  return c.name;  
              }  
          }  
          return null;  
      }  
      // get set 方法  
      public String getName() {  
          return name;  
      }  
      public void setName(String name) {  
          this.name = name;  
      }  
      public int getIndex() {  
          return index;  
      }  
      public void setIndex(int index) {  
          this.index = index;  
      }  
  }
  ```



## 覆盖枚举的方法

* ```java
  public enum Color {  
      RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4);  
      // 成员变量  
      private String name;  
      private int index;  
      // 构造方法  
      private Color(String name, int index) {  
          this.name = name;  
          this.index = index;  
      }  
      //覆盖方法  
      @Override  
      public String toString() {  
          return this.index+"_"+this.name;  
      }  
  }
  ```



## 实现接口

* ```java
  //所有的枚举都继承自java.lang.Enum类。由于Java 不支持多继承，所以枚举对象不能再继承其他类
  public interface Behaviour {  
      void print();  
      String getInfo();  
  }  
  public enum Color implements Behaviour{  
      RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4);  
      // 成员变量  
      private String name;  
      private int index;  
      // 构造方法  
      private Color(String name, int index) {  
          this.name = name;  
          this.index = index;  
      }  
  //接口方法  
      @Override  
      public String getInfo() {  
          return this.name;  
      }  
      //接口方法  
      @Override  
      public void print() {  
          System.out.println(this.index+":"+this.name);  
      }  
  }
  ```



## 使用接口组织枚举

* ```java
  public interface Food {  
      enum Coffee implements Food{  
          BLACK_COFFEE,DECAF_COFFEE,LATTE,CAPPUCCINO  
      }  
      enum Dessert implements Food{  
          FRUIT, CAKE, GELATO  
      }  
  }
  ```



## 关于枚举集合的使用

* java.util.EnumSet和java.util.EnumMap是两个枚举集合。EnumSet保证集合中的元素不重复；EnumMap中的 key是enum类型，而value则可以是任意类型。关于这个两个集合的使用就不在这里赘述