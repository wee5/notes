# java：代理模式4：cglib动态代理

* cglib动态代理优势：动态代理中，要求目标类和代理类实现同一个接口；若目标类不存在接口，则无法使用；cglib可以解决
* 原理：生成目标类的子类，而子类是增强过的，该子类对象就是代理对象；要求目标类必须能被继承，即不能用final修饰
* 结构：![](H:\markdown笔记\Java基础\图片引用\1560259473879216.jpg)



## 代码

* 依赖

  * ```xml
    <dependency>
        <groupId>cglib</groupId>
        <artifactId>cglib</artifactId>
        <version>2.2.2</version>
    </dependency>
    <dependency>
        <groupId>asm</groupId>
        <artifactId>asm</artifactId>
        <version>3.1</version>
    </dependency>
    ```

* Tank类

  * ```java
    public class Tank {
    
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

* MyCglibFactory类

  * ```java
    public class MyCglibFactory implements MethodInterceptor {  //public interface MethodInterceptor extends Callback
    
        private Tank target;
        public MyCglibFactory(Tank target) {
            this.target = target;
        }
    
        public Tank myCglibCreator() {
            Enhancer enhancer = new Enhancer();
            // 设置需要代理的对象 :  目标类(target) , 也是父类
            enhancer.setSuperclass(Tank.class);
            // 设置代理对象， 这是回调设计模式:  设置回调接口对象 :
            enhancer.setCallback(this); // this代表当前类的对象，因为当前类实现了Callback
            return (Tank) enhancer.create();
        }
    
        // 这个就是回调方法（类A的方法）
        public Object intercept(Object obj, Method method, Object[] args, MethodProxy proxy) throws Throwable {
    
            // 在前面做一些事情: 记录开始时间
            long start = System.currentTimeMillis();
            System.out.println("start time : " + start);
    
            method.invoke(target, args);
    
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
            Tank proxyTank = new MyCglibFactory(new Tank()).myCglibCreator();
            proxyTank.move();
        }
    }
    ```

    