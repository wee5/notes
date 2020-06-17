# java：异常1：intro



## 概念

**定义**：异常就是有异于常态，和正常情况不一样，有错误出现。在java中，阻止当前方法或作用域的情况，称之为异常

**分类**：![avatar](图片引用\20190612234942627.png)

**Error**：是程序中无法处理的错误，表示运行应用程序中出现了严重的错误。此类错误一般表示代码运行时JVM出现问题，当此类错误发生时，应用不应该去处理此类错误。
通常有Virtual MachineError（虚拟机运行错误）、NoClassDefFoundError（类定义错误）等。比如说当jvm耗完可用内存时，将出现OutOfMemoryError。此类错误发生时，JVM将终止线程。非代码性错误。

**Exception**：程序本身可以捕获并且可以处理的异常

**运行时异常(不受检异常)**：RuntimeException类极其子类表示JVM在运行期间可能出现的错误。编译器不会检查此类异常，并且不要求处理异常
比如用空值对象的引用（NullPointerException）、数组下标越界（ArrayIndexOutBoundException）。此类异常属于不可查异常，一般是由程序逻辑错误引起的，在程序中可以选择捕获处理，也可以不处理。

**非运行时异常(受检异常)**：Exception中除RuntimeException极其子类之外的异常。编译器会检查此类异常，如果程序中出现此类异常
比如说IOException，必须对该异常进行处理，要么使用try-catch捕获，要么使用throws语句抛出，否则编译不通过



## 异常处理

throw：用在方法内，用来抛出一个异常对象，将这个异常对象传递到调用者处，并结束当前方法的执行

```java
throw new 异常类名(参数)
```

throws：用于方法声明之上，用于表示当前方法不处理异常，而是提醒该方法的调用者来处理异常

```java
//修饰符 返回值类型 方法名（参数） throws 异常类名1，异常类名2 ... { }
public static  void readFile() throws FileNotFoundException {
```

try catch finally

```java
try {
   ...  //监视代码执行过程，一旦返现异常则直接跳转至catch，
        // 如果没有异常则直接跳转至finally
} catch (SomeException e) {
    ... //可选执行的代码块，如果没有任何异常发生则不会执行；
        //如果发现异常则进行处理或向上抛出。
} finally {
    ... //必选执行的代码块，不管是否有异常发生，
        // 即使发生内存溢出异常也会执行，通常用于处理善后清理工作。
}
```



## 自定义异常

```java

/**
 * 自定义异常类
 * 用户不存在异常信息类
 */
public class UserNotExistsException extends RuntimeException{
 
    public UserNotExistsException() {
        super();
    }
    public UserNotExistsException(String message) {
        super(message);
    }
}
```



## 自定义异常使用

```java
//自定义异常 继承Exception
public class MyException  extends Exception{
    public MyException(String message){
        //出现异常打印的语句
        super(message);
    }
}

//包含异常的方法
public class Student {
    //显示抛出异常 ，可以同时抛出多个,
    //那么，调用此方法的就必须捕获此异常或者继续抛出
    public void stu(int age) throws MyException,ArithmeticException{
        if(age<18){
            throw new MyException("靓仔，你年龄不够");
        }
        System.out.println("欢迎，报名！");
    }
}

//捕获异常处理
public class Run {
    public static void main(String[] args) {
        Student student = new Student();
        try {
            student.stu(18);
        } catch (ArithmeticException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (MyException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}

//抛异常处理
public class Run {
    public static void main(String[] args) throws ArithmeticException,MyException {
        Student student = new Student();
        student.stu(18);
    }
}
```

