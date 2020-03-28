# java：事务3：实现声明式事务

* **区别：**spring提供两种事务支持
  * 编程式事务入侵到业务代码里，提供更详细的事务管理
  * 声明式事务基于AOP，实现事务管理的同时，不影响业务代码的具体实现



## 编程式事务

* 实现：略



## 声明式事务

* 参考https://blog.csdn.net/trigl/article/details/50968079

### 配置方式

* 一：每个bean都有一个代理

  * <!-- 定义事务管理器（声明式的事务） --> 
        <bean id="transactionManager"
            class="org.springframework.orm.hibernate3.HibernateTransactionManager">
            <property name="sessionFactory" ref="sessionFactory" />
        </bean>

        <!-- 配置DAO -->
        <bean id="userDaoTarget" class="com.bluesky.spring.dao.UserDaoImpl">
            <property name="sessionFactory" ref="sessionFactory" />
        </bean>
        
        <bean id="userDao" 
            class="org.springframework.transaction.interceptor.TransactionProxyFactoryBean"> 
               <!-- 配置事务管理器 --> 
               <property name="transactionManager" ref="transactionManager" />    
            <property name="target" ref="userDaoTarget" /> 
             <property name="proxyInterfaces" value="com.bluesky.spring.dao.GeneratorDao" />
            <!-- 配置事务属性 --> 
            <property name="transactionAttributes"> 
                <props> 
                    <prop key="*">PROPAGATION_REQUIRED</prop>
                </props> 
            </property> 
        </bean>

* 二：所有bean共享一个代理基类

  * <!-- 定义事务管理器（声明式的事务） --> 
        <bean id="transactionManager"
            class="org.springframework.orm.hibernate3.HibernateTransactionManager">
            <property name="sessionFactory" ref="sessionFactory" />
        </bean>

        <bean id="transactionBase" 
                class="org.springframework.transaction.interceptor.TransactionProxyFactoryBean" 
                lazy-init="true" abstract="true"> 
            <!-- 配置事务管理器 --> 
            <property name="transactionManager" ref="transactionManager" /> 
            <!-- 配置事务属性 --> 
            <property name="transactionAttributes"> 
                <props> 
                    <prop key="*">PROPAGATION_REQUIRED</prop> 
                </props> 
            </property> 
        </bean>   
        
        <!-- 配置DAO -->
        <bean id="userDaoTarget" class="com.bluesky.spring.dao.UserDaoImpl">
            <property name="sessionFactory" ref="sessionFactory" />
        </bean>
        
        <bean id="userDao" parent="transactionBase" > 
            <property name="target" ref="userDaoTarget" />  
        </bean>

* 三：使用拦截器

  * <!-- 定义事务管理器（声明式的事务） --> 
        <bean id="transactionManager"
            class="org.springframework.orm.hibernate3.HibernateTransactionManager">
            <property name="sessionFactory" ref="sessionFactory" />
        </bean> 

        <bean id="transactionInterceptor" 
            class="org.springframework.transaction.interceptor.TransactionInterceptor"> 
            <property name="transactionManager" ref="transactionManager" /> 
            <!-- 配置事务属性 --> 
            <property name="transactionAttributes"> 
                <props> 
                    <prop key="*">PROPAGATION_REQUIRED</prop> 
                </props> 
            </property> 
        </bean>
        
        <bean class="org.springframework.aop.framework.autoproxy.BeanNameAutoProxyCreator"> 
            <property name="beanNames"> 
                <list> 
                    <value>*Dao</value>
                </list> 
            </property> 
            <property name="interceptorNames"> 
                <list> 
                    <value>transactionInterceptor</value> 
                </list> 
            </property> 
        </bean> 
        
        <!-- 配置DAO -->
        <bean id="userDao" class="com.bluesky.spring.dao.UserDaoImpl">
            <property name="sessionFactory" ref="sessionFactory" />
        </bean>

* 四：使用tx标签配置的拦截器

  * <context:annotation-config />
    <context:component-scan base-package="com.bluesky" /><!-- 定义事务管理器（声明式的事务） --> 
        <bean id="transactionManager"
            class="org.springframework.orm.hibernate3.HibernateTransactionManager">
            <property name="sessionFactory" ref="sessionFactory" />
        </bean>

  *     <tx:advice id="txAdvice" transaction-manager="transactionManager">
            <tx:attributes>
                <tx:method name="*" propagation="REQUIRED" />
            </tx:attributes>
        </tx:advice>
        
        <aop:config>
            <aop:pointcut id="interceptorPointCuts"
                expression="execution(* com.bluesky.spring.dao.*.*(..))" />
            <aop:advisor advice-ref="txAdvice"
                pointcut-ref="interceptorPointCuts" />       
        </aop:config>

* 五：全注解（在DAO上加上注解@Transactional）

  * <context:annotation-config />
        <context:component-scan base-package="com.bluesky" />

        <tx:annotation-driven transaction-manager="transactionManager"/>
        
        <bean id="sessionFactory" 
                class="org.springframework.orm.hibernate3.LocalSessionFactoryBean"> 
            <property name="configLocation" value="classpath:hibernate.cfg.xml" /> 
            <property name="configurationClass" value="org.hibernate.cfg.AnnotationConfiguration" />
        </bean> 
        
        <!-- 定义事务管理器（声明式的事务） --> 
        <bean id="transactionManager"
            class="org.springframework.orm.hibernate3.HibernateTransactionManager">
            <property name="sessionFactory" ref="sessionFactory" />
        </bean>



### 一个声明式事务实例

* 参考https://blog.csdn.net/trigl/article/details/50968079

