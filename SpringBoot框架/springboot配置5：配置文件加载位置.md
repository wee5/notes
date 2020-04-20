# springboot配置5：配置文件加载位置



* springboot会扫描以下位置的application.properties或application.yaml作为默认配置文件（**优先级由高到低**）
  * file:/config/	（file是项目根路径）
  * file:/
  * classpath:/config/
  * classpath:/



* spring.config.location：改变默认配置文件位置
  * 将项目打包好，通过命令行参数形式，启动项目，并**加载项目外的路径的配置文件**；路径共同起作用，互补
  * 命令行：jar包路径下>java -jar jar包名称.jar --spring.config.location=硬盘某个位置的配置文件
  * 补充：高版本的springboot中，spring-config.location会覆盖内部配置；使用spring.config.additional-lacation可以和其他配置文件互补



## 外部配置记载顺序

* springboot还可以从以下位置加载配置：（优先级由高到低，覆盖及互补）
  * 命令行参数
  * 来之java:comp/env的JNDI属性
  * java系统属性（System.getProperties())
  * 操作系统环境变量
  * RandomValuePropertySource配置的random.*属性值
  * *优先加载带profile，并由外到内*
  * **jar包外部的application-{profile}.properties或application.yml（带spring.profile）配置文件**
  * **jar包内部的application-{profile}.properties或application.yml(带spring.profiles)配置文件**
  * *再加载不带profile*，并由外到内
  * **jar包外部的application.properties或application.yml（不带spring.profile）配置文件**
  * **jar包内部的application.properties或application.yml（不带spring.profile）配置文件**
  * @Configuration注解类上的@PropertySource
  * 通过SpringApplication.setDefaultProperties指定的默认属性