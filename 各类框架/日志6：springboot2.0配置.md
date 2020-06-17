# 日志6：springboot2.0配置



## 概述

### 配置文件的相关配置

```yml
logging:
  file:
  	#path和name可以同时存在，以name为准；path指定路径，name指定路径和文件名称；这是boot2.0的配置方式，和boot1.0不同
    path: H:/
    name: H:/urspring.log
  level:
  	#全局日志级别
    root: warn
    #包下日志级别，全局和包下日志级别可以同时存在，若有相同的日志，包下覆盖全局日志设置
    com.company.logtest.config: debug
```