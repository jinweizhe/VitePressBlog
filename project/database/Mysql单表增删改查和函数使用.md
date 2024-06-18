# 创建表
```sql
create table employee(
	id int auto_increment comment 'ID' primary key,
	workno varchar(10) comment '工号',
	name varchar(10) comment '姓名',
	gender char(1) comment '性别',
	age tinyint unsigned comment '年龄',
	idcard char(18) comment '身份证',
	entrydate date comment '入职日期'
)comment '员工表';
```
# 增(DCL)
**1.给指定字段添加数据**

`insert into 表名(字段名1,字段名2,...) values (值1,值2);`
```sql
insert into employee(id,workno,name,gender,age,idcard,entrydate) values (null,'1','萧寂','男',10,'123456789123456789','2001-01-01')
```
**2.给全部字段添加数据**

`insert into 表名 values (值1,值2,...);`
```sql
insert into employee values (null,'1','萧寂','男',10,'123456789123456789','2001-01-01')
```
**3.批量添加数据**

`insert into 表名(字段名1,字段名2,...) values (值1,值2,...), (值1,值2,...), (值1,值2,...);`
```sql
insert into employee(id,workno,name,gender,age,idcard,entrydate) values (null,'4','萧寂4','男',10,'123456789123456789','2001-01-01'),(null,'5','萧寂5','男',10,'123456789123456789','2001-01-01'),(null,'6','萧寂6','男',10,'123456789123456789','2001-01-01')
```
或者

`insert into 表名 values (值1,值2,...), (值1,值2,...), (值1,值2,...);`
```sql
insert into employee values (null,'1','萧寂','男',10,'123456789123456789','2001-01-01'),(null,'2','萧寂2','男',10,'123456789123456789','2001-01-01'),(null,'3','萧寂3','男',10,'123456789123456789','2001-01-01')
```

注意:
- 插入数据时，指定的字段顺序需要与值的顺序是一一对应的。
- 字符串和日期型数据应该包含在引号中。
- 插入的数据大小，应该在字段的规定范围内。

# 改(DCL)
`update 表名 set 字段名1=值1,字段名2=值2,...,[where条件]`

**注意:修改语句的条件可以有，也可以没有，如果没有条件，则会修改整张表的所有数据。**

**小案例**
1. 修改id为1 的数据，将name修改为xiaoji
```sql
update employee set name = 'xiaoji' where id = 1;
```
2. 修改id为1 的数据，将name修改为小昭 gender 修改为女
```sql
update employee set name = '小昭',gender = '女' where id = 1;
```
3. 将所有的员工入职日期修改为 2008-01-01
```sql
update employee set entrydate = '2008-01-01'
```
# 删(DCL)
`delete from 表名 [where条件]`

**注意:删除语句的条件可以有，也可以没有，如果没有条件，则会删除整张表的所有数据。**

**DELETE 语句不能删除某一个字段的值(可以使用UPDATE)。**

**小案例**
1. 删除gender为女的员工
```sql
delete from employee where gender = '女'
```
2. 删除所有员工
```sql
delete from employee
```
# 查(DQL)
**建表语句**
```sql
create table emp(
 id int comment '编号',
 workno varchar(10) comment '工号',
 name varchar(10) comment '姓名',
 gender char comment '性别',
 age tinyint unsigned comment '年龄',
 idcard char(18) comment '身份证号',
 workaddress varchar(50) comment '工作地址',
 entrydate date comment '入职时间'
) comment '员工表';

insert into emp(id,workno,name,gender,age,idcard,workaddress,entrydate) values 
(1,'1','柳岩','女',20,'123456789012345678','北京','2000-01-01'),
(2,'2','张无忌','男',20,'123456789012345670','北京','2005-01-01'),
(3,'3','韦一笑','男',20,'123456789712345670','上海','2005-09-01'),
(4,'4','赵敏','女',20,'123456757123845670','北京','2009-12-01'),
(5,'5','小昭','女',20,'123456769012345678','上海','2007-07-01'),
(6,'6','杨逍','男',28,'12345678931234567X','北京','2006-01-01'),
(7,'7','范瑶','男',40,'123456789012345671','北京','2006-01-01'),
(8,'8','黛绮丝','女',38,'123456789012345672','天津','2005-05-01'),
(9,'9','范凉凉','女',45,'123456789012345673','北京','2015-05-01'),
(10,'10','陈友谅','男',53,'123456789012345674','上海','2010-04-01'),
(11,'11','张士诚','男',55,'123456789012345675','江苏','2011-01-01'),
(12,'12','常遇春','男',32,'123456789012345676','北京','2015-05-01'),
(13,'13','张三丰','男',88,'123456789012345677','江苏','2004-02-01'),
(14,'14','灭绝','女',65,'123456789012345678','西安','2020-11-01'),
(15,'15','胡青牛','男',70,'123456789012345679','西安','2019-05-01'),
(16,'16','周芷若','女',18,'123456789012345670','北京','2018-04-01')
```
**查询语法**
```sql
select
	字段列表
from
	表名列表
where
	条件列表
group by
	分组字段列表
having
	分组后条件列表
order by
	排序后字段列表
limit
	分页参数
```
- 基本查询
- 条件查询(WHERE)
- 聚合函数(count、max、min、avg、sum)
- 分组查询(GROUP BY)
- 排序查询(ORDER BY)
- 分页查询(LIMIT)

