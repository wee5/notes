# java：+=和=+



## a+=b 等同于 a=（a的类型）a+b

* ```
  byte i=100;
  short l=100;
  System.out.println(i+=l);//结果-56
  ```

* ```
  byte i=100;
  short l=1000;
  System.out.println(i+=l);//结果76
  ```

* ```
  byte i=100;
  short l=1000;
  System.out.println(l+=i);//结果1100
  ```

* ```
  byte i=100;
  System.out.println(i+=1000);
  ```

* 分析：**a+=b在计算形式上可以等同于a=a+b**

  * 例一：等同 i=（byte）i+l；i+l=200超出byte最大值，根据机器码，结果为-56
  * 例二：（byte）1100=76；
  * 例三：byte（小）强转为long（大）；值不改变
  * 例四：常量1000默认为int类型，原理同例一，结果同例二

* 总结：两个过程

  * 计算：低类型向高类型转换
  * 赋值：比较左右两边类型；低类型可以直接赋值给高类型；高类型不能隐式赋值给低类型，需要强制转换



## =+为正号赋值

* ```
  int n;
  short l=1000;
  System.out.println(n=+l);//结果1000
  ```

* ```
  int n;
  System.out.println(n=+1000);//结果1000
  ```

* ```
  byte i=100;
  System.out.println(i=+1000);//编译不通过
  ```

* ```
  byte i=100;
  short l=1000;
  System.out.println(l=+i);//编译不通过
  ```

* 总结：

  * 赋正号：若右边为负数，赋正号后一样为负数
  * 赋值：同样需要比较左右两边类型；高类型赋值给低类型，若不强转，则编译不通过