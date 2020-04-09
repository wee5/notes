# java：反射2：解剖字节码

- 反射机制：将.class文件中的类加载出来，并解剖出字节码中对应类的相关内容（构造函数、属性、方法）；可以得到字段的名字，字段的值和修改字段的值；得到方法的申明，方法的定义和唤起方法（执行方法）

- **Animal.java**

  - ```java
    public class Animal {
    	public String name ="Dog";
    	private int   age  =30 ;
    	
    	//默认无参构造函数
    	public Animal(){
    		System.out.println("Animal");
    	}
    	//带参数的构造函数 
    	public Animal(String name , int age){
    		System.out.println(name+","+age);
    	}
    	//公开 方法  返回类型和参数均有
    	public String sayName(String name){
    		return "Hello,"+name;
    	}
    }
    ```

- **ReflectTest.java**

  - ```java
    public class ReflectTest{
        public static void main(String args[]) throws Exception{
            //由完全限定名加载类，得到字节码对象
            Class class1=Class.forName("com.appleyk.reflect.Animal");
            //由字节码对象解剖，得到构造方法
            Constructor ctor1=class1.getConstructor();
            //构造器对象创建实例，得到字节码对象对应的类
            Animal animal1=(Animal)ctor1.newInstance();
            
            //同上
            Constructor ctor2=class1.getConstructor(String.class,int.class);
            Animal animal2=(Animal)ctor2.newInstance("Cat",20);
            
            //由字节码对象得到所有字段
            Field[] fields=class1.getDeclaredFields();
            for(Field field : fields){
                System.out.println(field);
            }
            
            //由字节码对象获得（包括父类的）公有变量
            fields=class1.getFields();
            for(Field field : fields){
                System.out.println(field+"，字段值="+field.get(animal1));
                //先判断field对象的名称是“name”，且field对象的类型为String.class;再赋新值
                if(field.getName()=="name"&&field.getType().equals(String.class)){
                    String value=(String)field.get(animal1);
                    //由field对象设置对应变量的值，给定所属对象和值
                    field.set(animal1,"Dogg");
                }
            }
            
            //由字节码对象得到所有方法
            Method[] methods=class.getDeclaredMethods();
            for(Method m : methods){
                System.out.println(m);
            }
            
            //由字节码对象得到（包括父类和接口的）公有变量
            methods=class.getMethods();
            for(Method m : methods){
                System.out.println(m);
            }
            
            //由字节码对象得到方法，给定方法名和返回类型
            Method sayNameMethod=class1.getMethod("sayName",String.class);
            //由method对象唤起（执行）对应的方法，给定所属对象和参数值
            System.out.println(sayNameMethod.invoke(animal1,"Tom"));
        }
    }
    ```

- Class对象的方法

  - Class.forName("com.appleyk.reflect.Animal")
  - class.getConstructor()
  - class.getDeclaredFields()
  - class.getFields()
  - class.getDeclaredMethods()
  - class.getMethods()
  - class.newInstance()，在spring IOC中见
  - class.getClass()，在spring IOC见

- Constructor对象的方法

  - constructor.newInstance()

- Field对象的方法

  - field.get(animal1)
  - field.set(animal1,"Dogg")
  - field.getName()
  - field.getType()

- Method对象的方法

  - method.invoke(animal1,"Tom")