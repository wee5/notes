# sql细节

## where和having的区别

* 原理差别：where是先筛选再select，having是先select再筛选

* ```mermaid
  graph LR
  A(where语句) --> |where筛选| B(临时表1减少若干记录)
  B --> |select查询| C(结果表)
  
  D(having语句) --> |select查询| E(临时表2减少若干字段)
  E --> |having筛选| F(结果集)
  ```



## where，group by，having的执行顺序

* 执行顺序：

* ```mermaid
  graph LR
  A(原表) --> |where| B(临时表1)
  B -->|group by| C(临时表2)
  D(临时表2) -->|select| E(临时表3)
  E-->|having| F(结果表)
  ```




## on和where区别

* 执行顺序：**on > where**

* ```mermaid
  graph LR
  A(原表) --> |join|B(笛卡尔集表)
  B --> |on为联表条件|C(临时表)
  C --> |where|D(结果表)
  ```

* on的意义在于：左联（右联）时，无论条件是否成立，均会保留左表（右表）全部记录。

