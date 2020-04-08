# java：Jmockit2：使用

* 使用JMockit API来mock被依赖的代码，从而进行隔离测试。

  - 类级别整体mock和部分方法重写
  - 实例级别整体mock和部分mock
  - mock静态方法、私有变量、局部方法
  - 灵活的参数匹配

* 依赖：

  * 如果使用Junit4.5以上，jmockit依赖需要在junit4之前；或者在测试类上添加注解 @RunWith(JMockit.class)。

  * 如果是TestNG 6.2+ 或者 JUnit 5+， 没有位置限制

  * 		<dependency>
          			<groupId>org.jmockit</groupId>
          			<artifactId>jmockit</artifactId>
          			<version>1.30</version>
          			<scope>test</scope>
          		</dependency>
          		<dependency>
          			<groupId>junit</groupId>
          			<artifactId>junit</artifactId>
          			<version>4.11</version>
          			<scope>test</scope>
          		</dependency>

* 基本流程：
  * **record**：录制；设置将要被调用的方法和返回值
    * Expections中的方法至少被调用一次，否则会出现missing  invocation错误，调用次数和顺序不限
    * StricExpectations中方法调用的次数和顺序都必须严格执行。若出现了在StrictExpectations中没有申明的方法，会出现unexpected invocation错误
  * **replay**：回放；调用（未被）录制的方法，被录制的方法调用会被JMockit拦截并重定向到record阶段设定的行为
  * **verify**：验证；基于行为的验证，测试CUT是否正确调用类依赖类，包括调用了哪些方法，通过怎样的参数，调用了多少次调用的相对顺序（VerificationsInOrder）等；可以使用times，minTimes，maxTimes来验证



## 基本使用

- @Test
  public void mockProcessTest(final @Mocked PersonService target){
  //录制预期行为
  new Expectations(){
  {
  target.showName(anyString);
  result = "test1";
  }
  };
  //测试代码
  Assert.assertTrue("test1".equals(target.showName("test2")));
  //验证
  new Verifications(){
  {
  target.showName("test1");
  times = 0; //执行了0次。参数一致的才会计数
  }
  };
  }



## 详细使用

- 所有实例所有方法：**@Mocked**
  - 所有实例被mock；未录制方法返回默认值
  - **mocked可以作用在测试类成员变量，测试类方法参数，Expectations中**
- 指定实例所有方法：**@Injectable**
  - 被mock的实例，未录制方法返回默认值；未被mock的实例，调用原始方法
- 实例级别的部分方法：**@new Expectations（personService）**
  - 录制方法按录制返回，未录制方法返回默认值
- 类级别的部分方法：**new MockUp<PersonService>(){**
  - @Mock
  - public String showName（String name）{
    - reture "mocked"；}}
  - 被录制方法按录制返回，未录制方法返回默认值
- 静态方法：**new Expectations（CollectionUtils.class）{{**
  - COllectionUtils.isEmpty（（Collection<?>） any）；
  - result=true；}}
- mock私有变量和局部方法
  - **new Expectations（personService）**{{
  - Deencapsulation.setField（类名，变量名，变量值）；
  - Deencapsulation.invoke（类名，方法名，参数）；
  - result=返回值；
- 参数问题
  - 基本类型
    - 任意值：anyXXX
    - 实际值：需要录制时参数和回放时参数相同
  - 复合类型
    - 任意值：（复合类型）any
    - 实际值：需要录制时参数和回放时参数相同
- 注入：
  - 不要同时在参数和类中注入，会影响执行结果
- Expectations和StrictExpectations
  - Expectations中方法至少调用一次
  - StrictExpectations中方法调用次数和顺序要严格执行



## 代码展示

* ```
  public class PersonTest {
  
      @Mocked
      Person person;
  
      @Test
      public void test(){//所有实例所有方法
          new Expectations(){
              {
                  person.method(anyString);
                  result="iphoneplus";
              }
          };
  
          Assert.assertEquals("iphoneplus",person.method("iphone"));//录制方法
          Assert.assertEquals(null,person.method2("iphone"));//未录制方法
  
          new Verifications(){
              {
                  person.method("iphone");
                  times=1;
              }
          };
      }
  
      @Test
      public void getName() {//实例级别部分方法
          new Expectations(person){{
              person.method(anyString);
              result="anyplus";
          }
          };
          Assert.assertEquals("anyplus",person.method("iphone "));//录制方法
          Assert.assertEquals("anyweezy",person.method2("any"));//未录制方法
          new Verifications(){
              {
                  person.method("iphone ");
                  times=1;
                  person.method2("any");
                  times=1;
              }
          };
      }
  
  }
  ```

- ```
  public class ManTest {
  
      @Injectable
      Man man;
  
      @Test
      public void method() {//指定实例的所有方法
          new Expectations(){
              {
                  man.method(anyString);
                  result="nothing";
              }
          };
  
          Assert.assertEquals("nothing",man.method("something"));//指定实例的录制方法
          Assert.assertEquals(null,man.method2("s"));//指定实例的未录制方法
          Assert.assertEquals("splus",new Man().method("s"));//新建实例的所有方法
  
          new Verifications(){
              {
                  man.method("something");
                  times=1;
                  man.method2("s");
                  times=1;//不计算new出来的对象，对象都不是同一个
              }
          };
      }
  
      @Test
      public void InjectableTest(){//类级别部分方法,与Injectable注入不冲突，可以省略注入
          new MockUp<Person>(){
              @Mock
              public String method2(String s){
                  return s+"plus";
              }
          };
          Assert.assertEquals("iphoneplus",new Person().method2("iphone"));
          Assert.assertEquals("weezyplus",new Person().method("weezy"));
      }
  
  }
  ```



- 参考：https://www.cnblogs.com/s