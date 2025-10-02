# MySQL

## 0 SQL初识

| 名称           | 全称                                                         | 简称                                 |
| -------------- | ------------------------------------------------------------ | ------------------------------------ |
| 数据库         | 存储数据的仓库，数据是有组织的进行存储                       | DataBase（DB）                       |
| 数据库管理系统 | 操纵和管理数据库的大型软件                                   | DataBase  Management  System（DBMS） |
| SQL            | 操作关系型数据库的编程语言，定义了一套操作关系型数据库统一**标准** | Structured  Query  Language（SQL）   |



目前主流的关系型数据库管理系统的市场占有率排名如下：

* Oracle：大型的收费数据库，Oracle 公司产品，价格昂贵
* MySQL：开源免费的中小型数据库，后来 Sun 公司收购了 MySQL，而 Oracle 又收购了 Sun 公司
    目前 Oracle 推出了收费版本的 MySQL，也提供了免费的社区版本
* SQL Server：Microsoft 公司推出的收费的中型数据库，`C#`、`.net` 等语言常用
* PostgreSQL：开源免费的中小型数据库
* DB2：IBM 公司的大型收费数据库产品
* SQLLite：嵌入式的微型数据库，Android 内置的数据库采用的就是该数据库
* MariaDB：开源免费的中小型数据库，是 MySQL 数据库的另外一个分支、另外一个衍生产品，与 MySQL 数据库有很好的兼容性



不论我们使用的是上面的哪一个关系型数据库，最终在操作时，都是使用SQL语言来进行统一操作

因为我们前面讲到 `SQL` 语言，是操作关系型数据库的==统一标准==



### 0.1 安装 MySQL

MySQL下载地址：https://downloads.mysql.com/archives/installer/



注意：

输入 MySQL 中 root 用户的密码，一定记得记住该密码

![](assets\root密码.png) 





安装后，在设置中打开 `系统-系统信息-高级系统设置`，修改 `Path` **系统**环境变量

![](assets\Path环境变量.png) 



MySQL 安装完成之后，在系统启动时，会自动启动MySQL服务，我们无需手动启动了

当然，也可以手动的通过指令启动停止，以管理员身份运行 `cmd`，进入命令行执行如下指令：

``` sh
net start mysql80	# 启动
net stop mysql80 	# 停止
```

注意 ： 上述的 mysql80 是我们在安装 MySQL 时，默认指定的 MySQL的系统服务名，不是固定的，如果未改动，默认就是mysql80





### 0.2 客户端连接

使用系统自带的命令行工具执行指令



在终端中输入以下指令

```sql
mysql [-h 127.0.0.1] [-P 3306] -u root -p
# 参数：
# -h : MySQL服务所在的主机IP
# -P : MySQL服务端口号, 默认3306
# -u : MySQL数据库用户名
# -p ： MySQL数据库用户名对应的密码

mysql -u root -p
```

`[]` 内为可选参数，如果需要连接远程的 MySQL，需要加上这两个参数来指定远程主机IP、端口，如果连接本地的MySQL，则无需指定这两个参数



之后输入正确的密码进入 MySQL

![](assets\进入MySQL.png) 







### 0.3 关系型数据库

关系型数据库（RDBMS）是建立在关系模型基础上，由多张相互连接的二维表组成的数据库



所谓二维表，指的是由行和列组成的表，如下图（就类似于Excel表格数据，有表头、有列、有行，还可以通过一列关联另外一个表格中的某一列数据）

简单说，基于二维表存储数据的数据库就成为关系型数据库，不是基于二维表存储数据的数据库，就是非关系型数据库



![](assets\二维表.png)

特点：

* 使用表存储数据，格式统一，便于维护
* 使用SQL语言操作，标准统一，使用方便



MySQL 是关系型数据库，是基于二维表进行数据存储的，具体的结构图下：

<img src="assets\连接客户端.png" style="zoom: 67%;" />



我们可以通过 MySQL 客户端连接数据库管理系统 DBMS，然后通过 DBMS 操作数据库

可以使用 SQL 语句，通过数据库管理系统操作数据库，以及操作数据库中的表结构及数据

一个数据库服务器中可以创建多个数据库，一个数据库中也可以包含多张表，而一张表中又可以包含多行记录



```mermaid
flowchart LR
    服务器 --> 数据库 --> 表 --> 列
```









## 1 SQL语法

SQL 全称 Structured Query Language，结构化查询语言

操作关系型数据库的编程语言，定义了一套操作关系型数据库==统一标准==



### 1.1 语法

统一语法：

* SQL 语句可以单行或多行书写，以分号结尾
* SQL 语句可以使用空格/缩进来增强语句的可读性
* MySQL 数据库的 SQL 语句不区分大小写，关键字建议使用大写
* 注释：
    单行注释：`--` 或 `#` 跟注释内容
    多行注释：`/* 注释内容 */`



SQL语句，根据其功能，主要分为四类：DDL、DML、DQL、DCL

| 分类 | 全称                       | 说明                                                   |
| ---- | -------------------------- | ------------------------------------------------------ |
| DDL  | Data Definition Language   | 数据定义语言，用来定义数据库对象（数据库，表，字段）   |
| DML  | Data Manipulation Language | 数据操作语言，用来对数据库表中的数据进行增删改         |
| DQL  | Data Query Language        | 数据查询语言，用来查询数据库中表的记录                 |
| DCL  | Data Control Language      | 数据控制语言，用来创建数据库用户、控制数据库的访问权限 |







### 1.2 数据类型

MySQL中的数据类型有很多，主要分为三类：数值类型、字符串类型、日期时间类型



1. 数值类型

| 类型        | 大小   | 有符号（signed）范围                                  | 无符号（unsigned）范围                                      | 描述           |
| ----------- | ------ | ----------------------------------------------------- | ----------------------------------------------------------- | -------------- |
| tinyint     | 1byte  | (-128，127)                                           | (0，255)                                                    | 小整数值       |
| smallint    | 2bytes | (-32768，32767)                                       | (0，65535)                                                  | 大整数值       |
| mediumint   | 3bytes | (-8388608，8388607)                                   | (0，16777215)                                               | 大整数值       |
| int/integer | 4bytes | (-2147483648，2147483647)                             | (0，4294967295)                                             | 大整数值       |
| bigint      | 8bytes | (-2^63^，2^63^-1)                                     | (0，2^64^-1)                                                | 极大整数值     |
| flaot       | 4bytes | (-3.402823466 E+38，3.402823466351E+38)               | 0 和  (1.175494351 E-38，3.402823466 E+38)                  | 单精度浮点数值 |
| double      | 8bytes | (-1.7976931348623157 E+308，1.7976931348623157 E+308) | 0 和  (2.2250738585072014 E-308，1.7976931348623157  E+308) | 双精度浮点数值 |

