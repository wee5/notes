# 插件：JsonFormat



- 利用**json**格式字符串自动**编写**类

## 配置文件

```yml
spring:
  jackson:
    date-format: yyyy-MM-dd HH:mm:ss
    time-zone: GMT+8
```



## 使用

快捷键：alt j（在需要编写的类文件界面下使用），打开GsonFormat界面，写入json格式数据即可自动编写

在vo类中使用注解

```java
public class Host implements Serializable {

    //前端传入json字符串，匹配接收参数时，用到的别名
    @JsonAlias("host")
    private String name;
    
	//将字符串转换为指定格式日期类型
    @JsonFormat(pattern="yyyy/MM/dd",timezone="GMT+8")
    private Date date;
    
    //接收json数据时，忽略该属性
    @JsonIgnore
    private int id;
```