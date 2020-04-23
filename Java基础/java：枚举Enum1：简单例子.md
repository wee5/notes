# java：枚举Enum1：简单例子

* ```java
  //一个枚举类Day
  public enum Day {
      MONDAY,TUESDAY,WEDNESDAY,THURSDAY,FRIDAY,SATURDAY,SUNDAY;
  }
  
  //测试类
  public class TestEnum {
  
      public static void main(String[] args) {
          Day today=Day.FRIDAY;
          switch(today)
          {
          case MONDAY:
              System.out.println("today is monday");
              break;
          case TUESDAY:
              System.out.println("today is tuesday");
              break;
          case WEDNESDAY:
              System.out.println("today is webnesday");
              break;
          case THURSDAY:
              System.out.println("today is thursday");
              break;
          case FRIDAY:
              System.out.println("today is firday");
              break;
          case SATURDAY:
              System.out.println("today is saturday");
              break;
          case SUNDAY:
              System.out.println("today is sunday");
              break;
          }
      }
  
  }//测试结果：today is firday
  ```

* ```java
  //枚举类本质就是一个类，编译器会自动生成Day类，通过反编译得到该类
  final class Day extends Enum
  {
      //编译器添加的静态values()方法
      public static Day[] values()
      {
          return (Day[])$VALUES.clone();
      }
      //编译器添加的静态valueOf()方法，注意间接调用了Enum类的valueOf方法
      public static Day valueOf(String s)
      {
          return (Day)Enum.valueOf(com/zejian/enumdemo/Day, s);
      }
      //私有构造函数
      private Day(String s, int i)
      {
          super(s, i);
      }
       //前面定义的7种枚举实例
      public static final Day MONDAY;
      public static final Day TUESDAY;
      public static final Day WEDNESDAY;
      public static final Day THURSDAY;
      public static final Day FRIDAY;
      public static final Day SATURDAY;
      public static final Day SUNDAY;
      private static final Day $VALUES[];
  
      static 
      {    
          //每个枚举类型（即星期数）都是Day类的一个实例对象
          MONDAY = new Day("MONDAY", 0);
          TUESDAY = new Day("TUESDAY", 1);
          WEDNESDAY = new Day("WEDNESDAY", 2);
          THURSDAY = new Day("THURSDAY", 3);
          FRIDAY = new Day("FRIDAY", 4);
          SATURDAY = new Day("SATURDAY", 5);
          SUNDAY = new Day("SUNDAY", 6);
          $VALUES = (new Day[] {
              MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
          });
      }
  }
  ```

* ```java
  //被继承的Enum类
  public abstract class Enum<E extends Enum<E>> implements Comparable<E>, Serializable {
  
      private final String name; //枚举字符串名称
  
      public final String name() {
          return name;
      }
  
      private final int ordinal;//枚举顺序值
  
      public final int ordinal() {
          return ordinal;
      }
  
      //枚举的构造方法，只能由编译器调用
      protected Enum(String name, int ordinal) {
          this.name = name;
          this.ordinal = ordinal;
      }
  
      public String toString() {
          return name;
      }
  
      public final boolean equals(Object other) {
          return this==other;
      }
  
      //比较的是ordinal值
      public final int compareTo(E o) {
          Enum<?> other = (Enum<?>)o;
          Enum<E> self = this;
          if (self.getClass() != other.getClass() && // optimization
              self.getDeclaringClass() != other.getDeclaringClass())
              throw new ClassCastException();
          return self.ordinal - other.ordinal;//根据ordinal值比较大小
      }
  
      @SuppressWarnings("unchecked")
      public final Class<E> getDeclaringClass() {
          //获取class对象引用，getClass()是Object的方法
          Class<?> clazz = getClass();
          //获取父类Class对象引用
          Class<?> zuper = clazz.getSuperclass();
          return (zuper == Enum.class) ? (Class<E>)clazz : (Class<E>)zuper;
      }
  
  
      public static <T extends Enum<T>> T valueOf(Class<T> enumType,
                                                  String name) {
          //enumType.enumConstantDirectory()获取到的是一个map集合，key值就是name，value则是枚举变量值   
          //enumConstantDirectory是class对象内部的方法，根据class对象获取一个map集合的值       
          T result = enumType.enumConstantDirectory().get(name);
          if (result != null)
              return result;
          if (name == null)
              throw new NullPointerException("Name is null");
          throw new IllegalArgumentException(
              "No enum constant " + enumType.getCanonicalName() + "." + name);
      }
  
      //.....省略其他没用的方法
  }
  ```

* 