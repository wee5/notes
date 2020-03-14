# sql语句

## 数据库基础命令

* 展示数据库：show databases
* 创建数据库：create database DATABASE_NAME
* 删除数据库：drop database DATABASE_NAME
* 选择数据库：use DATABASE_NAME

## 表的基础命令

* 展示表：show tables
* 选中表：use TABLE_NAME
* 删除表：drop table TABLE_NAME
* 显示表结构：desc TABLE_NAME
* 根据旧表创建新表：
* * create table TABLE_NEW like TABLE_OLD

  * create table TABLE_NEW as SELECT_QUERY
* 直接创建新表



## 六约束和auto_increment

### 创建约束

#### 建表时

- **字段约束**：**不支持外键，check必须放在最后**
  - **primary key not null unique auto_increment default 'hello' check(COL1>0)**
- **统一约束**：**不支持not null，default，auto_increment**
  - 建议加上**约束名constraint c1**
  - **primary key (COL1)**
  - **foreign key (COL2) references TAB2(COL1)**
  - **unique (COL1,COL2)**
  - **check (COL1>0)**

#### 建表后

- **add：适用范围和语法同统一约束一样**
- **modify和change**：**适用范围和语法同字段约束一样，但不支持check，外键**
  - **Key：包含primary key，foreign key，unique，Key不受重置影响**
  - **重置：Null为YES；Default为NULL；Extra为无**（Extra包含auto_increment）
    - **主键的重置：**主键非空特性，其Null不受重置影响，总为No；一旦设置过主键default（除null），主键Default受到重置影响，变为对应类型的默认值，而不是NULL，同样因为主键非空特性；
  - **区别：**change可以**修改字段名**；字段类型和长度都可以改；
- add仅能增加约束；modify和change能重置非Key约束，即可以删除非Key约束；**（怎么才能删除Key的约束）**
- **增加字段：add colunm ..**

### 撤销约束

- 撤销约束
  - alter table TABLE_NAME
  - drop CONSTRAINT/constraint CONS_NAME
- 示例：**（重新整理，不太对）**
  - 撤销not null：modify role_id int(20) null;
  - 删除unique约束：drop index 唯一约束名;
  - 删除primary key约束：drop primary key;
  - 删除foreign key约束：drop foreign key 外键名;



## 函数

* **Aggregate 函数**（合计函数）：将某列处理成一个值，结果单条记录
  * AVG() - 平均值
  * COUNT() - 行数，**不计null**，count(*)查询表总行数
  * FIRST() - 第一个记录的值，仅MS Access 支持
  * LAST() - 最后一个记录的值，仅MS Access 支持
  * MAX() - 最大值
  * MIN() - 最小值
  * SUM() - 总和
* 将聚合函数作为一个值放在条件中**被比较:**(不能直接用聚合函数表示该值)
* select * from num where num>(select avg(num) from num);



* **Scalar 函数**：基于输入值，返回一个单一个值
  * UCASE() - 将某个字段转换为大写
  * LCASE() - 将某个字段转换为小写
  * MID() - 从某个文本字段提取字符，MySql 中使用
  * SubString(字段，1，end) - 从某个文本字段提取字符
  * LEN() - 返回某个文本字段的长度
  * ROUND() - 对某个数值字段进行指定小数位数的四舍五入
  * NOW() - 返回当前的系统日期和时间
  * FORMAT() - 格式化某个字段的显示方式



* **GROUP BY语句：**用于结合聚合函数，根据一个或多个字段对结果集进行分类

  * 无group by的语句可以认为是默认分成一大组
  * 通常配合聚合函数使用

* **group by**配合**聚合函数**（或单独使用聚合函数）

  * 分组按升序排列，相同的为一组

  * **每组分别筛选**，聚合函数返回单个记录，其他字段返回第一条记录

  * 

    * | id   | num  | age  |
      | ---- | ---- | ---- |
      | 1    | 60   | 17   |
      | 2    | 50   | 16   |
      | 3    | 70   | 15   |

* * 例子：select id,avg(num),age from table;

