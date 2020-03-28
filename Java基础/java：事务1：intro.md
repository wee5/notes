# java：事务1：intro

* **定义：**一般指要做的或所做的事情；计算机术语：指访问并可能更新数据库中各种数据项的一个程序执行单元
* **意义：**事务是为解决数据安全操作提出的，事务控制实际上就是控制数据的安全访问
* 编程式事务：
  * 由程序员编程事务控制代码
  * OpenSessionInView编程式事务
* 声明式事务
  * 事务控制代码由spring写好，程序员只需要申明哪些方法需要事务控制



## 事务的四个特性（ACID）

* **原子性：**事务是数据库的**逻辑工作单位**，而且必须是原子工作单位；对于数据修改，do or not
* **一致性：**事务在完成时，必须是所有的数据都**保持一致状态**；在相关数据库中，所有规则都必须应用于事务的修改，以保持所有的数据的完整性；
* **隔离性：**一个事务的执行不能被其他事务影响
* **持久性：**一个事务一旦提交，**事务的操作**便永久性的保存在DB中；即便实在数据库系统遇到故障的情况下也不会丢失提交事务的操作



## 事务管理器

* **spring不直接管理事务，而是提供多种事务管理器；为不同的事务API提供一致的编程模型**

### JDBC事务

* spring-mybatis.xml文件中：
  * <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
            <property name="dataSource" ref="dataSource" />
        </bean>

### Java持久化API事务（JPA）

* spring-mybatis.xml文件中：
  * <bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager">
            <property name="sessionFactory" ref="sessionFactory" />
        </bean>
* JpaTransactionManager只需要装配一个JPA实体管理工厂（javax.persistence.EntityManagerFactory接口的任意实现）。JpaTransactionManager将与由工厂所产生的JPA EntityManager合作来构建事务。

### Java原生API事务

* 适用：**跨越多个事务管理源；入多个不同数据源**
* spring-mybatis文件中：
  * <bean id="transactionManager" class="org.springframework.transaction.jta.JtaTransactionManager">
            <property name="transactionManagerName" value="java:/TransactionManager" />
        </bean>



## 基本事务属性的定义

* 基本事务的属性包含：**传播行为，隔离规则，回滚规则，事务超时，是否只读**

### 传播行为

* **当事务方法被另一个事务方法调用时，必须指定事务如何传播**
* 各厂商对传播行为的支持有所差异

| 传播行为                  | 含义                                                         |
| ------------------------- | ------------------------------------------------------------ |
| PROPAGATION_REQUIRED      | 表示当前方法**必须运行在事务中**；若当前事务存在，则运行该方法，否则**启动一条新事务** |
| PROPAGATION_SUPPORTS      | 表示当前方法**不需要事务上下文**；但若存在当前事务，方法也会在当前事务运行 |
| PROPAGATION_MANDATORY     | 表示该方法**必须在事务中运行**；当前事务不存在则**抛出异常** |
| PROPAGATION_REQUIRED_NEW  | 表示当前方法必须**运行在它自己的事务中**；一个事务将被启动，若存在当前事务，该方法执行期间，当前事务会被挂起；若使用JTATransactionManager，则需要访问TransactionManager |
| PROPAGATION_NOT_SUPPORTED | 表示该方法**不应该运行在事务中**；若存在当前事务，该方法运行期间，**当前事务被挂起**；若使用JTATransactionManager，则需要访问TransactionManager |
| PROPAGATION_NEVER         | 表示当前方法**不应该运行在事务上下文中**；若当前正有一个事务在运行，则会**抛出异常** |
| PROPAGATION_NESTED        | 表示若当前存在一个事务，则该方法会**在嵌套事务中运行**；**嵌套事务可以独立于当前事务进行单独地提交或回滚**；若当前事务不存在，其行为与PROPAGATION_REQUIRED |

### 隔离级别

* 定义一个事务可能受到其他并发事务影响的程度
* 并发事务引起的问题
  * **脏读：**一个事务**读取**另一个事务尚未提交的数据；若改写被回滚，则读取的数据时无效的
  * **不可重复读：**一个事务前后两次**查询**一个数据，得到不同结果；通常因为查询期间另一个并发事务更新了该数据
  * **幻读：**一个事务先**读取**了几行数据，再**查询**时发现**多或少**了一些**记录**；通常因为期间另一个并发事务插入或删除了数据
  * **控制角度看：**
    * 不可重复读：需要锁住瞒住条件的记录
    * 幻读：需要锁住瞒住条件极其相近的记录

| 隔离级别                   | 含义                                                         |
| -------------------------- | ------------------------------------------------------------ |
| ISOLATION_DEFAULT          | 使用后端数据库默认的隔离级别                                 |
| ISOLATION_READ_UNCOMMITTED | 最低隔离级别，**允许读取尚未提交**的数据变更，导致**脏读，不可重复读** |
| ISOLATION_READ_COMMITTED   | **允许读取并发事务已经提交**的数据，阻止**脏读**，导致**不可重复读，幻读** |
| ISOLATION_REPEATABLE_READ  | 同一字段多次读取结果都是一致的，除非数据本身事务所修改，阻止**脏读，不可重复读**，导致**幻读** |
| ISOLATION_SERIALIZABLE     | 最高隔离界别，完全服从ACID的隔离级别，阻止**脏读，不可重复读，幻读**， 最慢的事务隔离级别，通过完全锁定事务相关数据库实现 |

### 只读

* 设置事务只读后，数据库可以进行特定的优化

### 事务超时

* 事务不能运行太长时间，因为事务可能锁定后端数据库，占用资源
* 事务超时是事务的定时器，在特定时间内事务没有执行完，就会**自动回滚**，而不是等待其结束

### 回滚规则

* 规定哪些异常会导致事务回滚而哪些不会；默认情况下，事务只遇到运行期异常时才会回滚，遇到检查型异常时，不回回滚；
* 可以声明事务在遇到特定检查型异常时像遇到运行期异常那样回滚，或遇到特定异常不会滚，即使是运行期异常

### 事务状态

* 略