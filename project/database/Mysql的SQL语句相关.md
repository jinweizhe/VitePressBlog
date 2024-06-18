[当前文章只有关于SQL语句的,想看更为详细的点击查看这一篇文章](./MySql学习笔记(基础+进阶))

## 常用的字段数据类型
```sql
int：整型
double：浮点型，例如double(5,2)表示最多5位，其中必须有2位小数，即最大值为999.99；
char：固定长度字符串类型； char(10)  如果不足10位则会自动补足10位：'abc       '
varchar：可变长度字符串类型；varchar(10) 如果不足10位不会补足：'abc',性能不如char高
text：字符串类型;适用于大文本内容。
blob：字节类型；
date：日期类型，格式为：yyyy-MM-dd；
time：时间类型，格式为：hh:mm:ss
timestamp：时间戳类型 yyyy-MM-dd hh:mm:ss  会自动赋值
datetime:日期时间类型 yyyy-MM-dd hh:mm:ss
```
## 例子
```sql
有个users表  四个字段id username password status，四个字段代表四列,其中id为自增列，status默认值为0,可选值0，1
id自增，username分别为zs,ls,wu  password分别为：123456 abcdef 123abc  status为0，1，1
查询整张表的所有数据
select * from users
查询指定列的所有数据
select username,password from users
指定某列添加数据
insert into users(username,password) values('萧寂','1234')
指定某列修改数据
update users set username="你好a",password="1234567",status=1 where id=2
根据id删除行
delete from users where id=4
查询status为1的所有用户
SELECT *FROM users WHERE status=1
查询id大于2的所有用户
SELECT *FROM users WHERE id>2
查询username不等于admin的所有用户
SELECT *FROM users WHERE username<>'admin'
使用AND来显示所有status为0，并且id 小于3的用户:
SELECT * FROM users WHERE status=0 AND id<3
使用OR来显示所有status为1，或者username为zs的用户
SELECT* FROM users WHERE status=1 OR username='zs'
对users表中的数据，按照status字段进行升序排序
SELECT * FROM users ORDER BY status;（升序排序在status后加上ASC效果等同）
select * from users order by status asc
根据id降序排序,降序排序使用desc关键字
select * from users order by id desc
多重排序 对users 表中的数据，先按照status字段进行降序排序，再按照username的字母顺序，进行升序排序
SELECT * FROM users ORDER BY status DESC,username asc
查询id为1的数据返回的总条数
select count(*) from users where id=1
将列名称从COUNT(*)修改为total
SELECT COUNT(*) AS total FROM users WHERE id=1
给username列添加uname别名，给password列添加upwd别名
select username as uname,password as upwd from users
```
#### DML(基础增删改查)扩展
```sql
查询指定列
select 列名1,列名2... from 表名
去除指定列的重复数据(如果没去掉就检查一下是否有空格之类的)
SELECT DISTINCT 列名1,列名2... from 表名
两个数字列进行计算
SELECT 列名1,列名2,列名1+(-/*)列名2 FROM 表名
两个数字列进行计算如果有null值则计算结果也为null,这是给个非空判断(如果为null就赋值0)
SELECT 列名1,列名2,列名1+(-/*)IFNULL(列名2,0) FROM 表名
起别名(在列后加上as再加上想要起的名字)(as可省略,列名和起的名字中间要有空格代替)
SELECT password as "密码",status as "状态",password+status from users
```
## DDL
```sql
创建数据库：
create database mydata ;
指定字符编码：
create database mydata2 character set gbk;
查看数据库：
show databases;
查看数据的定义信息：
show create database mydata;
修改数据库：
将mydata2数据库的编码修改为utf-8；
alter database mydata2 character set utf8;
删除数据库：
drop database mydata2;
查看当前使用的数据库：
select database();
切换数据库：
use study2;
查看数据库中所有的表：
show tables;
查看表的字段信息：
desc student;
员工表的基本上增加一个address列：
alter table student add address varchar(1000);
修改name列，使其长度为30：
alter table student modify name varchar(30);
删除address列,一次只能删一列：
alter table student drop address;
表名改为user：
rename table student to user;
查看表格的创建细节：
show create table user;
修改表的字符集为gbk：
alter table user character set gbk;
列名name修改为username：
alter table user change name username varchar(20)；
删除表：
drop table user;
```
## DQL(查询语句:排序查询,聚合函数,分组查询,分页查询)
#### 条件查询运算符
```sql
大于:>  
大于等于:>=   
等于:=   
非等于:!=   非等于:<> 
并且:AND(&&) (下有案例) 
在..之间:between...and (下有案例)
或者:or(||)(下有案例)
包含:in(下有案例)
是否:is(NULL不能用=(!=)判断)(下有案例) is null
非:not(!)  is not null
例如查询年龄20到30之间的三种方式
select * from student age>=20 && age<=30
select * from student age>=20 AND age<=30
select * from student age BETWEEN 20 AND 30
例如查询指定年龄(22,18,25)的;两种方式
SELECT *FROM student WHERE age=22 OR age=18 OR age=25
SELECT *FROM student WHERE age in(22,18,25)
例如判断年龄是空
SELECT *FROM student WHERE age = null  --语法错误,NULL不能用=(!=)判断
SELECT *FROM student WHERE age is null  --语法正确,用is成功解决非空判断
例如判断年龄不为空
SELECT *FROM student WHERE age is not null
```
#### 模糊查询
```sql
like
	占位符:
		_:单个任意字符
		%:任意多个字符
例如:查询user表name列姓王的人
SELECT * from user WHERE name LIKE '王%'
查询第二个名字第二个字为化的人
SELECT * from user WHERE name LIKE '_化%'
姓名是三个字的人
SELECT * from user WHERE name LIKE '___'(三个_)
姓名包含王的人
SELECT * from user WHERE name LIKE '%王%'
```
#### 排序查询
```sql
ASC升序排序(默认)
DESC降序排序
如果有多个排序条件，则当前边的条件值一样时，才会判断第二条件。
SELECT * from 表名 ORDER BY 列名 ASC(升序)
SELECT * from 表名 ORDER BY 列名 DESC(降序)
案例:按照数学成绩排名，如果列一成绩一样,则按照列二成绩排名
SELECT * from 表名 ORDER BY 列一 ASC , 列二 ASC;(当第一条件一样时,按照第二种条件排序)
```
#### 聚合函数
```sql
定义:将一列数据作为一个整体，进行纵向的计算。
1.count:计算数量个数
2.max:计算最大值
3.min:计算最小值
4.sum:计算和
5.avg:计算平均值
注意:聚合函数的计算，排除null值。,可以使用下面方式将null改为0
select count(ifnull(列名,0)) from 表名 --表示当这个字段为null时,就替换0

count使用:统计某一列的数量,*代表整个表
select count(列名) from 表名

max和min、sun、avg的使用:
查询成绩最高的
select MAX(成绩列) from 表名
查询成绩最低的
select MIN(成绩列) from 表名
求当前列总和
select SUM(成绩列) from 表名
求当前列平均数
select AVG(成绩列) from 表名
```
#### 分组查询
```sql
语法:group by 分组字段
where和having的区别?
	分组前每条数据是一个个体。分组后每条数据就是一个组了
	where在分组之前进行限定,如果不满足条件,则不参与分组
	having在分组之后进行限定,如果不满足结果则不会被查询出来
	where后不可以跟聚合函数,having可以进行聚合函数的判断
注意:
1。分组之后查询的字段:分组字段、聚合函数

案例:按照性别分组查男女同学平均分
select 组名(性别列) , AVG(分数列)  from 表名 group by 组名(性别列)
按照性别分组。分别查询男、女同学的平均分,人数
select 组名(性别列) , AVG(分数列),count(id列) from 表名 group by 组名(性别列)

带条件的分组
按照性别分组。分别查询男、女同学的平均分,人数(要求,分数低于70分的不参与分组)
select 组名(性别列) , AVG(分数列),count(id列) from 表名 where score > 70 group by 组名(性别列)
按照性别分组。分别查询男、女同学的平均分,人数(要求,分数低于70分的不参与分组),分组之后人数大于二
select 组名(性别列) , AVG(分数列),count(id列) from 表名 where score > 70 group by 组名(性别列) having count(id列)>2
```
#### 分页查询
```sql
语法:limit 开始索引,每页查询的条数
每页显示三条记录
SELECT * FROM 表名 LIMIT 0,3 --第一页 0索引到2索引  数量3
SELECT * FROM 表名 LIMIT 3,3 --第二页 3索引开始往后走三个

通用公式(开始索引=(当前页码-1)*每页显示的条数)
SELECT * FROM 表名 LIMIT (当前页码-1)*每页显示的条数,每页显示的条数
```
## 约束