注意：

* 使用无符号数值类型，需要在类型后面加上 `unsigned` 关键字，如 `tinyint unsigned`，有符号类型可以省略 `signed`
* 使用浮点数数值类型，需要在类型后面加上 `(M,D)`，M表示数字总长度，D表示保留小数位数，如 `doule(3,1)`



2. 字符串类型

| 类型       | 大小               | 描述                         |
| ---------- | ------------------ | ---------------------------- |
| char       | 0-255 bytes        | 定长字符串(需要指定长度)     |
| varchar    | 0-65535 bytes      | 变长字符串(需要指定长度)     |
| tinyblob   | 0-255 bytes        | 不超过255个字符的二进制数据  |
| tinytext   | 0-255 bytes        | 短文本字符串                 |
| blob       | 0-65535 bytes      | 二进制形式的长文本数据       |
| text       | 0-65535 bytes      | 长文本数据                   |
| mediumblob | 0-16777215 bytes   | 二进制形式的中等长度文本数据 |
| mediumtext | 0-16777215 bytes   | 中等长度文本数据             |
| longblob   | 0-4294967295 bytes | 二进制形式的极大文本数据     |
| longtext   | 0-4294967295 bytes | 极大文本数据                 |

注意：

* 使用 ` char ` 和 `varchar` 字符串类型时，需要在类型后面加上 `(len)`指定字符串长度，如 `varchar(50)`

* `char` 与 `varchar` 都可以描述字符串

    `char` 是定长字符串，指定长度多长，就占用多少个字符，和字段值的长度无关 

    `varchar` 是变长字符串，指定的长度为最大占用长度（相对来说，`char`的性能会更高些）



3. 日期时间类型

| 类型      | 大小 | 范围                                       | 格式                | 描述                     |
| --------- | ---- | ------------------------------------------ | ------------------- | ------------------------ |
| date      | 3    | 1000-01-01  至 9999-12-31                  | YYYY-MM-DD          | 日期值                   |
| time      | 3    | -838:59:59  至 838:59:59                   | HH:MM:SS            | 时间值或持续时间         |
| year      | 1    | 1901 至 2155                               | YYYY                | 年份值                   |
| datetime  | 8    | 1000-01-01 00:00:00 至 9999-12-31 23:59:59 | YYYY-MM-DD HH:MM:SS | 混合日期和时间值         |
| timestamp | 4    | 1970-01-01 00:00:01 至 2038-01-19 03:14:07 | YYYY-MM-DD HH:MM:SS | 混合日期和时间值，时间戳 |







## 2 DDL 数据定义

### 2.1 数据库操作

1. 查询数据库

```sql
show databases ;		-- 查询所有数据库
select database() ;		-- 查询当前数据库
```



2. 创建数据库

```sql
create database [ if not exists ] 数据库名 [ default charset 字符集 ] [ collate 排序规则 ] ;

#例
create database itcast;		-- 创建一个 itcast 数据库, 使用数据库默认的字符集
create database if not extists itcast;
							-- 通过if not exists 参数来解决数据库已经存在的问题, 数据库不存在, 则创建该数据库, 如果存在, 则不创建
create database itheima default charset utf8mb4;
							-- 创建一个 itheima 数据库, 并且指定字符集
```



3. 删除数据库

```sql
drop database [ if exists ] 数据库名 ;

#例
drop database test;				-- 删除 test 数据库
drop database if exists test;	-- 通过参数 if exists, 如果数据库存在, 再执行删除, 否则不执行删除
```



4. 切换数据库

要操作某一个数据库下的表时，就需要通过该指令，切换到对应的数据库下

```sql
use 数据库名 ;

#例  切换到 sys 这个系统数据库
use sys;
```









### 2.2 表操作

1. 查询表

```sql
show tables;	-- 查询当前数据库所有表
desc 表名 ;	   -- 查看指定表结构
show create table 表名 ;
				-- 查看建表语句
```



2. 创建表

```sql
create table 表名(
    字段1 字段1类型 [comment 字段1注释 ],
    字段2 字段2类型 [comment 字段2注释 ],
    字段3 字段3类型 [comment 字段3注释 ],
    ......
    字段n 字段n类型 [comment 字段n注释 ]	-- 最后一个字段后面没有逗号
) [ COMMENT 表注释 ] ;


#例  创建一个用户表, 每个用户有编号, 姓名, 年龄, 性别这几个属性
create table tb_user(
    id int comment '编号',
    name varchar(50) comment '姓名',
    age int comment '年龄',
    gender varchar(1) comment '性别'
) comment '用户表';
```



3. 删除表

```sql
# 删除表
drop table [ if exists ] 表名;
			-- 可选项 if exists 代表，只有表名存在时才会删除该表，表名不存在，则不执行删除操作

# 清空表(删除指定表后再重新创建, 表中的全部数据都会被删除)
truncate table 表名;
```









### 2.3 字段操作（修改表）

1. 添加字段

```sql
alter table 表名 add 字段名 类型 (长度) [ comment 注释 ] [ 约束 ];

#例  为 emp 表增加一个新的字段 "昵称" 为nickname, 类型为 varchar(20)
alter table emp add nickname varchar(20) comment '昵称';
```



2. 修改数据类型

```sql
alter table 表名 modify 字段名 新数据类型 (长度);
```



3. 修改字段名和字段类型

```sql
alter table 表名 change 旧字段名 新字段名 类型 (长度) [ comment 注释 ] [ 约束 ];

#例 	将 emp 表的 nickname 字段修改为 username, 类型为 varchar(30)
alter table emp change nickname username varchar(30) comment '昵称';
```



4. 删除字段

```sql
alter table 表名 drop 字段名;

#例  将 emp 表的字段 username 删除
alter table emp drop username;
```



5. 修改表名

```sql
alter table 表名 rename to 新表名;

#例  将 emp 表的表名修改为 employee
alter table emp rename to employee;
```









## 3 DML 数据操作

### 3.1 添加数据

1. 给指定字段添加数据

```sql
insert into 表名 (字段名1, 字段名2, ...) values (值1, 值2, ...) ;

#例  指定字段插入数据到 employee 表
insert into employee(id,name,gender,age) values(1,,'Itcast','男',10);
```



