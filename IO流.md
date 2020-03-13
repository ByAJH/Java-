# IO流
* 主体是内存，客体是硬盘
## OutputStream字节输出流类，为`抽象类`
>     成员方法
>     public void close（）：关闭此输出流并释放与此流相关联的任何系统资源
>     public void flush（）：刷新此输出流并强制任何缓冲的输出字节被写出
>     public void write（byte[] b）：将b.length字节从指定的字节数组写入此输出流
>     public void write（byte[] b，int off，int len）：从指定的字节数组写入len字节，从偏移量off开始输出到此输出流
>     public abstract void write（int b）：将指定的字节输出流
### FileOutputStream：文件字节输出流
* 作用：把内存中的数据写入到硬盘的文件中
* 构造方法：
>     FileOutputStream（String name）创建一个向具有指定名称的文件中写入数据的输出文件夹
>     FileOutputStream（File file）创建一个向指定File对象表示的文件中写入数据的文件输出流。
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