## 基本查询
**1.查询多个字段**

`select 字段1,字段2,字段3,... from 表名;`

`select * from 表名`

**2.设置别名**

`select 字段1 [as 别名1], 字段2 [as 别名2],... from 表名;`

**3.去除重复记录**

`select distinct 字段列表 from 表名;`

**示例**

```sql
-- 查询指定字段name,workno,age返回
select name,workno,age from emp
-- 查询所有字段返回
select * from emp
-- 查询所有员工的工作地址,起别名
select workaddress as '工作地址' from emp -- as 可省略
-- 查询公司员工的上班地址(不要重复)
select distinct workaddress '工作地址' from emp
```
## 条件查询
`select 字段列表 from 表名 where 条件列表`

**条件**
|比较运算符| 功能 |
|--|--|
| > | 大于 |
| >= | 大于等于 |
| < | 小于 |
| <= | 小于等于 |
| = | 等于 |
| <> 或 != | 不等于 |
| between...and... | 在某个范围之内(含最小、最大值) |
| in(...) | 在in之后列表中的值,多选一 |
| like 占位符 | 模糊匹配(_[匹配单个字符,%匹配任意个字符]) |
| is null | 是null |
| and 或 && | 并且,多个条件同时成立 |
| or 或 ll | 或者(多个条件任意一个成立) |
| not 或 ! | 非,不是 |

**示例**

```sql
-- 1.查询年龄等于88的员工
select * from emp where age = 88;
-- 2.查询年龄小于 20 的员工信息
select * from emp where age < 20;
-- 3.查询年龄小于等于 20 的员工信息
select * from emp where age <= 20;
-- 4.查询没有身份证号的员工信息
select * from emp where idcard is null;
-- 5.查询有身份证号的员工信息
select * from emp where idcard is not null;
-- 6.查询年龄不等于 88 的员工信息
select * from emp where age != 88;
select * from emp where age <> 88;
-- 7.查询年龄在15岁(包含)到20岁(包含)之间的员工信息
select * from emp where age >= 15 && age <= 20;
select * from emp where age >= 15 and age <= 20;
select * from emp where age between 15 and 20;
-- 8.查询性别为女且年龄小于 25岁的员工信息
select * from emp where gender = '女' and age < 25;
-- 9.查询年龄等于18或20或40 的员工信息
select * from emp where age = 18 or age = 20 or age = 40;
select * from emp where age in(18,20,40);
-- 10.查询姓名为两个字的员工信息
select * from emp where name like '__'
-- 11.查询身份证号最后一位是X的员工信息
select * from emp where idcard like '%X'
select * from emp where idcard like '_________________X'
```
## 聚合函数
**将一列数据作为一个整体，进行纵向计算。**

`count、max、min、avg、sum`
|函数| 功能 |
|--|--|
| count | 统计数量 |
| max | 最大值 |
| min | 最小值 |
| avg | 平均值 |
| sum | 求和 |

`select 聚合函数(字段列表) from 表名`

**示例代码**
```sql
-- 1.统计该企业员工数量
select count(*) from emp;
select count(idcard) from emp;
-- 2.统计该企业员工的平均年龄
select avg(age) from emp;
-- 3.统计该企业员工的最大年龄
select max(age) from emp;
-- 4.统计该企业员工的最小年龄
select min(age) from emp;
-- 5.统计西安地区员工的年龄之和
select sum(age) from emp where workaddress = '西安'
```
## 分组查询
`group by`
`select 字段列表 from 表名 [where 条件] group by 分组字段名 [having 分组后的过滤条件];`