2. 给全部字段添加数据

```sql
insert into 表名 values (值1, 值2, ...) ;

#例  全部字段插入数据到 employee 表
insert into employee values(2,'2','张无忌','男',18,'123456789012345670','2005-01-01');
```



3. 批量添加数据

```sql
insert into 表名 (字段名1, 字段名2, ...) values (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...) ;
insert into 表名 values (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...) ;

#例  批量插入数据到 employee 表
insert into employee values(3,'3','韦一笑','男',38,'123456789012345670','2005-01-01'),(4,'4','赵敏','女',18, '123456789012345670','2005-01-01');
```







### 3.2 修改数据

1. 修改数据

```sql
update 表名 set 字段名1 = 值1 , 字段名2 = 值2 , ....  [ where 条件 ] ;

#例
update employee set name = 'itheima' where id = 1;					# 将id为1的数据的name修改为itheima
update employee set name = '小昭' , gender = '女' where id = 1;	  # 将id为1的数据的name修改为小昭, gender修改为 女
update employee set entrydate = '2008-01-01';						# 将所有的员工入职日期修改为 2008-01-01
```

注意：修改语句的条件可以有，也可以没有，如果没有条件，则会修改整张表的所有数据




2. 删除数据

```sql
delete from 表名 [ where 条件 ] ;

#例
delete from employee where gender = '女';	# 删除gender为女的员工
delete from employee;						 # 删除所有员工
```

注意：

* `delete` 语句的条件可以有，也可以没有，如果没有条件，则会删除整张表的所有数据
* `delete` 语句不能删除某一个字段的值（可以使用UPDATE，将该字段值置为NULL即可）
* 当进行删除全部数据操作时，`datagrip` 会提示我们，询问是否确认删除，我们直接点击 `Execute` 即可





## 4 DQL 数据查询

### 4.1 基础查询

1. 查询字段

```sql
select 字段1, 字段2, 字段3 ... from 表名 ;
select * from 表名 ;

#例
select name,workno,age from emp;	# 查询指定字段
select * from emp;					# 查询字段全部
```

注意 ：`*` 号代表查询所有字段，在实际开发中尽量少用（不直观、影响效率）



2. 查询并设置别名

```sql
select 字段1 [ as 别名1 ] , 字段2 [ as 别名2 1 ] ... from 表名;
select 字段1 [ 别名1 ] , 字段2 [ 别名2 ] ... from 表名;

#例  查询所有员工的工作地址, 起别名
select workaddress as '工作地址' from emp;
select name , workaddress as '工作地址' from emp;
```

注意：起别名可以将查询时的字段改成起的别名，方便查看



3. 查询并去除重复记录

```sql
select distinct 字段列表 from 表名;

#例  查询公司员工的上班地址有哪些, 并不要去掉重复内容
select distinct workaddress '工作地址' from emp;
```







### 4.2 条件查询

常用的比较运算符如下: 

| 比较运算符          | 功能                                             |
| ------------------- | ------------------------------------------------ |
| >                   | 大于                                             |
| >=                  | 大于等于                                         |
| <                   | 小于                                             |
| <=                  | 小于等于                                         |
| =                   | 等于                                             |
| <>  或 !=           | 不等于                                           |
| between ... and ... | 在某个范围之内(含  小、 大值)                    |
| in(...)             | 在 in 之后的列表中的值，多选一                   |
| like 占位符         | 模糊匹配( `_` 匹配单个字符,  `%` 匹配任意个字符) |
| is null             | 判断是否是是null                                 |



常用的逻辑运算符如下:

| 逻辑运算符  | 功能                        |
| ----------- | --------------------------- |
| and 或  &&  | 并且 (多个条件同时成立)     |
| or 或  \|\| | 或者 (多个条件任意一个成立) |
| not 或  !   | 非 , 不是                   |

 

**例：**

```sql
# 查询年龄等于 88 的员工
select * from emp where age = 88;

# 查询年龄不等于 88 的员工信息
select * from emp where age != 88;
select * from emp where age <> 88;

# 查询没有身份证号的员工信息
select * from emp where idcard is null;

# 查询有身份证号的员工信息
select * from emp where idcard is not null;

# 查询年龄在15岁(包含) 到 20岁(包含)之间的员工信息
select * from emp where age >= 15 && age <= 20;
select * from emp where age >= 15 and age <= 20;
select * from emp where age between 15 and 20;

# 查询性别为 女 且年龄小于 25岁的员工信息
select * from emp where gender = '女' and age < 25;

# 查询年龄等于18 或 20 或 40 的员工信息
select * from emp where age = 18 or age = 20 or age =40;
select * from emp where age = 18 || age = 20 || age =40;
select * from emp where age in(18,20,40);

# 查询姓名为两个字的员工信息
select * from emp where name like '__';

# 查询身份证号最后一位是X的员工信息
select * from emp where idcard like '%X';
select * from emp where idcard like '_________________X';
```







### 4.3 聚合函数

将一列数据作为一个整体，进行纵向计算



常见的聚合函数：

| 函数  | 功能     |
| ----- | -------- |
| count | 统计数量 |
| max   | 大值     |
| min   | 小值     |
| avg   | 平均值   |
| sum   | 求和     |



**例：**

```sql
select 聚合函数(字段列表) from 表名 ;

# 统计该企业员工数量
select count(*) from emp; 		-- 统计的是员工数
select count(idcard) from emp; 	-- 统计的是 idcard 字段不为 null 的员工数

# 统计该企业员工的平均年龄
select avg(age) from emp;

# 统计该企业员工的最大/最小年龄
select max(age) from emp;
select min(age) from emp;

# 统计西安地区员工的年龄之和
select sum(age) from emp where workaddress = '西安';
```

注意 : `NULL`  值是不参与所有聚合函数运算的







### 4.4 分组查询

用于将数据按照某些字段进行分组（将该字段的值一样的内容分配到一组），然后对每组数据进行聚合计算



`where` 与 `having` 区别：

* 执行时机不同：`where` 是分组之前进行过滤，不满足 `where` 条件，不参与分组；而 `having` 是分组之后对结果进行过滤
* 判断条件不同：`where`  不能对聚合函数进行判断，而 `having` 可以



**例：**

