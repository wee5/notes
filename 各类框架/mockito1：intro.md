# mockito1：intro



参考：https://blog.csdn.net/wslyk606/article/details/81772010



## 依赖

```xml
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-all</artifactId>
    <version>2.0.2-beta</version>
</dependency>

<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.9</version>
    <scope>test</scope>
</dependency>
```



## mock对象和stub桩

mock对象：替代实际对象的对象，该模拟对象可以通过特定参数调用特定的方法，并返回预期结果

stub桩：替代具体功能的程序段，对调用方法的模拟，桩程序可以用来模拟已有程序的行为或是对未完成开发程序的一种临时替代



## 前提

```java
@RunWith(MockitoJUnitRunner.class)
public class MyTest {
    @Mock
    private List mockList;

    @Test
    public void test_method() {
        //各种验证，下面有列举
    }
}
```





## 验证行为verify some behavior（是否发生）

```java
@Test
public void verify_behaviours() {
    //using mock object
    mockList.add("one");
    mockList.clear();

    //验证行为是否发生
    verify(mockList).add("one");
    verify(mockList).clear();
}
```



## stub期望结果

```java
@Test 
public void when_thenReturn() {
    when(mockList.get(0)).thenReturn("first");
    when(mockList.get(1)).thenThrow(new RuntimeException());

    assertEquals("first", mockList.get(0));
    try {
        mockList.get(1);
    } catch (RuntimeException exception) {
        System.out.println(exception.getMessage());
    }
}
```



## 验证调用次数

```java
@Test 
public void verify_times(){
    mockList.add("once");

    mockList.add("twice");
    mockList.add("twice");

    mockList.add("three times");
    mockList.add("three times");
    mockList.add("three times");
    //下面的两种写法是一样的，times(1)默认使用
    verify(mockList).add("once");
    verify(mockList,times(1)).add("once");

    verify(mockList,times(2)).add("twice");
    verify(mockList,times(3)).add("three times");

    //never()表示从来没有的
    verify(mockList,never()).add("never happened");

    //atLeast,atMost意思也很明显
    verify(mockList,atLeastOnce()).add("three times");
    verify(mockList,atLeast(2)).add("three times");
    verify(mockList,atMost(5)).add("three times");
}
```



## 模拟方法抛出异常

```java
@Test(expected = RuntimeException.class)
public void doThrow_when(){
    doThrow(new                    RuntimeException()).when(mockList).clear();

    //下面的语句抛出RuntimeException
    //如果不加expected的方式会抛出异常
    mockList.clear();
}
```



## 验证顺序

@Test 
public void verify_order() {
    //A.单个mock，其方法必须以特定顺序调用
    List singleMock = mock(List.class);

    //使用单个mock
    singleMock.add("was added first");
    singleMock.add("was added second");
    
    //为单个mock创建一个顺序验证程序
    InOrder inOrder1 = inOrder(singleMock);
    
    //以下是调用顺序的验证
    inOrder1.verify(singleMock).add("was added first");
    inOrder1.verify(singleMock).add("was added second");
    
    //B.多个mock必须按特定顺序调用
    List firstMock = mock(List.class);
    List secondMock = mock(List.class);
    
    firstMock.add("was called first");
    secondMock.add("was called second");
    
    InOrder inOrder2 = inOrder(firstMock,secondMock);
    
    inOrder2.verify(firstMock).add("was called first");
    inOrder2.verify(secondMock).add("was called second");
}



## 确保模拟对象没有互动发生

```java
@Test 
public void verify_zero_interactions() {
    //mock- 只有mockOne发生交互
    List mockOne = mock(List.class);
    List mockTwo = mock(List.class);
    List mockThree = mock(List.class);

    mockOne.add("one");
    
    //普通验证
    verify(mockOne).add("one");
    //验证从未在模拟上调用该方法
    verify(mockOne, never()).add("two");
    //验证其他模拟没有交互
    verifyZeroInteractions(mockTwo, mockThree);
}
```



## 查找冗余调用

```java
@Test 
public void verify_no_more_interactions(){
    List mockList = mock(List.class);

    mockList.add("one");
    mockList.add("two");

    verify(mockList).add("one");
    
    //以下验证将失败
    verifyNoMoreInteractions(mockList);
}
```



## 连续调用

@Test public void test_consecutive_calls() {
    List mockList = mock(List.class);

    when(mockList.get(0))
            .thenThrow(new RuntimeException())
            .thenReturn("foo");
    
    try {
        //第一次调用会抛出异常，为了保证正常运行进行捕获
        mockList.get(0);
    } catch (RuntimeException e) {
        System.out.println(e.getMessage());
    }
    //后面的调用输出结果都为 "foo"
    System.out.println(mockList.get(0));
    System.out.println(mockList.get(0));
}



## 通过回调生成期望值(callbacks)

@Test 
public void test_callbacks(){
    List mockList = mock(List.class);

    when(mockList.get(anyInt())).thenAnswer(
            new Answer<Object>() {
                @Override
  public Object answer(InvocationOnMock in) throws Throwable {
                    Object[] args = in.getArguments();
                    Object mock = in.getMock();
                    return "called with arguments: " + Arrays.toString(args);
                }
            }
    );

    assertEquals("called with arguments: [0]",mockList.get(0));
    assertEquals("called with arguments: [1]",mockList.get(1));
    assertEquals("called with arguments: [2]",mockList.get(2));
}



## doReturn()、doThrow()、doAnswer()、doNothing()、doCallRealMethod()方法

略



## Spy真实对象

@Test 
public void test_spy_on_real_objects() {
    List list = new LinkedList();
    List spy = spy(list);

    when(spy.size()).thenReturn(100);
    
    spy.add("one");
    spy.add("two");
    
    System.out.println(spy.size());
    System.out.println(spy.get(0));
    
    verify(spy).add("one");
    verify(spy).add("two");
}

@Test 
public void test_spy_on_real_objects() {
    List list = new LinkedList();
    List spy = spy(list);

    doReturn(100).when(spy).size();
    doReturn("foo").when(spy).get(0);
    
    System.out.println(spy.size());
    System.out.println(spy.get(0));
}