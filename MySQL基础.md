# 一.MySQL介绍
javaEE：企业级Java开发 Web
前端（页面） 后端（连接数据库JDBC，连接前端：控制视图跳转，给前端传数据） 数据库（存数据 txt.excel，word）

## 1.1什么是数据库

数据库（DB，DataBase）

概念：数据仓库，软件，安装在操作系统上

作用：存数据，管理数据

## 



## 1.2数据库分类

关系型：

+ MySQL，Oracle，SqL Server，DB2，SQLlite
+ 通过表与表之间，行和列之间的关系进行数据的存储

非关系型 ：

+ Redis，MongDB
+ 非关系型数据库，对象存储，通过对象自身的属性来决定

## 1.3 DBMS

数据库管理软件，科学管理我们的数据。维护和获取数据

+ MySQL

## 1.4连接数据库
```sql
net start mysql  --启动MySQL服务

mysql -u root -p123456
mysql -u root -p 			--连接数据库

show databases; --查看所有的数据库

use school --切换数据库
Database changed	--代表切换成功

show tables;  --查看切换数据库所有的表

desc student; --查看表中信息 desc+表名

create database westos; --创建一个数据库

exit；--退出连接

--单行注释

/*
	多行注释
*/





```

数据库 XXX语言

DDL   定义

DML	操作

DQL	查询

DCL 	控制

# 二.操作数据库

1.操作数据库----》2.操作数据库中的表----》3.操作表中数据

## 2.1操作数据库（了解）

MySQL 关键字不区分大小写

+ 创建数据库
+ ```sql

  CREATE [IF NOT EXISTS] DATABASE +名称 --如果不存在才创建，【】里的是表示可选的
  ```

+ 删除数据库
+ ```sql

  DROP DATABASE [IF EXISTS] westos  -
  ```

+ 使用数据库
 ```sql
  use +名称 
 use `school`
 ```
+ 查看数据库
```sql
show databases; --查看所有的数据库
```
  如果表名或者字段是特殊字段 加``

## 2.2数据库的列类型
数值 INT(4)
+ tinyint		1个字节 非常小的数据
+ smallint		2个字节 较小的数据
+ mediumint     3个字节 中等的
+ **int			4个字节 标准的整数**
+ bigint		8个字节 较大的数据
+ float  		4      浮点数
+ double    	8		浮点数
+ decimal 		字符串的浮点数  金融计算
字符串	
+ char 			大小固定字符串 0~255
+ **varchar       可变字符串 0~65535**	 对应String
+ tinytext 		微型文本 2^8 -1
+ **text      	文本串   2^16 -1  保存大文本**

时间日期
+ data 			YYYY-MM-DD
+ time 			HH:mm:ss
+ **datatime 	YYYY-MM-DD HH:mm:ss  最长用的时间格式**
+ **timetamp  	时间戳 1970.1.1到现在的毫秒数**
+ year		 	年份表示

null

+ 没有值 空置
+ 不要使用null运算

## 2.3数据库的字段属性（重点）

unsigned:

+ 无符号整数
+ 作用就是 该列不能为负数

zerofill:

+ o填充的
+ 不足的位数用0来填充

自增 AUTO_INCREMENT

+ 自动在上一条记录上加1（默认）
+ 可以用来设置主键 必须是整数类型
+ 可以自定义设计主键的起始值和步长

非空 NOT NULL

+ 设置为not null ，如果不给它赋值 就会报错
+ 如果不填值 默认为null（与这个字段无关）

默认

+ 设置默认值
+ eg:sex 默认为 男

## 2.4操作数据库中的表

+ 表的名称和字段尽量使用``括起来

+ 字符串使用单引号括起来!

+ 所有的语句后而加，( 英文的)，最后一个不用加
+ PRIMARYKEY主键，--般一个表只有一个唯一-的主键!
+ ENGINE=INNODB  是表类型