> 	概念∶对表中的数据进行限定，保证数据的正确性、有效性和完整性。
	分类:
		主键约束:primary key
		非空约束:not null
		唯一约束:unique
		外键约束:foreign key

#### 主键约束
```sql
单词:primary key
主键:含义:非空且唯一
		一张表只能有一个字段为主键
		主键就是表中记录的唯一标识
1.创建表时添加主键约束
	create table xiaoji(
	id int primary key ,  --给id添加主键约束
	name varchar(20)
	);
2.创建表后添加主键约束
alter table 表名称 modify 列名 数据类型 primary key;
删除主键(因为只有一个,所以不需要指定列名)
ALTER TABLE 表名 drop primary key;;
```
###### 主键约束--自动增长
```sql
1.概念:如果某一列是数值类型的,使用auto_increment 可以来完成值的自动增长
2.在创建表时,添加主键约束,并且完成主键自动增长
	CREATE TABLE jin(
	id int PRIMARY KEY auto_increment,
	name VARCHAR(20)
);
创建表后给id添加自动增长
alter table 表名称 modify id int auto_increment;
删除自增(一般都是给id加主键自增,所以这里直接写id了)
ALTER TABLE 表名 MODIFY id int;
```
#### 非空约束
```sql
单词:not null
某一列的值不能为null  数据为空不会被添加
1.创建表时添加约束,如下
CREATE TABLE xi(
	id INT,
	NAME VARCHAR(20) NOT NULL  -- name为非空约束
);
2.创表后添加
alter table 表名称 modify 列名 数据类型 not null;

删除约束
ALTER TABLE 表名 MODIFY 带约束列名 (数据类型VARCHAR(20));
```
#### 唯一约束
```sql
单词: unique
注意:唯一约束可以有null值,但是此列只能有一次为null
添加:
	1.创建表添加
		CREATE TABLE jin(
				id int,
				phone_number VARCHAR(20) UNIQUE  -- phone_number为非空约束
			)
	2.创建表后添加
	alter table 表名称 modify 列名 数据类型 UNIQUE;
	删除唯一约束
	ALTER TABLE 表名 drop index 带约束的列名
```
#### 外键约束
```sql
单词:foreign key  让表与表之间产生关系,从而保证数据的正确性
含义:定义员工表和职位表,员工表在最后定义一个外键id,这个id关联第二张表的主键,当关联的主键被删除,则绑定这个主键的外键员工也要对应删除,否则找不到一一对应关系,外键约束就是删除职位表时会判断关联的外键是否删除,如果未删除则删除不掉对应的职位,当所有绑定此主键的外键的员工被删除,这个职位2才能被删除,这就是外键约束.

1.在创建表时添加外键
 语法:
 	create table 表名(
	...... -- 这些是创建其他列的,这里就不写了,只写外键创建方式
	外键列名(dep_id) 数据类型, -- 外键对应主表的主键
	constraint 外键名(随便起) foreign key (当前表的外键列名dep_id) references 主表名称(主表关联的主键)
)
创建表之后添加外键
alter table 表名 add constraint 外键名 foreign key(当前表的外键列名) references 主表名称(主表关联的主键)
例如创建个学生表和班级表
学生表绑定班级表,学生绑定的对应的班级外键未删除,当前班级就不能删除
	CREATE TABLE student(
				id int,
				name VARCHAR(20),
				stu_id int,
				constraint school foreign key (stu_id) references class(id)
	)
	CREATE TABLE class(
				id int PRIMARY KEY auto_increment,
				classname VARCHAR(20),
	)

删除外键
alter table 表名 drop foreign key 外键名;
```
###### 外键约束--级联操作
```sql
含义:当主表主键想要更改值时,由于外键约束会导致失败,此时只能将关联表的外键先赋值为null,时关联表没有值与此主键对应,主键才能修改,步骤为:关联表外键==null->主表修改值->关联表更换新值,这种操作比较麻烦,所以有了级联操作(级联更新)

1.在添加外键时设置级联更新,
	在添加外键语句后加on update cascade(级联更新) on delete cascade(级联删除,可以在级联更新后面直接写)
例如
(级联更新)
constraint 外键名(随便起) foreign key (当前表的外键列名dep_id) references 主表名称(主表关联的主键) on update cascade

(级联更新和级联删除)
constraint 外键名(随便起) foreign key (当前表的外键列名dep_id) references 主表名称(主表关联的主键) on update cascade on delete cascade

创建表之后添加外键--级联更新
alter table 表名 add constraint 外键名 foreign key(当前表的外键列名) on update cascade

创建表之后添加外键--级联更新和级联删除
alter table 表名 add constraint 外键名 foreign key(当前表的外键列名) on update cascade on delete cascade

(级联更新)
添加完以后,在主表修改绑定的主键的值,然后回到关联表会发现,所有与主表主键关联的值都自动更新了新值,不用手动赋null再去更新值了

(级联删除)
和更新一样,删除完主表对应的主键,关联表与此主键绑定的数据也自动删除
```
## 多表关系(数据库的设计)
#### 一对多(多对一)
```sql
如部门与员工(一个员工只能有一个部门,一个部门能有多个员工)

实现方式:
	在多的一方建立外键,指向一的一方的主键。
```
#### 多对多
```sql
如学生和课程关系(一个课程被多个学生选择,一个学生也可以有很多课程)

实现方式:
	新建一个第三个中间表(最少两个列),这两个字段作为第三张表的外键,分别指向两张主表的主键,并且在前面添加一个复合主键,如下:
	两个表主键分别为rid,uid
	第三个表设计为:
		cerate table 第三方表名(
			-- 创建最少两个列设置外键绑定两张表主键
			rid int, --表一绑定的外键
			data datetime, --日期(额外添加的列)
			uid int, --表二绑定的外键
			-- 创建复合主键(相当于这两个列合起来作为这个表的主键,两个列的情况不能重复)
			primary key(rid,uid)
			-- 将两个绑定多表的列进行外键绑定
			constraint 外键名 foreign key (rid(当前表的)) references 绑定的表一名(rid(表一主键))
			constraint 外键名二 foreign key (uid(当前表的)) references 绑定的表二名(uid(表二主键))
	)
```
#### 一对一(了解)
```sql
如人和身份证的关系(一个人只能有一个身份证,一个身份证也只有一个人)

实现方式:
	两张表任意一个表添加外键指向另一方的主键,让外键唯一,使之不会出现一对多的情况
```
## 数据库三大范式

