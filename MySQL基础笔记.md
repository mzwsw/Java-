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
   
2. 删除数据：

   * 语法：
     * delete from 表名 [where 条件]
   * 注意：
     1. 如果不加条件，则删除表中所有记录
     2. 如果要删除所以记录
        1. delete from 表名； --不推荐使用。有多少条记录就会执行多少次删除操作
        2. TRUNCATE TABLE 表名； --先删除表，然后创建一张一模一样的空表（**推荐使用**）

3. 修改数据：

   * 语法：
     * update 表名 set 列名1 = 值1，列名2=值2，....[where 条件]
   * 注意：
     1. 如果不加任何条件，则会将表中所有记录全部修改

## DQL：查询表中的记录

1. 语法：

   select

   ​		字段列表

   from

   ​		表名列表

   where

   ​		条件列表

   group by

   ​		分组字段

   having

   ​		分组之后的条件

   order by

   ​		排序

   limit

   ​		分页限定

   

2. 基础查询

   1. 多个字段的查询

      select 字段名1，字段名2，。。。。，from 表名；

      * 注意：
        * 如果查询所有字段，可以使用*替代字段列表。

   2. 去除重复

      * distinct 关键字

   3. 计算列

      * 一般可以使用四则运算计算一些列的值。（一般只会进行数值型的计算）
      * ifnull(表达式1，表达式2)：
        * 表达式1：哪个字段需要判断是否为null
        * 表达式2：替换为的值

   4. 起别名

      * as：as可以使用空格代替

3. 条件查询

   1. where 子句后跟条件

   2. 运算符

      * \>, <, <=, >=, =， <>(不等于)
      * BETWEEN ... AND
      * IN(集合)
      * LIKE ：模糊查询
        * 占位符：
          * _：单个任意字符
          * %：多个任意字符
      * IS NULL
      * and 或 &&
      * or 或 ||
      * not 或 ！

      1. 查询年龄大于20岁

         select * from student where age > 20;

         select * from student where age >= 20;

      2. 查询年龄等于20岁

         select * from student where age = 20;

4. 排序查询

   * 语法：order by 子句
     * order by 排序字段1 排序方式1， 排序字段2 排序方式2。。。
   * 排序方式：
     * ASC：升序（默认）
     * DESC：降序。
   * 注意：
     * 如果有多个排序条件，则当前面条件值一样时，才会使用后面的条件

5. 聚合函数

   **将一列数据作为一个整体，进行纵向的计算**

   1. count：计算个数

      select count(列名) from 表名；

   2. max：计算最大值

      select max(列名) from 表名；

   3. min：计算最小值

      select min(列名) from 表名；

   4. sum：计算和

      select sum(列名) from 表名；

   5. avg：计算平均值

   * **注意：聚合函数会略过NULL值**
     * 解决方案：
       1. 选择不包含空的列进行计算
       2. IFNULL函数

6. 分组查询

   1. 语法：group by 字段
   2. 注意：
      * 分组之后查询的字段：分组字段、聚合函数
      * **where和having的区别？**
        1. where在分组之前进行限定，如果不满足条件，则不参与分组。having在分组之后进行限定，如果不满足条件，则不会被查询出来。
        2. where后不可以跟聚合函数，而having可以利用聚合函数判断

7. 分页查询

   1. 语法：limit 开始的索引，每页查询的条数
   2. 公式：开始的索引 = （当前的页码 - 1） * 每页现实的条数
   3. **limit分页操作是一个MySQL“方言”**

## 约束

* 概念：对表中的数据进行限定，保证数据的正确性、有效性和完整性。

* 分类：
  1. 主键约束：primary key
  2. 非空约束：not null
  3. 唯一约束：unique
  4. 外键约束：foreign key

* **非空约束**

  1. 创建表时添加约束

     CREATE TABLE stu(

     ​		id INT,

     ​		NAME VARCHAR(20) NOT NULL  --name非空

     )；

  2. 创建表完后，添加非空约束

     ALTER TABLE stu MODIFY NAME VARCHAR(20) NOT NULL;

  3. 删除非空约束

     ALTER TABLE stu MODIFY NAME VARCHAR(20);

* **唯一约束:unique**

  1. 创建表时，添加唯一约束

     CREATE TABLE stu(

     ​		id INT,

     ​		phone_number VARCHAR(20)  UNIQUE   --添加了唯一约束

     );

     * 注意：mysql中，唯一约束限定的列的值可以有多个null

  2. 删除唯一约束

     ALTER TABLE stu DROP INDEX phone_number;

  3. 在创建表后，添加唯一约束

     ALTER TABLE stu MODIFY phone_number VARCHAR(20) UNIQUE;

