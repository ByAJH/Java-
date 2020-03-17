* 协议分类：
  * UDP协议：用户数据报协议，消耗资源少，通信效率高，但偶尔会丢失一两个数据包
  * TCP协议：传输控制协议。安全，效率没有udp高。
* TCP通信的三次握手
  * 第一次握手，客户端发请求
  * 第二次握手，服务器送响应
  * 第三次握手，客户端答确认
* TCP 通信能实现两台计算机之间的数据交互，通信的两端，要严格区分为客户端（Client）与服务端（Server）。
  * 两端通信时步骤：
    * 1、服务端程序，需要事先启动，等待客户端的连接；
    * 2、客户端主动连接服务器端，连接成功才能通信。服务端不可以主动连接客户端。
* 在 Java 中，提供了两个类用于实现 TCP 通信程序：
    * 1、客户端：java.net.Socket 类表示。创建 Socket 对象，向服务端发出连接请求，服务器响应请求，两者建立连接开始通信。
    * 2、服务端：java.net.ServerSocket 类表示。创建 ServerSocket 对象，相当于开启一个服务，并等待客户端的连接。
## 客户端Socket 类：该类实现客户端套接字，套接字指的是两台设备之间通讯的端点，包含了IP地址和端口号的网络单位
* 构造方法public Socket(String host, int port) :创建套接字对象并将其连接到指定主机上的指定端口号。
>     参数
>>      String host：服务器主机的名称/服务器的IP地址
>>      int port：服务器的端口号
>     成员方法
>>      public InputStream getInputStream() ： 返回此套接字的输入流。
>>      public OutputStream getOutputStream() ： 返回此套接字的输出流
>>      public void close() ：关闭此套接字
>>      public void shutdownOutput() ： 禁用此套接字的输出流
>     实现步骤
>>      1、创建一个客户端对象Socket，构造方法绑定服务器的IP地址和端口号
>>      2、使用Socket对象中的方法getOutputStream（）获取网络字节输出流OutputStream对象
>>      3、使用网络字节输出流OutputStream对象中的方法write，给服务器发送数据
>>      4、使用Socket对象中的方法getInputStream（）获取网络字节输入流InputStream对象
>>      5、使用网络字节输入流InputStream对象中的方法read，读取服务器回写的数据
>>      6、释放资源（Socket）
>     注意
>>      1、客户端和服务器端进行交互，必须使用Socket中提供的网络流，不能使用自己创建的流对象
>>      2、当我们创建客户端对象Socket的时候，就回去请求服务器，然后与服务器经过3此握手之后建立连接通路
>>        如果服务器未启动，就会抛出异常，如果服务器已启动，就可进行交互
## 服务器端ServerSocket类：此类实现服务器套接字
* 构造方法：ServerSocket（int port）创建绑定到特定端口的服务器套接字
    * 服务器端必须明确一件事情，必须得知道是哪个客户端请求的服务器，所以可以使用accept方法获取到请求的客户端对象Socket
>     成员方法
>>      Socket accept（）监听并接受到此套接字的连接。
>    服务器的实现步骤
>>      1、创建一服务器对象ServerSocket和系统要指定的端口号
>>      2、使用ServerSocket对象中的方法accept（）获取取到请求的客户端对象Socket
>>      3、使用Socket对象中的方法getInputStream（）获取网络字节输入流InputStream对象
>>      4、使用网络字节输入流InputStream对象中的方法read，读取客户端发送的数据
>>      5、使用网络字节输出流OutputStream对象中的方法write，给客户端回写数据
>>      6、释放资源（Socket，ServerSocket）
* TCP通信的文件上传案例：
* 原理：客户端读取本地的文件，把文件上传到服务器，服务器再把长传的文件保存到服务器的硬盘上。
* 分析：
>   1、客户端使用本地字节输入流，读取要上传的文件。
>   2、客户端使用网络字节输出流，把读取到的文件上传到服务器。
>   3、服务器使用网络字节输入流，读取客户端上传的文件。
>   4、服务器使用本地字节输出流，把读取到的文件，保存到服务器的硬盘上。
>   5、服务器使用网络字节输出流，给客户端回写一个“上传成功”。
>   6、客户端使用网络字节输入流，读取服务器回写的数据。
>   7、释放资源。
* 注意：
   * 客户端和服务器在本地硬盘读写，需要使用自己创建的字节流对象(本地流)
   * 客户端和服务器之间进行读写，必须使用Socket中提供的字节流对象(网络流)
   
    
