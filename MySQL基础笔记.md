# MySQL基础笔记

## 一、数据库的基本概念

1. 什么是数据库
   * 用于存储和管理数据的仓库
2. 数据库的特点：
   * 持久化存储数据。数据库就是一个文件系统
   * 方便存储和管理数据
   * 使用同一的方式操作数据库  --  SQL

## 二、MySQL数据库软件

1. 安装
   * 参见《MySQL基础.pdf》
2. 卸载
   * 去mysql的安装目录找到my.ini文件
     * 复制datadir =  "C:/ProgramData/MySQL/MySQL Server 5.5/Data/"
   * 卸载MySQL
   * 删除C:/ProgramData目录下的MySQL文件夹。
3. 配置
   * MySQL服务启动
     * 手动
     * cmd--> services.msc  打开服务的窗口
     * 使用管理员打开cmd
       * net start mysql   :启动mysql的服务
       * net stop mysql    ：关闭mysql的服务
   * MySQL登录
     1. mysql -uroot -p密码
     2. mysql -hip地址 -uroot -p连接目标的密码
     3. mysql --host=ip --user=root  --password=连接目标的密码
   * MySQL退出
     1. exit
     2. quit
   * MySQL目录结构
     1. MySQL安装目录：basedir="D:develop/MySQL/"
        * 配置文件 my.ini
     2. MySQL数据目录：datadir="C:/ProgramData/MySQL/MySQL Server 5.5/Data/"
        * 几个概念
          * 数据库：文件夹
          * 表：文件
          * 记录：数据

## 三、SQL

1. 什么是SQL？

   Structured Query Language:结构化查询语言

   其实就是定义了操作所有关系型数据库的规则。每一种数据库操作的方式存在不一样的地方，称为“方言”。

2. SQL通用语法
    	1. SQL 语句可以单行或多行书写，以分号结尾。
    	2. 可使用空格和缩进来增强语句的可读性。
    	3. MySQL 数据库的 SQL 语句不区分大小写，关键字建议使用大写。
    	4. 3 种注释
        * 单行注释: -- 注释内容 或 # 注释内容(mysql 特有) 
        * 多行注释: /* 注释 */

3. SQL分类

   * DDL(Data Definition Language)数据定义语言

     用来定义数据库对象：数据库，表，列等。关键字：create,drop,alter等

   * DML(Data Manipulation Language)数据操作语言

     用来对数据库中表的数据进行增删改。关键字：insert,delete,update等

   * DQL(Data Query Language)数据查询语言

     用来查询数据库中表的记录（数据）。关键字：select,where等

   * DCL(Data Control Language)数据控制语言（了解）

     用来定义数据库的访问权限和安全级别，及创建用户。关键字：GRANT，REVOKE等。

## DDL：操作数据库、表

1. 操作数据库：CRUD

   1. C(Create):创建
      * 创建数据库：
        * create database 数据库名称;
      * 创建数据库，判断不存在，再创建：
        * create database if not exists 数据库名称;
      * 创建数据库，并指定字符集
        * create database 数据库名称 character set 字符集名;
      * 练习：创建db4数据库，判断是否存在，并指定字符集为gbk
        * create database if not exists db4 character set gbk
   2. R(Retrieve)：查询
      * 查询所有数据库的名称：
        * show databases;
      * 查询某个数据库的字符集：查询某个数据库的创建语句
        * show create database 数据库名称;
   3. U(Update)：修改
      * 修改数据库的字符集
        * alter database 数据库名称 character set 字符集名称;
   4. D(Delete)：删除
      * 删除数据库
        * drop database 数据库名称;
      * 判断数据库存在，再删除
        * drop database if exists 数据库名称：
   5. 使用数据库
      * 查询当前正在使用的数据库名称
        * select database();
      * 使用数据库
        * use 数据库名称;

2. 操作表

   1. C(Create)：创建

      1. 语法：

         create table 表名(

         ​	列名1	数据类型1，

         ​	列名2	数据类型2，

         ​	......

         ​	列名n	数据类型n

         );

         * 注意：最后一行不需要逗号
         * 数据库类型：
           1. int：整数类型
              * age int，
           2. double：小数类型
              * score double(5,2),   一共5位，小数点后2位
           3. date：日期，只包含年月日，yyyy-MM-dd
           4. datatime：日期，包含年月日时分秒  yyyy-MM-dd HH-mm-ss
           5. timestamp:时间类型  格式同上
              * 如果不给此类型字段赋值或赋值为null，则默认使用当前系统时间自动赋值
           6. varchar：字符串
              * name varchar(20)：姓名最大20字符
              * zhangsan 8个字符

      2. 创建表

         create table student(

         ​	id int,

         ​	name varchar(32),

         ​	age int,

         ​	score double(4,1),

         ​	birthday date,

         ​	insert_time timestamp

         );

      3. 复制表

         * create table 表名 like 被复制的表名

   2. R(Retrieve)：查询

      * 查询某个数据库中所有的表名称
        * show tables;
      * 查询表结构
        * desc 表名;

   3. U(Update)：修改

      * 修改表名
        * alter table 表名 rename to 新的表名
      * 修改表的字符集
        * alter table 表名 character set 字符集名称
      * 添加一列
        * alter table 表名 add 列名 数据类型
      * 修改列名称 类型
        * alter table 表名 change 列名 新列名 新数据类型
        * alter table 表名 modify 列名 新数据类型
      * 删除列
        * alter table 表名 drop 列名

   4. D(Delete)：删除

      * drop table 表名;
      * drop table if exists 表名;

## DML：增删改表中数据

1. 添加数据：

   * 语法：

     * insert into 表名(列名1，列名2，...，列名n) values(值1，值2，...，值n)；

   * 注意：

     1. 列名要和值一一对应。

     2. 如果表名后，不定义列名，则默认给所有列添加值

        insert  into 表名 values(值1，值2，....，值n)；

     3. 除了数字类型，其他类型需要使用引号（单双都可）引起来。