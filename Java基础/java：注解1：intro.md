# java：注解1：intro



* 注解（Annontation）
  * 是java5开始引入的新特征；用来将任何的**信息或元数据**（metadata）与**程序元素**（类，方法，成员变量等）进行关联。为程序的元素（类，方法，成员变量）加上更直观的说明，这些说明**与程序的业务逻辑无关**，并且供指定的工具或框架使用
  * java注解是附加在代码中的一些**元信息**，用于一些工具在**编译，运行时进行解析和使用**，起到说明和配置的功能；包含在java.lang.annotation包中
* **代理模式，反射，注解的关系**
  - 静态代理模式利用接口的多态，动态代理利用反射
  - 注解利用动态代理，反射

## 总结

* 注解与程序的业务逻辑无关
* 如果注解类型声明中不存在 Retention 注解，保留策略默若为**RetentionPolicy.CLASS，则不能通过反射获取注解信息**
* 四个元注解的定义都是`@Retention(RetentionPolicy.RUNTIME)`，**即编译器`将把注解记录在`类文件中**，可以通过`反射`读取
* 当获取字节码对象时，可以同时获取该对象和作用在它上的注解信息
* 推荐：https://blog.csdn.net/weixin_30460489/article/details/99591780

## 作用

* **生成文档**；java最常见最早提供的注解；如@Param，@Return
* 跟踪代码依赖性，实现**替代配置文件**的功能；如Dagger2依赖注入
* **编译时进行格式检查**；如@Override

## 原理（结合动态代理过程分析）

* 注解本质是继承了Annotation的特殊接口；其**具体实现类是java运行时生成的动态代理类**；（目标类继承接口）
* 而我们**通过反射获取注解**时，返回的是java运行时生成的**动态代理对象$Proxy1**；（生成动态代理对象）
* 通过代理对象调用**自定义注解（接口）的方法（即多态）**，会最终调用**AnnotationInvocationHandler（动态代理类）**的invoke方法，
* 该方法会从memberValues这个Map中索引出对应的值，而memberValues的来源是java常量池

## 元注解

* java.lang.annotation提供了四种元注解，专门注解其他的注解（在自定义注解的时候，需要使用到元注解）
* **@Documented：**注解是否将包含在javaDoc中
  * 一个简单的Annotaions标记注解，表示是否将注解信息添加在java文档中
* **@Retention：**什么时候使用该注解
  * 定义该注解的声明周期
  * Retention.SOURCE：在编译阶段丢弃；这些注解在编译结束之后就不再有任何意义，所以不会写入字节码；如@Override，@SuppressWarnnings
  * RetentionPolicy.CLASS：在类加载时丢弃；在字节码文件的处理中有用。注解默认使用这种方法
  * RetentionPolicy.RUNTIME：始终不会丢弃，运行期也保留该注解；因此**可以使用反射机制读取该注解的信息**；自定义注解通常使用这种方法
* **@Target：**注解用于什么地方
  * 表示该注解用于什么地方；默认值为任何元素，表示该注解用于什么地方
  * ElementType.CONSTRUCTOR: 用于描述构造器
  * ElementType.FIELD: 成员变量、对象、属性（包括enum实例）
  * ElementType.LOCAL_VARIABLE: 用于描述局部变量
  * ElementType.METHOD: 用于描述方法
  * ElementType.PACKAGE: 用于描述包
  * ElementType.PARAMETER: 用于描述参数
  * ElementType.TYPE: 用于描述类、接口(包括注解类型) 或enum声明
* **@Inherited：**是否允许子类继承该注解
  * 这是一个标记注解，阐述了某个被标注的类型是会被继承的；

## 常见标准的Annotation

* Override：java.lang.Override 是一个标记类型注解，它被用作标注方法。它说明了被标注的方法重载了父类的方法，起到了断言的作用。如果我们使用了这种注解在一个没有覆盖父类方法的方法时，java 编译器将以一个编译错误来警示
* Deprecated：也是一种标记类型注解。当一个类型或者类型成员使用@Deprecated 修饰的话，编译器将不鼓励使用这个被标注的程序元素。所以使用这种修饰具有一定的“延续性”：如果我们在代码中通过继承或者覆盖的方式使用了这个过时的类型或者成员，虽然继承或者覆盖后的类型或者成员并不是被声明为@Deprecated，但编译器仍然要报警
* SuppressWarnings：不是一个标记类型注解。它有一个类型为String[] 的成员，这个成员的值为被禁止的警告名。对于javac 编译器来讲，被-Xlint 选项有效的警告名也同样对@SuppressWarings 有效，同时编译器忽略掉无法识别的警告名