where与having区别
- 执行时机不同:where是分组之前进行过滤，不满足where条件，不参与分组;而having是分组之后对结果进行过滤。
- 判断条件不同:where不能对聚合函数进行判断，而having可以。

**示例代码**

```sql
-- 1.根据性别分组，统计男性员工和女性员工的数量
select gender,count(*) from emp group by gender;
-- 2.根据性别分组，统计男性员工和女性员工的平均年龄
select gender,avg(age) from emp group by gender;
-- 3.查询年龄小于45的员工 ，并根据工作地址分组，获取员工数量大于等于3的工作地址
select workaddress,count(*) from emp where age <45 group by workaddress having count(*) >= 3
```
注意
- 执行顺序: where >聚合函数>having 。
- 分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义。

## 排序查询
`select 字段列表 from 表名 order by 字段1 排序方式1, 字段2 排序方式2;`
`order by`

排序方式:
- ASC:升序(默认值)
- DESC:降序

`注意:如果是多字段排序，当第一个字段值相同时，才会根据第二个字段进行排序`

**示例代码**

```sql
-- 1.根据年龄对公司的员工进行升序排序和降序排序
select * from emp order by age;  -- 升序
select * from emp order by age asc; -- 升序
select * from emp order by age desc; -- 降序
-- 2.根据入职时间，对员工进行降序排序
select * from emp order by entrydate desc;
-- 3.根据年龄对公司的员工进行升序排序，年龄相同，再按照入职时间进行降序排序
select * from emp order by age asc , entrydate desc;
```
## 分页查询
`select 字段列表 from 表名 limit 起始索引,查询记录数`
`limit`

**注意:**
- 起始索引从0开始，起始索引=(查询页码 - 1) * 每页显示记录数。
- 分页查询是数据库的方言，不同的数据库有不同的实现，MySQL中是LIMIT。
- 如果查询的是第一页数据，起始索引可以省略，直接简写为limit 10。
- 一般格式为:limit (页码-1) * 展示记录数 , 展示记录数 

**示例代码**

```sql
-- 1.查询第1页员工数据，每页展示10条记录
select * from emp limit 0,10;
select * from emp limit 10;
-- 2.查询第2页员工数据，每页展示10条记录
select * from emp limit 10,10;
```
## DQL查询小练习
```sql
-- 1.查询年龄为20,21,22,23岁的员工信息。
select * from emp where gender = '女' and age in(20,21,22,23);
-- 2.查询性别为 男 ，并且年龄在 20-40 岁(含)以内的姓名为三个字的员工。
select * from emp where gender = '男' and (age between 20 and 40) and name like '___';
-- 3.统计员工表中,年龄小于60岁的，男性员工和女性员工的人数。
select gender, count(*) from emp where age < 60 group by gender;
-- 4.查询所有年龄小于等于35岁员工的姓名和年龄，并对查询结果按年龄升序排序，如果年龄相同按入职时间降序排序。
select name,age from emp where age <= 35 order by age asc , entrydate desc;
-- 5.查询性别为男，且年龄在20-40 岁(含)以内的前5个员工信息，对查询的结果按年龄升序排序，年龄相同按入职时间升序排序。
select * from emp where gender = '男' and (age between 20 and 40) order by age asc,entrydate asc limit 5;
```
# MySQL函数
## 字符串函数
|函数| 功能 |
|--|--|
| concat(S1,S2,...Sn) | 字符串拼接,将S1,S2,...,Sn拼接成一个字符串 |
| lower(str) | 将字符串str全部转为小写 |
| upper(str) | 将字符串str全部转为大写 |
| lpad(str,n,pad) | 左填充,用字符串pad对str的左边进行填充,达到n个字符串长度 |
| rpad(str,n,pad) | 右填充,用字符串pad对str的右边进行填充,达到n个字符串长度 |
| trim(str) | 去掉字符串头部和尾部的空格 |
| substring(str,start,len) | 返回字符串str从start位置起的len个长度的字符串 |

**函数演示**
```sql
-- concat
select concat('hello','mysql');

-- lower
select lower('Hello');

-- upper
select upper('Hello');

-- lpad
select lpad('01',5,'-')

-- rpad
select rpad('01',5,'-')

-- trim
select trim(' Hello Mysql ')

-- substring
select substring('Hello Mysql',1,5)
```
**示例代码**

