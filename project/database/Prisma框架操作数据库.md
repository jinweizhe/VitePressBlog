# 初始化项目

[中文网站](https://prisma.yoga/)

**点击快速开始,点击创建sql项目,后面一步一步往后走**

[这个博主也挺全的,推荐下](https://blog.csdn.net/deepbodhi/article/details/135676745?ops_request_misc=&request_id=&biz_id=102&utm_term=prisma%E7%B1%BB%E5%9E%8B&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-135676745.142%5Ev100%5Epc_search_result_base1&spm=1018.2226.3001.4187)

[可以看这个页面初始化项目跟我下面是一样的,这里用得是ts,我下面是js,不需要额外的配置了](https://prisma.yoga/getting-started/setup-prisma/start-from-scratch/relational-databases-typescript-postgres/)

## 1.vscode打开一个空文件夹
## 2.npm init -y 初始化package.json
## 3.安装相关依赖
```js
npm install prisma
// 或者
yarn add prisma
```
继续安装
```js
yarn add @prisma/client
```
## 4.指定数据库
```js
// 如果不想安装或者配置数据环境就用下面这个sqlite,轻量级
npx prisma init --datasource-provider sqlite
// 下面这个是指定连接mysql的
npx prisma init --datasource-provider mysql
```
**这时你会发现项目目录下多了 schema 文件和 env 文件:**
## 5.env文件,内容大概如下(sqlite数据库可以跳过这一步)
**这个文件里面存的就是连接信息**

```sql
# Environment variables declared in this file are automatically made available to Prisma.
# See the documentation for more detail: https://pris.ly/d/prisma-schema#accessing-environment-variables-from-the-schema

# Prisma supports the native connection string format for PostgreSQL, MySQL, SQLite, SQL Server, MongoDB and CockroachDB.
# See the documentation for all the connection string options: https://pris.ly/d/connection-strings

DATABASE_URL="mysql://root:admin@localhost:3306/mydb"

# DATABASE_URL="SqlName://USER:PASSWORD@HOST:PORT/DATABASE?schema=SCHEMA"
# SqlName: 使用的数据库类型
# USER: 你的数据库用户名
# PASSWORD: 数据库用户的密码
# PORT: 数据库服务器运行的端口（通常5432用于 PostgreSQL）
# DATABASE: 数据库名称
# SCHEMA: 数据库中schema的名称(这个可以固定写死,可以忽略)
```
## 6.在schema文件夹下面的.schema文件内新增模型(数据库的表)
**先测试下有没有连接数据库**
执行`npx prisma db pull`
- 然后数据库如果存在的话,并且里面还有表的话,那么表的创建集合的语句就会在.schema文件内被创建出来

**如果.schema文件代码没有高亮显示的话,去插件安装一下Prisma这个插件,安装完成就有代码高亮效果了**
```js
// This is your Prisma schema file,
// learn more about it in the docs: https://pris.ly/d/prisma-schema

// Looking for ways to speed up your queries, or scale easily with your serverless or edge functions?
// Try Prisma Accelerate: https://pris.ly/cli/accelerate-init

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "mysql"
  url      = env("DATABASE_URL")
}

model Post {
  id        Int      @id @default(autoincrement())
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  title     String   @db.VarChar(255)
  content   String?
  published Boolean  @default(false)
  author    User     @relation(fields: [authorId], references: [id])
  authorId  Int
}

model Profile {
  id     Int     @id @default(autoincrement())
  bio    String?
  user   User    @relation(fields: [userId], references: [id])
  userId Int     @unique @map("user_id")
  @@map("profile ")
}

model User {
  id      Int      @id @default(autoincrement())
  email   String   @unique
  name    String?
  posts   Post[]
  profile Profile?
}
```
- @id 是主键
- @default(autoincrement()) 是指定默认值是自增的数字
- @unique 是添加唯一约束
- @relation 是指多对一的关联关系,通过authorld关联User的id
- ? 指当前字段不是必填项
- @default() 设置默认值 
- @map("") 给字段起别名 
- @@map("profile ") 表的别名
- @db.XXX 指定具体的数据类型,以mysql为例db.VarChar(255) 打点的时候vscode会提示关于mysql的相关数据类型,使用db.XXX相当于使用mysql具体的数据类型
- @@index([字段1,字段2])  联合索引
- @@id([字段1,字段2]) 联合主键(适用于多对多关联表的中间表)
## 7.执行下面代码生成(更新)表
**推荐使用第二个db push,如果需要查看创建表的sql语句推荐第一个**
**都是没有表会创建表,有表则会同步数据**
```js
// 后面的name值随便写(这个命令会生成建表结构,在prisma/migrations/文件夹/里面)
// 还会生成client代码
npx prisma migrate dev --name xiaoji

// 或者
npx prisma db push  // 无sql文件产生
```

## 8.在node_modules/.prisma/client/index.js找到相关信息
**如果文件内包含我们刚刚创建的数据库,然后就可以用 @prisma/client 来做 CRUD 了。**
```js
exports.Prisma.ModelName = {
  Post: 'Post',
  Profile: 'Profile',
  User: 'User'
};
```
![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/96c76c63f0b548dca3da187600b7b1ca.png)
# 快速入门ORM框架Peisma并使用CRUD小试一下
## 单张表添加数据
根目录下创建`src/index.js`内容如下:
```js
import { PrismaClient } from "@prisma/client";
// const prisma = new PrismaClient(); // 不会打印sql语句
const prisma = new PrismaClient({
    log: [{ emit: "stdout", level: "query" }], // 可以打印sql语句
  });
  
async function test1(){
    // 在user表新增一条数据
    await prisma.user.create({
        data:{
            name:"xiaoji",
            email:"111@qq.com"
        }
    })

    // 在user表再新增一条数据
    await prisma.user.create({
        data:{
            name:"sengren",
            email:"222@qq.com"
        }
    })

   // 将数据查询出来
   const users = await prisma.user.findMany();
   console.log('users',users);
}
test1()
```
**下载安装插件**
![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/e37f222fad544298aebdb935c3f1a2c2.png)

在当前index.js文件内直接右键->run Code->查看控制台

打印结果为:
```js
users [
  { id: 1, email: '111@qq.com', name: 'xiaoji' },
  { id: 2, email: '222@qq.com', name: 'sengren' }
]
```
**数据库结果为:**
![**在这里插入图片描述**](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/186256502e1c4cbf865c63dc22bb6a3d.png)
## 一对多添加数据 
接下来再来插入新的user数据和它的两个post(表关联的数据)
**新建js文件或者把刚刚的文件替换下内容,内容如下:**
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient({
  log: [{ emit: "stdout", level: "query" }], // 可以打印sql语句
});

async function test1() {
  // 在user表新增一条数据
  const user = await prisma.user.create({
    data: {
      name: "hahaha",
      email: "333@qq.com",
      posts:{
        create:[
            {
                title:"aaa",
                content:"aaaaa"
            },{
                title:"bbb",
                content:"bbbbb"
            }
        ]
      }
    },
  });
  console.log("users", user);
}
test1();
```
**右键->runCode运行**

![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/363ee940df9741c0a0c498b8acfe2dc8.png)
**如果报错import错误,则在package.json里面新增一个属性,具体如下**
```js
{
  "name": "prisma",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "type": "module",  // 新增(将js文件模块化,就可以正常使用import了)
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@prisma/client": "^5.15.0",
    "prisma": "^5.15.0"
  }
}
```
**然后重新右键runCode即可**

查看user表数据

![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/4760069c1d434ad7b678cbbc003d8d78.png)

查看post表

![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/3c6fb76d170e476a9d870530d615825a.png)
## 单表更新
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient({
    log: [{ emit: "stdout", level: "query" }], // 可以打印sql语句
  });
  
async function test1(){
    // 更新post表的id字段为3的数据的content为nihao
    await prisma.post.update({
        where:{
            id:3
        },
        data:{
            content:"nihao"
        }
    })
}
test1()
```
**效果图**
![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/9cfc4d8df8b14e6497e76fdd06739775.png)
## 单表删除
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient({
  log: [{ emit: "stdout", level: "query" }], // 可以打印sql语句
});

async function test1() {
    // 删除post表id为3的数据
    await prisma.post.delete({
        where:{
            id:3
        }
    })
}
test1();
```
# 生成对应的模型文档(html页面)
## 安装
```js
yarn add prisma-docs-generator
```
## 配置
在prisma/schema.prisma新增一条
```js
generator docs {
  provider = "node node_modules/prisma-docs-generator"
}
```
## 更新配置
```js
npx prisma generate
```
**然后prisma下面就新增了一个docs文件夹**

![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/b1aed554760e4be99c6f9ada86fc53cf.png)
## 运行index.html
**看到的页面如下所示**

![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/ceacb2f0debf4575862bae5eb6cec58a.png)
**生成这个文档对于做项目查询相关的crud操作非常方便**

# 一对一和一对多和多对多关系的表创建
## 一对多的表创建
```js
// 部门 一的一方
model Department {
  id Int @id @default(autoincrement())
  name String @db.VarChar(20)
  createTime DateTime @default(now()) // @default(now()) 插入数据自动填入当前时间
  updateTime DateTime @updatedAt // 更新时间使用@updatedAt 会自动设置当前时间
  employees Emplyee[] // 员工表
}

// 员工 多的一方
model Emplyee {
  id Int @id @default(autoincrement())
  name String @db.VarChar(20)
  phone String @db.VarChar(30)
  deaprtmentId Int
  department Department @relation(fields: [deaprtmentId],references: [id])
}
// 员工和部门是多对一的关系,在员工的一侧加deaprtmentId字段跟部门表的id字段进行关联
// 通过@relation声明deaprtmentId的列引用department的id列
```
**运行下面命令生成表**
```js
npx prisma db push
```
可以查看可视化工具发现外键已经关联上了

![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/dbe5b1dad054459084603d34429d9cb9.png)
## 多对多的表的创建
```js
// Post表
model Post {
  id Int @id @default(autoincrement())
  title String
  content String?
  published Boolean @default(false)
  tags TagOnPosts[]
}

// 文章表
model Tag {
  id Int @id @default(autoincrement())
  name String
  posts TagOnPosts[]
}

// 中间表
model TagOnPosts {
  // 关联posts表id
  postId Int
  post Post @relation(fields: [postId],references: [id])

  // 关联文章id
  tagId Int
  tag Tag @relation(fields: [tagId],references: [id])

  // 联合主键
  @@id([postId,tagId])
}
```
**运行下面命令生成表**
```js
npx prisma db push
```
可以查看可视化工具发现外键已经关联上了
![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/9b47b573f3a443c588e602ef3e50af3e.png)
## 一对一的表的创建
**跟一对多代码一样,将多的一方的字段加个@unique即可,也就是将多的一方的数据改成了唯一值,这样就变成了一对一的表**
```js
model Department {
  id Int @id @default(autoincrement())
  name String @db.VarChar(20)
  createTime DateTime @default(now()) 
  updateTime DateTime @updatedAt 
  employees Emplyee[] 
}

model Emplyee {
  id Int @id @default(autoincrement())
  name String @db.VarChar(20)
  phone String @db.VarChar(30)
  deaprtmentId Int @unique  // 这里我还拿之前的一对多的代码,这员工这个关联字段这里加个唯一值,这样就是一对一表的建立
  department Department @relation(fields: [deaprtmentId],references: [id])
}
```
## 枚举类型
```js
model AAA {
  id Int @id @default(autoincrement())
  name String?
  role EEE @default(CCC)
}

enum EEE {
  BBB
  CCC
  DDD
}
```
**运行下面命令生成表**
```js
npx prisma db push
```
可以查看可视化工具发现枚举已经生成了

![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/0ee2ac45dfab40ddae78cd4a1446f9cc.png)
# 单表的CRUD全部示例
**安装文档插件**
```js
yarn add prisma-docs-generator
```
## 数据模型如下
```js
// This is your Prisma schema file,
// learn more about it in the docs: https://pris.ly/d/prisma-schema

// Looking for ways to speed up your queries, or scale easily with your serverless or edge functions?
// Try Prisma Accelerate: https://pris.ly/cli/accelerate-init

generator client {
  provider = "prisma-client-js"
}

generator docs {
  provider = "node node_modules/prisma-docs-generator"
}

datasource db {
  provider = "mysql"
  url      = env("DATABASE_URL")
}

model AAA {
  id Int @id @default(autoincrement())
  email String @unique
  name String?
}
```
**运行如下代码生成数据库**
```js
npx prisma db push
```
## 查看文档方法
右键`prisma/docs/index.html`运行,找到方法栏,这里就是单表操作所有方法了

![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/ec2538b23c254feea02f861a79e11be3.png)
- findUnique 找到唯一的数据
- findFirst 查找第一个
- findMany 查找多个(批量查找)
- create 新增数据
- delete 删除数据
- update 修改数据
- deleteMany 批量删除
- updateMany 批量修改
- upsert 创建或者更新(有数据则更新,无数据则创建)
- createMany 批量添加数据(图上丢失了)
## 创建目录
根目录下创建`src/index.js`目前代码都在这里面写
## 添加
### 批量添加(createMany)
`src/index.js`
**先做这个是为了有数据进行后续操作**
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient({
    log: [{ emit: "stdout", level: "query" }], // 可以打印sql语句
  });
  
async function test1(){
    // 在aaa表新增5条数据
    await prisma.aAA.createMany({
        data:[
            {
                name:'aaa',
                email:"aaa@xx.com"
            },
            {
                name:'bbb',
                email:"bbb@xx.com"
            },
            {
                name:'ccc',
                email:"ccc@xx.com"
            },
            {
                name:'ddd',
                email:"ddd@xx.com"
            },
            {
                name:'eee',
                email:"eee@xx.com"
            }
        ]
    })
}
test1()
```
在`src/index.js`右键runCode执行

**查看可视化工具**

![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/e5f9fc3b49624051a377a4d78cb2fb50.png)
### 单条添加(create)
`src/index.js`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient()

async function test1(){
    // 在user表新增一条数据
    await prisma.aAA.create({
        data:{
            name:"xiaoji",
            email:"111@qq.com"
        }
    })

    // 在user表再新增一条数据
    await prisma.aAA.create({
        data:{
            name:"sengren",
            email:"222@qq.com"
        }
    })

   // 将数据查询出来
   const users = await prisma.aAA.findMany();
   console.log('users',users);
}
test1()
```
**效果图**
![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/9b417046630b4191a30c995a8bf4d22a.png)

## 查找唯一值(findUnique)
`src/index.js`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient({
  log: [{ emit: "stdout", level: "query" }], // 可以打印sql语句
});

async function test1() {
  // 在aaa表查找唯一值
  // 在我们这个例子里面where条件只能查id或者email
  // 因为一个是主键自增,一个被设计了唯一值,都符合这个方法的条件
  // name没做限制,因此name不存在唯一,所以这里的where不能根据name传入条件,否则报错
  const data = await prisma.aAA.findUnique({
    where: {
      // id: 2,
      email:"aaa@xx.com"
    },
    select:{  // 设置查找到数据后返回的字段,这里设置了id为true,则查找到数据后只会返回id
        id:true,
        // email:true
    }
  });
  console.log("data", data);
}
test1();
```
**查找不到时data打印为null**

![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/d295772d3d8b42a9898863213c370673.png)
## 批量查找(findMany)
### 查询所有
`src/index.js`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient({
  log: [{ emit: "stdout", level: "query" }], // 可以打印sql语句
});

async function test1() {
  // 在aaa表批量查询
  const data = await prisma.aAA.findMany(); // 查询所有
  console.log("data", data);
}
test1();
```
**效果图**
![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/fb409cf774724a7cb908af6c154eeba4.png)
### 查询前N条记录
`src/index.js`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient({
  log: [{ emit: "stdout", level: "query" }], // 可以打印sql语句
});

async function test1() {
  // 在aaa表批量查询
  const AAA = await prisma.aAA.findMany({ take: 10 }) // 查询前十条记录
  console.log("data", AAA);
}
test1();
```
### 模糊查询和升序降序
`src/index.js`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient({
  log: [{ emit: "stdout", level: "query" }], // 可以打印sql语句
});

async function test1() {
  // 在aaa表批量查询
  // 下面的意思是查找email里面包含xx的数据,将查找到的数据根据name字段降序排序
  const AAA = await prisma.aAA.findMany({ 
    where:{
        email:{
            contains:"xx" // 模糊查询,包含
        }
    },
    orderBy:{
        name:'desc'
    }
}) 
  console.log("data", AAA);
}
test1();
```
**效果图**
![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/791b3c7f517640aaa36dc56db593dc29.png)
### where参数说明
#### where里面的参数
```js
where: {
	... // 这个...代表的是当前表的表字段(下面是where表字段里面的参数详细说明)
    AND: [
        { field1: { equals: 'value1' } },    // 条件1：field1 等于 'value1'
        { field2: { contains: 'value2' } }   // 条件2：field2 包含 'value2'
    ],
    OR: [
        { field3: { startsWith: 'value3' } },// 条件3：field3 以 'value3' 开头
        { field4: { endsWith: 'value4' } }   // 条件4：field4 以 'value4' 结尾
    ],
    NOT: {
        field5: { in: ['value5', 'value6'] } // 条件5：field5 不在 ['value5', 'value6'] 中
    }
}
```

> 使用示例:
> 假设你有一个 User 模型，你想要查询如下条件的用户：
年龄大于 30 并且名字包含 'John'，或者
邮箱以 'example.com' 结尾，但
用户名不包含 'admin'。

**示例代码**
```js
import { PrismaClient } from '@prisma/client';
const prisma = new PrismaClient({
  log: [{ emit: 'stdout', level: 'query' }],
});

async function testQuery() {
  const users = await prisma.user.findMany({
    where: {
      AND: [
        {
          age: { gt: 30 }, // 年龄大于 30
        },
        {
          name: { contains: 'John' }, // 名字包含 'John'
        }
      ],
      OR: [
        {
          email: { endsWith: 'example.com' }, // 邮箱以 'example.com' 结尾
        }
      ],
      NOT: {
        username: { contains: 'admin' } // 用户名不包含 'admin'
      }
    }
  });
  console.log('Users:', users);
}
testQuery();
```
#### where里面表字段的参数
```js
where: {
    name: {
        contains: '',    // 字符串包含，匹配 name 中包含指定值的记录
        endsWith: '',    // 以指定字符串结尾，匹配 name 以给定值结尾的记录
        equals: '',      // 精确匹配，匹配 name 完全等于指定值的记录
        gt: '',          // 大于，匹配 name 大于指定值的记录
        gte: '',         // 大于或等于，匹配 name 大于或等于指定值的记录
        in: [''],        // 包含在数组中，匹配 name 在给定数组中的记录
        lt: '',          // 小于，匹配 name 小于指定值的记录
        lte: '',         // 小于或等于，匹配 name 小于或等于指定值的记录
        not: '',         // 不等于，匹配 name 不等于指定值的记录
        notIn: [''],     // 不包含在数组中，匹配 name 不在给定数组中的记录
        startsWith: ''   // 以指定字符串开头，匹配 name 以给定值开头的记录
    }
}
```
**示例代码**
```js
const results = await prisma.someModel.findMany({
  where: {
    name: {
      contains: 'abc',   // 查找名称包含 'abc' 的记录
      endsWith: 'xyz',   // 查找名称以 'xyz' 结尾的记录
      equals: 'exact',   // 查找名称完全等于 'exact' 的记录
      gt: '100',         // 查找名称大于 '100' 的记录
      gte: '100',        // 查找名称大于或等于 '100' 的记录
      in: ['a', 'b'],    // 查找名称在 ['a', 'b'] 列表中的记录
      lt: 'z',           // 查找名称小于 'z' 的记录
      lte: 'z',          // 查找名称小于或等于 'z' 的记录
      not: 'abc',        // 查找名称不等于 'abc' 的记录
      notIn: ['a', 'b'], // 查找名称不在 ['a', 'b'] 列表中的记录
      startsWith: 'abc'  // 查找名称以 'abc' 开头的记录
    }
  }
});

```
### 分页查询
`src/index.js`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient({
  log: [{ emit: "stdout", level: "query" }], // 可以打印sql语句
});

async function test1() {
  // 在aaa表批量查询
  // 下面的意思是第一页,每页两条数据
  let page = 1
  let per = 2
  const AAA = await prisma.aAA.findMany({ 
    take:per * 1, // 每页数据条数(要数字类型,这里转一下)
    skip:(page - 1) * per, // 跳过多少页
}) 
  console.log("data", AAA);
}
test1();
```
**效果图**

![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/9cefe0a9f7754753b2bec9116a1f99bb.png)
### 逻辑或(OR)
`src/index.js`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient({
  log: [{ emit: "stdout", level: "query" }],
});

async function test1() {
  // 在aaa表批量查询
  // 查询name包含aaa或者bbb的数据
  const AAA = await prisma.aAA.findMany({
    where: {
      OR: [
        {
          name: {
            contains: "aaa",
          },
        },
        {
          name: {
            contains: "bbb",
          },
        },
      ],
    }
  });
  console.log("data", AAA);
}
test1();
```
**效果图**

![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/fd1d6d87fa63407482d0d371fdd66253.png)

### 查找第一个(findFirst)
`src/index.js`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient({
  log: [{ emit: "stdout", level: "query" }],
});

async function test1() {
  // 在aaa表批量查询
  // 查询第一条数据
  const AAA = await prisma.aAA.findFirst();
  console.log("data", AAA);
}
test1();
```
**效果图**

![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/cd9c24aee375412f82a0882e5fd00d11.png)
## 修改数据
### 单条修改(update)
`src/index.js`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient()

async function test1(){
    // 在user表新增一条数据
    // 修改id为1的email数据为hahaha@qq.com
    // 返回的字段只包含id和email
    let data = await prisma.aAA.update({
        where:{
            id:1
        },
        data:{
            email:"hahahha@qq.com"
        },
        select:{
            id:true,
            email:true
        }
    })

   console.log('data',data);
}
test1()
```
**效果图**
![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/d6a4935e154d450a915b6c85189d78dd.png)

### 批量修改(updateMany)
`src/index.js`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient()

async function test1(){
    // 在user表新增一条数据
    // 批量修改email包含xx.com的数据的name为hahaha
    // 返回的字段只包含id和email
    let res = await prisma.aAA.updateMany({
        where:{
            email:{
                contains:'xx.com'
            }
        },
        data:{
            name:"hahaha"
        }
    })

   console.log('res',res);
}
test1()
```
**效果图**

![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/3791ca6c2bc44fb390d2c7d715c84ca0.png)
## 更新或新增(upsert)
`src/index.js`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();

async function test1() {
  // 在user表新增或修改一条数据
  // 会先去数据库找id为11的数据,没找到则创建,找到了则修改
  let res = await prisma.aAA.upsert({
    where: {
      id: 11
    },
    update:{
        email:'11@qq.com'
    },
    create:{
        id:11,
        name:'aaa',
        email:'121@xx.com'
    }
  });
  
  console.log("res", res);
}
test1();
```
## 删除
### 单条删除(delete)
`src/index.js`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();

async function test1() {
  // 在user表删除一条数据
  // 删除id为11的数据
  // 删除不存在的数据会报错,这里建议trycatch一下
  try {
    let res = await prisma.aAA.delete({
        where: {
          id: 10
        }
      });
      
      console.log("res", res);
  } catch (error) {
    console.log('删除的数据不存在');
  }
}
test1();
```
### 批量删除(deleteMany)
`src/index.js`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();

async function test1() {
  // 在user表批量删除数据
  // 删除id为1,2,3的数据
  // 批量删除时删除不存在的数据不会报错
    let res = await prisma.aAA.deleteMany({
      where: {
        id: {
          in: [1, 2, 3],
        },
      },
    });

    console.log("res", res);
}
test1();
```
**效果图**
![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/36411b97410641de8ea12a062dd2449c.png)
## 统计数量(count)
`src/index.js`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();

async function test1() {
  // 在user表统计数据条数
    let res = await prisma.aAA.count();

    console.log("res", res);
}
test1();
```
## 统计相关(aggregate)
**由于目前表内没有数字相关的列,因此在模型新增一个数字字段,重新更新下数据表,更新后的schema.prisma文件内容为**
```js
// This is your Prisma schema file,
// learn more about it in the docs: https://pris.ly/d/prisma-schema

// Looking for ways to speed up your queries, or scale easily with your serverless or edge functions?
// Try Prisma Accelerate: https://pris.ly/cli/accelerate-init

generator client {
  provider = "prisma-client-js"
}

generator docs {
  provider = "node node_modules/prisma-docs-generator"
}

datasource db {
  provider = "mysql"
  url      = env("DATABASE_URL")
}

model AAA {
  id Int @id @default(autoincrement())
  email String @unique
  name String?
  age Int @default(0)  // 加个数字列
}
```
**执行下面命令更新数据库**
```js
npx prisma db push
```
`src/index.js`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();

async function test1() {
    // 在user表统计数据
    let res = await prisma.aAA.aggregate({
        where:{
            email:{
                contains:'xx.com'
            }
        },
        _count:{
            _all:true  // 统计满足 where 条件的所有记录数量
        },
        _max:{
            age:true   // 找出满足 where 条件的记录中，age 的最大值
        },
        _min:{
            age:true   // 找出满足 where 条件的记录中，age 的最小值
        },
        _avg:{
            age:true    // 计算满足 where 条件的记录中，age 的平均值
        },
        _sum:{
            age:true    // 计算满足 where 条件的记录中，age 的总和
        }
    });

    console.log("res", res);
}
test1();
```
**数据库信息**
![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/9aaab7fc763446da932b4fade99852ef.png)

**打印的res数据**
![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/e0f74b3baa7d4b5f988e1539a5a5e365.png)
## 分组
`src/index.js`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();

async function test1() {
    // 根据name相同的进行分组操作
    let res = await prisma.aAA.groupBy({
        by:['name'],
        _count:{
            _all:true  // 统计满足 where 条件的所有记录数量
        },
        _max:{
            age:true   // 找出满足 where 条件的记录中，age 的最大值
        },
        _min:{
            age:true   // 找出满足 where 条件的记录中，age 的最小值
        },
        _avg:{
            age:true    // 计算满足 where 条件的记录中，age 的平均值
        },
        _sum:{
            age:true    // 计算满足 where 条件的记录中，age 的总和
        }
    });

    console.log("res", res);
}
test1();
```
**效果图**

![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/538c59da8e384a74aa86140b6241e436.png)
# 多表的CRUD全部示例
## 创建数据模型
```js
// This is your Prisma schema file,
// learn more about it in the docs: https://pris.ly/d/prisma-schema

// Looking for ways to speed up your queries, or scale easily with your serverless or edge functions?
// Try Prisma Accelerate: https://pris.ly/cli/accelerate-init

generator client {
  provider = "prisma-client-js"
}

generator docs {
  provider = "node node_modules/prisma-docs-generator"
}

datasource db {
  provider = "mysql"
  url      = env("DATABASE_URL")
}

// 一的一方表
model Department {
  id Int @id @default(autoincrement())
  name String @db.VarChar(20)
  createTime DateTime @default(now())
  updateTime DateTime @updatedAt
  employees Employee[]
}
// 多的一方表
model Employee{
  id Int @id @default(autoincrement())
  name String @db.VarChar(20)
  phone String @db.VarChar(30)
  deaprtmentId Int   // 外键
  department Department @relation(fields: [deaprtmentId],references: [id])
}
```
**执行下面命令更新数据库**
```js
npx prisma db push
```
**刷新数据库,查看两张表**
![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/923262b0b42a483785258f7758aca313.png)
## 创建目录
根目录下创建`src/index.js`目前代码都在这里面写

## 添加数据
### 单条添加
`src/index.js`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();

async function test1() {
// 创建部门和员工信息
 let res = await prisma.department.create({
    data:{
        name:"技术部",
        employees:{
            create:[
                {
                    name:"员工A",
                    phone:'1'
                },{
                    name:"员工B",
                    phone:'2'
                }
            ]
        }
    }
 })
 console.log('res',res);
}
test1();

```
**同样右键->runcode即可运行**

**可视化数据库查看数据**
![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/a9d338044c474374bc3cdc19d5f51a58.png)
![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/6f2a9cba6b3a4bd6b8e9b523da5e0cf5.png)
**也可以使用createMany进行添加员工,在外面相当于又包了一层,个人觉得小题大作,create就可以用**
`下面是createMany的示例代码`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();

async function test1() {
 let res = await prisma.department.create({
    data:{
        name:"技术部",
        employees:{
            createMany:{  // 这里多包了一层
                data:[
                    {
                        name:"员工C",
                        phone:'3'
                    },{
                        name:"员工D",
                        phone:'4'
                    }
                ]
            }
        }
    }
 })
 console.log('res',res);
}
test1();
```
## 关联查询(findUnique查询唯一)
`src/index.js`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();

async function test1() {
 let res = await prisma.department.findUnique({
    where:{
        id:1  // 查询部门为1的数据
    },
    include:{
        employees:true  // 是否显示员工表数据
    }
 })
 console.log('res',res);
}
test1();
```
**显示员工表情况(默认不显示)**

![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/6955e8e95b29485797568435f5aea974.png)
**不显示员工表情况**
![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/8a8ed68f82704037b5a3a80340075956.png)
### 关联查询指定关联表条件
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();

async function test1() {
 let res = await prisma.department.findUnique({
    where:{
        id:1
    },
    include:{
        employees:{ // 关联表值为对象里面有键值对,就代表为true,会显示关联表
            where:{ // 通过where条件查询到当前部门下符合where条件的员工
                name:"员工A"
            },
            select:{ // 指定关联表显示的字段为id和name
                name:true,
                id:true
            }
        }  
    }
 })
 console.log('res',res);
}
test1();
```
**打印结果为:**

![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/49f83e80ac67420cb90402babc65cdf1.png)
### 直接取当前部门下所有的员工信息(不显示当前部门信息)
`src/index.js`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();

async function test1() {
  let res = await prisma.department
    .findUnique({
      where: {
        id: 1,
      },
    })
    .employees(); // 查询条件返回符合的部门直接在查询条件后打点员工表就行
  console.log("res", res);
}
test1();
```
**效果图**
![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/a4038fc60e3743d7a8f7a92cd50a791d.png)
## 批量查询(findMany)
`src/index.js`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();

async function test1() {
  // 查询所有的部门和对应的数据
  let res = await prisma.department.findMany({
    include: {
      employees: true,
    },
  });
  console.log("res", res);
}
test1();
```
**效果图**

![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/c882256dbd4e4f4b974a1065952b599a.png)
## 更新数据
`src/index.js`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();

async function test1() {
  // 修改id为1的部门的名称为销售部,同时新增一个员工
  // 同时连接一个老员工(将4号员工也从技术部转到销售部)
  // 同时修改4号员工的姓名
  // 并且删除3号员工(注意:如果该员工未在该部门下,删除的时候会报错,建议trycatch)
  // 查找id为100的,查找不到则创建员工数据,查找到则不做处理
  
  // 修改查找和删除也可以使用单表去操作,但是添加需要从部门添加,因为要关联部门
  let res = await prisma.department.update({
    where: {
      id: 1,
    },
    data: {
      name: "销售部",
      employees: {
        create: [
          {
            name: "新员工",
            phone: "new",
          },
        ],
        connect: {
          id: 4,
        },
        update: {
          where: {
            id: 4, // 条件：员工 ID 为 4
          },
          data: {
            name: "老员工", // 更新该员工的姓名为 "新姓名"
          },
        },
        delete: {
          id: 14, // 删除员工 ID 为 3 的记录
        },
        connectOrCreate: { // 查找id为100的,查找不到则创建员工数据,查找到则不做处理
          where: {
            id: 100,
          },
          create: {
            name: "11",
            phone: "2",
          },
        },
      },
    },
  });
  console.log("res", res);
}
test1();
```
## 删除数据
`src/index.js`
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();

async function test1() {
  // 先将部门id为1的下面所有的员工删除,再删除部门,负责部门不能被删除,会报错
  let res = await prisma.employee.deleteMany({
    where: {
      deaprtmentId: 1,
    },
  });
  // 删除部门
  let res2 = await prisma.department.delete({
    where: {
      id: 1,
    },
  });
  console.log("res", res,res2);
}
test1();
```
# 直接执行sql语句
`src/index.js`
**查询数据使用$queryRaw**
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();

async function queryData() {
  try {
    // 执行原始 SQL 查询
    // 模糊查询department表的name字段包含技的数据
    const result = await prisma.$queryRaw`
      SELECT * FROM department
      WHERE name LIKE ${'技%'}
    `;

    console.log("查询结果：", result);
  } catch (err) {
    console.error("查询失败：", err.message);
  }
}

queryData();
```
**效果图**
![在这里插入图片描述](
https://vite-press.oss-cn-beijing.aliyuncs.com/database/11bb39ef29c94a8ea8098e65972dd3d0.png)
**插入更新删除数据使用$executeRaw**
```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();

async function modifyData() {
  try {
    // 插入数据
    await prisma.$executeRaw`
      INSERT INTO employee (name, phone, deaprtmentId)
      VALUES ('新员工', '1234567890', 2)
    `;

    // 更新数据
    await prisma.$executeRaw`
      UPDATE employee
      SET phone = '98765432101'
      WHERE name = '新员工'
    `;

    // // 删除数据
    await prisma.$executeRaw`
      DELETE FROM employee
      WHERE name = '新员工'
    `;

    console.log("操作成功");
  } catch (err) {
    console.error("操作失败：", err.message);
  }
}

modifyData();
```
[参考视频](https://www.bilibili.com/video/BV12w411w7LV/?spm_id_from=333.1007.top_right_bar_window_history.content.click)