> 概念:设计数据库时,需要遵循的一些规范(要遵循后边的范式要求，必须先遵循前边的所有范式要求)
> 设计关系数据库时，遵从不同的规范要求，设计出合理的关系型数据库，这些不同的规范要求被称为不同的范式，各种范式呈递次规范，越高的范式数据库冗余越小。


#### 第一范式
```sql
第一范式(1NF) :每一列都是不可分割的原子数据项(每个列下面不能有多个子列,只能有当前列的数据)

存在的问题:
	1.存在非常严重的数据冗余(重复)(每个有多个子列,例如班级列下面又存在学生和老师两个子列)
	2.数据添加存在问题,比如单独添加学生姓名,但没有添加学号之类的,造成了数据不合法.
	3.数据删除存在问题,例如:xiaoji同学毕业了,删除学生姓名的同时	会将院系,班级等的数据一并删除
```
#### 第二范式
```sql
第二范式(2NF)∶在1NF的基础上，非码属性必须完全依赖于候选码（在1NF基础上消除非主属性对主码的部分函数依赖)
	几个概念:
		1.函数依赖:A->B,如果通过A属性(属性组)的值,可以确定唯一B属性的值.则称B依赖于A.
			属性:
				例如:可以通过学号(A)来确定某个学生(B)
			属性组:
				例如:当多个学生拥有同一个学号时,可以通过 学号和年龄 或 学号和班级 确定某个学生,这种组的形式确定数据的形式称为属性组依赖
		2.完全函数依赖,A->B,如果A是一个属性组,则B属性值的确定需要依赖A属性组中所有的属性值.
			例如上面:学号和年龄 或 学号和班级 被学生给完全依赖了
		3.部分函数依赖,A->B,如果A是一个属性组,则B属性值的确定只需要依赖于A属性组中某一些值即可。
			例如上面:学号、年龄 或 学号 、班级 被 学生 给部分依赖了
		4.传递函数依赖:A->B->c,如果通过A属性(属性组)的值,可以确定唯一B属性的值,再通过B属性(属性组)的值可以确定唯一C属性的值,则称C传递函数依赖于A,
			例如:可以通过学号确定院系,通过院系可以确定系主任,那么就是系主任传递函数依赖于学号
		5.码:如果在一张表中，一个属性或属性组，被其他所有属性所完全依赖，则称这个属性(属性值)为该表的码
			例如:通过学号可以确定唯一的姓名,院系,系主任,通过课程名称和学号可以确定成绩,也可以确定以上的姓名、院系、系主任,则在此表中,学号和课程名称就是码,因为它们被其他属性完全依赖
			主属性:码属性组中的所有属性
			非主属性:除码属性组的属性外的属性
			消除非主属性对主码的部分函数依赖:
				例如上面:消除 姓名,院系,系主任 对 学号和课程名称 的依赖
(第二范式解决了第一范式的1条问题)
```
#### 第三范式
```sql
第三范式（3NF)︰在2NF基础上，任何非主属性不依赖于其它非主属性（在2NF基础上消除传递依赖)
类似于多对一的关系:创建一个第三方表.(第三范式解决了第一范式的2,3条问题)
```
## 数据库的备份和还原
```sql
方式:
	1.命令行:
		语法:
			备份:mysqldump -u用户名 -p密码 数据库名 > 保存的路径
			还原:
				1.登录数据库(mysql -u用户名 -p密码)
						查看数据库(先查看有没有,有的话就删了重新创建,因为要演示的是还原操作,不删除就相当于存在了)：show databases;
						删除数据库:drop database 库名;
				2.创建数据库(create database 库名)
				3.使用数据库(use 库名)
				4.执行文件.(source sql文件路径)
	2.图形化工具:
		本人用的是:Navicat Premium16.0.13版本
			导出:选中数据库名-右键-转储sql文件-结构和数据(不要仅结构,不然没数据)
			导入:创建一个数据库,右键库名-运行sql文件-选择路径-运行即可
```
## 多表查询

