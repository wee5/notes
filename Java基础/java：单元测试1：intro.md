# java：单元测试1：intro

* **定义：**对单独的代码块(方法)分别进行测试,以保证它们的正确性
* **测试方法必须是public  void，即公共、无返回数据。可以抛出异常**
* idea自动添加junit4单元测试，快捷键ctrl+shift+t
* 单元测试核心是**断言**



## 测试类常用注解

| 注解         |                                  |                                            |                              |
| ------------ | -------------------------------- | ------------------------------------------ | ---------------------------- |
| @BeforeClass | 全局只会执行一次，第一个运行     | 包含公共部分的方法，如数据库连接，读取文件 | public，static，void，无返回 |
| @Before      | 测试方法运行之前运行             | 独立用例之间的准备工作                     | public void，非static        |
| @Test        | 测试方法                         |                                            | public void，可以抛异常      |
| @After       | 测试方法运行之后运行             | 对应@Before                                |                              |
| @AfterClass  | 全局只会执行一次，而且是最后执行 | 处理测试后续工作，如数据清理，恢复现场     | public，static，void，无返回 |
| @Ignore      | 忽略此方法                       | 暂时不运行的方法                           |                              |
| @Runwith     |                                  | 放在测试类名之前                           |                              |



## @Runwith

* 概念：

  * **测试方法：**用@Test注解的方法
  * **测试类：**包含一个或多个测试方法的**Test.java文件
  * **测试集：**一个suite，可能包含多个测试类
  * **测试运行器：**决定用什么方式偏好去运行这些测试集/类/方法
  * **@Runwith就是放在 *测试类名* 之前，用来确定这个类怎么运行的。也可以不标注，会使用默认运行器**

* 运行器：

  * @RunWith(Parameterized.class) 参数化运行器，配合@Parameters使用junit的参数化功能

  * @RunWith(Suite.class)

    @SuiteClasses({ATest.class,BTest.class,CTest.class})

    测试集运行器配合使用测试集功能

  * @RunWith(JUnit4.class)

    junit4的默认运行器

  * 一些其它运行器具备更多功能。例如@RunWith(SpringJUnit4ClassRunner.class)集成了spring的一些功能

  * @Parameters

     用于使用参数化功能



## 限时测试

* 对于逻辑复杂，循环嵌套比较深的程序，可能会出现死循环
* 示例：@Test(timeout=1000)



## 异常测试

* 需要抛出异常的方法，实际并未抛出异常，也是一种bug
* @Test(ArithmeticException.class)



## 参数化测试

* 看实例



参考https://blog.csdn.net/qq_36098284/article/details/80684303