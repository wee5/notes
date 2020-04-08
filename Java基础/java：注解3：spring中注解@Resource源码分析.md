# java：注解3：spring中注解@Resource源码分析

* ## 总结

  - 注解代表的是某种业务意义，注解背后处理器的工作原理如下源码实现：
  - 首先解析所有属性，判断属性上是否存在指定注解，如果存在则根据搜索规则取得bean，然后利用反射原理注入。如果标注在字段上面，也可以通过字段的反射技术取得注解，根据搜索规则取得bean，然后利用反射技术注入。

* ```java
  package com.wxy.annotation;
   
  import java.lang.annotation.ElementType;
  import java.lang.annotation.Retention;
  import java.lang.annotation.RetentionPolicy;
  import java.lang.annotation.Target;
   
  @Retention(RetentionPolicy.RUNTIME)
  @Target( { ElementType.FIELD, ElementType.METHOD })
  public @interface WxyResource {
      String name() default "";
  }
  ```



* ```java
  public class PeopleServiceBean implements PeopleService {
      private PeopleDao peopleDao;
      private String    name = "wxy";
      private Integer   id   = 1;
      /** 
       * @param peopleDao the peopleDao to set
       */
      @WxyResource
      public void setPeopleDao(PeopleDao peopleDao) {
          this.peopleDao = peopleDao;
      }
      …
  }
  ```



* ```java
  public class WxyClassPathXMLApplicationContext {
   
      //存放BeanDefinition的列表，在beans.xml中定义的bean可能不止一个
      private final List<BeanDefinition> beanDefines = new ArrayList<BeanDefinition>();
      //将类名作为索引，将创建的Bean对象存入到Map中
      private final Map<String, Object>  sigletons   = new HashMap<String, Object>();
      public WxyClassPathXMLApplicationContext(String fileName) {
          //读取xml配置文件
          this.readXML(fileName);
          //实例化bean
          this.instanceBeans();
          //处理注解方式
          this.annotationInject();
          //注入对象
          this.injectObject();
      }
      /** 
       * 使用注解方式注入对象方法实现
       * @throws IntrospectionException 
       */
      private void annotationInject() {
          //循环所有bean对象
          for (String beanName : sigletons.keySet()) {
              //获取bean对象
              Object bean = sigletons.get(beanName);
              //如果bean不为空,取得bean的属性
              if (bean != null) {
                  try {
                      //按属性注入
                      PropertyDescriptor[] ps = Introspector.getBeanInfo(bean.getClass())
                          .getPropertyDescriptors();
                      for (PropertyDescriptor properdesc : ps) {
                          //获取属性的setter方法
                          Method setter = properdesc.getWriteMethod();
                          //判断注解是否存在
                          if (setter != null && setter.isAnnotationPresent(WxyResource.class)) {
                              //取得注解
                              WxyResource resource = setter.getAnnotation(WxyResource.class);
                              Object value = null;
                              //如果按名字找到
                              if (resource.name() != null && !"".equals(resource.name())) {
                                  //取得容器中的bean对象
                                  value = sigletons.get(resource.name());
   
                              } else {//没有按名字找到，按类型寻找
                                  //取得容器中的bean对象
                                  value = sigletons.get(resource.name());
                                  if (value == null) {
                                      for (String key : sigletons.keySet()) {
                                          if (properdesc.getPropertyType().isAssignableFrom(
                                              sigletons.get(key).getClass())) {
                                              value = sigletons.get(key);
                                              break;
                                          }
                                      }
                                  }
                              }
                              //把引用对象注入到属性
                              setter.setAccessible(true); 
                              setter.invoke(bean, value);
                          }
                      }
                      //按字段注入
                      Field[] fields = bean.getClass().getDeclaredFields();
                      for (Field field : fields) {
                          //如果注解存在
                          if (field.isAnnotationPresent(WxyResource.class)) {
                              //取得注解
                              WxyResource resource = field.getAnnotation(WxyResource.class);
                              Object value = null;
                              //如果按名字找到
                              if (resource.name() != null && !"".equals(resource.name())) {
                                  //取得容器中的bean对象
                                  value = sigletons.get(resource.name());
                              } else {//没有按名字找到，按类型寻找
                                  //取得容器中的bean对象
                                  value = sigletons.get(field.getName());
                                  if (value == null) {
                                      for (String key : sigletons.keySet()) {
                                          if (field.getType().isAssignableFrom(
                                              sigletons.get(key).getClass())) {
                                              value = sigletons.get(key);
                                              break;
                                          }
                                      }
                                  }
                              }
                              //允许访问private 
                              field.setAccessible(true);
                              field.set(bean, value);
                          }
                      }
                  } catch (IntrospectionException e) {
                      // TODO Auto-generated catch block
                      e.printStackTrace();
                  } catch (IllegalArgumentException e) {
                      // TODO Auto-generated catch block
                      e.printStackTrace();
                  } catch (IllegalAccessException e) {
                      // TODO Auto-generated catch block
                      e.printStackTrace();
                  } catch (InvocationTargetException e) {
                      // TODO Auto-generated catch block
                      e.printStackTrace();
                  }
              }
          }
      }
      /** 
       *为bean对象的属性注入值
       */
  　　private void injectObject() {
  　　　　….
  　　}
  　　….
  }
  ```



* ```java
  public class MyTest { 
      /**
       * @param args
       */
      public static void main(String[] args) {
          //MyIOC容器实例化
          WxyClassPathXMLApplicationContext ac = new WxyClassPathXMLApplicationContext("beans.xml");
          PeopleService peopleService = (PeopleService) ac.getBean("peopleService");
          peopleService.save();
      }
  }
  ```