> 笛卡尔积:有两张表AB,取这两张表的所有组成情况,例如:A有3条数据,B有
> 5条数据,那么多表查询select * from A,B 的情况就有15条数据
> 要完成多表查询,需要消除无用的数据,从而出现了(内连接,外连接,子查询)

#### 内连接
```sql
内连接查询:
	隐式内连接:(使用where条件消除无用数据)
		select 表一别名.表一列名,表二别名.表二列名...  from 表一名 表一别名,表二名 表二别名 where 条件(表一别名.id=表二别名.id)
	显示内连接:()
		语法: select 列名1,列名2,列名3... from 表名一 inner join 表名二 on 条件(表名一.id=表名二.id)
		inner可选---可省略
		语法: select 列名1,列名2,列名3... from 表名一 join 表名二 on 条件(表名一.id=表名二.id)

注意:
	要明白以下几点:
		1.从哪些表中查询数据
		2.条件是什么
		3.查询哪些字段
```
#### 外连接
```sql
分类:
	左外连接:(查询的是左表所有数据以及其交集部分)
		语法:select 列名1,列名2,列名3... from 表一 left outer join 表二 表二别名 on 条件(表名一.id=表名二.id)
		outer 可选---可省略
		语法:select 列名1,列名2,列名3... from 表一 表一别名 left join 表二 表二别名 on 条件(表名一.id=表名二.id)
		
	右外连接:(查询的是右表所有数据以及其交集部分)
		语法:select 列名1,列名2,列名3... from 表一 表一别名 right outer join 表二 表二别名 on 条件(表名一.id=表名二.id)
		outer 可选---可省略
		语法:select 列名1,列名2,列名3... from 表一 表一别名 right join 表二      表二别名 on 条件(表名一.id=表名二.id)
```
#### 子查询
```sql
概念:查询中嵌套查询,称嵌套查询为子查询  

例如查询工资最高的员工信息需要两步--
		1.查询工资最高的金额  select MAX(工资列) from 表名  -- 假设9000
		2.再查询最高金额对应的员工数据  select * from 表名 where 表名.工资列=9000;

使用子查询可以嵌套查询,一步到位
	sql语句:select * from 表名 where 表名.工资列=(select MAX(工资列) from 表名);
	这就是子查询,括号里面的查询就是称为子查询

子查询的情况:
	1.子查询的结果是单行单列的(上面那种情况就是单行单列的)
		子查询可以作为条件,使用运算符去判断(>,<,>=,<=.....)
			如:查询员工工资小于平均工资的人
				select * from 表名 where 表名.员工列 < (select AVG(工资列) from 表名);
	2.子查询的结果是多行单列的
		子查询可以作为条件,使用运算符in来判断
			如 查询'财务部'和'市场部'所有的员工信息(两张表)
				select * from 员工表 where 员工id in (select id from 部门表 where name='财务部' or name='市场部')
	3.子查询的结果是多行多列的
		子查询可以作为一张虚拟表
			如 查询员工入职日期是2011-11-11日之后的员工信息和部门信息
			-- select * from 员工表 where 员工表.日期列>'2011-11-11' 在这里是作为一张表计算的
			select * from 部门表名 部门表别名,(select * from 员工表 where 员工表.日期列>'2011-11-11') 新表别名 where 部门表别名.id=新表别名.id	
```
## 事务