```sql
select 字段列表 from 表名 [ WHERE 条件 ] group by 分组字段名 [ having 分组后过滤条件 ];

# 根据性别分组, 统计男性员工和女性员工的数量
select gender, count(*) from emp group by gender;

# 根据性别分组, 统计男性员工和女性员工的平均年龄
select gender, avg(age) from emp group by gender;

# 查询年龄小于45的员工, 并根据工作地址分组, 获取员工数量大于等于3的工作地址
select workaddress, count(*) address_count from emp where age < 45 group by workaddress having address_count >= 3;

# 统计各个工作地址上班的男性及女性员工的数量
select workaddress, gender, count(*) '数量' from emp group by gender , workaddress;
```

注意：

* 分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义
* 执行顺序: `where` > 聚合函数 > `having `
* 支持多字段分组，具体语法为：`group by columnA,columnB`







### 4.5 排序查询

排序方式：

* ASC：升序(默认值)
* DESC：降序



**例：**

```sql
select 字段列表 from 表名 order by 字段1 排序方式1 , 字段2 排序方式2 ;

# 根据年龄对公司的员工进行升序排序
select * from emp order by age asc;
select * from emp order by age;

# 根据入职时间, 对员工进行降序排序
select * from emp order by entrydate desc;

# 根据年龄对公司的员工进行升序排序, 年龄相同, 再按照入职时间进行降序排序
select * from emp order by age asc , entrydate desc;
```

注意：

* 如果是升序，可以不指定排序方式 `ASC` ;
* 如果是多字段排序，当第一个字段值相同时，才会根据第二个字段进行排序 ;







### 4.6 分页查询

通过选择起始序号和页数，来查询一段指定区间的数据



**例：**

```sql
select 字段列表 from 表名 limit 起始索引, 查询记录数 ;

# 查询第1页员工数据, 每页展示10条记录
select * from emp limit 0,10;
select * from emp limit 10;

# 查询第2页员工数据, 每页展示10条记录 --------> 第二页是从第10条记录开始展示,第一个参数是展示的起始记录序号
select * from emp limit 10,10;
```

注意：

* 起始索引从0开始，起始索引 = （查询页码 - 1）* 每页显示记录数
* 分页查询是数据库的方言，不同的数据库有不同的实现，`MySQL`中是 `LIMIT`
* 如果查询的是第一页数据，起始索引可以省略，直接简写为 `limit 10`







### 4.7 执行顺序

编写顺序：

```mermaid
flowchart LR
    a[select] --> b[from]
    b --> c[where]
    c --> d[group by]
    d --> e[having]
    e --> f[order by]
    f --> g[limit]
```

执行顺序：

```mermaid
flowchart LR
    a[from] --> b[where]
    b --> c[group by]
    c --> d[having]
    d --> e[select]
    e --> f[order by]
    f --> g[limit]
```

先决定查找的逻辑，再去查找，最后决定显示的顺序







## 5 DCL 数据控制

### 5.1 管理用户

1. 查看用户

```sql
select * from mysql.user;
```

通过显示 `mysql` 数据库下的 `user` 表来查看每个用户的权限



2. 创建用户

```sql
create user '用户名'@'主机名' identified by '密码';

#例
create user 'itcast'@'localhost' identified by '123456';	-- 创建用户, 只能够在当前主机访问
create user 'heima'@'%' identified by '123456';				-- 创建用户, 可以在任意主机访问该数据库
```



3. 修改用户密码

```sql
alter user '用户名'@'主机名' identified with mysql_native_password by '新密码' ;

#例
alter user 'heima'@'%' identified with mysql_native_password by '1234';
```



4. 删除用户

```sql
drop user '用户名'@'主机名' ;

#例
drop user 'itcast'@'localhost';
```

注意：

* 在 MySQL 中需要通过 `用户名@主机名` 的方式，来唯一标识一个用户
* 主机名可以使用 `%` 通配，这类SQL开发人员操作的比较少，主要是DBA（ Database Administrator 数据库管理员）使用







### 5.2 管理权限

常用权限：

| 权限                 | 说明               |
| -------------------- | ------------------ |
| all,  all privileges | 所有权限           |
| select               | 查询数据           |
| insert               | 插入数据           |
| update               | 修改数据           |
| delete               | 删除数据           |
| alter                | 修改表             |
| drop                 | 删除数据库/表/视图 |
| create               | 创建数据库/表      |



1. 查询权限

```sql
show grants for '用户名'@'主机名' ;

#例
show grants for 'heima'@'%';
```



2. 授予权限

```sql
grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';

#例  授予用户该数据库所有表的所有操作权限
grant all on itcast.* to 'heima'@'%';
```





3. 撤销权限

```sql
revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';

#例  撤销用户的该数据库的所有权限
revoke all on itcast.* from 'heima'@'%';
```

注意：

* 多个权限之间，使用逗号分隔
* 授权时， 数据库名和表名可以使用 `*` 进行通配，代表所有







## 6 函数

### 6.1 字符串函数

常用字符串函数：

| 函数                     | 功能                                                      |
| ------------------------ | --------------------------------------------------------- |
| concat(s1,s2,...sn)      | 字符串拼接，将s1，s2，...  sn拼接成一个字符串             |
| lower(str)               | 将字符串str全部转为小写                                   |
| upper(str)               | 将字符串str全部转为大写                                   |
| lpad(str,n,pad)          | 左填充，用字符串pad对str的左边进行填充，达到n个字符串长度 |
| rpad(str,n,pad)          | 右填充，用字符串pad对str的右边进行填充，达到n个字符串长度 |
| trim(str)                | 去掉字符串头部和尾部的空格                                |
| substring(str,start,len) | 返回从字符串str从start位置起的len个长度的字符串           |



**例：**

```sql
# 字符串拼接
select concat('Hello' , ' MySQL');		-- Hello MySQL

# 字符串转小写/大写
select lower('Hello');		-- hello
select upper('Hello');		-- HELLO

#左填充/右填充
select lpad('01', 5, '0');		-- 00001
select rpad('01', 5, '-');		-- 01000

# 去除空格
select trim(' Hello MySQL ');		-- Hello MySQL

# 截取子字符串
select substring('Hello MySQL',1,5);	-- Hello
```









### 6.2 数值函数

常见的数值函数如下：

| 函数       | 功能                               |
| ---------- | ---------------------------------- |
| ceil(x)    | 向上取整                           |
| floar(x)   | 向下取整                           |
| mod(x,y)   | 返回x/y的模                        |
| rand()     | 返回0~1内的随机数                  |
| round(x,y) | 求参数x的四舍五入的值，保留y位小数 |



**例：**

