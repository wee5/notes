# springboot配置1：intro



## 配置文件

* springboot的两种全局配置文件，**配置文件名是固定的**
  * application.properties
  * application.yml
* 配置文件作用：修改springboot自动配置的默认值（springboot在底层自动配置好了）

## YAML(YAML Ain‘t Markup Language)

* YAML A Markup Language：是一个标记语言

* YAML isn‘t Markup Language：不是一个标记语言

* 标记语言：old配置文件；大多使用xxx.xml文件

* YAML：以数据为中心，比json，xml等更适合做配置文件

  * ```yaml
    server:
      port: 8081
    ```

  * ```xml
    语法区别于xml
    <server>
    	<port>8081</port>
    </server>
    ```

## YAML语法

* 基本语法

  * **k: v**：表示一对键值对（**冒号和value之间必须有空格**）
  * **以空格缩进控制层级关系（对象属性）**；左对齐的所有数据都是同层级的
  * 属性和值是大小写敏感的

* 值的写法（**值可以为字面量，对象，数组等**）

  * 字面量：普通的值（数字，字符串，布尔）：

    * k: v：字面直接写
    * 字符串默认不用加单双引号
      * 双引号：不会转义字符串内的特殊字符
      * 单引号：会转义特殊字符
      * 特殊字符不表示字面意思，\n不转义表示换行，转义表示\n

  * 对象，map（属性和值）（键值对）：

    * k: v：在下一行写对象的属性和值的关系

    * ```yaml
      换行写法
      friends:
      	lastName: zhangsan
      	age: 20
      	
      行内写法
      friends: {lastName: zhangsan，age: 18}
      ```

  * 数组（List，Set）：

    * 用-值表示数组中的一个元素

    * ```yaml
      换行写法
      pets
       - cat	-后面有空格
       - dog
       - pig
       
      行内写法
      pets: [cat,dog,pig]
      ```