#### 事务的基本介绍
```sql
概念:
	如果一个包含多个步骤的业务操作，被事务管理，那么这些操作要么同时成功，要么同时失败
	操作:
		1.开启事务 start transaction;
		2.回滚: rollback;
		3.提交: commit;

案例:一张表两个数据,张三,1000元,李四,1000元,做转账操作
	张三账户减少500,李四账户加上500
	
	下面两个命令需要同时执行,才能完成转账
	update 表名 set 金额列=金额列-500 where name='张三'
	update 表名 set 金额列=金额列+500 where name='李四'

	下面三行要同时执行,会发现报错了,但是第一条语句执行成功,第二条没有执行到,造成了张三减少500,李四金额没变(500元不翼而飞了),这样就需要事务进行管理了
	update 表名 set 金额列=金额列-500 where name='张三'
	出错了....(不是注释)
	update 表名 set 金额列=金额列+500 where name='李四'

事务的简单使用:
	1.开启事务	
	2.张三账户减500  
	3.李四账户加500
	将下面几行同时选中执行,当有任意一条失败就会回滚到第一条命令,将已经执行过的操作全部撤销
		start transaction; -- 开启事务
		update 表名 set 金额列=金额列-500 where name='张三'
		update 表名 set 金额列=金额列+500 where name='李四'
		rollback; -- 当发现出问题了,就回滚事务到第一条
		commit; -- 提交事务,当所有操作都成功就会提交(不提交数据没有任何变化会一直保持事务状态),提交后才是真正的操作完了

mysql数据库中事务默认自动提交
Oracle数据库默认手动提交,必须写commit;才会提交
	一条DML(增删改)语句会自动提交一次事务
	事务提交的两种方式:
		自动提交:
			mysql数据库中事务默认自动提交
		手动提交:
			需要先开启事务,再提交
	修改事务的默认提交方式:
		查看事务的默认提交方式
			select @@autocommit; -- 1代表自动提交,0代表手动提交
		修改事务的提交方式:
			set @@autocommit = 0;
			修改完再执行DML语句不会自动提交了,需要手动在后面写commit;与DML语句一起执行才会提交
```
#### 事务的四大特征
```sql
1:原子性:(是不可分割的最小操作单位,要么同时成功要么同时失败)
2.持久性:当事务提交或回滚后,数据库会持久化的保存数据
3.隔离性:多个事务之间.相互独立.
4.一致性:事务操作前后,数据总量保持不变
```
#### 事务的隔离级别(了解,一般为了应付面试)
```sql
	概念:多个事务之间隔离的,相互独立的.但是如果多个事务操作同一批数据，则会引发一些问题，设置不同的隔离级别就可以解决这些问题。
存在问题:
	1.脏读:一个事务中,读取到另一个事务中没有提交的数据
	2.不可重复读(虚读):在同一个事务中,两次读取到的数据不一致
	3.幻读:一个事务操作(DML)数据表中所有记录,另一个事务添加了一条数据,则第一个事务查询不到自己的修改.

隔离级别:
	1.read uncommitted : 读未提交(易产生:脏读,幻读,不可重复读)
	2.read committed:读已提交(易产生:幻读,不可重复读)
	3.repeatable read:可重复读(易产生:幻读)
	4.serializable : 可串行化(无任何问题)

注意:隔离级别从小到大安全性越来越高，但是效率越来越低
mysql默认:repeatable read:可重复读(易产生:幻读)
Oracle默认:read committed:读已提交(易产生:幻读,不可重复读)

数据库查询隔离级别:
	select  @@tx_isolation
数据设置隔离级别:
	set global transaction isolation level 上面隔离级别单词;
	设置完要断开连接重新打开才生效
```
## DCL(公司DBA就是数据库管理员)
#### 管理用户("增删查")
```sql
```
#### 管理用户("修改密码")
```sql
1.添加用户:
	创建用户
		create user '用户名' @ '主机名' identified by '密码'
		例如:create user 'xaioji'@'localhost' identified by '123' -- 主机名为%代表所有的主机
2.删除用户:
	drop user '用户名' @ '主机名'
3.修改用户密码:
	update user set password = password('新密码') where user = '用户名'
	简化形式:
	set password for '用户名' @ '主机名' = password('新密码')
4:查询用户:
	1.切换到mysql数据库
	user mysql
	2.查询user表
		select * from user
		通配符: %表示可以在任意主机使用用户登录数据库
```
#### 管理权限
```sql
1.查询权限
	语法:
		show grants for '用户名' @ '主机名(localhost)' 
2.授予权限
	语法:
	权限:select,update,delete,insert等等....
		grant 权限列表 on 数据库名.表名 to '用户名' @ '主机名(localhost)'
		all代表所有权限,所有库和表都为*
		-- 以下是最高权限的设置
		grant all on * . * to '用户名' @ '主机名(localhost)'
3.撤销权限
	语法:
		revoke 权限列表 on 数据库名.表名 from '用户名' @ '主机名'
```