```sql
# 向上/下取整
select ceil(1.1);		-- 2
select floor(1.9);		-- 1

# 取模
select mod(7,4);	-- 3

# 获取随机数
select rand();		-- 0.08378636286015284 (0-1之间)

# 去除空格
select trim(' Hello MySQL ');		-- Hello MySQL

# 四舍五入
select round(2.344,2);	-- 2.3

# 生成一个六位随机验证码
select lpad(round(rand()*1000000 , 0), 6, '0');
```









### 6.3日期函数

常见的日期函：

| 函数                                | 功能                                              |
| ----------------------------------- | ------------------------------------------------- |
| curdate()                           | 返回当前日期                                      |
| curtime()                           | 返回当前时间                                      |
| now()                               | 返回当前日期和时间                                |
| year(date)                          | 获取指定date的年份                                |
| month(date)                         | 获取指定date的月份                                |
| day(date)                           | 获取指定date的日期                                |
| date_add(date,  interval expr type) | 返回一个日期/时间值加上一个时间间隔expr后的时间值 |
| datediff(date1,date2)               | 返回起始时间date1  和  结束时间date2之间的天数    |



**例：**

```sql
# 当前日期/时间/日期和时间
select curdate();
select curtime();
select now();

# 当前年月日
select year(now());
select month(now());
select day(now());

# 增加指定的时间间隔
select date_add(now(), 1 interval 70 year);		-- 70年后的日期和时间

# 去除空格
select trim(' Hello MySQL ');		-- Hello MySQL

# 获取两个日期相差的天数
select datediff('2021-10-01', '2021-12-01');	-- -61, 前面的日期减去后面日期的结果, 可以为负数
```









### 6.4 流程函数

流程函数也是很常用的一类函数，可以在 SQL 语句中实现条件筛选，从而提高语句的效率

| 函数                                                         | 功能                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| if(value  , t , f)                                           | 如果 value 为 true，则返回 t，否则返回 f                     |
| ifnull(value1  , value2)                                     | 如果 value1 不为空，返回 value1，否则返回 value2             |
| case when [ val1 ] then [res1] ...  else [ default ] end     | 如果 val1 为 true，返回 res1，...  否则返回 default 默认值   |
| case [ expr ] when [ val1 ] then  [res1]  ... else [ default ] end | 如果 expr 的值等于 val1，返回  res1，...  否则返回 default 默认值 |



**例：**

```sql
# if
select if(false, 'Ok', 'Error');	-- Error

# ifnull
select ifnull('Ok','Default');		-- Ok
select ifnull('','Default');		-- Default
select ifnull(null,'Default');		-- Default
select ifnull(0,1);					-- 0

# case when then else end
select
    name,
    ( case workaddress when '北京' then '一线城市' when '上海' then '一线城市' else '二线城市' end ) as '工作地址'
from emp;	-- 北京, 上海显示一线城市, 其余显示二线城市
```

注意：SQL 中，０不是 false









## 7 约束

约束是作用于表中字段上的规则，用于限制存储在表中的数据，保证数据库中数据的正确、有效性和完整性



### 7.1 约束概念

分类：

| 约束                       | 描述                                                         | 关键字         |
| -------------------------- | ------------------------------------------------------------ | -------------- |
| 非空约束                   | 限制该字段的数据不能为 null                                  | not null       |
| 唯一约束                   | 保证该字段的所有数据都是唯一、不重复的                       | unique         |
| 主键约束                   | 主键是一行数据的唯一标识，要求非空且唯一                     | primary key    |
| 默认约束                   | 保存数据时，如果未指定该字段的值，则采用默认值               | default        |
| 检查约束（8.0.16版本之后） | 保证字段值满足某一个条件                                     | check          |
| 外键约束                   | 用来让两张表的数据之间建立连接，保证数据的一致性和完整性     | foreign key    |
| 递增（常用于主键）         | 不是约束但用法相似，会使该字段的值随着每次插入新记录而自动增加<br/>插入数据时可以忽略该字段，系统默认按照上一个的值递增1，默认起始为1 | auto_increment |

注意：约束是作用于表中字段上的，可以在创建表/修改表的时候添加约束，递增不是约束



**例：**

根据需求，完成表结构的创建，需求如下：

| 字段名 | 字段含义   | 字段类型    | 约束条件                  | 约束关键字                  |
| ------ | ---------- | ----------- | ------------------------- | --------------------------- |
| id     | ID唯一标识 | int         | 主键，并且自动增长        | primary key，auto_increment |
| name   | 姓名       | varchar(10) | 不为空，并且唯一          | not null，unique            |
| age    | 年龄       | int         | 大于0，并且小于等于120    | check                       |
| status | 状态       | char(1)     | 如果没有指定该值，默认为1 | default                     |
| gender | 性别       | char(1)     | 无                        |                             |




```sql
create table user(
    id int auto_increment primary key comment 'ID唯一标识',		-- 递增, 非空, 唯一
    name varchar(10) not null unique comment '姓名' ,			 -- 非空, 唯一
    age int check (age > 0 && age <= 120) comment '年龄' ,	 -- 0 < age <= 120
    status char(1) default '1' comment '状态',				 -- 默认为 1
    gender char(1) comment '性别'
);		-- 创建用户表

insert into user(name,age,status,gender) values ('Tom1',19,'1','男'), ('Tom2',25,'0','男');
insert into user(name,age,status,gender) values ('Tom3',19,'1','男');

insert into user(name,age,status,gender) values (null,19,'1','男');		-- name 不能为空
insert into user(name,age,status,gender) values ('Tom3',19,'1','男');	-- name 不能重复

insert into user(name,age,status,gender) values ('Tom4',80,'1','男');

insert into user(name,age,status,gender) values ('Tom5',-1,'1','男');	-- age 必须大于 0
insert into user(name,age,status,gender) values ('Tom5',121,'1','男');	-- age 必须小于等于 120

insert into user(name,age,gender) values ('Tom5',120,'男');
# 上述插入都没有插入 id, id 自动递增, 分别为 1 2 3 4 5
```







### 7.2 外键约束

外键可以用来让两张表的数据之间建立连接，从而保证数据的一致性和完整性

![](assets\外键约束.png)



注意：目前上述两张表，只是在逻辑上存在这样一层关系；在数据库层面，并未建立外键关联，所以是无法保证数据的一致性和完整性的



![](assets\无外键删除.png)

如果删除部门表中的任意一条数据（以研发部举例）

