# Spring Aop配置

* 需要jar包：

## xml配置实现

* 示例（命名空间略，下图中为‘定义切面’，而非‘定义增强’）

![1576684118703](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1576684118703.png)

| aop             | method        | returning/throwing |
| --------------- | ------------- | ------------------ |
| before          | before        |                    |
| after-returning | afterReturn   | returnVal          |
| around          | around        |                    |
| after           | after         |                    |
| after-throwing  | afterThrowing | throwable          |

* returnVal必须与类中声明的名称一样
* throwing="throwable必须与类中声明的名称一样
* execution格式：
* * （* com.company.dao.Class.method(..))
* * (返回类型  包名.类名.方法（参数）)
  * 包名. . * . * (. .)：包及子包下所有类的方法
  * 包名 . * . * (. .)：包下所有类的方法



## 注解实现

### 示例

* 编写切面类

* ```
  @Aspect
  public class MyAspect {
  
      @Before("execution(* com.zejian.spring.springAop.dao.UserDao.addUser(..))")
      public void before(){}
  
      @AfterReturning(value="execution(* com.zejian.spring.springAop.dao.UserDao.addUser(..))",returning = "returnVal")
      public void AfterReturning(Object returnVal){}
  
      @Around("execution(* com.zejian.spring.springAop.dao.UserDao.addUser(..))")
      public Object around(ProceedingJoinPoint joinPoint) throws Throwable {
          System.out.println("环绕通知前....");
          Object obj= (Object) joinPoint.proceed();
          System.out.println("环绕通知后....");
          return obj;}
  
      @AfterThrowing(value="execution(* com.zejian.spring.springAop.dao.UserDao.addUser(..))",throwing = "e")
      public void afterThrowable(Throwable e){
          System.out.println("出现异常:msg="+e.getMessage());}
  
      @After(value="execution(* com.zejian.spring.springAop.dao.UserDao.addUser(..))")
      public void after(){}
      
  } 
  ```

* spring ioc配置文件（命名空间略）

* ```
  <!-- 启动@aspectj的自动代理支持-->
      <aop:aspectj-autoproxy />
  <!--定义aspect类-->
  <bean .../>
  
  ```

### 内容扩充

* 切入点函数

* ```
  <!--定义切点函数-->
  @Pointcut("execution(* com.zejian.spring.springAop.dao.UserDao.addUser(..))")
  private void myPointcut(){}
  
  <!--应用切点-->
  @After(value="myPointcut()")
  public void afterDemo(){}
  ```



* 高阶（https://www.cnblogs.com/junzi2099/p/8274813.html）
* 切入点指示符
* * 通配符
  * 类型签名表达式
  * 方法签名表达式
  * 其他指示符



* 传递参数

* 参数也作为’切入点指示符‘去匹配连接点，表示包含或不仅包含userId的连接点

* ``` 
  @Before("execution(public * com.zejian..*.addUser(..)) && args(userId,..)")  
  public void before(int userId) {  
      //调用addUser的方法时如果与addUser的参数匹配则会传递进来会传递进来
      System.out.println("userId:" + userId);  
  } 
  ```

* aspect优先级

* 同一切面的增强织入同一切点函数，根据在切面类中的声明顺序执行

* 不同切面的增强织入同一切点函数，根据优先级执行

* 优先级原则：返回值越低，优先级越高，越远离连接点；即提前进场，最后离场

* 格式:实现Ordered，并重写方法getOrder（）

* * ```
    @Override
        public int getOrder() {
            return 0;
        }
    ```