* 客户端代码
```
/*
    文件上传案例的客户端：读取本地文件，上传到服务器，读取服务器回写数据
    明确：
        数据源：G:\Demo02\src\1.png
        目的地：服务器
    实现步骤：
        1.创建一个本地字节输入流FileInputStream对象，构造方法中绑定要读取的数据源
        2.创建一个客户端Socket对象，构造方法中绑定服务器的IP地址和端口号
        3.使用Socket中的方法getOutputStream，获取网络字节输出流OutputStream对象
        4.使用本地字节输入流FileInputSteam对象中的方法read，读取本地文件
        5.使用网络字节输出流OutPutStream对象中的方法write，把读取到的文件上传到服务器
        6.使用Socket中的方法getInputStream，获取网络字节输入流InputStream对象
        7.使用网络字节输入流InputSteam对象中的方法read读取服务器回写的数据
        8.释放资源(FileInputStream，Socket)
*/
public class Demo02TCPClient {
    public static void main(String[] args) throws IOException {
        //1.创建一个本地字节输入流FileInputStream对象，构造方法中绑定要读取的数据源
        FileInputStream fis = new FileInputStream("G:\\Demo02\\src\\1.png");
        //2.创建一个客户端Socket对象，构造方法中绑定服务器的IP地址和端口号
        Socket socket = new Socket("127.0.0.1",8888);
        //3.使用Socket中的方法getOutputStream，获取网络字节输出流OutputStream对象
        OutputStream os = socket.getOutputStream();
        //4.使用本地字节输入流FileInputSteam对象中的方法read，读取本地文件
        int len = 0;
        byte[] bytes = new byte[1024];
        while ((len = fis.read(bytes))!=-1){
            //5.使用网络字节输出流OutPutStream对象中的方法write，把读取到的文件上传到服务器
            os.write(bytes,0,len);
        }

        /*
            解决：上传完文件，给服务器写一个结束标记
            void shutdownOutput() 禁止此套接字的输出流
            对于 TCP 套接字，任何以前写入的数据都将被发送，并且后跟 TCP 的正常连接终止序列
         */
        socket.shutdownOutput();

        //6.使用Socket中的方法getInputStream，获取网络字节输入流InputStream对象
        InputStream is = socket.getInputStream();
        //7.使用网络字节输入流InputSteam对象中的方法read读取服务器回写的数据
        while ((len = is.read(bytes))!=-1){
            System.out.println(new String(bytes,0,len));
        }
        //8.释放资源(FileInputStream，Socket)
        fis.close();
        socket.close();
    }
}

```
* 服务器端代码
```


/*
    文件上传服务器端： 读取客户端上传的文件，保存到服务器的硬盘，给客户端回写"上传成功"

    明确：
        数据源：客户端上传的文件
        目的地：服务器的硬盘 G:\\Demo02\\upload

    实现步骤：
        1.创建一个服务器ServerSocket对象，和系统要指定的端口号
        2.使用ServerSocket对象中的方法accept，获取到请求客户端Socket对象
        3.使用Socket对象中的方法getInputStream，获取到网络字节输入流InputStream对象
        4.判断G:\\Demo02\\upload文件夹是否存在，不存在则创建
        5.创建一个本地字节输入流FileOutputStream对象，构造方法中绑定要输出的目的地
        6.使用本地字节输出流InputStream对象中的方法read，读取客户端上传的文件
        7.使用本地字节输出流FileOutputStream对象中的方法write，把读取到的文件保存到服务器的硬盘上
        8.使用Socket对象中的方法getOutputStream，获取到网络字节输出流OutputStream对象
        9.使用网络字节输入流OutputStream对象中的方法write，给客户端回写"上传成功"
        10.释放资源(FileOutputStream，Socket，ServerSocket)
 */
public class Demo03TCPServer {
    public static void main(String[] args) throws IOException {
        //1.创建一个服务器ServerSocket对象，和系统要指定的端口号
        ServerSocket server = new ServerSocket(8888);
        //2.使用ServerSocket对象中的方法accept，获取到请求客户端Socket对象

        /*
            让服务器一直处于监听状态(死循环accept方法)
            有一个客户端上传文件，就保存一个文件
         */
        while (true){
            Socket socket = server.accept();

            /*
                使用多线程技术，提高程序的效率
                有一个客户端上传文件，就开启一个线程，完成文件的上传
             */
            new Thread(new Runnable() {
                //完成文件的上传
                @Override
                public void run() {
                    try{
                        //3.使用Socket对象中的方法getInputStream，获取到网络字节输入流InputStream对象
                        InputStream is = socket.getInputStream();
                        //4.判断G:\\upload文件夹是否存在，不存在则创建
                        File file = new File("G:\\Demo02\\upload");
                        if (!file.exists()){
                            file.mkdirs();
                        }

                        /*
                            自定义一个文件的命名规则：防止同名文件被覆盖
                            规则：域名+毫秒值+随机数
                         */
                        String fileName = "itcase" + System.currentTimeMillis() + new Random().nextInt(99999) + ".png";

                        //5.创建一个本地字节输入流FileOutputStream对象，构造方法中绑定要输出的目的地
                        FileOutputStream fos = new FileOutputStream(file+"\\"+fileName);
                        //6.使用本地字节输出流InputStream对象中的方法read，读取客户端上传的文件
                        int len = 0;
                        byte[] bytes = new byte[1024];
                        while ((len = is.read(bytes))!=-1){
                            //7.使用本地字节输出流FileOutputStream对象中的方法write，把读取到的文件保存到服务器的硬盘上
                            fos.write(bytes,0,len);
                        }
                        //8.使用Socket对象中的方法getOutputStream，获取到网络字节输出流OutputStream对象
                        //9.使用网络字节输入流OutputStream对象中的方法write，给客户端回写"上传成功"
                        socket.getOutputStream().write("上传成功".getBytes());
                        //10.释放资源(FileOutputStream，Socket，ServerSocket)
                        fos.close();
                        socket.close();
                    }catch (IOException e){
                        System.out.println(e);
                    }
                }
            }).start();
        }
        //服务器就不用关闭，因为一直在启动
        //server.close();
    }
}

```

# 上传文件案例















  
