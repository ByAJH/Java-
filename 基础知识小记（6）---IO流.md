# IO流
* 主体是内存，客体是硬盘
# 字节流：一次读一个字节
## OutputStream字节输出流类，为`抽象类`，所有输出流父类
>     成员方法
>     public void close（）：关闭此输出流并释放与此流相关联的任何系统资源
>     public void flush（）：刷新此输出流并强制任何缓冲的输出字节被写出
>     public void write（byte[] b）：将b.length字节从指定的字节数组写入此输出流
>>      如果写的第一个字节是正数（0-127），那么显示的时候会查询ASCII表
>>      如果写的第一个字节是负数，那第一个字节会和第二个字节，两个字节组成一个中文显示，查询系统默认码表（GBK）
>     public void write（byte[] b，int off，int len）：从指定的字节数组写入len字节，从偏移量off开始输出到此输出流
>>     把字节数组的一部分写入到文件中
>>     off：数组的开始索引；len：写几个字节
>     public abstract void write（int b）：将指定的字节输出流
>     byte[] getBytes（） 把字符串转换为字节数组
>>     byte[] byte = “你好”.getBytes();     fos.write（byte）；将你好写入文件
### FileOutputStream：文件字节输出流
* 作用：把内存中的数据写入到硬盘的文件中
* 构造方法：
>     FileOutputStream（String name）创建一个向具有指定名称的文件中写入数据的输出文件夹，并指向该文件
>     FileOutputStream（File file）创建一个向指定File对象表示的文件中写入数据的文件输出流，并指向该文件。
>     参数：写入数据的目的
>     String name：目的地是一个文件的路径
>     File file：目的地是一个文件
>     构造方法的作用：
>     1、创建一个FileOutputStream对象
>     2、会根据构造方法中传递的文件或文件路径，创建一个空的文件
>     3、会把FileOutputStream对象指向创建好的文件
#### 写入数据的原理（内存--->硬盘）
* java程序--->JVM--->OS--->OS调用写数据方法--->把数据写入到文件中
* 字节输出流的使用步骤（重点）
>     1、创建一个FileOutputStream对象，构造方法中传递写入数据的目的地
>     2、调用FileOutputStream的方法write，写数据到文件中
>     3、释放资源（流使用会占用一定的内存，使用完毕要把内存清空，提供程序的效率）;     流.close
* 追加写/续写：使用两个参数的构造方法
>     FileOutputStream（String name，boolean append）创建一个向具有指定name的文件中写入数据的输出文件夹
>     FileOutputStream（File file，boolean append）创建一个向指定File对象表示的文件中写入数据的文件输出流。
>     参数：
>     String name、File file：目的地是一个文件的目的地
>     boolean append：追加写开关，  true：创建对象不会覆盖原文件，继续在文件袋额末尾追加写数据，false：创建新文件，覆盖原文件
>     写换行： Windows：\r\n,    linux:/n,   mac:/r,也是字符串
## InputStream字节输出流类，为`抽象类`，所有输入流父类
>     成员方法：
>     int read（）从输入流中读取数据的下一个字节
>     int read（byte[] b）从输入流中读取一定数量的字节，并将其存储在缓冲区数组b中
>     void close（）：关闭此输入流并释放与此流相关联的任何系统资源
* FileInputStream:文件字节输入流，把硬盘文件中的数据，读取到内存中使用
>     FileOutputStream（String name）
>     FileOutputStream（File file）
>     参数：
>>     String name：文件的路径
>>     File file：文件
>     构造方法的作用：
>>     1、创建一个FileOutputStream对象
>>     2、把FileInputStream对象指定到构造方法中要读取的文件
### 读取数据的原理（硬盘->内存）
* java程序--->JVM--->OS--->OS调用读数据方法--->读取文件
* 字节输入流的使用步骤（重点）
>     1、创建一个FileOutputStream对象，构造方法中绑定要读取的数据源
>     2、调用FileOutputStream的方法read，读取文件，返回-1读取完毕
>     3、释放资源（流使用会占用一定的内存，使用完毕要把内存清空，提供程序的效率）;     流.close
* 字节输入流一次读取多个字节的方法：
>      int read（byte[] b）；
>>      先创建一个字节数组，表明要读几个byte[] byte = new byte[2]
>>      读取int len = fis.read(bytes);         数据在bytes里面，len表示读取的长度
>>**方法的参数byte[]的作用**
>>      起到缓冲作用，存储每次读取到的多个字节
>>      数组的长度定义为1024或其整数倍
>>**方法的返回值int是什么**
>>      每次读取的有效字节个数
* 可用String类的构造方法将结果转为字符串
>     String（byte[] bytes）：把字节数组转换为字符串
>     String（byte[] bytes， int offset， int length）：把字节数组的一部分转换为字符串  offset：数组的开始索引  length：转换的字节个数
* 使用字节流读取中文会产生乱码问题
## 字符流：一次读一个字符
### 字符输入流，抽象类Reader
>     共性成员方法
>>      int read（）读取单个字符并返回
>>      int read（char[] cbuf）一次读取多个字符，将字符读入数组
>>      void close（）释放资源
>      FileReader：文件字符输入流
>>      作用：把硬盘文件找那个的数据以字符的方式读取到内存中。
>      构造方法：
>>      FileReader（String fileName）
>>      FileReader（File fileName）
>>      参数：读取文件的数据源；String fileName文件路径，File fileName一个文件 
>     构造方法的作用：
>>     1、创建一个FileReader对象
>>     2、把FileReader对象指定到构造方法中要读取的文件
* 字符输入流的使用步骤（重点）
>     1、创建一个FileReader对象，构造方法中绑定要读取的数据源
>     2、调用FileReader的方法read，读取文件，返回-1读取完毕
>     3、释放资源（流使用会占用一定的内存，使用完毕要把内存清空，提供程序的效率）;     流.close
### 字符输出流，抽象类Writer
>     共性成员方法
>>      void write（int c）写入单个字符
>>      void write（char[] cbuf）一次写入多个字符到数组
>>      abstract void write（char[] cbuf，int off，int len）写入字符数组的len个字符，从偏移量off开始输出到此输出流
>>      void write（String str）写入字符串
>>      void write（String str，int off，int len）写入字符串的len个字符，从偏移量off开始输出到此输出流
>>      void flush（）刷新该流的缓冲
>>      void close（）释放资源
>      FileWriter：文件字符输出流
>>      作用：把内存中的字符数据写入到文件中。
>      构造方法：
>>      FileWriter（String fileName）根据给定的文件名构造一个FileWriter对象
>>      FileWriter（File fileName）根据给定的File对象构造一个FileWriter对象    
>>      参数：写入数据的目的地；String fileName文件路径，File fileName一个文件 
>     构造方法的作用：
>>     1、创建一个FileWriter对象
>>     2、根据构造方法中传递的文件或其路径创建一个文件
>>     3、创建好的FileWriter对象指向该文件
* 字符输入流的使用步骤（重点）
>     1、创建一个FileWriter对象，构造方法中绑定要写入的数据目的地
>     2、调用FileWriter的方法write，把数据写入到内存缓冲区中（字符转换为字节的过程）
>     3、使用FileWriter的方法flush，把内存缓冲区中的数据，刷新到文件中
>>      刷新flush之后，流可以继续使用，相当于一个空的流，而close之后流就消失了
>     4、释放资源（会先把内存缓冲区中的数据刷新到文件中）;     流.close 
* 追加写/续写：使用两个参数的构造方法
>     FileWriter（String name，boolean append）创建一个向具有指定name的文件中写入数据的输出文件夹
>     FileWriter（File file，boolean append）创建一个向指定File对象表示的文件中写入数据的文件输出流。
>     参数：
>     String name、File file：目的地是一个文件的目的地
>     boolean append：追加写开关，  true：创建对象不会覆盖原文件，继续在文件袋额末尾追加写数据，false：创建新文件，覆盖原文件
>     写换行： Windows：\r\n,    linux:/n,   mac:/r,也是字符串
## IO流异常处理
* 使用try_catch_finally代码块
>     将有问题的代码放在try中，close放在finally，无论是否有问题都关闭流（要提高流对象的作用域 ）。
* 改进
> JDK7之后新特性
>>      try（）{}catch{}    将创建对象流代码写在try之后的（）内，这样try执行完后会自动释放流
> JDK9之后新特性
>>     try前边定义流对象 
>>      在try后边的（）中可以直接引入流对象的名称（变量名）
>>      在try代码执行完毕，流对象也可释放，不用写finally
>     格式
>>      A a = new A（）；
>>      B b = new B（）；
>>      try（a，b）{可能会产生异常的代码}catch（异常类变量 变量名）{异常处理逻辑}
>>      如果是流对象，try前面还是会有异常 


