# 数据库设计的范式：需要遵循的一些规范

* 概念：设计关系数据库时，遵从不同的规范要求，设计出合理的关系型数据库，这些不同的规范要求被称为不同的范式，各种范式呈递次规范，越高的范式数据库冗余越小。 
* 分类：
  *  第一范式（1NF）： 每一列都是不可分割的原子数据项（即列下再没有分列），而不能是集合，数组，记录等非原子数据项 
  *  第二范式（2NF）： 在1NF的基础上，非码属性必须完全函数依赖于候选码（在1NF基础上消除非主属性对主码的部分函数依赖） 
     * 几个概念：
       1. 函数依赖：B依赖于A，如果通过A属性（属性组）的值，可以确定唯一B属性的值，则称B依赖于A。
          * 例如：姓名依赖于学号。分数依赖于（学号，课程名称）
       2. 完全函数依赖：如果A是一个属性组，则B属性值的确定需要依赖于A属性组中的所有的属性值。
          * 分数完全依赖于（学号，课程名称）
       3. 部分函数依赖：如果A是一个属性组，则B属性值的确定需要依赖于A属性组中的某些的属性值。
          * 姓名完全依赖于（学号，课程名称）
       4. 传递函数依赖：A-->B,   B-->C ，如果通过A属性（属性组）的值，可以确定唯一B属性的值，通过B属性（属性组）的值，又可以确定唯一C属性的值，则称C传递函数依赖于A
          * 例如：学号-->系名，系名-->系主任
       5. 码   ：<img src="https://ss0.bdstatic.com/94oJfD_bAAcT8t7mm9GUKT-xh_/timg?image&quality=100&size=b4000_4000&sec=1584785100&di=030aacc700a05d847da6e0b7ef2ec361&src=http://img.wxcha.com/file/201809/17/9bcaa4fcd8.jpg" width = "70" height = "50" alt="图片说明" align=center />如果在一张表中，一个属性或属性组，被其他所有属性所完全依赖，则称这个属性（属性值）为该表的码
          1. 如（学号，课程名称）
          2. 主属性：码属性组中的所有属性
          3. 非主属性：除码属性组的属性
  *  第三范式（3NF） ： 在2NF基础上，任何非主属性不依赖于其它非主属性（在2NF基础上消除传递依赖） 

-----------------

## 数据库的备份和还原

1. 命令行的方式：

   1. 语法： 

      ```bash
      备份 ：mysqldump -u用户名 -p密码 数据库名 > 保存的路径
      ```

   2. 还原：

      1. 登录数据库
      2. 创建数据库
      3. 使用数据库
      4. 执行文件。source 文件路径

2. 图形界面mysql备份还原步骤

-----

------

# 多表查询

* 分类：

  1. 内连接查询

     1. 隐式内连接 ：用where语句来清除一些无用的数据既可。

     ```mysql
     select 
     	t1.name,-- 员工表的姓名
     	t1.gender,-- 员工表的性别
     	t2.name-- 部门表的名称
     from 
     	emp t1,dept t2 -- 取别名t1和t2,笛卡尔乘积
     where
     	t1.dept_id = t2.id;
     ```

     2. 显式内连接

        1. 语法：inner可省略

        ```mysql
        select 字段列表 from 表名1 inner join 表名2 on 条件；
        例如：
        select * from emp inner join dept on emp.dept_id=dept.id；
        ```

     3. 内连接查询注意事项：

        1. 从哪些表中查询数据
        2. 条件是什么
        3. 查询哪些字段

  2. 外连接查询

     1. 左外连接：查询的是左表所有数据以及其交集部分 

        1. 语法：outer可省略

           ```mysql
           select 字段列表 from 表名1 left outer join 表名2 on 条件；
           ```

     2. 右外连接

        1. 1. 语法：outer可省略

              ```mysql
              select 字段列表 from 表名1 right outer join 表名2 on 条件；
              ```

  3. 子查询

     1. 概念：查询中嵌套查询，称嵌套查询为子查询

     ```mysql
     -- 查询工资最高的员工信息
     -- 1 查询最高的工资是多少  900
     select max（salary） from emp；
     -- 2 查询员工信息，并且工资等于9000的
     select * from emp where emp.salary = 9000;
     
     -- 一条语句搞定以上两步
     select * from emp where emp.salary = （select max（salary） from emp）；
     ```

     2. 子查询不同情况

        1. 子查询的结果是单行单列的：

           * 子查询可以作为条件，使用运算法去判断。 运算符：> >= < <= =

             ```mysql
             -- 查询员工工资小于平均工资的人
             select * from emp where emp.salary < (select avg (salary) from emp);
             ```

        2. 子查询的结果是多行单列的：

           * 子查询可以作为条件，使用运算符in来判断

           ```mysql
           -- 查询'财务部'和'市场部'所有员工信息
           select id from dept where name = '财务部' or name = '市场部';
           select * from emp where dept_id = 2 or dept_id = 3;
           -- 子查询
           select * from emp where dept_id in (select id from dept where name = '财务部' or name = '市场部');
           ```

        3. 多行多列

           * 子查询可以作为一张虚拟表。

           ```mysql
           -- 查询员工入职日期是2011-11-11日之后的员工信息和部门信息
           select * from dept t1, (select * from emp where join_date > '2011-11-11') t2 where t1.id = t2.dept_id;
           ```

           

