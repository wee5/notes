# java：监听器2：伪实现



## 事件源

* ```java
  // 事件源：机器人
  public class Robot {
  
      private RobotListener listener;
  
      // 注册机器人监听器
      public void registerListener(RobotListener listener){
       this.listener  = listener;
      }
  
      // 工作
      public void working(){
          if(listener!=null){
              Even even = new Even(this);
              this.listener.working(even);
          }
          System.out.println("机器人开始工作......");
      }
  
      // 跳舞
      public void dancing(){
          if(listener!=null){
              Even even = new Even(this);
              this.listener.dancing(even);
          }
          System.out.println("机器人开始跳舞......");
      }
  }
  ```

## 事件

* ```java
  // 事件对象
  public class Even {
  
      private Robot robot;
  
      public Even(){
          super();
      }
      public Even(Robot robot){
          super();
          this.robot = robot;
      }
  
  
      public Robot getRobot() {
          return robot;
      }
  
      public void setRobot(Robot robot) {
          this.robot = robot;
      }
  }
  ```

## 监听器

* ```java
  //监听器接口
  public interface RobotListener {
      public void working(Even even);
      public void dancing(Even even);
  }
  
  //监听器实现类
  public class MyRobotListener implements  RobotListener{
      @Override
      public void working(Even even) {
          Robot robot = even.getRobot();
          System.out.println("机器人工作提示：请看管好的你机器人，防止它偷懒！");
      }
  
      @Override
      public void dancing(Even even) {
          Robot robot = even.getRobot();
          System.out.println("机器人跳舞提示：机器人跳舞动作优美，请不要走神哦！");
      }
  }
  ```

## 测试类

* ```java
  public class TestListener {
      public static void main(String[] args) {
          Robot robot = new Robot();
          robot.registerListener(new MyRobotListener());
          robot.working();
          robot.dancing();
      }
  }
  ```

## 总结

* 根据这种伪实现
* 监听器需要事件作为传参，事件包含事件源，事件源包含监听器