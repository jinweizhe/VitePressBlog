# 一对多SQL表创建
![在这里插入图片描述](https://vite-press.oss-cn-beijing.aliyuncs.com/database/20fcb51ab402452281e33aee07938d6b.png)
```sql
-- 创建部门表(父表)
create table dept(
	id int auto_increment comment 'ID' primary key,
	name varchar(50) not null comment '部门名称'
) comment '部门表';

-- 给部门表插入数据
insert into dept (name) values ('研发部'),('市场部'),('财务部'),('销售部'),('总经办');


-- 创建员工表(子表)
create table emp(
	id int auto_increment comment 'ID' primary key,
	name varchar(50) not null comment '姓名',
	age int comment '年龄',
	job varchar(20) comment '职位',
	salary int comment '薪资',
	entrydate date comment '入职时间',
	managerid int comment '直属领导ID',
	dept_id int comment '部门ID',
	constraint fk_emp_dept_id foreign key (dept_id) references dept(id) 
)comment '员工表';

-- 向员工表添加数据
insert into emp (name,age,job,salary,entrydate,managerid,dept_id) values 
('金庸',66,'总裁',20000,'2000-01-01',null,5),
('张无忌',20,'项目经理',12500,'2005-12-05',1,1),
('杨逍',33,'开发',8400,'2000-11-03',2,1),
('韦一笑',48,'开发',11000,'2002-02-05',2,1),
('常遇春',43,'开发',10500,'2004-09-07',3,1),
('小昭',19,'程序员鼓励师',6600,'2004-10-12',2,1),
('灭绝',60,'财务总监',8500,'2002-09-07',1,3),
('周芷若',19,'会计',48000,'2006-06-02',7,3),
('丁敏君',23,'出纳',5250,'2009-05-13',7,3),
('赵敏',20,'市场部总监',12500,'2004-10-12',1,2),
('鹿杖客',56,'职员',3750,'2006-10-03',10,2),
('鹤笔翁',19,'职员',3750,'2007-05-09',10,2),
('方东白',19,'职员',5500,'2009-02-12',10,2),
('张三丰',88,'销售总监',14000,'2004-10-12',1,4),
('俞莲舟',38,'销售',4600,'2004-10-12',14,4),
('宋远桥',40,'销售',4600,'2004-10-12',14,4),
('陈友谅',42,null,2000,'2011-10-12',1,null);
```
# 多对多SQL表创建
![在这里插入图片描述](https://vite-press.oss-cn-beijing.aliyuncs.com/database/d3fa10b1603d448b8abd52ff494f2f55.png)
```sql
create table student(
    id   int auto_increment comment '主键ID' primary key,
    name varchar(10) null comment '姓名',
    no   varchar(10) null comment '学号'
)comment '学生表';

INSERT INTO student (name, no) VALUES ('黛绮丝', '2000100101');
INSERT INTO student (name, no) VALUES ('谢逊', '2000100102');
INSERT INTO student (name, no) VALUES ('殷天正', '2000100103');
INSERT INTO student (name, no) VALUES ('韦一笑', '2000100104');



create table course(
    id int auto_increment comment '主键ID' primary key,
    name varchar(10) null comment '课程名称'
)comment '课程表';


INSERT INTO course (name) VALUES ('Java');
INSERT INTO course (name) VALUES ('PHP');
INSERT INTO course (name) VALUES ('MySQL');
INSERT INTO course (name) VALUES ('Hadoop');




create table student_course(
    id int auto_increment comment '主键' primary key,
    studentid int not null comment '学生ID',
    courseid  int not null comment '课程ID',
    constraint fk_courseid foreign key (courseid) references course (id),
    constraint fk_studentid foreign key (studentid) references student (id)
)comment '学生课程中间表';

INSERT INTO student_course (studentid, courseid) VALUES (1, 1);
INSERT INTO student_course (studentid, courseid) VALUES (1, 2);
INSERT INTO student_course (studentid, courseid) VALUES (1, 3);
INSERT INTO student_course (studentid, courseid) VALUES (2, 2);
INSERT INTO student_course (studentid, courseid) VALUES (2, 3);
INSERT INTO student_course (studentid, courseid) VALUES (3, 4);
```
# 一对一SQL表创建
![在这里插入图片描述](https://vite-press.oss-cn-beijing.aliyuncs.com/database/d41c176713b644848062df1a5552a22a.png)
```sql
-- 创建用户基本信息表
create table tb_user(
	id int auto_increment primary key comment '主键ID',
	name varchar(10) comment '姓名',
	age int comment '年龄',
	gender char(1) comment '1:男 2:女',
	phone char(11) comment '手机号'
)comment '用户基本信息表';

-- 给用户基本信息表添加数据
insert into tb_user(name,age,gender,phone) values ('黄渤',45,'1','18800001111'),('冰冰',35,'2','18800002222'),('码云',55,'1','18800008888'),('李彦宏',50,'1','18800009999');

-- 创建用户教育信息表
create table tb_user_edu(
	id int auto_increment primary key comment '主键ID',
	degree varchar(20) comment '学历',
	major varchar(50) comment '专业',
	primaryschool varchar(50) comment '小学',
	middleschool varchar(50) comment '中学',
	university varchar(50) comment '大学',
	userid int unique comment '用户ID',
	constraint fk_userid foreign key (userid) references tb_user(id)
)comment '用户教育信息表';

-- 给用户教育信息表添加数据
insert into tb_user_edu(degree,major,primaryschool,middleschool,university,userid) values ('本科','舞蹈','静安区第一小学','静安区第一中学','北京舞蹈学院',1),('硕士','表演','朝阳区第一小学','朝阳区第一中学','北京电影学院',2),('本科','英语','杭州市第一小学','杭州市第一中学','杭州师范大学',3),('本科','应用数学','阳泉区第一小学','阳泉区第一中学','清华大学',4);
```
# 多表查询
```sql
-- 使用上面的一对多的表进行操作

-- 查询所有员工信息和相关部门
select * from emp,dept where emp.dept_id = dept.id
```
## 连接查询
![在这里插入图片描述](https://vite-press.oss-cn-beijing.aliyuncs.com/database/02ee23f096c24d5ea9203a4b8cfdd0a0.png)
### 内连接
- 相当于查询A和B的交集的部分(`C`部分)
#### 隐式内连接
```sql
-- 语法
select 字段列表 from 表1,表2,... where 连接条件...;

-- 案例(使用上面的一对多的表进行操作)
-- 查询每一个员工的姓名,以及关联的部门的名称(隐式内连接实现)
select emp.name,dept.name from emp,dept where emp.dept_id = dept.id
```
#### 显式内连接
```sql
-- 语法
select 字段列表 from 表1 [inner] join 表2 on 连接条件...;

-- 案例(使用上面的一对多的表进行操作)
-- 查询每一个员工的姓名,以及关联的部门的名称(显式内连接实现)
select emp.name,dept.name from emp inner join dept on emp.dept_id = dept.id
```
#### 起别名
```sql
-- 当表名比较长且繁琐时,使用表的别名去查询,具体如下
-- 上面的隐式内连接可以改为如下所示
select e.name,d.name from emp e,dept d where e.dept_id = d.id

-- 上面的显式内连接可以改为如下所示
select e.name,d.name from emp e inner join dept d on e.dept_id = d.id
```
### 外连接
#### 左外连接
- 查询左表所有数据，以及两张表交集部分数据(`A+C`部分)

```sql
-- 语法
-- 相当于查询表1(左表)的所有数据 包含 表1和表2交集部分的数据
select 字段列表 from 表1 left [outer] join 表2 on 连接条件...;

-- 案例(使用上面的一对多的表进行操作)
-- 查询emp表的所有数据，和对应的部门信息(左外连接)
select e.*,d.name from emp e left outer join dept d on e.dept_id = d.id
```
#### 右外连接
- 查询右表所有数据，以及两张表交集部分数据(`C+B`部分)

```sql
-- 语法
-- 相当于查询表2(右表)的所有数据 包含 表1和表2交集部分的数据
select 字段列表 from 表1 right [outer] join 表2 on 连接条件...;

-- 案例(使用上面的一对多的表进行操作)
-- 查询dept表的所有数据，和对应的员工信息(右外连接)
select e.*,d.* from emp e right outer join dept d on e.dept_id = d.id
```
#### 自连接
- 当前表与自身的连接查询，自连接必须使用表别名

```sql
-- 语法
select 字段列表 from 表1 别名1 join 表1 别名2 on 连接条件...;

-- 案例(使用上面的一对多的表进行操作)
-- 查询员工以及所属领导的名字
select a.name,b.name from emp a,emp b where a.managerid = b.id

-- 查询所有员工及其领导的名字,如果员工没有领导，也需要查询出来
select a.name,b.name from emp a left join emp b on a.managerid = b.id
```
## 联合查询(union)
涉及`union`和`union all`两个关键字

- `union`会对返回的结果进行去重处理
- `union all`会返回查询到的所有结果,不会进行去重处理(相当于把多次查询到的结果叠加到一起展示了)

**对于union查询，就是把多次查询的结果合并起来，形成一个新的查询结果集。**

```sql
-- 语法
select 字段列表 from 表1... union [all] select 字段列表 from 表2...;

-- 案例(使用上面的一对多的表进行操作)
-- 将薪资低于5000的员工，和年龄大于50岁的员工全部查询出来,
-- 先查询薪资低于5000的员工
select * from emp where salary < 5000
-- 再查询年龄大于50岁的员工
select * from emp where age > 50
-- 最终进行一个联合查询
select * from emp where salary < 5000 union all select * from emp where age > 50 -- 这样写的话两次查询的结果有重复的话不会去重
select * from emp where salary < 5000 union select * from emp where age > 50 -- 这样写的话两次查询的结果有重复的话会去重
```
`对于联合查询的多张表的列数必须保持一致，字段类型也需要保持一致。`
## 子查询
`概念:SQL语句中嵌套SELECT语句，称为嵌套查询，又称子查询`

**格式如下**
```sql
select * from 表1 where column1 = (select column1 from 表2)
```
`子查询外部的语句可以是INSERT/UPDATE/DELETE/SELECT的任何一个，`
`根据子查询位置，分为:WHERE之后、FROM之后、SELECT之后。`
### 标量子查询(子查询结果为单个值(数字,字符串,日期))
常用操作符`=、<>、>、>=、< <=`
```sql
-- 案例(使用上面的一对多的表进行操作)
-- 查询“研发部”的所有员工信息
-- select id from dept where name = '研发部'  查询结果为1
select * from emp where dept_id = (select id from dept where name = '研发部')

-- 查询在“常遇春”入职之后的员工信息
select * from emp where entrydate > (select entrydate from emp where name = '常遇春')
```

### 列子查询(子查询结果为一列,可以是多行)
常用操作符`IN、NOTIN、ANY、SOME、ALL`
|操作符| 描述 |
|--|--|
| IN  | 在指定的集合范围之内，多选一 |
| NOT IN|不在指定的集合范围之内  |
| ANY  | 子查询返回列表中，有任意一个满足即可  |
| SOME  | 与ANY等同，使用SOME的地方都可以使用ANY |
| ALL | 子查询返回列表的所有值都必须满足 |
```sql
-- 案例(使用上面的一对多的表进行操作)
-- 查询“研发部”和“总经办”的所有员工信息
select * from emp where dept_id in (select id from dept where name = '研发部' or name = '总经办')

-- 查询比财务部所有人工资都高的员工信息
select * from emp where salary > all (select salary from emp where dept_id = (select id from dept where name = '财务部'))

-- 查询比研发部其中任意一人工资高的员工信息
select * from emp where salary > any (select salary from emp where dept_id = (select id from dept where name = '研发部'))
```
### 行子查询(子查询结果为一行,可以是多列)
常用操作符`=、<> 、IN、NOT IN`
```sql
-- 案例(使用上面的一对多的表进行操作)
-- 查询与“张无忌”的薪资及直属领导相同的员工信息
select * from emp where (salary,managerid) = (select salary,managerid from emp where name = '张无忌')
```
### 表子查询(子查询结果为多行多列)
常用操作符`IN`

```sql
-- 案例(使用上面的一对多的表进行操作)
-- 查询与“张无忌”，“韦一笑”的职位和薪资相同的员工信息
select * from emp where (job,salary) in (select job,salary from emp where name ='张无忌' or name = '韦一笑')
-- 查询入职日期是“2003-01-01”之后的员工信息，及其部门信息
select e.*,d.* from (select * from emp where entrydate > '2003-01-01') e left join dept d on e.dept_id = d.id;
```
# 案例
## 新增一张表
```sql
create table salgrade(
	grade int, -- 等级
	losal int, -- 工资范围初始值
	hisal int  -- 工资范围结束值
) comment '薪资等级表';

insert into salgrade values (1,0,3000),(2,3001,5000),(3,5001,8000),(4,8001,10000),(5,10001,15000),(6,15001,20000),(7,20001,25000),(8,25001,30000)
```

**下面与上面的一对多的两张表联合使用**
1. 查询员工的姓名、年龄、职位、部门信息(隐式内连接)
```sql
-- 表:emp , dept
-- 连接条件: emp.dept_id = dept.id
select e.name,e.age,e.job from emp e , dept d where e.dept_id = d.id
```
2. 查询年龄小于30岁的员工姓名、年龄、职位、部门信息。(显式内连接)
```sql
-- 表:emp , dept
-- 连接条件: emp.dept_id = dept.id
select e.name,e.age,e.job from emp e inner join dept d on e.dept_id = d.id where e.age < 30
```
3. 查询拥有员工的部门ID、部门名称。(内连接)
```sql
-- 表: emp, dept
-- 连接条件: emp.dept_id = dept.id
select distinct d.id,d.name from emp e,dept d where e.dept_id = d.id
```
4. 查询所有年龄大于40岁的员工,及其归属的部门名称; 如果员工没有分配部门,也需要展示出来(左外连接)
```sql
-- 表: emp , dept
-- 连接条件: emp.dept_id = dept.id
select * from emp e left join dept d on e.dept_id = d.id where e.age > 40
```
5. 查询所有员工的工资等级。
```sql
-- 表: emp , salgrade
-- 连接条件: emp.salary >= salgrade.losal and emp.salary <= salgrade.hisal
select e.*,s.grade,s.losal,s.hisal from emp e , salgrade s where e.salary >= s.losal and e.salary <= s.hisal

-- 第二种写法,使用between...and...
select e.*,s.grade,s.losal,s.hisal from emp e , salgrade s where e.salary between s.losal and  s.hisal
```
6. 查询"研发部"所有员工的信息及工资等级。
```sql
-- 表: emp , salgrade
-- 连接条件: e.salary between salgrade.losal and  salgrade.hisal  emp.dept_id = dept.id
-- 查询条件: dept.name = '研发部'
select e.*,s.grade from emp e,dept d,salgrade s where e.dept_id = d.id and (e.salary between s.losal and s.hisal) and d.name='研发部'
```
7. 查询"研发部"员工的平均工资。
```sql
-- 表: emp , dept
-- 连接条件: emp.dept_id = dept.id
select avg(e.salary) from emp e , dept d where e.dept_id = d.id and d.name = '研发部'
```
8. 查询工资比"灭绝"高的员工信息。
```sql
select * from emp where salary > (select salary from emp where name = '灭绝')
```
9. 查询比平均薪资高的员工信息。
```sql
select * from emp where salary > (select avg(salary) from emp)
```
10. 查询低于本部门平均工资的员工信息。
```sql
select *,(select avg(e1.salary) from emp e1 where e1.dept_id = e2.dept_id) '平均薪资' from emp e2 where e2.salary < (select avg(e1.salary) from emp e1 where e1.dept_id = e2.dept_id)
```
11. 查询所有的部门信息,并统计部门的员工人数。
```sql
select id ,name, (select count(*) from emp e WHERE e.dept_id = d.id) '人数' from dept d;
```
**下面与上面的一对一的两张表联合使用**
12. 查询所有学生的选课情况,展示出学生名称,学号,课程名称
```sql
-- 表:student,course,student_course
-- 连接条件:student.id= student_course.studentid ,course.id = student_course.courseid
select s.name,s.no,c.name from student s,student_course sc,course c where s.id = sc.studentid and sc.courseid = c.id
```