* 笛卡尔积：有两个集合A，B，取这两个集合的所有组成情况

  * 利用多表查询消除无用数据

---------------------

## 事务

1. 事务的基本介绍

   1. 概念：

      * 如果一个包含多个步骤的业务操作被事务管理，那么这些操作要么同时成功要么同时失败 

   2. 操作

      1. 开启事务：start transaction；

      2. 回滚：rollback；

      3. 提交：commit；

      4. 在mysql数据库中事务默认自动提交

         * 事务提交的两种方式
           * 自动提交：
             * mysql就是自动提交的
             * 一条DML（增删改）语句会自动提交一次事务。
           * 手动提交
             * oracle数据库是手动提交事务:arrow_backward:
             * 需要先开启事务，再提交
         * 修改事务的默认提交方式：
           * 查看事务的默认提交方式：select @@autocommit; -- 1 代表自动提交  0  代表手动提交
           * 修改默认提交方式：set @@autocommit = 0；

         

2. 事务的四大特征

   1. 原子性：是不可分割的最小操作单位，要么同时成功要么同时失败。
   2. 持久性：当事务提交或回滚后，数据库会持久化的保存数据。
   3. 隔离性：多个事务之间相互独立。
   4. 一致性：事务操作前后数据总量不变。 

3. 事务的隔离级别（了解）

   * 概念：多个事务之间隔离的，相互独立的。但是如果多个事务操作同一批数据，则会引发一些问题，设置不同的隔离级别就可以解决这些问题。

   * 存在问题：

     1. 脏读：一个事务，读取到另一个事务中没有提交的数据
     2. 不可重复读（虚读）：在同一个事务中，两次读取到的数据不一样。
     3. 幻读：一个事务操作（DML）数据表中所有记录，另一个事务中添加了一条数据，则第一个事务查询不到自己的修改。

   * 隔离级别：

     1. read uncommitted：本事务未提交，读取其他未提交事务带来的改变
        * 产生的问题：脏读、不可 重复读、幻读
     2. read   committed：本事务未提交，读取其他已提交事务带来的改变，而未提交的读取不了。
        * 产生的问题：不可 重复读、幻读
     3. repeatable read：可重复读（mysql默认），本事务提交之后，才能看到另一事务带来的改变。
        * 产生的问题：幻读
     4. serializable：串行化，就是锁表，就像多线程加锁，一个表，在某时刻，只能被一个事务操作。
        * 可解决所有问题

     * 注意：隔离级别从小到大越来越安全，但效率也越低

     * 数据库查询与设置隔离级别：

       ```mysql
       select @@tx_isolation;-- 查询
       set global transaction isolation level 级别字符串;-- 设置隔离级别
       ```

------

## DCL：

* 数据库管理员

* DCL：管理用户，授权

  1. 管理用户

     1. 添加用户：
        * 语法：create user '用户名'@'主机名' identified by '密码';
     2. 删除用户：
        * 语法：Drop user '用户名'@'主机名';
     3. 修改用户密码：

     ```mysql
     -- 第一种：update user set password = password('新密码') where user = '用户名';
     -- 第二种dcl特有的: set password for '用户名'@'主机名' = password('新密码');
     -- password()是加密函数
     ```

     4. root密码忘了怎么办
        1. cmd -->net stop mysql 停止mysql服务。管理员运行cmd
        2. 使用无验证方式启动mysql服务：mysql --skip-grant-tables
        3. 打开新的cmd窗口直接输入mysql命令，敲回车，直接登录，改root密码
        4. use mysql;
        5. update user set password = password('你的新密码') where user = 'root';
        6. 关闭两个窗口
        7. 打开任务管理器，手动结束mysqld.exe的进程
        8. 启动mysql服务
        9. 使用新密码登录
     5. 查询用户：

     ```mysql
     -- 1 . 切换到mysql数据库
     use mysql;
     -- 2 . 查询user表
     select * from user;
     
     -- 通配符：% 表示可以在任意主机使用用户登录数据库
     ```

  2. 权限管理

     1. 查询权限

        ```mysql
        show grants for '用户名'@'主机名';
        ```

     2. 授予权限：

        ```mysql
        grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';
        -- 用通配符
        grant all on *.* to '用户名'@'主机名';
        ```

     3. 撤销权限：

        ```mysql
        revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
        
        ```

        