成功删除研发部之后，部门表不存在 id 为 1 的部门，而在员工表中还有很多的员工，关联的为 id 为 1 的部门

此时就出现了数据的不完整性，而要想解决这个问题就得通过数据库的外键约束



添加外键：

```sql
create table 表名(
    字段名 数据类型,
    ...
    [constraint] [外键名称] foreign key (外键字段名) references 主表 (主表列名)	-- 在创建表时设置外键约束
);

alter table 表名 add constraint 外键名称 foreign key (外键字段名) references 主表 (主表列名) ;	-- 在创建表后设置外键约束
# 外键名称可以用来标识外键约束的名称

#例  为 emp 表的 dept_id 字段添加外键约束, 关联 dept 表的主键 id
alter table emp add constraint fk_emp_dept_id foreign key (dept_id) references dept(id);
```



添加了外键约束之后，我们再到部门表（父表）删除 id 为 1 的记录，此时将会报错，不能删除或更新父表记录，因为存在外键约束

![](assets\外键删除.png)



删除外键：

移除两表之间的外键约束，不修改两个表的数据

```sql
alter table 表名 drop foreign key 外键名称;

#例  删除emp表的外键fk_emp_dept_id
alter table emp drop foreign key fk_emp_dept_id;
```







### 7.3 外键的删除/更新行为

添加了外键之后，再删除父表数据时产生的约束行为，我们就称为删除/更新行为



具体的删除/更新行为有以下几种：

| 行为                     | 说明                                                         |
| ------------------------ | ------------------------------------------------------------ |
| no action /<br/>restrict | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新（默认行为） |
| cascade                  | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有，则也删除/更新外键在子表中的记录 |
| set  null                | 当在父表中删除对应记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为null（这就要求该外键允许取null） |
| set  default             | 父表有变更时，子表将外键列设置成一个默认的值  (Innodb不支持) |



```sql
# 语法
alter table 表名 add constraint 外键名称 foreign key (外键字段)
references 主表名 (主表字段名) on update 行为 on delete 行为;

# no action
alter table emp add constraint fk_emp_dept_id foreign key (dept_id)
references dept(id) on update no action on delete no action;
-- 该行为是默认行为, 可以省略行为的标准部分

# cascade
alter table emp add constraint fk_emp_dept_id foreign key (dept_id)
references dept(id) on update cascade on delete cascade;

# set null
alter table emp add constraint fk_emp_dept_id foreign key (dept_id)
references dept(id) on update set null on delete set null;
```









## 8 多表查询

### 8.1 多表关系

项目开发中，在进行数据库表结构设计时，会根据业务需求及业务模块之间的关系，分析并设计表结构

由于业务之间相互关联，所以各个表结构之间也存在着各种联系，基本上分为三种：

* 一对多（多对一）
* 多对多
* 一对一



1. 一对多

* 案例：部门与员工的关系
* 关系：一个部门对应多个员工，一个员工对应一个部门
* 实现：在多的一方建立外键，指向一的一方的主键

<img src="assets\一对多.png"  />



2. 多对多

* 案例：学生 与 课程的关系
* 关系：一个学生可以选修多门课程，一门课程也可以供多个学生选择
* 实现：建立第三张中间表，中间表至少包含两个外键，分别关联两方主键

![](assets\多对多.png)



3. 一对一

* 案例：用户 与 用户详情的关系
* 关系：一对一关系，多用于单表拆分，将一张表的基础字段放在一张表中，其他详情字段放在另一张表中，以提升操作效率
* 实现：在任意一方加入外键，关联另外一方的主键，并且设置外键为唯一的（unique）

![](assets\一对一.png)









### 8.2 数据准备

为下面的演示提前准备数据

```sql
-- 创建dept表，并插入数据
create table dept
(
    id   int            comment 'ID'        auto_increment primary key,
    name varchar(50)    comment '部门名称'    not null
) comment '部门表';

insert into dept (id, name)
values
    (1, '研发部'),
    (2, '市场部'),
    (3, '财务部'),
    (4, '销售部'),
    (5, '总经办'),
    (6, '人事部');

-- 创建emp表，并插入数据
create table emp
(
    id        int           comment 'ID'        auto_increment primary key,
    name      varchar(50)   comment '姓名'       not null,
    age       int           comment '年龄',
    job       varchar(20)   comment '职位',
    salary    int           comment '薪资',
    entrydate date          comment '入职时间',
    managerid int           comment '直属领导ID',
    dept_id   int           comment '部门ID'
) comment '员工表';

-- 添加外键
alter table emp add constraint fk_emp_dept_id foreign key (dept_id) references dept (id);

insert into emp (id, name, age, job, salary, entrydate, managerid, dept_id)
values
    (1, '金庸', 66, '总裁', 20000, '2000-01-01', null, 5),
    (2, '张无忌', 20, '项目经理', 12500, '2005-12-05', 1, 1),
    (3, '杨逍', 33, '开发', 8400, '2000-11-03', 2, 1),
    (4, '韦一笑', 48, '开发', 11000, '2002-02-05', 2, 1),
    (5, '常遇春', 43, '开发', 10500, '2004-09-07', 3, 1),
    (6, '小昭', 19, '程序员鼓励师', 6600, '2004-10-12', 2, 1),
    (7, '灭绝', 60, '财务总监', 8500, '2002-09-12', 1, 3),
    (8, '周芷若', 19, '会计', 48000, '2006-06-02', 7, 3),
    (9, '丁敏君', 23, '出纳', 5250, '2009-05-13', 7, 3),
    (10, '赵敏', 20, '市场部总监', 12500, '2004-10-12', 1, 2),
    (11, '鹿杖客', 56, '职员', 3750, '2006-10-03', 10, 2),
    (12, '鹤笔翁', 19, '职员', 3750, '2007-05-09', 10, 2),
    (13, '方东白', 19, '职员', 5500, '2009-02-12', 10, 2),
    (14, '张三丰', 88, '销售总监', 14000, '2004-10-12', 1, 4),
    (15, '俞莲舟', 38, '销售', 4600, '2004-10-12', 14, 4),
    (16, '宋远桥', 40, '销售', 4600, '2004-10-12', 14, 4),
    (17, '陈友谅', 42, null, 2000, '2011-10-12', 1, null);
```

​    







### 8.3 多表查询概述

多表查询就是指从多张表中查询数据

如果要执行多表查询，就只需要使用逗号分隔多张表即可，例如 `select * from emp , dept ;` 查询员工表和部门表的数据



