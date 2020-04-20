# java：代理模式2：静态代理

* 代理类在程序运行前就已经定义好，其与目标类的关系在程序运行前就已经确立



## 原理

* 一个类Tank时，调用Tank.move();
* Tank类实现Movable接口时，调用(Movable)target.move();
* Tank类和TimeProxy类实现Movable接口时，调用(Movable)target.move();
* 因为类型和方法都是固定的，不影响他们在被依赖类的内部调用代码（不同的只是变量名，而变量名是任意的）



## 示例

* 描述：一个可以移动的坦克，它的主要方法是 `move() `，但是我们需要记录它移动的时间，以及在它移动前后做日志
* 静态代理结构：
  * Tank实例化TimeProxy接口，LogProxy引用该接口
  * ![avatar](图片引用\1560259472971678.jpg)

* 两个代理类结构关系
  * ![avatar](图片引用\1560259472124202.jpg)

### 代码

* Movable接口

  * ```java
    public interface Movable {
        void move();
    }
    ```

* Tank类

  * ```java
    public class Tank implements Movable{ //目标对象
    
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

* TankTimeProxy

  * ```java
    public class TankTimeProxy implements Movable{    //计时代理对象
        private Movable tank;
    
        public TankTimeProxy(Movable tank) {
            this.tank = tank;
        }
    
        public void move() {
            System.out.println("Tank Log start.......");// tank 移动前记录日志
            tank.move();
            System.out.println("Tank Log end.......");// tank 移动后记录日志
        }
    }
    ```

* TankLogProxy

  * ```java
    public class TankLogProxy implements Movable{ //  日志代理对象
        private Movable tank;
    
        public TankLogProxy(Tank tank) {
            this.tank = tank;
        }
    
        public void move() {
            // 在前面做一些事情: 记录开始时间
            long start = System.currentTimeMillis();
            System.out.println("start time : " + start);
    
            tank.move();
    
            // 在后面做一些事情: 记录结束时间,并计算move()运行时间
            long end = System.currentTimeMillis();
            System.out.println("end time : " + end);
            System.out.println("spend all time : " + (end - start)/1000 + "s.");
        }
    }
    ```

* 测试类

  * ```java
    public class Client {
        public static void main(String[] args) {
    
            //两个选一个
            //Movable target=new TankLogProxy(new Tank());
            Movable target=new TankTimeProxy(new Tank());
    
            target.move();
    
    
            /*
            * 两个代理对象内部都 有着被代理对象(target)实现的接口的引用 ；
            * 且两个代理对象都 实现了被代理对象(target)实现的接口 ；
            *
            * 这样不管是单独使用目标类，还是使用代理类
            * 使用时生成的类都是Movable类型，执行方法都是target.move()
            * 即Movable target=new 对象();target.move();
            * 需要什么代理就new什么
            * */
        }
    }
    ```

