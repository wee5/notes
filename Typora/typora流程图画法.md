# typora流程图画法

* 插入代码块，申明语言flow

## 横向流程图

* ```mermaid
  graph LR
  A[方形] --> B(圆角)
  B --> C{条件a}
  C --> |a=1| D[结果1]
  C --> |a=2| E[结果2]
  F[横向流程图]
  ```

## 竖向流程图

* ```mermaid
  graph TD
  A[方形] --> B(圆角)
  B --> C{条件a}
  C --> |a=1| D[结果1]
  C --> |a=2| E[结果2]
  F[竖向流程图]
  ```

## 标准流程图

* ```flow
  st=>start: 开始框
  op=>operation: 处理框
  cond=>condition: 判断框
  sub1=>subroutine: 子流程
  io=>inputoutput: 输入输出框
  e=>end: 结束框
  st->op->cond
  cond(yes)->io->e
  cond(no)->sub1(right)->op
  ```

* ```flow
  st=>start: 开始框
  op=>operation: 处理框
  cond=>condition: 判断框(是或否?)
  sub1=>subroutine: 子流程
  io=>inputoutput: 输入输出框
  e=>end: 结束框
  st(right)->op(right)->cond
  cond(yes)->io(bottom)->e
  cond(no)->sub1(right)->op
  ```

* ```sequence
  对象A->对象B: 对象B你好吗? (请求)
  Note right of 对象B: 对象B的描述
  Note left of 对象A: 对象A的描述(提示)
  对象B --> 对象A: 我很好(响应)
  对象A --> 对象B: 你真的好吗?
  ```

补充：时序图，甘特图

