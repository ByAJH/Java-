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

-------------------

## JDBC控制事务

* 使用connection对象来管理事务
  * 开启事务：setAutoCommit（boolean autoCommit）：调用该方法设置参数为false，即开启事务
    * 在执行sql之前开启事务
  * 提交事务：commit（）
    * 当所有sql都执行完提交事务
  * 回滚事务：rollback（）
    * 在catch中回滚事务
* * * 

-------------------------

## 数据库连接池

1. 概念：其实就是一个容器（集合），存放数据库连接的容器
   1. 当系统初始化好后，容器被创建，容器中会申请一些连接对象，当用户来访问数据库时，从容器中获取连接对象，用户访问完后，会将连接对象归还给容器。
2. 好处：
   1. 节约资源
   2. 用户访问高效
3. 实现：
   1. 标准接口：DataSource     javax.sql包下的
      1. 方法：
         * 获取连接：getConnection（）
         * 归还连接：Connection.close()   如果连接对象Connection是从连接池中获取的，那么调用Connection.close（）方法，则不会再关闭连接了。而是归还连接
   2. 一般我们不去实现它，有数据库厂商来实现
      1. C3P0：数据库连接池技术
         1. 导入jar包（两个）：c3p0-0.9.5.2.jar和mchange-commons-java-0.2.12.jar
         2. 定义配置文件
            * 名称：c3p0.properties  或者 c3p0-config.xml
            * 路径：直接将文件放在src目录下即可。
         3. 创建核心对象  ： 数据库连接池对象  ComboPooledDataSource
         4. 获取连接：getConnection 
      2. Druid：数据库连接池实现技术，由阿里巴巴提供的 
         * 步骤：
           1. 导入jar包  druid-1.0.9.jar
           2. 定义配置文件：
              1. 是properties形式的
              2. 可以叫任意名称，可以放在任意目录下
           3. 加载配置文件。properties
           4. 获取数据库连接池对象：通过工厂类来获取   DruidDataSourceFactory
           5. 获取连接：getConnection
         * 定义工具类
           1. 定义一个类   JDBCUtils
           2. 提供静态代码块加载配置文件，初始化连接池对象
           3. 提供方法
              1. 获取连接方法：通过数据库连接池获取连接
              2. 释放资源
              3. 获取连接池的方法

## Spring JDBC

* Spring框架对JDBC的简单封装。提供了一个JDBCTemplate对象简化JDBC的开发
* 步骤：
  1. 导入jar包
  2. 创建JDBCTemplate对象。依赖于数据源DataSource
     * JdbcTemplate    template = new JdbcTemplate（ds）；
  3. 调用JdbcTemplate的方法来完成CRUD的操作
     * update（）：执行DML语句。用于执行增删改语句
     * queryForMap（）：查询结果将结果集封装为map集合
       * 注意：这个方法查询的结果集长度只能是1
     * queryForList（）：查询结果将结果集封装为List集合
       * 注意：将每条记录封装成一个map集合，再将这些map集合装入list集合中  
     * query（）：查询结果将结果集封装为JavaBean对象
       * template.query（sql，new RowMapper<Object>（）{    }）
       * template.query（sql，new BeanPropertyRowMapper<Object>（Object.class））
     * queryForObject（）：查询结果将结果集封装为对象，执行一些聚合函数
  4. 练习：
     * 需求 
       1. 修改1号数据某值为1000
       2. 添加一条记录
       3. 删除刚才添加的记录
       4. 查询id为1的记录，将其封装为map集合
       5. 查询所有记录
       6. 查询所有记录，将其封装为list集合
       7. 查询总记录数
