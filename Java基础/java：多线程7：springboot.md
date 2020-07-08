# java：多线程7：springboot



springboot配置多线程



## 配置类

```java
@Configuration
@ComponentScan("com.company.multithreadtest.demo")
@EnableAsync//开启异步任务支持
public class ThreadConfig implements AsyncConfigurer {
    //返回一个ThreadPoolTaskExecutor，这就就获得了一个基于线程池的TaskExecutor
    @Override
    public Executor getAsyncExecutor() {
        ThreadPoolTaskExecutor taskExecutor = new ThreadPoolTaskExecutor();
        taskExecutor.setCorePoolSize(5);//核心线程池
        taskExecutor.setMaxPoolSize(10);//最大线程池
        taskExecutor.setQueueCapacity(25);//任务队列
        taskExecutor.initialize();
        return taskExecutor;
    }
}
```



## @Async注解使用

在需要开启线程的方法上使用