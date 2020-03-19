# Junit单元测试
* 测试分类
>     1.黑盒测试:不需要写代码，给输入值，看程序是否能够输出期望的值。
>     2.白盒测试:需要些代码的。关注程序具体的执行流程。
* Junit的使用
  * 步骤：
    * 1 定义一个测试类（测试用例）
      1.建议：测试类名：被测试的类名Test      ，包名：xxx.xxx.xx.test  
    * 2 定义测试方法：可独立运行
      1.建议：方法名：test测试的方法名       ，返回值：void            ，参数列表：空参
    * 3 给方法加@Test
    * 4 导入Junit依赖环境
* Assert.assertEquals(期望值，实际值)，两者不对测试失败
>     用于判定结果：红色：失败，，，，绿色：成功
* @Before  使用了该注解的方法会在所有测试方法执行之前先执行
* @After   使用了该注解的方法会在所有测试方法执行之后自动执行，即使其他方法有异常也会执行。    
# 反射：框架设计的灵魂
* 框架：半成品软件，可以在框架的基础上进行软件开发，简化编码
* 概念：将类的各个组成部分封装为其他对象，这就是反射机制
* 好处：
  * 1.可以在程序的运行过程中操作这些对象
  * 2.可以解耦，提高程序的可扩展性
* 获取Class对象的方式（字节码文件对象）
  * 1.Class.forName("全类名"):将字节码文件加载进内存，返回Class对象
    * 多用于配置文件，将类名定义在配置文件中。读取文件，加载类
  * 2.类名.Class：通过类名的属性获取
    * 多用于参数的传递
  * 3.对象.getClass（）：getClass（）方法在Object类中定义着。
    * 多用于对象的获取字节码的方式
* 结论：同一个字节码文件（*.class）在一次程序运行过程中，只会被加载一次，不论通过哪一种方式获取的Class对象都是同一个
* Class对象功能
  * 获取功能：
    * 1.获取成员变量们：
      >     Field[] getFields（）:获取所有public修饰的成员变量
      >     Field getField（String name）:获取指定名称的public修饰的成员变量
      
      >     Field[] getDeclaredFields（）：获取所有的成员变量，不考虑修饰符
      >     Field getDeclaredField（String name）：获取指定的成员变量，不考虑修饰符
    * 2.获取构造方法们
      >     Constructor<?>[] getConstructors（）
      >     Constructor<T> getConstructor（类<?>... parameterTypes）
      
      >     Constructor<?>[] getDeclaredConstructors（）:获取所有的构造方法，不考虑修饰符
      >     Constructor<T> getDeclaredConstructor（类<?>... parameterTypes）
    * 3.获取成员方法们
      >     Method[] getMethods（）
      >     Method getMethod（String name,类<?>... parameterTypes）
      
      >     Method[] getDeclaredMethods（）:获取所有的方法，不考虑修饰符
      >     Method getDeclaredMethod（String name,类<?>... parameterTypes）
    * 4.获取类名
      >     String getName()
* Field：成员变量对象
  * 操作：
    1. 设置值
        * void set（Object obj， Object value）
    2. 获取值
        * get（Object obj）
    3. 忽略访问权限修饰符的安全检查
        * setAccessible（true）：暴力反射
* Constructor：构造方法对象
  * 创建对象：
      * T newInstance（Object... initargs（构造方法的参数））
      * 创建空参的构造方法时可直接用   Class对象.newInstance()
* Method：方法对象
  * 执行方法
      * Object invoke（Object obj， Object... args）
  * 获取方法名称
      * String getName：获取方法名
* 案例：
    * 需求：写一个“框架”，不能改变该类的任何代码的前提下，可以帮我们创建任意类的对象，并且执行其中任意方法
        * 实现：要用到
            1. 配置文件
            2. 反射
        * 步骤：
            1. 将需要创建的对象的全类名和需要执行的方法定义在配置文件中
            2. 在程序中加载读取配置文件
            3. 使用反射技术来加载类文件进内存
            4. 创建对象
            5. 执行方法
    
    
    
    
    
     
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
