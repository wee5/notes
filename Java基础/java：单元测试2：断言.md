# java：单元测试2：断言



| 方法          | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| assertTrue    | 断言条件为真                                                 |
| assertFalse   | 断言条件为假                                                 |
| assertEquals  | 断言两个对象相等                                             |
| assertNotNull | 断言对象不为null                                             |
| assertNull    | 断言对象为null                                               |
| assertSame    | 断言两个引用指向同一个对象                                   |
| assertNotSame | 断言两个引用指向不同对象                                     |
| fail          | 让测试失败，并给出指定信息                                   |
| 断言不满足    | 方法抛出带这相应的信息（若有的话）的AssertionFailedError异常 |

* 补充：
  * 上面方法都可以它的重载方法，在一个位置多带一个参数；若断言不通过，抛出的异常信息就会封装这我们传入的第一个异常参数信息
  * 使用AssertEquals方法时，若比较的的是两个double或者float类型时，第三个参数还可以传入一个误差范围
  * 在使用上面断言时，可以静态一次导入Assert类
  * junit4以后必须import进来org.junit下面的断言类（**import static org.junit.Assert.*;**）
* 具体使用方法：https://blog.csdn.net/km_moon/article/details/84434493
* assertj使用方法：https://www.cnblogs.com/zjk1/p/9299101.html