```sql
CREATE TABLE IF NOT EXISTS `student`(
 `id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
 `name` VARCHAR(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
 `pwd` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码',
 `sex` VARCHAR(2) NOT NULL DEFAULT '男' COMMENT '性别',
 `birthday` DATETIME DEFAULT NULL COMMENT '出生日期',
 `address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址', 
  PRIMARY KEY (`id`) 
)ENGINE=INNODB DEFAULT CHARSET=utf8; 
```

SHOW CREATE TABLE student --查看创建表的语句

SHOW CREATE DATABASE school--查看创建数据库的语句

DESC student --查看表的结构

### 1）关于数据库引擎

默认使用 INNODB

早些年使用 MyISAM

|            | MyISAM                 | INNODB                 |
| ---------- | ---------------------- | ---------------------- |
| 事务支持   | 不支持（最新貌似支持） | 支持                   |
| 数据行锁定 | 不支持                 | 支持                   |
| 外键       | 不支持                 | 支持                   |
| 全文索引   | 支持                   | 不支持（最新貌似支持） |
| 表空间大小 | 较小                   | 较大                   |

MyISAM 节约空间 速度快

INNODB 安全行高 事物处理 多表多用户操作

+ 物理存在空间

所有数据文件都在data目录

数据库引擎在文件上的区别

+   INNODB MySQL-8.0没有.frm表结构文件，并入.ibd文件中了

.frm  表结构定义文件

+    MyISAM 
    .MYD 数据文件
    .mYI 索引文件
+ 设置数据库字符集编码

默认的字符集编码不支持中文

可以在创表时设置

也可以在my.ini 里配置

## 2.5修改删除表

### 修改

```sql
alter table student rename stu --修改表名

alter table 表名 add 字段名  列属性 --增加表字段

alter table 表名 modify 字段名 列属性  --修改表的字段的类型和约束 ，不能重命名
ALTER TABLE stu MODIFY age1 VARCHAR(11)

alter table 表名 change 字段名 新字段名 列属性--字段重命名
ALTER TABLE stu CHANGE age1 age INT(1)

alter table 表名 drop age --删除表的字段
ALTER TABLE stu DROP age
```

### 删除

```sql
drop table [if exists] 表名 
```

+ 创建删除 操作 尽量加上判断

# 三.mysql数据管理

## 3.1外键（了解即可）

数据库层面 物理外键 不推荐使用

应该在应用层实现 否则维护麻烦

如果公共关键字在一个关系中是主关键字，那么这个公共关键字被称为另一个关系的外键。由此可见，外键表示了两个关系之间的相关联系。以另一个关系的外键作主关键字的表被称为主表，具有此外键的表被称为主表的从表。

在实际操作中，将一个表的值放入第二个表来表示关联，所使用的值是第一个表的主键值(在必要时可包括复合主键值)。此时，第二个表中保存这些值的属性称为外键(foreign key)。

外键作用

保持数据一致性，完整性，主要目的是控制存储在外键表中的数据,约束。使两张表形成关联，外键只能引用外表中的列的值或使用空值。

## 3.2 DML语言（要记住）

**管理数据库数据方法：**

- 通过SQLyog等管理工具管理数据库数据
- 通过**DML语句**管理数据库数据
- **DML语言**  ：数据操作语言
  - 用于操作数据库对象中所包含的数据
  - 包括 :
    - INSERT (添加数据语句)
    - UPDATE (更新数据语句)
    - DELETE (删除数据语句)

## 3.3添加

```sql
INSERT INTO 表名[(字段1,字段2,字段3,...)] VALUES('值1'),('值2'),('值3') --多行插入数据
insert into `student1` (`name`)values('李雷'),('张三')

INSERT INTO 表名[(字段1,字段2,字段3,...)] VALUES('值1','值2','值3') --多列插入数据
INSERT INTO `student1` (`name`,`address`)VALUES('李雷','山西')

insert into `student1` (`name`)values('李雷')
insert into `student1` values('李雷')
--如果不写表的字段 会自动从第一个一一匹配 

INSERT INTO `student1` (`name`,`address`)VALUES('李一','山西'),('李二','山西'),('李三','山西')


```

## 3.4修改

```sql
UPDATE`student1` SET `name`='王五' WHERE id = 1
-- id为1的name设置为王五

UPDATE`student1` SET `name`='王五' 
-- 不指定条件会改掉所有的name

UPDATE`student1` SET `name`='王六' ,`sex` ='女' WHERE id = 1
```

条件：where条件子句

+ <
+  >

+ <> 就是!=  

+ between 2 and 4    2到4之间的 闭区间

eg:WHERE id between 2 and 4 

+ and      用来增加多个条件
+ or

## 3.5删除

DELETE命令

```sql
DELETE FROM `student1` 
-- 删除全部（避免这样）
DELETE FROM `student1` WHERE id = 1
-- 删除表中指定数据
```



TRUNCATE命令 truncate

```sql
TRUNCATE `student1`
-- 删除表的全部数据
```

**区别**

- 相同 : 都能删除数据 , 不删除表结构 , 但TRUNCATE速度更快

- 不同 :

  - 使用TRUNCATE TABLE 重新设置AUTO_INCREMENT计数器

    计数器会归零  DELETE再加数据会从以前的计数开始

  - 使用TRUNCATE TABLE不会对事务有影响 

  了解：

  ```sql
  同样使用DELETE清空不同引擎的数据库表数据.重启数据库服务后
  
  -- InnoDB : 自增列从初始值重新开始 (因为是存储在内存中,断电即失)
  
  -- MyISAM : 自增列依然从上一个自增数据基础上开始 (存在文件中,不会丢失)
  ```
# 四.DQL（查询语言）

```sql
SELECT [ALL | DISTINCT]
{* | table.* | [table.field1[as alias1][,table.field2[as alias2]][,...]]}
FROM table_name [as table_alias]
    [left | right | inner join table_name2]  -- 联合查询
    [WHERE ...]  -- 指定结果需满足的条件
    [GROUP BY ...]  -- 指定结果按照哪几个字段来分组 
    [HAVING]  -- 过滤分组的记录必须满足的次要条件
    [ORDER BY ...]  -- 指定查询记录按一个或多个条件排序
    [LIMIT {[offset,]row_count | row_countOFFSET offset}];
    --  指定查询的记录从哪条至哪条

原文链接：https://blog.csdn.net/qq_33369905/article/details/105872921
```

**<u>需要遵循 上面的顺序格式</u>**！！



## 4.1**查询指定字段**

```sql
--查询指定字段

SELECT `studentno`,`studentname` FROM student

--别名，给结果起一个名字AS 也可以给字段起别名 也可以给表起别名
方便阅读

SELECT `studentno` AS `学号`,`studentname` AS `学生姓名` FROM student AS `s`

--函数 CONCAT(a,b) 给查询结构连接一个固定字符串  
SELECT CONCAT('姓名: ',studentname) AS `新名字` FROM student
```



## 4.2**去重**

```sql

--查询一下有哪些同学参加了考试成绩
SELECT  `studentno` FROM `result` 
--去除重复数据 
SELECT DISTINCT `studentno` FROM `result`
```

## 4.3**数据库中的表达式 文本值 列 null 函数 表达式**

```sql
--查询系统版本
SELECT VERSION()

 --用来计算
SELECT 100*5-100 AS 计算

--查询自增的步长
SELECT @@auto_increment_increment

--学员考试成绩 
SELECT  `studentno`,`studentresult`+1 AS `提分后` FROM `result`
```

select 表达式 from 表

## 4.4**where 条件子句**

**作用：检索数据中 符合条件 的检索数据**

**逻辑运算符**  

and &&   逻辑与

or ||	 逻辑或

not !	  逻辑取反

```sql
--查询考试成绩在95到100分之间
SELECT `stUdentno` ,`studentresult` FROM `result`
 WHERE `studentresult`>=95 &&`studentresult`<=100

--模糊查询(区间)

SELECT `studentno`,`studentresult` FROM `result`
WHERE `studentresult` BETWEEN 80 AND 100


SELECT `studentno`,`studentresult` FROM `result`
WHERE `studentno`!=1000

SELECT `studentno`,`studentresult` FROM `result`
WHERE NOT `studentno`=1000
```
**模糊查询：比较运算符**



运算符	  		语法			描述
is null	       	a is null		如果为null 结果为真
is not null		a is not null		如果操作符为not null 结果为真
between			a between b and c	若a在b到c之间则结果为真
like					a like b			SQL匹配 如果a匹配b 则结果为真
in				a in(a1,a2,a3…)		假如a在a1 或者a2…其中的一个则返回为真

```sql
--like结合 %(代表0到任意个字符) _(一个字符)   在like里使用
SELECT `studentname`,`studentno` FROM `student`
WHERE `studentname` LIKE '张%'

--查询名字后面只有一个字的
SELECT `studentno`,`studentname` FROM `student`
WHERE `studentname` LIKE '%张%'
```

```sql
---IN (具体的一个或者多个值)====
SELECT `studentno`,`studentname` FROM `student`
WHERE `studentno` IN(1001,1002,1003)

SELECT `studentno`,`studentname` FROM `student`
WHERE `address` IN('北京朝阳')
```

+ like 是里面有 张 的就查出来
+ in   是必须是1001，1002，1003 其中之一

## 4.5联表查询

**join对比**

left join ：展示左表所有数据，和右边符合on条件的数据。  从左表中返回值 即使右表中没有匹配

right join： “右连接”，表1右连接表2，以右为主，表示以表2为主，关联查询表1的数据，查出表2所有数据以及表1和表2有交集的数据

inner join :展示左右所有 。  如果表中至少有一个匹配，就返回行  ，

两个是表示一个的，内连接，表示以两个表的交集为主，查出来是两个表有交集的部分，其余没有关联就不额外显示出来，这个用的情况也是挺多的

-- join(连接表名) on(判断条件) 连接查询  (联表查询一般用on)

where 是查出来 再筛选 不太好

-- where 等值查询

```sql
/*
分析需求 分析查询的字段来自哪些表
确定使用哪种链接查询
确定交叉点(这两个表中哪个数据是相同的)
判断的条件 :学生表中的 studentno=成绩表 studentno
*/
SELECT s.`studentno`,`studentname`,`subjectno`,`studentresult`
 FROM `student` AS s
 INNER JOIN `result` AS r
 WHERE s.`studentno`=r.`studentno`
-- on s.`studentno`=r.`studentno`    


-- Right join




SELECT s.`studentno`,`studentname`,`subjectno`,`studentresult`
FROM `student` AS s
RIGHT JOIN `result` AS r
ON s.`studentno` = r.`studentno` 
WHERE s.`studentno` IS NOT NULL



-- left join
SELECT s.`studentno`,`studentname`,`subjectno`,`studentresult`
FROM `student` AS s
LEFT JOIN `result` AS r
ON s.`studentno` = r.`studentno` 


--多表查询   把一部分看作一个表 再连接其他表
SELECT s.`studentno`,s.`studentname`,su.`subjectname`,r.`studentresult`
FROM `student` AS s
RIGHT JOIN `result` AS r
ON r.`studentno`=s.`studentno`
INNER JOIN `subject` AS su
ON r.`subjectno`=su.`subjectno`
WHERE s.`studentno` IS NOT NULL
————————————————
```
## 4.6自连接

把一张表 看出两张表

![image-20210816195720753](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20210816195720753.png)1代表没有父类



![image-20210816195802337](C:\Users\DONG\AppData\Roaming\Typora\typora-user-images\image-20210816195802337.png)



![image-20210816195821291](C:\Users\DONG\AppData\Roaming\Typora\typora-user-images\image-20210816195821291.png)



```sql
-- 查询父子信息
SELECT a.`categoryname` AS `父栏目`,b.`categoryname` AS `子栏目` 
FROM `category` AS a,`category` AS b
WHERE a.`categoryid`=b.`pid`
```



## 4.7分页和排序

```sql
--order by
-- 排序 升序 asc 降序 desc

--降序
SELECT s.`studentno`,s.`studentname`,r.`studentresult`,sub.`subjectname`
FROM `student` AS s
INNER JOIN `result` AS r
ON s.`studentno`=r.`studentno`
INNER JOIN `subject` AS sub
ON s.`gradeid`=sub.`gradeid`
WHERE sub.`subjectname` LIKE '数据库结构%'
ORDER BY r.`studentresult` DESC
————————————————

--limmit
--分页 缓解数据库压力 给人的体验更好（现在图片展示多用瀑布流）
-- 分页，每页只显示五条数据

-- limit(起始位置,最大数量)   --一个条是0   
-- limmit 0,5		--第1行到第5行
-- limmit 5,5		--第6行到第10行
-- limmit 10,5		--第11行到第15行

-- 网页浏览时起始位置 (n-1) *pagesize  
-- limmit (当前页-1) *页面大小  pagesize  --网页显示的规律



SELECT s.`studentno`,s.`studentname`,r.`studentresult`,sub.`subjectname`
FROM `student` AS s
INNER JOIN `result` AS r
ON s.`studentno`=r.`studentno`
INNER JOIN `subject` AS sub
ON s.`gradeid`=sub.`gradeid`
WHERE sub.`subjectname` LIKE '数据库结构%'
ORDER BY r.`studentresult` ASC
LIMIT 0,5  
```

语法：limt(查询起始下标，页面大小)

## 4.8 子查询

本质：在where嵌套一个子查询语句
```sql
-- 方法二:使用子查询(执行顺序:由里及外)
SELECT studentno,subjectno,StudentResult
FROM result
WHERE subjectno=(
    SELECT subjectno FROM `subject`
    WHERE subjectname = '数据库结构-1'
--子查询返回的结果一般都是集合,故而建议使用IN关键字(4.4比较运算符);
```
```sql
什么是子查询?
    在查询语句中的WHERE条件子句中,又嵌套了另一个查询语句
    嵌套查询可由多个子查询组成,求解的方式是由里及外;
    子查询返回的结果一般都是集合,故而建议使用IN关键字;
*/

-- 查询 数据库结构-1 的所有考试结果(学号,科目编号,成绩),并且成绩降序排列
-- 方法一:使用连接查询
SELECT studentno,r.subjectno,StudentResult
FROM result r
INNER JOIN `subject` sub
ON r.`SubjectNo`=sub.`SubjectNo`
WHERE subjectname = '数据库结构-1'
ORDER BY studentresult DESC;

-- 方法二:使用子查询(执行顺序:由里及外)
SELECT studentno,subjectno,StudentResult
FROM result
WHERE subjectno=(
    SELECT subjectno FROM `subject`
    WHERE subjectname = '数据库结构-1'
)
ORDER BY studentresult DESC;

-- 查询课程为 高等数学-2 且分数不小于80分的学生的学号和姓名
-- 方法一:使用连接查询
SELECT s.studentno,studentname
FROM student s
INNER JOIN result r
ON s.`StudentNo` = r.`StudentNo`
INNER JOIN `subject` sub
ON sub.`SubjectNo` = r.`SubjectNo`
WHERE subjectname = '高等数学-2' AND StudentResult>=80

-- 方法二:使用连接查询+子查询
-- 分数不小于80分的学生的学号和姓名
SELECT r.studentno,studentname FROM student s
INNER JOIN result r ON s.`StudentNo`=r.`StudentNo`
WHERE StudentResult>=80

-- 在上面SQL基础上,添加需求:课程为 高等数学-2
SELECT r.studentno,studentname FROM student s
INNER JOIN result r ON s.`StudentNo`=r.`StudentNo`
WHERE StudentResult>=80 AND subjectno=(
    SELECT subjectno FROM `subject`
    WHERE subjectname = '高等数学-2'
)

-- 方法三:使用子查询
-- 分步写简单sql语句,然后将其嵌套起来
-- 由里及外
SELECT studentno,studentname FROM student WHERE studentno IN(
    SELECT studentno FROM result WHERE StudentResult>=80 AND subjectno=(
        SELECT subjectno FROM `subject` WHERE subjectname = '高等数学-2'
    )
)
```

## 4.9 分组和过滤

```sql
-- 查询不同课程的平均分，最高分，最低分
SELECT `subjectname`,AVG(r.`studentresult`) AS 平均分,MAX(r.`studentresult`) AS 最大分,MIN(r.`studentresult`) AS 最低分
FROM `result` AS r
INNER JOIN `subject` AS s
ON r.`subjectno`=s.`subjectno`
GROUP BY s.`subjectno` -- group by 通过什么字段来分组
HAVING 平均分>80  -- having 分组后必须满足的次要条件

```

全文参考原文链接：https://blog.csdn.net/a951273629/article/details/107094285/,
https://blog.csdn.net/qq_33369905/article/details/105872921

# 五.MySQL函数

参考

原文链接：https://blog.csdn.net/a951273629/article/details/107094285/

https://blog.csdn.net/qq_33369905/article/details/105872921

## 5.1常用函数（不常用）

```sql

--  =============常用函数=========================

-- 数学运算

SELECT ABS(-8)  -- 绝对值
SELECT CEILING(9.4) -- 向上取整
SELECT FLOOR(9.4) -- 向下取整
SELECT RAND() -- 返回0-1 随机数
SELECT SIGN(-10) -- 判断一个数的符号 负数返回-1 整数返回1


-- 字符串函数

SELECT CHAR_LENGTH('字符串长度') -- 字符串长度
SELECT CONCAT('李','是一个','张三') -- 拼接字符串
SELECT INSERT('超级人',1,2,'我是一个') -- 替换字符串,insert("原字符",'开始位置','替换长度','用作替换的字符')
我是一个人
SELECT LOWER('WuDiMengNan') -- 小写字母
SELECT UPPER('WuDiMengNan') -- 大写字母
SELECT INSTR('chaojiwudimengnan','wudi') -- 指定字符第一次出现的位置
SELECT REPLACE('ccccc','c','mengnan') -- 替换指定的字符串
SELECT SUBSTR('超级猛男无人能挡',3,6) -- 返回指定的子字符串(源字符串,起始位置,截取长度)
SELECT REVERSE('超级猛男') -- 翻转字符串


 -- 查询姓周的同学,改成邹
 SELECT REPLACE(studentname,'周','邹') AS 新名字
 FROM student WHERE studentname LIKE '周%';

-- 时间和日期函数 （记一下）
SELECT CURRENT_DATE() -- 获取当前日期
SELECT CURDATE() -- 获取当前日期
SELECT NOW() -- 获取当前的时间
SELECT LOCALTIME() -- 本地时间
SELECT SYSDATE() -- 系统时间

SELECT YEAR(NOW()) -- 年
SELECT MONTH(NOW()) -- 月
SELECT DAY(NOW()) -- 日
SELECT HOUR(NOW()) -- 时
SELECT MINUTE(NOW()) -- 分
SELECT SECOND(NOW()) -- 秒

-- 系统
SELECT SYSTEM_USER()
SELECT USER()
SELECT VERSION()

```

## 5.2 聚合函数（常用）



| 函数名称 | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| COUNT()  | 返回满足Select条件的记录总和数，如 select count(*) 【不建议使用 *，效率低】 |
| SUM()    | 返回数字字段或表达式列作统计，返回一列的总和。               |
| AVG()    | 通常为数值字段或表达列作统计，返回一列的平均值               |
| MAX()    | 可以为数值字段，字符字段或表达式列作统计，返回最大的值       |
| MIN()    | 可以为数值字段，字符字段或表达式列作统计，返回最小的值       |
| ...      | ...                                                          |

```sql
-- 统计表中的数 (想查询一个表中有多少个记录就是用count)
SELECT COUNT(`studentname`) FROM `student`; -- count(字段),会忽略所有的null值 --指定列 
SELECT COUNT(*) FROM `student`; -- count(*) 不会忽略null值
SELECT COUNT(1) FROM `result`;-- count(1) 本质 计算行数

SELECT SUM(`studentresult`) AS 总和 FROM `result`;
SELECT AVG(`studentresult`) AS 平均分 FROM `result`
SELECT MAX(`studentresult`) AS 最大分 FROM `result`
SELECT MIN(`studentresult`) AS 最低分 FROM `result`

-- 分组
-- 查询不同课程的平均分，最高分，最低分
SELECT `subjectname`,AVG(r.`studentresult`) AS 平均分,MAX(r.`studentresult`) AS 最大分,MIN(r.`studentresult`) AS 最低分
FROM `result` AS r
INNER JOIN `subject` AS s
ON r.`subjectno`=s.`subjectno`
GROUP BY s.`subjectno` -- group by 通过什么字段来分组
HAVING 平均分>80  -- having 分组后必须满足的次要条件


--where条件里不能有聚合函数 ，要使用HAVING..

where写在group by前面.
 要是放在分组后面的筛选
 要使用HAVING..
 因为having是从前面筛选的字段再筛选，而where是从数据表中的字段直接进行的筛选的


```

# 六.事务

**事务原则 : ACID 原子性 一致性 隔离性 持久性**

原子性(Atomic)

    整个事务中的所有操作，要么全部完成，要么全部不完成，不可能停滞在中间某个环节。事务在执行过程中发生错误，会被回滚（ROLLBACK）到事务开始前的状态，就像这个事务从来没有执行过一样。

一致性(Consist)

    一个事务可以封装状态改变（除非它是一个只读的）。事务必须始终保持系统处于一致的状态，不管在任何给定的时间并发事务有多少。也就是说：如果事务是并发多个，系统也必须如同串行事务一样操作。其主要特征是保护性和不变性(Preserving an Invariant)，以转账案例为例，假设有五个账户，每个账户余额是100元，那么五个账户总额是500元，如果在这个5个账户之间同时发生多个转账，无论并发多少个，比如在A与B账户之间转账5元，在C与D账户之间转账10元，在B与E之间转账15元，五个账户总额也应该还是500元，这就是保护性和不变性。
    ~~守恒原理~~

隔离性(Isolated)

    隔离状态执行事务，使它们好像是系统在给定时间内执行的唯一操作。如果有两个事务，运行在相同的时间内，执行相同的功能，事务的隔离性将确保每一事务在系统中认为只有该事务在使用系统。这种属性有时称为串行化，为了防止事务操作间的混淆，必须串行化或序列化请求，使得在同一时间仅有一个请求用于同一数据。

持久性(Durable)

```
在事务完成以后，该事务对数据库所作的更改便持久的保存在数据库之中，并不会被回滚。 commit之后就不能回滚了
```
基本语法 

```sql
-- 使用set语句来改变自动提交模式
SET autocommit = 0;   /*关闭*/
SET autocommit = 1;   /*开启*/

-- 注意:
---  1.MySQL中默认是自动提交
---  2.使用事务时应先关闭自动提交

-- 开始一个事务,标记事务的起始点
START TRANSACTION  

-- 提交一个事务给数据库
COMMIT

-- 将事务回滚,数据回到本次事务的初始状态
ROLLBACK

-- 还原MySQL数据库的自动提交
SET autocommit =1;

-- 保存点
SAVEPOINT 保存点名称 -- 设置一个事务保存点
ROLLBACK TO SAVEPOINT 保存点名称 -- 回滚到保存点
RELEASE SAVEPOINT 保存点名称 -- 删除保存点
```

测试

```sql
/*
课堂测试题目
A在线买一款价格为500元商品,网上银行转账.
A的银行卡余额为2000,然后给商家B支付500.
商家B一开始的银行卡余额为10000
创建数据库shop和创建表account并插入2条数据
*/

CREATE DATABASE `shop`CHARACTER SET utf8 COLLATE utf8_general_ci;
USE `shop`;

CREATE TABLE `account` (
  `id` INT(11) NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(32) NOT NULL,
  `cash` DECIMAL(9,2) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT INTO account (`name`,`cash`)
VALUES('A',2000.00),('B',10000.00)

-- 转账实现-- 一条一条执行
SET autocommit = 0; -- 关闭自动提交
START TRANSACTION;  -- 开始一个事务,标记事务的起始点
UPDATE account SET cash=cash-500 WHERE `name`='A';
UPDATE account SET cash=cash+500 WHERE `name`='B';
COMMIT; -- 提交事务

# rollback;

SET autocommit = 1; -- 恢复自动提交
```

隔离导致的一些问题

     脏读: 指一个事务读取了另一个事务未提交的数据
    不可重复读: 在一个事务内读取表中的某一行数据，多次读取结果不同。（这个不一定是错误，只是某些场合不对）
    虚读(幻读): 是指在一个事务内读取到了别的事务插入的数据，导致前后读取不一致。
    （一般是行影响，多了一行）






# 七.索引

 定义：索引（index）是帮助MySQL高效获取数据的数据结构（有序）  

索引就是数据结构  提高获取数据的速度

##  7.1 索引分类介绍 

> 主键索引


主键 : 某一个属性组能唯一标识一条记录

特点 :

- 最常见的索引类型
- 确保数据记录的唯一性
- 确定特定数据记录在数据库中的位置

> 唯一索引 (unique key)   

- 避免重复的列出现，唯一索引可以重复，多个列都可以标识唯一索引

> 常规索引(key / index)   

- 默认的 index key 关键字来设置

> 全文索引(fulltext)   

- 在特定的数据库引擎下

```sql
-- 索引
-- 1 在创建表的时候给字段增加索引
-- 2 创建完毕后，增加索引
-- 显示所有的索引信息
SHOW INDEX FROM `student`

-- 增加一个索引
ALTER TABLE `student` ADD FULLTEXT INDEX `studentname`(`studentname`)


-- explain 分析sql执行的状况
EXPLAIN SELECT * FROM `student` -- 非全文索引

EXPLAIN SELECT * FROM `student` WHERE MATCH(`studentname`) AGAINST('张')
```



**索引在大数据量的时候效率高**

## 7.2索引原则

- 索引不是越多越好
- 不要对经常变得数据加索引
- 小数据量的表不需要加索引
- 索引一般加在常用来查询的字段上

**索引的数据结构**

hash 类型的索引

Btree: innoDB 的默认数据结构

# 八.权限管理和备份

## 8.1 权限管理

+ 可视化管理工具（SQLyog ）

+ 命令行

```sql
-- ================权限管理
-- 创建用户
CREATE USER dong  IDENTIFIED BY '123456'

-- 修改密码(修改当前用户密码)
SET PASSWORD =PASSWORD('123456')
新版本：ALTER USER 'dong'@'%' IDENTIFIED BY '123456'
只可以在上面的创建方式基础上使用

-- 修改密码(修改指定 用户密码)
SET PASSWORD FOR dong = PASSWORD('123')

-- 重命名
RENAME USER dong TO menmen

-- 用户授权 all privileges 全部权限 库 表
-- all privileges 除了给别人授权，其他都能干
GRANT ALL PRIVILEGES ON *.* TO dong


-- 查询权限
SHOW GRANTS FOR dong  -- 查看指定用户的权限

SHOW GRANTS FOR root@localhost 

-- 撤销权限 
REVOKE ALL PRIVILEGES ON *.* FROM dong

-- 删除用户
DROP USER dong
```

## 8.2MySQL 备份

数据库备份必要性

- 保证重要数据不丢失
- 数据转移

**mysql数据库备份的方式**

- 直接拷贝物理文件
- 在sqlyog这种可视化工具中手动导出
  - 在想要导出的表或者库中，右键，选择备份或导出

+ 使用命令行导出 mysqldump 命令行使用

```sql
# mysqldump -h主机名 -u用户名 -p密码 数据库 表名 >导出到的 物理磁盘路径/文件名 
mysqldump -hlocalhost -uroot -p123456 school student >D:/1.sql

# mysqldump -h主机名 -u用户名 -p密码 数据库 表名1 表名2 表名3 >物理磁盘路径/文件名 
# 导出多张表
mysqldump -hlocalhost -uroot -p123456 school student result>D:/2.sql

# mysqldump -h主机名 -u用户名 -p密码 数据库>导出到的 物理磁盘路径/文件名 
mysqldump -hlocalhost -uroot -p123456 school  >D:/1.sql

```

```sql
# 导入 
# 先登陆 
mysql -uroot -p123456

#登陆的情况下
#切换到指定数据库
 use school
 
 #导入命令
 source d:/1.sql --将要导入文件的地址
 
 #未登录（不推荐）
 mysql -u用户名 -p密码 库名< 备份文件

```

#  九.规范数据库设计

## 9.1为什么要需要设计

当数据库比较复杂的时候，我们就需要设计了

糟糕的数据库设计

    数据冗余，浪费空间
    数据库插入和删除都会麻烦，[屏蔽使用物理外键]

良好的数据设计

    节省内存空间
    保证数据库的完整性
    方便我们开发系统

软件开发中，关于数据库的设计

    分析需求
    概要设计:设计关系图E-R图

设计数据库的步骤(个人博客)

    收集信息，分析需求
        用户表(用户名登陆注销，用户的个人信息，写博客，创建分类)
        分类表(文章分类,谁创建的)
        文章表(文章的信息)
        有链表(友链信息)
        自定义表(系统信息，某个关键的字，或者一些字段)
    
    标识实体()
## 9.2数据库设计的三大范式

三大范式

**第一范式（1NF）**

原子性 ：保证每一列不可再分

**第二范式（2NF）**

前提:满足第一范式

每张表只描述一件事情

**第三范式（3NF）**

第三范式需要确保数据表中的每一列数据都和主键直接相关，而不能间接相关



规范数据库的设计

**规范性和性能的问题**

关联查询的表不能超过三张表

- 考虑商业化的需求和目标 数据库性能更加重要
- 在规范性能的问题的时候，需要适当的考虑一下 规范性
- 故意给某表增加一些冗余的字段(从多表查询中变为单表查询)
- 故意增加一些计算列(从大数据量降低为小数据量的查询)



# 十.JDBC(重点)

## 10.1数据库驱动

![在这里插入图片描述](https://gitee.com/dong2645981073/picture-summary/raw/master//image/20200702221255656.png)

我们的程序会通过数据库驱动 和数据库打交道

## **10.2JDBC**



Java数据库连接，（Java Database Connectivity，简称JDBC）是Java语言中用来规范客户端程序如何来访问数据库的应用程序接口，提供了诸如查询和更新数据库中数据的方法。JDBC也是Sun Microsystems的商标。我们通常说的JDBC是面向关系型数据库的。

对于开发人员只需要使用jdbc即可

![在这里插入图片描述](https://gitee.com/dong2645981073/picture-summary/raw/master//image/20200702221304720.png)


默认就有的两个包：

java.sql

javax.sql

还需要到导入一个数据库驱动包



第一个JDBC程序

1.

```sql
--  jdbc测试数据 (提前准备的测试数据)

CREATE DATABASE jdbcStudy CHARACTER SET utf8 COLLATE utf8_general_ci;

USE jdbcStudy;

CREATE TABLE `users`(
id INT PRIMARY KEY,
NAME VARCHAR(40),
PASSWORD VARCHAR(40),
email VARCHAR(60),
birthday DATE
);

INSERT INTO `users`(id,NAME,PASSWORD,email,birthday)
VALUES(1,'zhansan','123456','zs@sina.com','1980-12-04'),
(2,'lisi','123456','lisi@sina.com','1981-12-04'),
(3,'wangwu','123456','wangwu@sina.com','1979-12-04')

```

2.导入jar包添加到项目中 mysql-connector-java-5.1.49.jar

```java
package com.dong.lesson01;

import java.sql.*;

public class JdbcFirstDemo {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        //1.加载驱动
        Class.forName("com.mysql.cj.jdbc.Driver"); //固定写法 //反射调用默认进行类的初始化

        //2.连接 用户 密码 和url
        String url ="jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=ture&characterEncoding=utf8&useSSl=true&serverTimezone=GMT%2B8";
        String username = "root";
        String password ="123456";
        //3.连接成功，数据库对象 //connection 代表数据库
        Connection connection = DriverManager.getConnection(url, username, password);

        //4.执行sql的对象// statement 执行sql的
        Statement statement = connection.createStatement();

        //5.执行sql的对象  去执行sql 查看
        String sql="SELECT * FROM users";

        ResultSet resultSet = statement.executeQuery(sql); //返回一个结果集,结果集封装了所有查询结果

        while (resultSet.next()){
            System.out.println("id="+resultSet.getObject("id"));
            System.out.println("name="+resultSet.getObject("NAME"));
            System.out.println("password="+resultSet.getObject("PASSWORD"));
            System.out.println("email="+resultSet.getObject("email"));
            System.out.println("birthday="+resultSet.getObject("birthday"));
            System.out.println("===================");
        }


        //6.释放连接
        resultSet.close();
        statement.close();
        connection.close();
    }
}
```



- 加载驱动

- 连接数据库DriverManager

- 获取执行sql的对象statement

- 获得返回的结果集

- 释放连接

  以后可能会写成一个工具类 直接传进去sql语句就可以了

**JDBC 对象解释**

+ DriverManager

     // 加载驱动
      Class.forName("com.mysql.jdbc.Driver"); //固定写法
     
       Connection connection = DriverManager.getConnection(url, username, password);
       //connection 代表数据库
       数据库写的代码 这个对象都可以
       		connection.commit();
            connection.setAutoCommit();
            connection.rollback();
     
     


+ URL


```Java
//协议://主机名:端口号/数据库名?参数1&参数2
String url="jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=true&characterEncoding=utf8";
//mysql 默认3306

//oralce 默认1521   "jdbc:oralce:think:@localhost:1521:sid

```

+ statement 执行sql的对象；prepareStatement 执行sql的对象

```
Statement statement = connection.createStatement();
PreparedStatement preparedStatement = connection.prepareStatement();（暂时没讲）
```

        statement.executeQuery();  // 查询
        statement.execute(); //执行任何sql //有判断的过程 相对来说效率会低一点
        statement.executeUpdate(); //更新 插入 删除都用这个，返回一个受影响行数
        statement.executeBatch()--多个sql执行
+ ResultSet 查询结果集,封装了所有的查询结果

     //和数据库中的类型一一匹配
        resultSet.getObject(); // 不知道列类型的情况下使用
        // 如果知道列的类型就使用指定的类型
        resultSet.getString(); 
        resultSet.getDate();
        resultSet.getInt();
        resultSet.getDouble();
+ 遍历指针

    resultSet.beforeFirst(); //移动到最前面
    resultSet.afterLast();  //移动到最后面
    resultSet.next()  // 移动到下一个 《-------最常用
    resultSet.previous(); // 移动到上一个
    resultSet.absolute(row); // 移动到指定行


+ 释放资源

```java
 // 释放连接 十分耗费资源
        resultSet.close();
        statement.close();
        connection.close();
```



## 10.3 statement对象

![image-20210824201123584](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20210824201123584.png)

![image-20210824203344290](C:\Users\DONG\AppData\Roaming\Typora\typora-user-images\image-20210824203344290.png)

![image-20210824203427698](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20210824203427698.png)

提取工具类

  ```java
  package com.dong.lesson02.utils;
  
  import java.io.InputStream;
  import java.sql.*;
  import java.util.Properties;
  
  public class JdbcUtils {
      //提升作用域
      private static String driver = null;
      private static String url = null;
      private static String username = null;
      private static String password = null;
  
      static {
          try {
              //这里是通过类获取反射对象，然后获取反射对象的类加载器，调用类加载器的获取资源的方法。一步一步来的
              InputStream in = JdbcUtils.class.getClassLoader().getResourceAsStream("db.properties");
              Properties properties = new Properties();
              properties.load(in);
  
              driver = properties.getProperty("driver");
              url = properties.getProperty("url");
              username = properties.getProperty("username");
              password = properties.getProperty("password");
  
              //加载只需一次 所有放在static下
              Class.forName(driver);
  
          } catch (Exception e) {
              e.printStackTrace();
          }
      }
      //获取连接
      public static  Connection getConnection() throws SQLException {
         // Connection connection = DriverManager.getConnection(url, username, password);
          return DriverManager.getConnection(url, username, password);
  
      }
  
      //释放连接资源
      public static void  release(Connection connection, Statement statement, ResultSet resultSet){
          if (resultSet!=null){
              try {
                  resultSet.close();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
          }
  
          if (statement !=null){
              try {
                  statement.close();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
          }
  
          if (connection!=null){
              try {
                  connection.close();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
          }
  
  
  
  
  
      }
  }
  
  ```

增删改 测试

```java
package com.dong.lesson02.utils;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class TestDelete {
    public static void main(String[] args) {
        Connection conn = null; //try 里面的外面拿不到 提升作用域,try里面的作用域在{}里
        Statement st = null;
        ResultSet re = null;
        try {
            conn= JdbcUtils.getConnection();
            st=conn.createStatement();

            String sql="DELETE FROM `users` WHERE id=4 ";
            int i = st.executeUpdate(sql);
            if (i>0){
                System.out.println("删除成功");
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtils.release(conn,st,re);
        }
    }
}

package com.dong.lesson02.utils;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class TestInsert {
    public static void main(String[] args) {
        Connection conn = null; //try 里面的外面拿不到 提升作用域,try里面的作用域在{}里
        Statement st = null;
        ResultSet re = null;
        try {
            conn= JdbcUtils.getConnection();
            st=conn.createStatement();

            String sql="INSERT INTO `users`(`id`,`NAME`,`PASSWORD`,`email`,`birthday`)" +
                    "VALUES(5,'dong','123465','264598@qq.com','2002-01-01')";
            int i = st.executeUpdate(sql);
            if (i>0){
                System.out.println("插入成功");
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtils.release(conn,st,re);
        }
    }
}
--改没有写 类似

```

 查询  测试

```java
package com.dong.lesson02.utils;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class TestSelect {
    public static void main(String[] args) {
        Connection conn=null;
        Statement st=null;
        ResultSet re=null;
        try {
             conn = JdbcUtils.getConnection();
             st = conn.createStatement();
            String sql = new String();
            sql="select * from `users` where id=1";
            re = st.executeQuery(sql);//结果集 ResultSet对象维护指向其当前数据行的游标。 最初，光标位于第一行之前
            while (re.next()){
                System.out.println(re.getString("NAME"));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtils.release(conn,st,re);
        }

    }
}
```

## 10.4 SQL注入问题

 比如登入业务中 用户名和密码匹配的登入成功 

正常传进去正确的 用户名和密码

不正常 eg: 用户名输入“    'or' 1=1    ” 密码输入 'or' 1=1   所有用户被查出

sql存在漏洞   本质：sql会被拼接

```java
public class SQLinject {
    public static void main(String[] args) throws SQLException {
             login("'' or 1=1","'' or 1=1");
    }
    public static  void login(String username,String password) throws SQLException {
        Connection connection=null;
        Statement statement=null;
        ResultSet rs=null;
        try {
            //SELECT * FROM `users` WHERE `NAME`='' or 1=1 AND `PASSWORD`='' or 1=1 ;
            //获取连接
            connection = jdbcutils.getConnection();
            //获取sql对象
            statement = connection.createStatement();
            //sql
            String sql="SELECT * FROM `users` WHERE `NAME`= "+"'"+username+"'"+" AND `PASSWORD`="+"'"+password+"'";
            //查询获取返回集合
            ResultSet query = statement.executeQuery(sql);
            //遍历
            while (query.next()){
                System.out.println(query.getObject("NAME"));
                System.out.println(query.getObject("PASSWORD"));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            jdbcutils.release(connection,statement,rs);
        }
    }
}
```



## 10.5 PreparedStatement对象

它可以防止sql注入,并且效率高

增删改查

```Java
package com.dong.lesson03;

import com.dong.lesson02.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.Date;

public class TestInsert2 {
    public static void main(String[] args) {
        Connection conn =null;
        PreparedStatement st =null;
        try {
            conn = JdbcUtils.getConnection();
            //?是占位符 代替参数
            String sql ="INSERT INTO `users`(`id`,`NAME`,`PASSWORD`,`email`,`birthday`) VALUES(?,?,?,?,?)";

            st =conn.prepareStatement(sql);//预编译sql 先写sql 但是不执行
            //手动给参数赋值
            st.setInt(1,4);
            st.setString(2,"abc");
            st.setString(3,"123456");
            st.setString(4,"264598@qq.com");
            //sql.Date
            //java.util.Date  //获得时间戳 然后传给sql.Date
            st.setDate(5,new java.sql.Date(new Date().getTime()));

            //以上填充完毕 然后执行
            int i = st.executeUpdate();
            if (i>0){
                System.out.println("插入成功");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtils.release(conn,st,null);
        }

    }
}
```

```Java
package com.dong.lesson03;

import com.dong.lesson02.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class TestDelete2 {
    public static void main(String[] args) {
        Connection conn =null;
        PreparedStatement st =null;
        try {
            conn = JdbcUtils.getConnection();
            //?是占位符 代替参数
            String sql ="delete  from users where id=?";

            st =conn.prepareStatement(sql);//预编译sql 先写sql 但是不执行
            //手动给参数赋值
            st.setInt(1,4);

            //以上填充完毕 然后执行
            int i = st.executeUpdate();
            if (i>0){
                System.out.println("删除成功");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtils.release(conn,st,null);
        }

    }
}
```



```Java
package com.dong.lesson03;

import com.dong.lesson02.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.Date;

public class TextUpdata {
    public static void main(String[] args) {
        Connection conn =null;
        PreparedStatement st =null;
        try {
            conn = JdbcUtils.getConnection();
            //?是占位符 代替参数
            String sql ="update users set `name` = ?  where id=? ;";

            st =conn.prepareStatement(sql);//预编译sql 先写sql 但是不执行
            //手动给参数赋值
           st.setString(1,"东");
           st.setInt(2,1);

            //以上填充完毕 然后执行
            int i = st.executeUpdate();
            if (i>0){
                System.out.println("更新成功");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtils.release(conn,st,null);
        }

    }
}
```

```Java
package com.dong.lesson03;

import com.dong.lesson02.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class testSelect {
    public static void main(String[] args) {
        Connection conn=null;
        PreparedStatement st=null;
        ResultSet re =null;
        try {
            conn = JdbcUtils.getConnection();
            String sql="Select * from users where id =?";

            st = conn.prepareStatement(sql);

            st.setInt(1,1);

             re = st.executeQuery();
             while (re.next()){
                 System.out.println(re.getString("NAME"));
                 System.out.println(re.getString("email"));
             }

        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtils.release(conn,st,re);
        }

    }
}
```





```java
import com.dong.lesson02.utils.JdbcUtils;
import java.sql.*;

public class LoginTest {
    public static void main(String[] args) throws SQLException {
        login("张三","123456");
    }
    public static  void login(String username,String password) throws SQLException {
        Connection connection=null;
        PreparedStatement statement=null;
        ResultSet rs=null;
        try {
            //SELECT * FROM `users` WHERE `NAME`='' or 1=1 AND `PASSWORD`='' or 1=1 ;
            //获取连接
            connection = jdbcutils.getConnection();
            //获取sql对象
            //PreparedStatement 防止sql注入的本质，把传递进来的参数当做字符
            // 假设其中存在转义字符,比如说' 会被忽略掉, '会被转义
            String sql="SELECT * FROM `users` WHERE `NAME`=? AND `PASSWORD`= ?";
           statement= connection.prepareStatement(sql);
            statement.setObject(1,username);
            statement.setObject(2,password);
            rs=statement.executeQuery();
            //sql

            //查询获取返回集合

            //遍历
            while (rs.next()){
                System.out.println("登陆成功");

            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            jdbcutils.release(connection,statement,rs);
        }
    }
}


```



## 10.6 IDEA 连接数据库

## 10.7 事务

​       

事务

ACID原则

原子性: 要么全都成功，要么全都失败

一致性: 结果不变，总数一直

隔离性: 多个进程互不干扰

持久性: 一旦提交不可逆，持久化到数据库了



隔离性的问题:

脏读 ：一个事务读取了另一个没有提交的事务

不可重复读：在同一个事务内，重复读取表中的数据，表数据发生了改变

虚读(幻读):在一个事务内，读取到了别人插入的数据，导致前后读出来的结果不一致（数据变多了）

>代码实现

1. 开启事务

2. 一组业务完毕，提交事务
3. 可以在catch 中显示回滚语句 默认失败回滚可以不写

```java
public class TestTransaction{
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement st = null;
        ResultSet re = null;

        try {
            conn = JdbcUtils.getConnection();
            //关闭数据库的自动提交且开启事务 开启事务是自动的
            conn.setAutoCommit(false);

            String sql1 = " update account set money=money-100 where name ='A'";
            st = conn.prepareStatement(sql1);
            st.executeUpdate(sql1);
            // int x=1/0  测试报错
            String sql2 = " update account set money=money+100 where name ='B'";
            st = conn.prepareStatement(sql2);
            st.executeUpdate(sql2);

            //业务完毕，提交事务
            conn.commit();
            System.out.println("操作成功");
        } catch (SQLException e) {
            try {
                conn.rollback(); //如果失败会回滚
                //这句话是默认就有的  可以不写
            } catch (SQLException ex) {
                ex.printStackTrace();
            }
            e.printStackTrace();
        }finally {
            JdbcUtils.release(conn,st,null);
        }
    }
}
```

## 10.9 数据库连接池

1. 数据库连接–执行完毕–释放 

连接 ，释放  十分浪费系统资源

池化技术:准备一些预先的资源,过来就连接预先准备好的

（服务员准备好了 过来就进行服务 直接扔sql）

假设 常用连接数是10

假设 有15个员工

**最小连接数: **10（常用连接数 可以是100，10等等）

**最大连接数:**15（业务最高重载上限）

超过15个的排队等待

**等待超时:100ms**



2. 编写连接池  需要实现DataSource接口

**开源数据源实现**（DataSource接口的实现类）

DBCP

C3P0

Druid:阿里巴巴

### 1. DBCP

**DBCP连接池配置**

```java
#连接设置
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/jdbcstudy?userUnicode=true&characterEncoding=utf8&uesSSL=true
username=root
password=123456

#<!-- 初始化连接 -->
initialSize=10

#最大连接数量
maxActive=50

#<!-- 最大空闲连接 -->
maxIdle=20

#<!-- 最小空闲连接 -->
minIdle=5

#<!-- 超时等待时间以毫秒为单位 6000毫秒/1000等于60秒 -->
maxWait=60000

#JDBC驱动建立连接时附带的连接属性属性的格式必须为这样：【属性名=property;】
#注意："user" 与 "password" 两个属性会被明确地传递，因此这里不需要包含他们。

connectionProperties=useUnicode=true;characterEncoding=utf8

#指定由连接池所创建的连接的自动提交（auto-commit）状态。
defaultAutoCommit=true

#driver default 指定由连接池所创建的连接的只读（read-only）状态。
#如果没有设置该值，则“setReadOnly”方法将不被调用。（某些驱动并不支持只读模式，如：Informix）
defaultReadOnly=

#driver default 指定由连接池所创建的连接的事务级别（TransactionIsolation）。
#可用值为下列之一：（详情可见javadoc。）NONE,READ_UNCOMMITTED, READ_COMMITTED, REPEATABLE_READ, SERIALIZABLE
defaultTransactionIsolation=READ_COMMITTED

```



**从数据源中获取连接**


```
import org.apache.commons.dbcp.BasicDataSourceFactory;
import javax.sql.DataSource;
import java.io.InputStream;
import java.sql.*;
import java.util.Properties;

public class jdbcutils_dbcp {
   private static   DataSource source=null;
    static {
        try {

            InputStream in =jdbcutils_dbcp.class.getClassLoader().getResourceAsStream("dbcpconfig.properties");
            Properties properties = new Properties();
            properties.load(in);
          //创建数据源 工厂模式--> 创建对象
             source = BasicDataSourceFactory.createDataSource(properties);


        }catch (Exception e){
            e.printStackTrace();
        }
    }


    // 获取连接
    public static Connection getConnection() throws SQLException {
        //从数据源中获取连接
        return source.getConnection();
    }
    //释放连接资源
    public static void  release(Connection conn, Statement st, ResultSet rs) throws SQLException {
        if(conn!=null) conn.close();

        if(st!=null) st.close();

        if(rs!=null) rs.close();
    }
}

```

**测试连接和查询**


```
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class testdbcp {
    public static void main(String[] args) throws SQLException {
        Connection connection=null;
        PreparedStatement statement=null;
        ResultSet rs=null;
        try {
            //获取连接
            connection = jdbcutils_dbcp.getConnection();
            //sql
            String sql="SELECT * from users WHERE id>?";
            //预编译sql
            statement= connection.prepareStatement(sql);
            //设置参数
            statement.setObject(1,1);
            //执行sql
            rs=statement.executeQuery();
            //遍历结果
            while (rs.next()){
                System.out.println(rs.getObject("NAME"));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            jdbcutils_dbcp.release(connection,statement,rs);
        }
    }
}
```
### 2. C3p0

**DBCP连接池配置**

c3p0-config.xml

xml 不用读取，加载会自动匹配

```java
<?xml version="1.0" encoding="UTF-8"?>
<c3p0-config>
    <!--
    c3p0的缺省（默认）配置
    如果在代码中"ComboPooledDataSource ds=new ComboPooledDataSource();"这样写就表示使用的是c3p0的缺省（默认）-->
    <default-config>
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/jdbcstudy?userUnicode=true&amp;characterEncoding=utf8&amp;uesSSL=true&amp;serverTimezone=UTC</property>
        <property name="user">root</property>
        <property name="password">123456</property>

        <property name="acquiredIncrement">5</property>
        <property name="initialPoolSize">10</property>
        <property name="minPoolSize">5</property>
        <property name="maxPoolSize">20</property>

    </default-config>

    <!--
    c3p0的命名配置
    如果在代码中"ComboPooledDataSource ds=new ComboPooledDataSource("MySQL");"这样写就表示使用的是mysql的缺省（默认）-->
    <named-config name="MySQL">
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/jdbcstudy?userUnicode=true&amp;characterEncoding=utf8&amp;uesSSL=true&amp;serverTimezone=UTC</property>
        <property name="user">root</property>
        <property name="password">123456</property>

        <property name="acquiredIncrement">5</property>
        <property name="initialPoolSize">10</property>
        <property name="minPoolSize">5</property>
        <property name="maxPoolSize">20</property>
    </named-config>
</c3p0-config>

```

**c3p0连接数据库**

```java
import com.mchange.v2.c3p0.ComboPooledDataSource;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;


public class jdbcutils_c3p0 {

   private static ComboPooledDataSource source=null;
    static {
        try {
             source= new ComboPooledDataSource("MySQL");
          //创建数据源 工厂模式--> 创建对象
            

        }catch (Exception e){
            e.printStackTrace();
        }
    }


    // 获取连接
    public static Connection getConnection() throws SQLException {
        //从数据源中获取连接
        return source.getConnection();
    }
    //释放连接资源
    public static void  release(Connection conn, Statement st, ResultSet rs) throws SQLException {
        if(conn!=null) conn.close();

        if(st!=null) st.close();

        if(rs!=null) rs.close();
    }
}


```

```java
无论使用什么数据源 本质还是一样 ,DataSource 接口不会变
```



```sql

 
```

### ***

BETWEEN  AND  包含边界
