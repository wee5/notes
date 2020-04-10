# java：反射1：intro



## java的反射机制

* 可以**在运行时加载、探知、使用编译期间完全未知的classes**；即Java程序可以加载一个运行时才得知名称的class，获悉其完整构造（不包括methods定义），并生成其对象实体（newInstance）或对其fields设值，或**唤起**（invoke）其methods方法
* 方法的申明和定义：申明不包括方法体，定义包括方法体
* **反射机制操作的是.calss文件**
* 反射机制的应用：
  * 动态代理
  * 控制反转IOC，利用配置文件
* 获得字节码对象的方法
  * Class.forName("完全限定名");
  * instance.getClass();得到实例instance类型的字节码对象



## .class文件

* java源码的转化
  * java源码被**编译**成.class文件字节码
  * jvm执行**解释**字节码给计算机
  * 计算机将结果呈现给用户
  * 因此java并不是编译机制，而是解释机制
* java反射机制和.class文件
  * 将.class文件字节码从硬盘加载到内存中
  * （反射机制）解剖出字节码中的构造函数，方法以及变量
* 查看.class文件
  * 在.class文件路径下打开cmd
  * 执行javap -c XXX.class查看

### 源码和字节码

* Animal.java

  * package com.appleyk.reflect;

    public class Animal {

    ```java
    public String name ="Dog";
    private int   age  =30 ;
    
    //默认无参构造函数
    public Animal(){
    	System.out.println("Animal");
    }
    
    //带参数的构造函数 
    public Animal(String name , int age){
    	System.out.println(name+","+age);
    }
    
    //公开 方法  返回类型和参数均有
    public String sayName(String name){
    	return "Hello,"+name;
    }
    ```

    }

* Animal.class

  * F:\Java\ReflectClass\bin\com\appleyk\reflect>javap -c Animal.class
    Compiled from "Animal.java"
    public class com.appleyk.reflect.Animal {
      public java.lang.String name;	**public变量，private变量未显示**

      public com.appleyk.reflect.Animal();	**无参构造方法**
        Code:
           0: aload_0
           1: invokespecial #12                 // Method java/lang/Object."<init>":
    ()V
           4: aload_0
           5: ldc           #14                 // String Dog
           7: putfield      #16                 // Field name:Ljava/lang/String;
          10: aload_0
          11: bipush        30
          13: putfield      #18                 // Field age:I
          16: getstatic     #20                 // Field java/lang/System.out:Ljava/
    io/PrintStream;
          19: ldc           #26                 // String Animal
          21: invokevirtual #28                 // Method java/io/PrintStream.printl
    n:(Ljava/lang/String;)V
          24: return

      public com.appleyk.reflect.Animal(java.lang.String, int);	**有参构造方法**
        Code:
           0: aload_0
           1: invokespecial #12                 // Method java/lang/Object."<init>":
    ()V
           4: aload_0
           5: ldc           #14                 // String Dog
           7: putfield      #16                 // Field name:Ljava/lang/String;
          10: aload_0
          11: bipush        30
          13: putfield      #18                 // Field age:I
          16: getstatic     #20                 // Field java/lang/System.out:Ljava/
    io/PrintStream;
          19: new           #39                 // class java/lang/StringBuilder
          22: dup
          23: aload_1
          24: invokestatic  #41                 // Method java/lang/String.valueOf:(
    Ljava/lang/Object;)Ljava/lang/String;
          27: invokespecial #47                 // Method java/lang/StringBuilder."<
    init>":(Ljava/lang/String;)V
          30: ldc           #49                 // String ,
          32: invokevirtual #51                 // Method java/lang/StringBuilder.ap
    pend:(Ljava/lang/String;)Ljava/lang/StringBuilder;
          35: iload_2
          36: invokevirtual #55                 // Method java/lang/StringBuilder.ap
    pend:(I)Ljava/lang/StringBuilder;
          39: invokevirtual #58                 // Method java/lang/StringBuilder.to
    String:()Ljava/lang/String;
          42: invokevirtual #28                 // Method java/io/PrintStream.printl
    n:(Ljava/lang/String;)V
          45: return

      public java.lang.String sayName(java.lang.String);	**public方法**
        Code:
           0: new           #39                 // class java/lang/StringBuilder
           3: dup
           4: ldc           #64                 // String Hello,
           6: invokespecial #47                 // Method java/lang/StringBuilder."<
    init>":(Ljava/lang/String;)V
           9: aload_1
          10: invokevirtual #51                 // Method java/lang/StringBuilder.ap
    pend:(Ljava/lang/String;)Ljava/lang/StringBuilder;
          13: invokevirtual #58                 // Method java/lang/StringBuilder.to
    String:()Ljava/lang/String;
          16: areturn
    }