```sql
-- 由于业务限业交更、金业的工的工号，统一为5位数、目藏不是5位数的金部在前围补0。比如:1号的工的工号应该为00001.
update emp set workno = lpad(workno,5,'0')
```
## 数值函数
| 函数 | 功能 |
|--|--|
| ceil(x) | 向上取整 |
| floor(x) | 向下取整 |
| mod(x,y) | 返回x/y的余数 |
| rand() | 返回0-1内的随机数 |
| round(x,y) | 求参数x的四舍五入的值,保留y位小数 |

**函数演示**
```sql
-- ceil
select ceil(1.1);

-- floor
select floor(1.9);

-- mod
select mod(7,4);

-- rand
select rand();

-- round
select round(2.3456,2)
```
**示例代码**

```sql
-- 通过数据库函数实现一个六位数随机验证码
select lpad(round(rand()*1000000,0),6,'0')
```

## 日期函数
| 函数                               | 功能                                                |
| ---------------------------------- | --------------------------------------------------- |
| curdate()                          | 返回当前日期 (年月日)                                       |
| curtime()                          | 返回当前时间 (时分秒)                                       |
| now()                              | 返回当前日期和时间                                  |
| year(date)                         | 获取指定 date 的年份                                |
| month(date)                        | 获取指定 date 的月份                                |
| day(date)                          | 获取指定 date 的日期                                |
| date_add(date, interval expr type) | 返回一个日期/时间值加上一个时间间隔 expr 后的时间值 |
| datediff(date1, date2)             | 返回起始时间 date1 和结束时间 date2 之间的天数      |

**函数演示**
```sql
-- curdate
select curdate();

-- curtime
select curtime();

-- now
select now();

-- year
select year(now())

-- month
select month(now())

-- day
select day(now())

-- date_add
select date_add(now() , interval 10 day) -- 未来的第10天
select date_add(now() , interval 70 month) -- 未来的第70周

-- datediff
select datediff('2021-12-01','2021-11-01') -- 两个时间段的差值(第一个时间减去第二个时间)
```
**示例代码**
```sql
-- 查询所有员工的入职天数，并根据入职天数倒序排序
select name,datediff(curdate(),entrydate) as 'dayCount' from emp order by dayCount desc;  
```
## 流程函数
| 函数                                                               | 功能                                                            |
| ------------------------------------------------------------------ | --------------------------------------------------------------- |
| if(value, t, f)                                                    | 如果 value 为 true，则返回 t，否则返回 f                        |
| ifnull(value1, value2)                                             | 如果 value1 不为空，返回 value1，否则返回 value2                |
| case when [ val1 ] then [ res1 ] ... else [ default ] end          | 如果 val1 为 true，返回 res1，... 否则返回 default 默认值       |
| case [ expr ] when [ val1 ] then [ res1 ] ... else [ default ] end | 如果 expr 的值等于 val1，返回 res1，... 否则返回 default 默认值 |

**函数演示**

```sql
-- if
select if(true,'ok','Error')

-- ifnull
select ifnull('ok','Error')
select ifnull('','Error')
select ifnull(null,'Error')

-- case when then else end
-- 需求:查询emp表的员工姓名和工作地址(北京/上海---->一线城市，其他---->二线城市)
select name,(case workaddress when '北京' then '一线城市' when '上海' then '一线城市' else '二线城市' end) as '工作地址' from emp;
```
**示例代码**
**新增一张表**
```sql
create table score(
	id int comment 'ID',
	name varchar(20) comment '姓名',
	math int comment '数学',
	english int comment '英语',
	chinese int comment '语文'
) comment '学员成绩表';

insert into score(id,name,math,english,chinese) values (1,'Tom',67,88,95),(2,'Rose',23,66,90),(3,'Jack',56,98,76);
```
```sql
-- 统计班级各个学员的成绩，展示的规则如下:>= 85，展示优秀>=60，展示及格否则，展示不及格
SELECT
	id,
	name,
	(case when math >= 85 then '优秀' when math >= 60 then '及格' else '不及格' end) '数学',
	(case when english >= 85 then '优秀' when english >= 60 then '及格' else '不及格' end) '英语',
	(case when chinese >= 85 then '优秀' when chinese >= 60 then '及格' else '不及格' end) '语文'
FROM
	score
```