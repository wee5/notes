# java：事务4：声明式事务实例

* 数据库表 

  * book(isbn, book_name, price) 
    account(username, balance) 
    book_stock(isbn, stock)

  

* XML配置

  * ```xml
    <beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context-3.0.xsd
    http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.5.xsd
    http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-2.5.xsd"><import resource="applicationContext-db.xml" />
    	
        <!-- 扫描事物类 -->
    	<context:component-scan base-package="com.springinaction.transaction"></context:component-scan>
    
        <!-- 开启事物注解 -->
    	<tx:annotation-driven transaction-manager="txManager"/>
    
        <!-- jdbc事物管理器 -->
    	<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        	<property name="dataSource" ref="dataSource" />
    	</bean>
        
    </beans>
    ```

    

* 代码

  * ```java
    //BookShopDao
    public interface BookShopDao {
        // 根据书号获取书的单价
        public int findBookPriceByIsbn(String isbn);
        // 更新书的库存，使书号对应的库存-1
        public void updateBookStock(String isbn);
        // 更新用户的账户余额：account的balance-price
        public void updateUserAccount(String username, int price);
    }
    ```

  * ```java
    //BookShopDaoImpl
    @Repository("bookShopDao")
    public class BookShopDaoImpl implements BookShopDao {
        
    	@Autowired
    	private JdbcTemplate JdbcTemplate;
    
    	@Override
    	public int findBookPriceByIsbn(String isbn) {
            String sql = "SELECT price FROM book WHERE isbn = ?";
            return JdbcTemplate.queryForObject(sql, Integer.class, isbn);
    	}
    
    	@Override
    	public void updateBookStock(String isbn) {
        	//检查书的库存是否足够，若不够，则抛出异常
        	String sql2 = "SELECT stock FROM book_stock WHERE isbn = ?";
        	int stock = JdbcTemplate.queryForObject(sql2, Integer.class, isbn);
        	if (stock == 0) {
            	throw new BookStockException("库存不足！");
        	}
        	String sql = "UPDATE book_stock SET stock = stock - 1 WHERE isbn = ?";
        	JdbcTemplate.update(sql, isbn);
    	}
    
    	@Override
    	public void updateUserAccount(String username, int price) {
        	//检查余额是否不足，若不足，则抛出异常
        	String sql2 = "SELECT balance FROM account WHERE username = ?";
            int balance = JdbcTemplate.queryForObject(sql2, Integer.class, username);
        	if (balance < price) {
            	throw new UserAccountException("余额不足！");
        	}       
        	String sql = "UPDATE account SET balance = balance - ? WHERE username = ?";
        	JdbcTemplate.update(sql, price, username);
    	}
    }
    ```

  * ```java
    //BookShopService
    public interface BookShopService {
         public void purchase(String username, String isbn);
    }
    ```

  * ```java
    //BookShopServiceImpl
    @Service("bookShopService")
    public class BookShopServiceImpl implements BookShopService {
    
        @Autowired
    	private BookShopDao bookShopDao;
        
        /**
     	* 1.添加事务注解
     	* 使用propagation 指定事务的传播行为，即当前的事务方法被另外一个事务方法调用时如何使用事务。
     	* 默认取值为REQUIRED，即使用调用方法的事务
     	* REQUIRES_NEW：使用自己的事务，调用的事务方法的事务被挂起。
     	*
     	* 2.使用isolation 指定事务的隔离级别，最常用的取值为READ_COMMITTED
     	* 3.默认情况下 Spring 的声明式事务对所有的运行时异常进行回滚，也可以通过对应的属性进行设置。通常情况下，默认值即可
     	* 4.使用readOnly 指定事务是否为只读。 表示这个事务只读取数据但不更新数据，这样可以帮助数据库引擎优化事务。若真的是一个只读取数据库值得方法，应设置readOnly=true
     	* 5.使用timeOut 指定强制回滚之前事务可以占用的时间
     	*/
    	@Transactional(propagation=Propagation.REQUIRED_NEW,isolation=Isolation.READ_COMMITTED,noRollbackFor={UserAccountException.class},readOnly=true, timeout=3)
    	@Override
    	public void purchase(String username, String isbn) {
        	//1.获取书的单价
        	int price = bookShopDao.findBookPriceByIsbn(isbn);
        	//2.更新书的库存
        	bookShopDao.updateBookStock(isbn);
        	//3.更新用户余额
        	bookShopDao.updateUserAccount(username, price);
    	}
    }
    ```

  * ```java
    //Cashier
    public interface Cashier {
        public void checkout(String username, List<String>isbns);
    }
    ```

  * ```java
    //CashierImpl
    //CashierImpl.checkout和bookShopService.purchase联合测试了事务的传播行为
    @Service("cashier")
    public class CashierImpl implements Cashier {
        
        @Autowired
        private BookShopService bookShopService;
        
        @Transactional
    	@Override
    	public void checkout(String username, List<String> isbns) {
        	for(String isbn : isbns) {
            	bookShopService.purchase(username, isbn);
        	}
    	}
    }
    ```

  * ~~~java
    //BookStockException
    public class BookStockException extends RuntimeException {
    
    	```
    	private static final long serialVersionUID = 1L;
    
    	public BookStockException() {
        	super();
        	// TODO Auto-generated constructor stub
    	}
    
    	public BookStockException(String arg0, Throwable arg1, boolean arg2,boolean arg3) {
        	super(arg0, arg1, arg2, arg3);
        	// TODO Auto-generated constructor stub
    	}
    
    	public BookStockException(String arg0, Throwable arg1) {
        	super(arg0, arg1);
        	// TODO Auto-generated constructor stub
    	}
    
    	public BookStockException(String arg0) {
        	super(arg0);
        	// TODO Auto-generated constructor stub
    	}
    
    	public BookStockException(Throwable arg0) {
        	super(arg0);
        	// TODO Auto-generated constructor stub
    	}
    	```
    
    }
    ~~~

  * ```java
    //UserAccountException
    public class UserAccountException extends RuntimeException {
    
    	private static final long serialVersionUID = 1L;
    
    	public UserAccountException() {
        	super();
        	// TODO Auto-generated constructor stub
    	}
    
    	public UserAccountException(String arg0, Throwable arg1, boolean arg2,boolean arg3) {
        	super(arg0, arg1, arg2, arg3);
        	// TODO Auto-generated constructor stub
    	}
    
    	public UserAccountException(String arg0, Throwable arg1) {
        	super(arg0, arg1);
        	// TODO Auto-generated constructor stub
    	}
    
    	public UserAccountException(String arg0) {
        	super(arg0);
        	// TODO Auto-generated constructor stub
    	}
    
    	public UserAccountException(Throwable arg0) {
        	super(arg0);
        	// TODO Auto-generated constructor stub
    	}
    }
    ```

  * ```java
    //测试类
    public class SpringTransitionTest {
    	private ApplicationContext ctx = null;
    	private BookShopDao bookShopDao = null;
    	private BookShopService bookShopService = null;
    	private Cashier cashier = null;
    	
    	{
        	ctx = new ClassPathXmlApplicationContext("config/transaction.xml");
        	bookShopDao = ctx.getBean(BookShopDao.class);
        	bookShopService = ctx.getBean(BookShopService.class);
        	cashier = ctx.getBean(Cashier.class);
    	}
    
    	@Test
    	public void testBookShopDaoFindPriceByIsbn() {
        	System.out.println(bookShopDao.findBookPriceByIsbn("1001"));
    	}
    
    	@Test
    	public void testBookShopDaoUpdateBookStock(){
        	bookShopDao.updateBookStock("1001");
    	}
    
    	@Test
    	public void testBookShopDaoUpdateUserAccount(){
        	bookShopDao.updateUserAccount("AA", 100);
    	}
    	
    	@Test
    	public void testBookShopService(){
        	bookShopService.purchase("AA", "1001");
    	}
    
    	@Test
    	public void testTransactionPropagation(){
        	cashier.checkout("AA", Arrays.asList("1001", "1002"));
    	}
    }
    ```

    

* 参考https://blog.csdn.net/trigl/article/details/50968079