* **主键约束：primary key**

  1. 注意：

     * 含义：非空且唯一
     * 一张表只能有一个字段为主键
     * 主键就是表中记录的唯一标识

  2. 在创建表时，添加主键约束

     CREATE TABLE stu(

     ​		id INT primary key,  -- 给id添加主键约束

     ​		name VARCHAR(20)

     );

  3. 删除主键

     ALTER TABLE stu DROP PRIMARY KEY;

  4. 创建表后，添加主键

     ALTER TABLE stu MODIFY id INT PRIMARY KEY;

  5. 自动增长：

     * 概念：如果某一列是数值类型的，使用auto_increment 可以来完成值的自动增长

     * 在创建表时，添加主键约束，并完成主键自动增长

       CREATE TABLE stu (

       ​		id INT PRIMARY KEY AUTO_INCREMENT,

       ​		NAME VARCHAR(20)

       );

     * 删除自动增长

       ALTER TABLE stu MODIFY id INT;

     * 添加自动增长

       ALTER TABLE stu MODIFY id INT AUTO_INCREMENT;

* **外键约束：foreign key**：让表与表产生关系，从而保证数据的正确性

  1. 在创建表时，可以添加外键

     * 语法：

       create table 表名(

       ​		......

       ​		外键列
       
       ​		constraint 外键名称 forengn key （外键列名称） references 主表名称（主表列名称）
       
       )；

  2. 删除外键

     ALTER TABLE 表名 DROP FOREIGN KEY 外键名称；

  3. 创建表之后，添加外键

     ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY （外键列名称） REFERENCES 主表名称（主表列名称）；

  4. 级联操作

     1. 添加级联操作

        * 语法：

          ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键列名称) REFERENCES 主表名称（主表列名称） ON UPDATE CASCADE ON DELETE CASCADE;

     2. 分类：

        1. 级联更新：ON UPDATE CASCADE
        2. 级联删除：ON DELETE CASCADE

## 数据库的设计

* 多表之间的关系
  * 分类：
    1. 一对一（了解）：
    2. 一对多（多对一）：
    3. 多对多：
  * 实现：
    1. 一对多（多对一）：
       * 如：部门和员工
       * 实现方式：在多的一方建立外键，指向一的一方的主键
    2. 多对多：
       * 如：学生和课程
       * 实现方式：多对多关系实现需要借助第三张中间表。中间表至少包含两个字段，这两个字段作为   第三张表的外键，分别指向两张表的主键。
    3. 一对一（了解）
* 数据库的范式
  * 概念：设计数据库时，需要遵循的一些规范。要遵循后边的范式要求，必须先遵循前面的所有范式要求。
  * 分类：
    1. 第一范式（1NF）：每一列都是不可分割的原子数据项
    2. 第二范式（2NF）：在1NF的基础上，非码属性必须完全依赖于候选码（在1NF基础上消除非主属性对主码的部分函数依赖）
       * 几个概念：
         1. 函数依赖：A-->B，如果通过A属性（属性组）的值，可以确定唯一的B属性的值。则B依赖于A。
         2. 完全函数依赖：A-->B，如果A是一个属性组，则B属性值的确定需要依赖于A属性组中所有的属性值。例如：（学号，课程名称）--> 分数
         3. 部分函数依赖：A-->B，如果A是一个属性组，则B属性值的去顶只需要依赖于A属性组中某一些值即可。例如：（学号，课程）-->姓名
         4. 传递函数依赖：A-->B, B-->C。 如果通过A属性（组）的值，可以确定唯一B属性的值，在通过B的值唯一确定C属性的值。则称C传递函数依赖于A。
         5. 码：如果在一张表中，一个属性或属性组，被其他所有属性所完全依赖，则称这个属性（属性组）为该表的码。
    3. 第三范式（3NF）：在2NF基础上，任何非主属性不依赖于其它非主属性（在2NF基础上消除传递依赖）

## 数据库的备份和还原

1. 命令行：
   * 语法：
     * 备份：mysqldump -u用户名 -p密码  数据库名称 > 保存路径
     * 还原：
       1. 登录数据库
       2. 创建数据库
       3. 使用数据库
       4. 执行文件： source 文件路径
2. 图形化工具：

## 多表查询

