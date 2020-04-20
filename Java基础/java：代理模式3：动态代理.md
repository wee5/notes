# java：代理模式3：动态代理

* 动态代理优势：静态代理中，一个目标类需要几个代理类，就编写几个代理类；若同样的代理，代理了不同的两个目标类，则**代理类内容重复**，代码冗余；这是静态代理的劣势，也是动态代理的优势，动态代理可以解决



## 代码

* Movable接口

  * ```java
    public interface Movable {
        void move();
    }
    ```

* Tank类

  * ```java
    public class Tank implements Movable {
    
        public void move() {
            // 坦克移动
            System.out.println("Tank Moving......");
            try {
                Thread.sleep(new Random().nextInt(5000)); // 随机产生 1~5秒, 模拟坦克在移动　
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    ```

* Flyable接口

  * ```java
    public interface Flyable {
        void fly();
    }
    ```

* Plane类

  * ```java
    public class Plane implements Flyable {
    
        public void fly() {
            System.out.println("Plane Flying......");
            try {
                Thread.sleep(new Random().nextInt(5000)); // 随机产生 1~5秒, 飞机在飞行　
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    ```

* MyTimeProxyInvocationHandler类：即动态代理类，需要实现方法（将需要做的事写在方法内）；因为参数是Object，所以可以代理不同的类

  * ```java
    public class MyTimeProxyInvocationHandler implements InvocationHandler {
    
        private Object target;//存储被代理类，因为可以为任何类代理，所以是Object
    
        public MyTimeProxyInvocationHandler(Object target) {
            this.target = target;
        }
    
        /*
        * proxy  : 代理对象  可以是一切对象 (Object)
        * method : 目标方法
        * args   : 目标方法的参数
        */
        public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
    
            // 在前面做一些事情: 记录开始时间
            long start = System.currentTimeMillis();
            System.out.println("start time : " + start);
    
            method.invoke(target, args); // 调用目标方法  invoke是调用的意思, 可以有返回值的方法(我们这里move和fly都没有返回值)
    
            // 在后面做一些事情: 记录结束时间,并计算move()运行时间
            long end = System.currentTimeMillis();
            System.out.println("end time : " + end);
            System.out.println("spend all time : " + (end - start)/1000 + "s.");
            return null;
        }
    }
    ```

* 测试类

  * ```java
    public class Client {
    
        public static void main(String[] args){
    
            Movable tank = new Tank();
            //可以为所有对象产生时间代理的 InvocationHandler
            MyTimeProxyInvocationHandler myInvocationHandler = new MyTimeProxyInvocationHandler(tank);
    
            /*
            * 代理类实例化
            *
            * 通过jdk的java.lang.reflect.Proxy类实现动态代理，
            * 其动态方法newProxyInstance(),依据目标对象，业务接口，业务增强逻辑，自动生成动态代理对象
            *
            * tank.getClass() 得到tank字节码对象，反射机制的体现
            * load：目标类的类加载器，通过对象反射获取
            * interfaces：目标类实现的接口数组，通过目标对象的反射可获得
            * handler：业务增强逻辑，定义在InvocationHandler类型的实例中的
            * */
            Movable tankProxy = (Movable) Proxy.newProxyInstance(
                    tank.getClass().getClassLoader(),
                    tank.getClass().getInterfaces(),
                    myInvocationHandler
            );
            /*
            *Proxy.newProxyInstance(*,*,*)内部会调用myInvocationHandler.invoke(*,*,*)
            *invoke方法的参数，在newProxyInstance内部通过newProxyInstance的传参获得
            */
            tankProxy.move();
          
            
            //代理飞机，原理同上
            Flyable plane = new Plane();
            myInvocationHandler = new MyTimeProxyInvocationHandler(plane);
            // 为飞机产生代理, 为..产生代理，这样可以为很多东西产生代理，静态代理做不到
            Flyable planeProxy = (Flyable) Proxy.newProxyInstance(
                    plane.getClass().getClassLoader(),
                    plane.getClass().getInterfaces(),
                    myInvocationHandler
            );
            planeProxy.fly();
        }
    }
    ```