# java：反射3：spring IOC

* spring IOC：即控制反转，通过第三方配置文件实现对对象的控制；简单说，**将设计好的对象交给容器控制，而不是直接交给程序内部进行对象的控制**

* 配置文件

  * ```xml
    <!-- 让spring管理sqlsessionfactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    	<!-- 数据库连接池 -->
        <property name="dataSource" ref="dataSource" />
        <!-- 加载mybatis的全局配置文件 -->
        <property name="configLocation" value="classpath:mybatis/SqlMapConfig.xml" />
    </bean>
    ```

* 伪代码实现

  * ```java
    //解析<bean .../>元素的id属性得到字符串值为
    String id="sqlSessionFactory";
    //解析<bean .../>元素的class属性得到字符串值为
    String class="org.mybatis.spring.SqlSessionFactoryBean";
    
    //得到完全限定名之后，用实例化无参的对象得到object
    Class class1=Class.forName(class);
    Object object=class1.newInstance();
    
    //将object对象放入spring容器管理，container表示spring容器
    container.put(id,object);
    
    //若该类需要另一个类的对象时，继续下面
    
    //解析<propert .../>元素的name属性得到字符串值为
    String name="dataSource";
    //解析<property .../>元素的ref属性得到字符串值为
    String ref="dataSource";
    
    //生成将要调用setter方法名；设置参数时都是调用set方法，像setName等
    String methodName="set"+name.substring(0,1).toUpperCase()+name.subString(1);
    
    //得到spring容器中键为ref的Bean（对象）
    Object paramBean=container.get(ref);
    
    //由字节码对象得到method对象，给定方法名和所属对象
    Method setterMethod=class1.getMethod(methodName,paramBean.getClass());
    //由方法对象唤起（执行）该类方法，给定所属对象和参数
    //该方法设置sqlSeesionFactory对象的dataSource属性值为 id为dataSource对应的对象
    setterMethod.invoke(object,paramBean);
    
    /*	这里只举例设置了第一个属性值，没有设置第二个属性configLocation
    	这个属性的值为classpath:mybatis/SqlMapConfig.xml
    	是配置文件的路径字符串，用来解析另个一配置文件
    	
    	猜想：配置文件路径可以由另一个配置文件包含
    	也可以规定路径及文件名，由spring自动解析
    */
    ```


* **反射的关键：得到完全限定名**

  * 得到完全限定名之前：**解析配置文件**（暂时不了解，涉及到xml知识，配置文件路径看上面）
  * 得到完全限定名之后：**解剖字节码**