* 多表查询的分类：

  1. 内连接查询：

     1. 隐式内连接：使用where条件消除无用数据

        * 例子：查询所有员工信息和对应的部分信息

          SELECT * FROM emp,dept WHERE emp.'dept_id' = dept.'id'

        * --查询员工表的名称，性别。部门表的名称

          SELECT emp.name, emp.gender, dept.name FROM emp,dept WHERE emp.'dept_id' = dept.'id'
          
          SELECT
          
          ​		t1.name,
          
          ​		t1.gender,
          
          ​		t2.name
          
          FROM
          
          ​		emp t1,
          
          ​		dept t2
          
          WHERE
          
          ​		t1.dept_id = t2.id;
        
     2. 显式内连接
     
        *  语法： SELECT 字段列表 from 表名1 [inner] join 表名2 on 条件
     
        * 例如：
        * SELECT * FROM emp INNER JOIN dept ON emp.'dept_id' = dept.'id'
          SELECT * FROM emp JOIN dept ON emp.'dept_id' = dept.'id'
     
     3. 内连接查询的条件：
     
        * 从哪些表中查询数据
        * 条件是什么
        * 查询哪些字段

  2. 外连接查询：

     1. 左外连接：
        * 语法： SELECT 字段列表 from 表名1 left [outer] join 表名2 on 条件
        * 查询的是左表所有数据以及其交集部分。
     2. 右外连接
        * 语法： SELECT 字段列表 from 表名1 right [outer] join 表名2 on 条件
        * 查询的是右表所有数据以及其交集部分。

  3. 子查询：

     * 概念：查询中嵌套查询，称嵌套查询为子查询

     * 子查询的不同情况

       1. 子查询的结果是单行单列：

          * 子查询可以作为条件，使用运算符去判断。 > >- < <= =

          * 例如：查询员工工资小于平均工资的人

            SELECT * FROM emp WHERE emp.salary < (SELECT AVG(salary) FROM emp);

       2. 子查询的结果是多行单列：

          * 子查询可以作为条件，使用运算符in来判断

          * --查询‘财务部’和‘市场部’所有的员工信息

            SELECT * FROM emp WHERE dept_id IN (SELECT id FROM dept WHERE NAME = '财务部' OR NAME = '市场部')；

       3. 子查询的结果是多行多列：

          * 子查询可以作为一张虚拟表。

          * --查询员工入职日期是2011-11-11之后的员工信息和部门信息

            SELECT * FROM dept t1, (SELECT * FROM emp WHERE emp.join_date > '2011-11-11') t2 WHERE t1.id = t2.dept_id;

          * 普通内连接实现

            SELECT * FROM emp t1, dept t2 WHERE t1.dept_id = t2.id AND t1.join_date > '2011-11-11';
          
       4. 子查询小结：
       
          * 子查询结果只要是单列，则在where后面作为条件
          * 子查询结果只要是多列，则在FROM后面作为表进行二次查询

## 事务

MySQL中可以有两种方式进行事务的操作：

* 手动提交事务

  1. SQL语句

     开启事务	start transaction

     提交事务	commit

     回滚事务	rollback

  2. 使用过程：

     * 执行成功的情况：开启事务-->执行多条SQL语句-->成功提交事务
     * 执行失败的情况：开启事务-->执行多条SQL语句-->事务的回滚

* 自动提交事务

  MySQL默认每一条DML（增删改）语句都是一个单独的事务，每条语句都会自动开启一个事务，语句执行完毕自动提交事务，MySQL默认开启自动提交事务。

事务的步骤：

*  客户端连接数据库服务器，创建连接时创建此用户临时日志文件
*  开启事务以后，所有的操作都会先写入到临时日志文件中
*  所有的查询操作从表中查询，但会经过日志文件加工后才返回
*  如果事务提交则将日志文件中的数据写到表中，否则清空日志文件。

回滚点：

* 设置回滚点	savepoint 名字
* 回到回滚点    rollback to 名字

**事务的隔离级别**：

1. 事务的四大特性ACID

   * 原子性：每个事务都是一个整体，不可再拆分，事务中所有的SQL语句要么				都执行成功，要么都失败。
   * 一致性：事务在执行前数据库的状态与执行后数据库的状态保持一致。
   * 隔离性：事务与事务之间不应该相互影响，执行时保持隔离的状态。
   * 持久性：一旦事务执行成功，对数据库的修改是持久的。就算关机，也是保   存下来的。

2. 事务的隔离级别

   * 脏读：一个事务读取到了另一个事务中尚未提交的数据
   * 不可重复读：一个事务中两次读取的**数据内容**不一致，要求的是一个事务中多次读取时数据是一致的，这是事务update时引发的问题。
   * 幻读：一个事务中两次读取的数据的**数量**不一致，要求在一个事务多次读取的数据的数量是一致的，这是insert或delete时引发的问题。

3. MySQL数据库有四种隔离级别

   上面的级别最低，下面的级别最高

   隔离级别越高，性能越差，安全性越高

   * 读未提交	read uncommitted	是	是	是
   * 读已提交    read committed         否    是    是
   * 可重复读    repeatable read         否    否    是
   * **串行化**        serializable                 否    否    否

4. 隔离级别相关命令

   查询全局事务隔离级别	select @@tx_isolation;

   设置事务隔离级别    set global transaction isolation level 级别字符串

## DCL(Data Control Language)

1. 创建用户

   create user ‘用户名’@‘主机名’ identified by ‘密码’；

2. 给用户授权

   grant 权限1，权限2..... on 数据库名.表名 to ‘用户名’@‘主机名’

3. 撤销授权

   revoke 权限1，权限2.... on 数据库.表名 from ‘用户名’@‘主机名’

4. 查看权限

   SHOW GRANTS FOR '用户名'@'主机名';

5. 删除用户

   DROP USER '用户名'@'主机名';