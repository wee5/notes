# java：注解2：自定义注解



## 自定义注解类编写的规则：

* Annotation型定义为@interface，所有的Annotation会自动继承java.lang.Annotation这一接口，**并且不能再继承别的类或接口**
* 参数成员只能用public或default访问权修饰
* 参数成员只能用**基本类型**byte，short，char，int，long，float，double，boolean八种基本数据类型和**String，Enum，Class，annotations**等数据类型，以及**一些类型的数组**
* 要获取类方法和字段的注解信息，必须通过**java反射技术来获取Annotation对象**，除此之外没有别的获取注解对象的方法
* 注解也可以没有定义成员，不过这样注解就没用了
* 自定义注解需要使用到元注解



## 示例

* **自定义注解**

  * ```java
    package com.wxy.annotation;
     
    import java.lang.annotation.Documented;
    import java.lang.annotation.ElementType; 
    import java.lang.annotation.Inherited;
    import java.lang.annotation.Retention;
    import java.lang.annotation.RetentionPolicy;
    import java.lang.annotation.Target;
    /**   
    *  定义注解Test
    *  注解中含有两个元素 id 和 description
    *  description元素有默认值"no description"  
    *   @create-time     2011-8-12   下午02:22:28   
    *   @revision          $Id
    */
     
    //该注解用于方法声明
    @Target(ElementType.METHOD)
    //VM将在运行期也保留注释，因此可以通过反射机制读取注解的信息
    @Retention(RetentionPolicy.RUNTIME)
    //将此注解包含在javadoc中
    @Documented
    //允许子类继承父类中的注解
    @Inherited
    public @interface Test { 
        public int id();
        public String description() default "no description";
    }
    ```



* 使用注解和**解析注解**

  * ```java
    package com.wxy.annotation;
    import java.lang.reflect.Method;
    /**
    *   使用注解和解析注解
    *  
    *   @creator            xiaoyu.wang   
    *   @create-time     2011-8-12   下午03:49:17   
    *   @revision          $Id
    */
    public class Test_1{
        
        /**
         * 被注释的三个方法
         */
        @Test(id = 1, description = "hello method1")
        public void method1() {
        }
        
        @Test(id = 2)
        public void method2() {
        }
        
        @Test(id = 3, description = "last method3")
        /**
         * 解析注释，将Test_1类所有被注解方法的信息打印出来
         * @param args
         */
        public static void main(String[] args) {
            Method[] methods = Test_1.class.getDeclaredMethods();
            for (Method method : methods) {
                //判断方法中是否有指定注解类型的注解
                boolean hasAnnotation = method.isAnnotationPresent(Test.class);
                if (hasAnnotation) {
                    //根据注解类型返回方法的指定类型注解
                    Test annotation = method.getAnnotation(Test.class);
                    System.out.println("Test(method=" + method.getName() + ",id=" + annotation.id()
                                       + ",description=" + annotation.description() + ")");
                }
            }
        }
    }
    ```

  * **解析注解的方法**

    * 字节码对象.getDeclaredMethods()：获取字节码对象的所有方法
    * method.isAnnotationPresent()：判断方法对象上是否存在注解
    * method.getAnnotation(字节码对象)：获取该方法的给定注解类型的注解对象；该方法是确定的一个对象
    * **解析注解的目的是获取注解的属性值**

