# java：Jmockit1：intro

* JMockit是用来帮助开发人员编写测试程序的一组工具和API，完全基于java 5 ee的java.lang.instrument包开发，内部使用ASM库来动态修改java的字节码，使得java这种静态语言可以想动态脚本语言一样动态设置被Mock对象私有属性，模拟静态、私有方法行为等
* 优势：即能mock接口、抽象类、final类、注解以及枚举等
* 两种mock方式：
  * Behavior-oriented（Expectations & Verifications）基于行为
  * 根据输入输出，**功能测试**，类似黑盒测试。需要把依赖的代码mock掉，实际相当于**改变了被依赖的代码的逻辑**
  * State-oriented（MockUp<GenericType）基于状态
  * 根据用例测试路径，**测试代码内部逻辑**，接近白盒测试。目的是**测试单元测试及其依赖代码的调用过程**，验证代码逻辑是否满足测试路径
* APIs和tools
  * ![avatar](图片引用\14982672_201307191104043YjaN.jpg)



* 参考https://blog.csdn.net/n8765/article/details/40976793