此时，我们看到查询结果中包含了大量的结果集（共有两表数量积条数据），而这其实就是员工表 `emp` 所有的记录与部门表 `dept` 所有记录的所有组合情况

这种现象称之为笛卡尔积，其是指在数学中，==两个集合 A 集合和 B 集合的所有组合情况==

![](assets\笛卡尔积.png)

而在多表查询中，我们是需要消除无效的笛卡尔积的，只保留两张表关联部分的数据

![](assets\消除笛卡尔积.png)



我们可以给多表查询加上连接查询的条件即可

```sql
select * from emp , dept where emp.dept_id = dept.id;
```

![](assets\多表查询演示.png)









### 8.4 连接查询

![](assets\内连接.png)

多表查询分类：

* 连接查询

    * 内连接：相当于查询A、B交集部分数据

    * 外连接：

        * 左外连接：查询左表所有数据，以及两张表交集部分数据

        * 右外连接：查询右表所有数据，以及两张表交集部分数据

    * 自连接：当前表与自身的连接查询，自连接必须使用表别名

* 子查询



1. 内连接

```sql
# 隐式内连接
select 字段列表 from 表1 , 表2 where 条件 ... ;

# 显式内连接
select 字段列表 from 表1 [ inner ] join 表2 on 连接条件 ... ;


# 查询每一个员工的姓名, 及关联的部门的名称 (隐式内连接实现)
select emp.name, dept.name from emp, dept where emp.dept_id = dept.id ;
select e.name,d.name from emp e , dept d where e.dept_id = d.id;	-- 为每一张表起别名, 简化SQL编写

# 查询每一个员工的姓名, 及关联的部门的名称 (显式内连接实现)
select emp.name, dept.name from emp join dept on emp.dept_id = dept.id;
select emp.name, dept.name from emp inner join dept on emp.dept_id = dept.id;
select e.name, d.name from emp e join dept d on e.dept_id = d.id;	-- 为每一张表起别名, 简化SQL编写
```

注意：

* 为表起别名：
    ` 表名1 as 别名1, 表名2 as 别名2 ;` 或 ` 表名1 别名1, 表名2 别名2 ;`
* 一旦为表起了别名，就不能再使用表名来指定对应的字段了，此时只能够使用别名来指定字段



2. 外连接

```sql
# 左外连接
select 字段列表 from 表1 left [ outer ] join 1 表2 on 条件 ... ;

# 右外连接
select 字段列表 from 表1 right [ outer ] join 表2 on 条件 ... ;


# 查询员工的所有数据, 和对应的部门信息
select emp.*, dept.name from emp left outer join dept on emp.dept_id = dept.id;
select emp.*, dept.name from emp left join dept on emp.dept_id = dept.id;
select e.*, d.name from emp e left join dept d on e.dept_id = d.id;			-- 为每一张表起别名, 简化SQL编写

# 查询部门的所有数据, 和对应的员工信息
select dept.*, emp.* from emp right outer join dept on emp.dept_id = dept.id;
select d.*, e.* from emp e right outer join dept d on e.dept_id = d.id;		-- 为每一张表起别名, 简化SQL编写
select dept.*, emp.* from dept left outer join emp on emp.dept_id = dept.id;
select d.*, e.* from dept d left outer join emp e on e.dept_id = d.id;		-- 为每一张表起别名, 简化SQL编写
```

注意：

* 左外连接和右外连接是可以相互替换的，只需要调整在连接查询时SQL中，表结构的先后顺序就可以了
* 而我们在日常开发使用时，更偏向于左外连接



3. 自连接

```sql
# 自连接查询
select 字段列表 from 表A 别名A join 表A 别名B on 条件 ... ;

# 联合查询
select 字段列表 from 表A ...
union [ all ]	-- 不添加 all 会去重
select 字段列表 from 表B ....;


# 查询员工及其所属领导的名字
select a.name , b.name from emp a , emp b where a.managerid = b.id;		-- 如果没用领导会跳过
select a.name '员工', b.name '领导' from emp a left join emp b on a.managerid = b.id;	-- 没有领导显示 null

# 将薪资低于 5000 的员工, 和 年龄大于 50 岁的员工全部查询出来
select * from emp where salary < 5000
union
select * from emp where age > 50;
```

注意：

* 在自连接查询中，必须要为表起别名，要不然我们不清楚所指定的条件、返回的字段，到底是哪一张表的字段
* 如果多条查询语句查询出来的结果，字段数量不一致，在进行 `union/union all` 联合查询时，将会报错

![](assets\字段数不同.png)









### 8.5 子连接

SQL 语句中嵌套 `select` 语句，称为嵌套查询，又称子查询

```sql
select * from t1 where column1 = ( select column1 from t2 );
```

子查询外部的语句可以是 `insert/updata/delete/select` 的任何一个



1. 标量子查询（子查询结果为单个值）

子查询返回的结果是单个值（数字、字符串、日期等），最简单的形式，这种子查询称为标量子查询
常用的操作符：`=`，`<>`，`>`，`>=`，`<`，`<=`



**例：**

```sql
# 查询 "销售部" 的所有员工信息
select id from dept where name = '销售部';		-- 查询 "销售部" 部门ID
select * from emp where dept_id = (select id from dept where name = '销售部');
											  -- 根据 "销售部" 部门ID, 查询员工信息

# 查询在 "方东白" 入职之后的员工信息
select entrydate from emp where name = '方东白';	-- 查询 方东白 的入职日期
select * from emp where entrydate > (select entrydate from emp where name = '方东白');
												  -- 查询指定入职日期之后入职的员工信息
```





2. 列／行子查询（子查询结果为一列／行）

子查询返回的结果是一列／行（可以是多行／列），这种子查询称为列／行子查询



常用的操作符（用法和函数相似）：

| 操作符 | 描述                                        |
| ------ | ------------------------------------------- |
| in     | 在指定的集合范围之内，多选一                |
| not in | 不在指定的集合范围之内                      |
| any    | 子查询返回列表中，有任意一个满足即可        |
| some   | 与 any 等同，使用 some 的地方都可以使用 any |
| all    | 子查询返回列表的所有值都必须满足            |



**例：**

