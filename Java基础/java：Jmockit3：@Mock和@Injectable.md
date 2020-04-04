# java：Jmockit3：@Mocked和@Injectable

* **@Mocked作用于测试类成员**

  * 整个测试类中被mock的类型的所有实例都将被mocked，被mocked的类所有非private方法都将处于mocked状态,被mocked的方法默认返回值null

* public class MockedAndInjectableTest1
  {
      @Mocked
      ToBeMocked instance;

      @Test
      public void test()
      {
          System.out.println(instance.fun()); //null
      
          ToBeMocked newInstance = new ToBeMocked();
          System.out.println(newInstance.fun()); //null
      }
  }

  

* **@Mocked作用于测试用例参数**

  * 该用例参数中被注解的类型将处于mock状态

* public class MockedAndInjectableTest
  {
      @Test
      public void test1(@Mocked final ToBeMocked instance)
      {
          System.out.println(instance.fun()); //null

          ToBeMocked newInstance = new ToBeMocked();
          System.out.println(newInstance.fun()); //null
      }
      
      @Test
      public void test2()
      {
          ToBeMocked newInstance = new ToBeMocked();
          System.out.println(newInstance.fun()); //call original method
      }
  }



* **@Injectable作用于测试类成员**

  * 被注解的单个实例在整个测试类中将处于mocked状态

* public class MockedAndInjectableTest
  {
      @Injectable
      ToBeMocked instance;

      @Test
      public void test()
      {
          System.out.println(instance.fun()); //null
      
          ToBeMocked newInstance = new ToBeMocked();
          System.out.println(newInstance.fun()); //call original method
      }
  }



* **@Injectable作用于测试类参数**

  * 被注解的单个实例在该测试用例中将处于mocked状态

* public class MockedAndInjectableTest
  {
      @Test
      public void test1(@Injectable final ToBeMocked instanceA)
      {
          System.out.println(instanceA.fun()); //null

          ToBeMocked newInstance = new ToBeMocked();
          System.out.println(newInstance.fun()); //call original method
      }
      
      @Test
      public void test2()
      {
          ToBeMocked newInstance = new ToBeMocked();
          System.out.println(newInstance.fun()); //call original method
      }
  }

