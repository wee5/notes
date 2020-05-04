# springboot：数据访问1：intro



* 对于数据访问，无论是SQL还是NOSQL，springboot默认采用整合spring data方式进行统一处理，添加大量自动配置，屏蔽了很多设置；引入各种xxxTemplate，xxxRepository来简化对数据访问层的操作，我们只需要进行简单的设置即可；
* springboot默认数据源是tomcat-jdbc；数据源和数据库驱动有区别