# JDBC

* 概念：java DataBase Connectivity  Java 数据库连接，java语言操作数据库
* JDBC本质：其实是官方（sun公司）定义的一套操作所有关系型数据库的规则，即接口。各个数据库厂商去实现这套接口，提供数据库驱动jar包。我们可以使用这套接口 （JDBC）编程，真正执行的代码是驱动jar包中的实现类。

* 快速入门：
  * 步骤
    1. 导入驱动jar包
       1. 复制mysql-connector-java包到项目目录下
    2. 注册驱动
    3. 获取数据库的连接对象  Connection
    4. 定义SQL
    5. 获取执行SQL语句的对象 Statement
    6. 执行SQL，接收返回结果
    7. 处理结果
    8. 释放资源

* 详解各个对象：

  1. DriverManager：驱动管理，一个类

     * 功能：

       1. 注册驱动：告诉程序该使用哪一个数据库驱动jar

       ```java
       static void registerDriver(Driver driver) //:注册与给定的驱动程序DriverManager
       //写代码使用：Class.forName("com.mysql.cj.jdbc.Driver");
       //通过查看源码发现：在com.mysql.cj.jdbc.Driver类中存在静态代码块
           static{
               try{
       			java.sql DriverManager.registerDriver(new Driver());
               }catch(SQLException E){
                   throw new RuntimeException("Can't register driver!")
               }
       	}
       //注意：mysql5之后的驱动jar包可省略注册驱动步骤
       ```

       

     * 获取数据库连接

       * 方法：static Connection getConnection（String url，String user，String password）
       * 参数：
         * url：指定连接的路径
           * 语法：jdbc：mysql：//ip地址（域名）：端口号/数据库名？服务时间
           * 例子：jdbc:mysql://localhost:3306/db1?serverTimezone=UTC
           * 细节：如果连接的是本机mysql服务器，并且mysql服务默认端口是3306，则url简写为jdbc:mysql:///db1?serverTimezone=UTC

  2. Connection：这是一个接口

     1. 功能：
        1. 获取执行sql的对象
           * Statement createStatement（）
           * PreparedStatement prepareStatement(String sql)
        2. 管理实务：
           * 开启事务：setAutoCommit（boolean autoCommit）：调用该方法设置参数为false，即开启事务
           * 提交事务：commit（）
           * 回滚事务：rollback（）

  3. Statement：用于执行静态sql语句，，，是一个接口

     1. 执行sql
        1. boolean execute（String sql）：可以执行任意的SQL  了解一下
        2. int executeUpdate（String sql）：执行DML（insert，update，delete）语句，DDL（create，alter，drop）语句
           * 返回值：影响的行数，可以通过这个影响的行数判断DML语句是否执行成功，返回值>0，成功，反之失败。
        3. ResultSet executeQuery（String sql）：执行DQL（select）语句

  4. ResultSet：结果集对象，封装查询的结果

     1. boolean  next（）：游标向下移动一行，判断当前行是否是最后一行末尾（是否有数据），如果是则返回false，反之返回true。
     2. getxxx（参数）：获取数据，
        1. xxx代表数据类型
        2. 参数
           1. int：代表列的编号。 如getString（1），从1开始，是编号不是索引
           2. String：列的名称。如getDouble（“姓名”）
     3. 注意：
        * 使用步骤：
          1. 游标向下移动一行
          2. 判断是否有数据
          3. 获取数据

  5. PreparedStatement：执行SQL的对象

     1. SQL注入问题：在拼接sql时，有一些sql的特殊关键字参与字符串的拼接，会造成安全性问题

        1. 输入用户随便，输入密码：a'  or 'a'  = 'a
        2. sql  ：   select * from user where   username = 'lkjsdflj' and   password  =   'a'  or 'a'  = 'a'

     2. 解决sql注入问题：PreparedStatement来解决

     3. 预编译的SQL：参数使用？作为占位符

     4. 步骤

        1. 导入驱动jar包

           1. 复制mysql-connector-java包到项目目录下

        2. 注册驱动

        3. 获取数据库的连接对象  Connection

        4. 定义SQL

           * 注意：sql的参数使用？作为占位符。r如

           ```mysql
           select * from user where username = ? and password = ?;
           ```

           

        5. 获取执行SQL语句的对象 PreparedStatement    Connection.preparedStatement(String sql)

        6. 给？赋值

           * 方法：setxxx（参数1，参数2）
             * 参数1：？的位置编号，从1开始
             * 参数2：？的值

        7. 执行SQL，不需要传参了，接收返回结果

        8. 处理结果

        9. 释放资源

     5. 注意：后期都会使用PreparedStatement来完成增删改查的所有操作

        1. 可以防止SQL注入
        2. 效率更高

 ## 抽取JDBC工具类：JDBCUtils

* 目的：简化书写
* 分析：
  1. 注册驱动也抽取
  2. 抽取一个方法获取连接对象
     * 需求：不想传递参数，还要保证工具类的通用性。
     * 解决：配置文件
       * 比如jdbc.properties
  3. 抽取一个方法释放资源

+++++++++++++++

## JDBC控制事务

* 使用connection对象来管理事务
  * 开启事务：setAutoCommit（boolean autoCommit）：调用该方法设置参数为false，即开启事务
    * 在执行sql之前开启事务
  * 提交事务：commit（）
    * 当所有sql都执行完提交事务
  * 回滚事务：rollback（）
    * 在catch中回滚事务