```sql
# 查询 "销售部" 和 "市场部" 的所有员工信息
select id from dept where name = '销售部' or name = '市场部';		-- 查询 "销售部" 和 "市场部" 的部门ID
select * from emp where dept_id in (select id from dept where name = '销售部' or name = '市场部');
																-- 根据部门ID, 查询员工信息

# 查询比 财务部 所有人工资都高的员工信息
select id from dept where name = '财务部';
select salary from emp where dept_id = (select id from dept where name = '财务部');	-- 查询所有财务部人员工资
select * from emp where salary > all ( select salary from emp where dept_id = (select id from dept where name = '财务部');
											  										  -- 比 财务部 所有人工资都高的员工信息

# 查询比研发部其中任意一人工资高的员工信息
select salary from emp where dept_id = (select id from dept where name = '研发部');	-- 查询研发部所有人工资
select * from emp where salary > any ( select salary from emp where dept_id = (select id from dept where name = '研发部') );
																					  -- 比研发部其中任意一人工资高的员工信息

# 查询 "张无忌" 的薪资及直属领导
select salary, managerid from emp where name = '张无忌';	-- 查询 "张无忌" 的薪资及直属领导
select * from emp where (salary,managerid) = (select salary, managerid from emp where name = '张无忌');
                                      					  -- 查询与 "张无忌" 的薪资及直属领导相同的员工信息 ;
```



3. 表子查询（子查询结果为多行多列）

子查询返回的结果是多行多列，这种子查询称为表子查询

常用的操作符：`in`

**例：**

```sql
# 查询与 "鹿杖客" , "宋远桥" 的职位和薪资相同的员工信息
select job, salary from emp where name = '鹿杖客' or name = '宋远桥';		-- 查询 "鹿杖客", "宋远桥" 的职位和薪资
select * from emp where (job,salary) in ( select job, salary from emp where name = '鹿杖客' or name = '宋远桥' );
																		-- 查询与 "鹿杖客", "宋远桥" 的职位和薪资相同的员工信息
# 查询入职日期是 "2006-01-01" 之后的员工信息 , 及其部门信息
select * from emp where entrydate > '2006-01-01';	-- 入职日期是 "2006-01-01" 之后的员工信息
select e.*, d.* from (select * from emp where entrydate > '2006-01-01') e left join dept d on e.dept_id = d.id ;
													-- 查询这部分员工, 对应的部门信息;
```









## 9 事务

事务是一组操作的集合，它是一个不可分割的工作单位

事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求，即这些操作要么同时成功，要么同时失败（类似操作系统中的原子操作）



**例：**

张三给李四转账1000块钱，张三银行账户的钱减少1000，而李四银行账户的钱要增加1000

这一组操作就必须在一个事务的范围内，要么都成功，要么都失败

<img src="assets\事务转账.png" style="zoom:75%;" />         ![](assets\事务步骤.png) 



* 正常情况: 转账这个操作，需要分为以下这么三步来完成，三步完成之后，张三减少1000，而李四增加1000，转账成功

* 异常情况: 转账这个操作，也是分为以下这么三步来完成

    在执行第三步是报错了，这样就导致张三减少1000块钱，而李四的金额没变，这样就造成了数据的不一致, 就出现问题了



为了解决上述的问题，就需要通过数据的事务来完成，我们只需要在业务逻辑执行之前开启事务，执行完毕后提交事务

如果执行过程中报错，则回滚事务，把数据恢复到事务开始之前的状态







### 9.1 控制事务

常用的事务控制方式有两种：

1. 关闭自动提交（若报错可以选择回滚，没报错正常提交即可）

```sql
select @@autocommit ;	-- 查看事务提交方式
set @@autocommit = 0 ;	-- 设置事务提交方式
	-- 只影响当前会话, 当会话结束后, 新的会话仍然会使用默认的 autocommit=1

commit;		-- 提交
rollback;	-- 回滚
```



2. 创建事务

```sql
start transaction ;
begin ;		-- 两种都是创建事务的方式

commit;		-- 提交
rollback;	-- 回滚
```

注意：

* 提交事务后数据才会更新
* 回滚事务可以使数据恢复事务开始前，可保证数据的完整性和正确性



**例：**

```sql
-- 开启事务
start transaction;

-- 1. 查询张三余额
select * from account where name = '张三';
    
-- 2. 张三的余额减少1000
update account set money = money - 1000 where name = '张三';

-- 模拟一个异常
异常报错

-- 3. 李四的余额增加1000
update account set money = money + 1000 where name = '李四';

-- 如果正常执行完毕, 则提交事务
commit;

-- 如果执行过程中报错, 则回滚事务
rollback;
```









### 9.2 事务四大特性

1. 原子性（Atomicity）：事务是不可分割的最小操作单元，要么全部成功，要么全部失败
2. 一致性（Consistency）：事务完成时，必须使所有的数据都保持一致状态
3. 隔离性（Isolation）：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行
4. 持久性（Durability）：事务一旦提交或回滚，它对数据库中的数据的改变就是永久的



上述就是事务的四大特性，简称 ==ACID==







### 9.3 并发事务问题

并发处理事务时会遇到的问题，面试会考



1. 赃读：一个事务读到另外一个事务还没有提交的数据

例：比如**事务B**读取到了**事务A**未提交的数据

![](assets\脏读.png)







2. 不可重复读：一个事务先后读取同一条记录，但两次读取的数据不同，称之为不可重复读

例：**事务A**两次读取同一条记录，但是读取到的数据却是不一样的，可能是**事务B**在**事务A**读第二次前修改了该数据

![](assets\不可重复读.png)





3. 幻读：一个事务按照条件查询数据时，没有对应的数据行，但是在插入数据时，又发现这行数据已经存在，好像出现了 "幻影"

例：**事务A**第一次查询数据时没有找到该数据，之后**事务B**添加了该数据，当**事务A**想添加该数据时该数据已经存在

​		由于解决了不可重复读，事务A只能查到和第一次相同的结果即为该数据不存在

![](assets\幻读.png)









### 9.4 事务隔离级别

为了解决并发事务所引发的问题，在数据库中引入了事务隔离级别

主要有以下几种：

| 隔离级别                     | 脏读 | 不可重复读 | 幻读 |
| ---------------------------- | ---- | ---------- | ---- |
| Read uncommitted             | √    | √          | √    |
| Read committed               | ×    | √          | √    |
| Repeatable Read（MySQL默认） | ×    | ×          | √    |
| Serializable                 | ×    | ×          | ×    |



相关指令：

```sql
# 查看事务隔离级别
select @@transaction_isolation;		-- 默认REPEATABLE-READ
# 设置事务隔离级别
set session transaction isolation level 隔离级别;
```

注意：事务隔离级别越高，数据越安全，但是性能越低

