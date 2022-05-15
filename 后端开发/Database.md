# mongoDB

## 特点

- 非关系型数据库NoSql (mongodb, radis) ,对象(键值对)
- 高性能、易部署、 易使用，存储数据非常方便。
- 支持的数据结构非常松散，是**类似于json**, 可以存储比较复杂的数据类型 。
- **查询语言强大**，几乎可以实现关系型数据库单表查询的绝大部分功能，还支持对数据建立索引
- 一个MongoDB 实例可以包含一组数据库，一个DataBase 可以包含一组Collection（集合），一个集合可以包含一组Document（文档）。
- 默认端口号 ： **27017** 

## 基本模型

| SQL概念  | MongoDB概念 |
| :------- | :---------- |
| database | database    |
| table    | collection  |
| row      | document    |
| column   | field       |

1. database 数据库，一个数据库可以包含多个集合。
2. collection集合，相当于 sql中的 table, 一个collection 中包含 多组文档（行）document 
3. document 文档，相当于SQL中的行(row)，一个文档由多个字段(列)组成，并采用bson(json)格式表示。

### 其他概念 

| SQL概念     | MongoDB概念 |
| :---------- | :---------- |
| primary key | _id         |
| foreign key | reference   |
| view        | view        |
| index       | index       |
| join        | $lookup     |
| transaction | trasaction  |
| group by    | aggregation |

1. _id 主键，MongoDB 默认使用一个_id 字段来保证文档的唯一性。
2. reference 引用，勉强可以对应于 外键(foreign key) 的概念，之所以是勉强是因为 **reference 并没实现任何外键的约束**，而只是由客户端(driver)自动进行关联查询、转换的一个特殊类型。
3. view 视图，MongoDB 3.4 开始支持视图，和 SQL 的视图没有什么差异，视图是基于表/集合之上进行动态查询的一层对象，可以是虚拟的，也可以是物理的(物化视图)。
4. index 索引，与SQL 的索引相同。提升查询速度
5. $lookup，这是一个聚合操作符，可以用于实现类似 SQL-join 连接的功能
6. transaction 事务，从 MongoDB 4.0 版本开始，提供了对于事务的支持
7. aggregation 聚合，MongoDB 提供了强大的聚合计算框架，group by 是其中的一类聚合操作。

## 基本语法

> 如果真的想把这个数据库创建成功，那么必须插入一个数据。

### 数据库  表 创建删除

- 查看所有数据库列表 show dbs

- 使用数据库、创建数据库 use myshop

- 创建集合(表)：db.manager.insert({“name”:”xiaoming”});

- 显示集合中的文档内容   db.manager.find(  ) ;

- 显示当前的数据集合 show collections

- 删除数据库 db.dropDatabase();

- 删除集合 db. manager.drop()

### 数据更新

- 查询

  - 查询 age > 22 的记录          db.manager.`find`({age: {$gt: 22}});

  - 查询 age <  22 的记录         db.manager.find({age: {$lt: 22}});

  - 查询 age >= 23 并且 age <= 26     db.manager.find({age: {$gte: 23, $lte: 26}});

  - 查询 name 中包含 mongo 的数据      db.manager.find({name: /mongo/});

  - 查询指定列 name、age 数据          db.manager.find({}, {name: 1, age: 1}); 

  -  查询指定列 name、age 数据, age > 25       db.manager.find({age: {$gt: 25}}, {name: 1, age: 1}); 

  - 按照年龄排序    1 升序    -1 降序          升序：db.manager.find().sort({age: 1});

  - or 与 查询     db.manager.find({$or: [{age: 22}, {age: 25}]});

  - findOne 查询第一条数据      db.manager.findOne();

  - 分页查询  

    ```js
    db.book.find({})
        .sort({"viewCount" : -1})
        .skip(10).limit(5)
    ```

- 更新

  - 更新: 查找名字叫做小明的，把年龄更改为 16 岁：
    - db. manager.`update`({"name":"小明"},{$set:{"age":16}});   

  - 年龄增加2岁          db.manager.update({},{ $inc:{age:2}} , false , true) 
    - 注意: 第一 false 不允许插入,第二true 允许多条的更新。

  - 根据ID更新和添加         db.manager.save(    )：![img](https://gitee.com/youngstory/images/raw/master/img/202111131650821.png)
    已有的 _id 是修改，不存在的_id 是添加

- 删除   
  
- db.manager.remove({id: 8});
  
- 分组

  - $sum:求和     db.user.aggregate([{$group:{_id:"$sex",sum:{$sum:1}}}])：

  - $avg:计算平均值   

  - $min:获取集合中所有文档对应值的最小值     $max 同理 

### MongoDB 索引   

> 1是升序索引  -1是降序索引

索引是对数据库表中一列或多列的值进行排序的一种结构，查询数据库更快。

- 创建：db.user.ensureIndex({"username":1})

- 获得：db.user.getIndexes()

- 删除索引: db.user.dropIndex({"username":1})

- 唯一索引:db.user.ensureIndex({"userid":1},{"unique":true})

- 复合索引:db.user.ensureIndex({"username":1, "age":-1})

## 数据库连接 

### 传统的连接

- [mongodb - npm (npmjs.com)](https://www.npmjs.com/package/mongodb) 参考链接

### BD类库（数据库封装） 

调用find（） insert(表名，字段对象)  update()  remove()

- 封装一个 BD 类 

- 直接通过类调用方法  ![img](https://gitee.com/youngstory/images/raw/master/img/202111131650564.png)

### ORM对象映射mongoose，

在程序中设计表字段结构  提供一对一 ，一对多设计方法，并且同步到数据库中 （首选 ）

- 安装引入mongoose  生成数据库 ![img](https://gitee.com/youngstory/images/raw/master/img/202111131651300.png)

- 设计表结构   Schema![img](https://gitee.com/youngstory/images/raw/master/img/202111131650229.png)

- 使用

  ![img](https://gitee.com/youngstory/images/raw/master/img/202111131650035.png)

# 2. Sequalize

## 2.1 查询数据库 

普通查询

```js
 static async getUserList(ctx) {
    const userData = await userModel.findAll();
    //console.log(userData)
    ctx.body = util.success(userData);
  }
```

联表查询

```js
static async getProductCategoryRank(ctx){
    const map = {
      is_on: 1,
    };
    let include = [{ model: categoryModel, attributes: ["name"] }];
    const res = await goodsModel.findAll({
      where: map,
      include,
      attributes: [
        "category.name",
        [Sequelize.fn("SUM", Sequelize.col("sales")), "sum"],
      ],
      group: "categoryId",
      raw: true,
    });
```

模糊查询

```js
static async getCateList(ctx) {
    const { name, state } = ctx.request.query;
    const params = {};
    if (name) params.name = {
      [Op.like]: "%" + name + "%"
    }
    if (state) params.state = state;
    let rootList = await categoryModel.findAll({
      where: params,
    });
    console.log(rootList); // 返回orm对象(多)
    console.log(rootList.dataValues); //  dataValues是真的对象信息
    const permissionList = util.getCateMenu(rootList, 0, []);  // category表递归级联菜单（基础重要重要）
    ctx.body = util.success(permissionList);
  }
```

## 2.2 删除数据

```js
await skucateModel.destroy({ where: { goodId: id } });
```

# 3. MySQL

## 3.1 安装

编辑my.ini  数据库配置文件

进入bin文件夹   注册服务  mysqld  -install  mysql1

初始化服务:  mysqld  --initialize --console -P端口号

修改密码 :  mysqladmin -uroot -p password  -P端口

## 3.2 数据库基础

- 关系型数据库

  - 数据管理:  人工、文件、数据库系统

  - 数据库 ： 层状、网状、关系

### 3.2.1 基本概念

- 数据库

- 数据表  ：  二维表 

- 列   ：  字段，属性

- 行  ：  元组   记录

- 主键    ： 唯一标识  --  行不重复 
  - 学号 ，身份证号

- 外键：参照约束；值来源于另一个表 

- 三层模式      外模式 --  模式--- 内模式

  - 模式 -- 关系    全体数据结构描述

  - 内模式  --- 存储结构

  - 外模式 --  局部视图   ； 

- MySQL

  - 单进程多线程，支持多用户 基于客户端/服务器 的关系数据库管理系统 

  - 服务器上运行的网络程序一般都是通过端口号来识别的      Mysql 默认 3306 

## 3.3 数据库操作

net  start/stop mysql/mysql56(服务名)  启用和停止服务     或者直接查服务 手动启动

### 3.3.1 数据类型

- 基本数据类型

  ![image-20220512220701624](https://gitee.com/youngstory/images/raw/master/img/image-20220512220701624.png)

  - TEXT  超长文本  BLOB  二进制
  - int和integer的基本使用对比 

    * Interger是int的包装类，int是基本数据类型
    * integer变量必须实例化后才能使用，int变量不需要
    * interger实际是对象的引用，指向此new的Interger对象， int是直接存储数据值
    * integer的默认值是null , int的默认值是 0 

- 空值null 

  - 插入  insert  into  表名 values(.... ,null ) ;
    - insert  into stu2 valuse('s12',' jerry', null );

  - 修改   update stu2 set tel =null where stuid='s11' ;

  - 查询   Null ---------- 运算符is 

- 浮点型    float (m,d)   double (m,d)    m总个数, d 小数位

- 字符

  - char(n)  定长，实际存储空间  -- n字符

  - varchar(n)    变长 ，实际存储空间----  输入字符的数量+1

- 日期

  - Date--- 日期

  - DateTime  ---  日期+时间 

  - TIMESTAMP  --- 自动获取当前系统时间     在insert 和 update 间获取时间  

- 运算符

  - 运算符分类

    

    - 比较运算符的结果是布尔值，显示1为真，0为假  

    - 举例：  select 12/5;   --- 2.4         

  - IN运算符     select 3 in (5,.7,9);   --0

  - Like 运算符 select   'tom' like  't%'   ---------  表示 第一个字符t匹配 

- SQL 

  - 动词

    - 数据查询   SELECT

    - 数据操纵 DML（表中的数据）   INSERT UPDATE DELETE

    - 数据定义 DDL（表、数据库）   CREATE   DROP ALTER

    - 数据的控制 DCL   GRANT（授权）   REVOKE   （收回）

  - 约束

    - 分类

      - Not null  非空
        - alter table 表名 modify   column 列名  类型  not null ; 

      - default  默认值   
        - alter table 表名 modify   column 列名  类型  default  值  ;

      - primary key  主键   唯一且非空
        - 添加主键约束  alter table  表名  add  pirmary key  字段名     

      - unique  唯一  可以为空
        - alter table 表名 add unique(字段名) ；

      - foreign key  外键，用于限制两个表的关系   外键列取值 参照另一个表（参照表）的主键值 

        - 外键定义在N方

        - 添加外键约束  
          - ALTER TABLE student     ADD CONSTRAINT  约束名 FOREIGN KEY (字段名)       REFERENCES  表名(字段名) ;

      - check 检查约束  

    - 自动增加

      - alter table 表名 modify column 列名 auto_increment ;

      - 使用default  表示使用自动递增值

      - 值为最大值+1  ，也可以指定值 

    - 

### 3.3.2 创建数据库 / 创建表 

- create database student  character set utf8 ；

- create table  表格名![img](https://gitee.com/youngstory/images/raw/master/img/dee97cc2-3e95-479f-abbd-6f4d38abac12-2519832.jpg)

- 创建表过程

  - 需求调研  --------系统所需所有数据

  - 概念模型  -------- 描述数据关系ER

  - 物理模型 --------  关系；  二维表

  - 规范化  -------  满足3NF \

- 修改表结构 

  - 增加列     ALTER 表名 add  列名 数据类型 约束条件 【first/after】已存在的字段名

  - 修改数据类型   Alter 表名 alter column 列名  数据类型 

  - 修改字段    ALTER TABLE 表名 MODIFY 字段名  数据类型

  - 删除字段 alter 表名 drop column 列名    （事先做好备份）

### 3.3.3 数据操纵

- 插入记录 

  ;![img](https://gitee.com/youngstory/images/raw/master/img/a006a08a-cd7f-459c-af9b-7b7ccefe871d-2519832.jpg)

  复制表的操作    CREATE table stu2 LIKE student ;    INSERT into stu2 SELECT * FROM student 

- 修改数据记录

  - update 表名  set  列名=‘值’  WHERE 列名='值 '

  - 多表更新
    - update 表，表2    set  表达式 where条件  ![img](https://gitee.com/youngstory/images/raw/master/img/ec0af9b9-785a-43bc-91ad-a59c7a5cbf63-2519832.jpg)

- 删除数据记录 

  - delete from 表名 where  列名=‘值 ’（条件）

  - 顺序删除 delete  from 表名  order by  字段名 limit  行  删除表中按指定列排序后的若干行 

  - 级联删除，有外键关联会受约束不能删除， 定义外键  on delete casced   

  - truncate  删除数据无法恢复 。

- 数据查询

  - select 语句  ![image-20220512221202826](https://gitee.com/youngstory/images/raw/master/img/image-20220512221202826.png)

    - 消除重复行   select distinct  列名 from 表名 

    - where 语句   SELECT * FROM stu1 WHERE stusex='男'  /       where 列名  between ...and   /  or  组合比较条件 /  in (   ) 集合  /  判断为空 is null   / 通配符查询  where 列名 like' S% ' ， %代表0到多个字符   _一个字符  /  order by  列名【asc】| 【desc】排序  也可多列查询 

    - 注 ： asc 升序   desc 降序 

  - 多表查询

    - 合并查询 union    要求列的数量要相同

    - 连接查询  分类

      - 交叉连接   ----------   笛卡尔积   连接表行数之积

      - 自然连接  -------- 同名列------ 等值连接

      - 内连接:  连接的结果 ----- 所有匹配的结果 （未匹配的不在结果中）

      - 外连接 

        - 左外连接 ： 连接结果：所有匹配结果+左表中未匹配的结果；
          - select 列 from 左表 left join  右表 on 条件 

        - 右外连接 ： 连接结果：所有匹配结果+右表中未匹配的结果；
          - select 列 from 左表 right join  右表 on 条件 

    - 嵌套查询

      - 一个查询语句还包括子查询   

      - 按照出现的位置分为三类 ：

        - from 子查询  
          - 必须定义别名  &&  外层查询的字段引用都应该包含在子查询中![img](https://gitee.com/youngstory/images/raw/master/img/1e75534f-a043-40d1-af93-ed03c48cc98a-2519832.jpg)

        - where 子查询 

          - 单个值
            - 比较运算符的过程中运算符的两侧基本都要求是一个明确的值 。![img](https://gitee.com/youngstory/images/raw/master/img/ee0f6414-45ce-4d2d-a132-2f72350c66e0-2519832.jpg)

          - 集合包含 
            - where集合包含运算in判断集合   &&  子查询只能是一列![img](https://gitee.com/youngstory/images/raw/master/img/763e10ec-c279-4cc6-a8b1-985dd83deb00-2519832.jpg)

          - 判断存在  exists 
            - ![img](https://gitee.com/youngstory/images/raw/master/img/e5562691-0ec0-48a6-8df0-d1a0dec875ee-2519832.jpg)

        - select 子查询 

## 3.4 视图

视图的建立，数据来源于基础表，是一个虚拟表

- 视图建立    create  or replace view   视图 名  as  查询语句   from 基本表   with check option ;  

- 使用视图    select * from  视图 

- 修改视图结构     alter 视图名  as 查询语句   ；  大多数时候    create  or replace view  直接修改  

- 修改视图数据     update  视图名   set      
  
- 同时有以下情况视图不予处理  ![img](https://gitee.com/youngstory/images/raw/master/img/e5376896-0667-4a87-a99a-2be630487b77-2519832.jpg)
  
- 删除视图    DROP  VIEW   视图名 

- 索引

  存储引擎用于快速找到记录的一种  数据结构

  - 建立  

    -  alter table add  unique| fulltext |  spatial index   索引名 

    - create index 索引名 on  表名 

## 3.5 数据库对象

- 运行

  - 创建对象

  - 对象存储过程     ![img](https://gitee.com/youngstory/images/raw/master/img/eee95da8-371b-4e28-b526-921c11ee0375-2519832.jpg)

  - begin  ...  end  包围了定义的程序代码  ，end后面有结束符  delimiter  ；

  - 如果只有一条执行语句，可以省略 begin  end 

  - 顺序执行语句 :

    - 声明变量 : declare    变量     类型 （长度） 

      - 局部变量   作用于当前的过程

      - 用户变量   不需要提前声明  直接使用    使用： @变量名

      - 全局变量:  查询-- 过程  ；  当前用户可以访问的任何环境中使用

    - 变量赋值  :  set 变量名=表达式 ![img](https://gitee.com/youngstory/images/raw/master/img/796b7875-13ec-493a-82e9-64051cfb1c9e-2519832.jpg)

    - 显示语句:   select     变量名  

    - 调用方法 call  过程名（  参数 ）  实参和形参 类型  数量要一一对应![img](https://gitee.com/youngstory/images/raw/master/img/d7e92ba3-e0ea-4db8-9f14-57a316d8d0ef-2519832.jpg)

- if 语句 
  
- if     结束需要 end if ; ![img](https://gitee.com/youngstory/images/raw/master/img/cdcc2849-6c23-4fba-875b-e57ef170ebf5-2519832.jpg)
  
- Case

  - case  表达式 

  - when 值1  then  语句 1   （当表达式的值=值1时，运行语句1）

  -  [when 值2   then  语句 2]

  - else  语句3

  - end  case ;    结束  ![img](https://gitee.com/youngstory/images/raw/master/img/58539cdf-b1f1-456c-8637-6396d2bca112-2519832.jpg)

- loop  循环 

  - [ label : ] loop 

  - set s=s+1 ;

  - select s;

  - end  loop;

  - 程序无法控制循环结束 ，所以人为结束  ---  设置个变量满足条件执行  leave  结束   ![img](https://gitee.com/youngstory/images/raw/master/img/e9e12188-09bc-40c5-a8cd-01efa3c2f837-2519832.jpg)

- while 循环 
  
-  while   条件 DO   语句     end while  ;  ![img](https://gitee.com/youngstory/images/raw/master/img/dde3c42a-11c5-4f0a-ba8a-d3af2741b4dc-2519832.jpg)
  
- 案列

  - 斐波那契![img](https://gitee.com/youngstory/images/raw/master/img/b7689650-2a6f-480b-940a-da20edc9be52-2519832.jpg)

  - 求最大公约数 ![img](https://gitee.com/youngstory/images/raw/master/img/7328af79-7226-40bf-81f4-b234e1f3f11d-2519832.jpg)

- 输入输出 

  - 参数类型  In     表示调用者向过程传入值    实参给形参赋值 

  - OUT     表示过程向调用者传出值    形参给实参赋值 

  - inout  双向赋值        

  -  形参和实参的数量要一一对应  ![img](https://gitee.com/youngstory/images/raw/master/img/48c6c392-fc41-49f9-aefd-5e5d74f0e1e3-2519832.jpg)

- 游标  Cursor

  - 使用户逐行访问由SQL语句返回的结果集。

  - 使用方法 定义 >打开>获取游标数据>关闭游标>释放游标  ; 

    ![image-20220512221408324](https://gitee.com/youngstory/images/raw/master/img/image-20220512221408324.png)

    - 游标声明要放在变量声明之后 

    - 变量的数量 类型  和查询的结果列 要一一对应 

  - 游标的循环读取

    - 通过循环 反复执行 fetch 

    - 判断 -是否到结尾 
      - Declare continue handler for not found set 变量=值 ；利用变量控制循环  

  - 异常处理

    - 异常（运行时错误） 出现时： 中断    

    - 需求:  异常时-- 继续完成后续的代码  

    - 语句： declare  continue | exit  handler for 异常     

      - 异常:  sqlstate_code / error_code / 异常名  

      - 常见的error_code  ![img](https://gitee.com/youngstory/images/raw/master/img/ee85277d-7a23-45f8-a0bf-40e18c108138-2519832.jpg)

## 3.6 函数

侧重于完成综合操作 

- 内置函数 

  - Max()   min()  sum()  avg()   count()  

  - floor()   round()   rand()       concat ()     year()  now()  

- 自定义函数   一定要有return 语句 ![img](https://gitee.com/youngstory/images/raw/master/img/ce89bdf6-54df-40dd-8f5c-143bee7d9090-2519832.jpg)

- 触发器  trigger

  由事件来触发，事先为某张表绑定一段代码，当表中的某些内容发生改变（insert   update  delete ），系统会自动触发代码并执行

  - 绑定在一个表上 ，触发器对应的操作有  insert    update  delete 

  - 触发时间

    - Before 在用户触发事件之前，自动运行触发器 --- 数据检验

    - After 在操作之后   自动运行触发器 -- 数据一致性 

  - 一个表最多可以定义6个触发器 

  - 一个事件如果before 触发器失败或者语句本身失败，将不执行AFTER 触发器

  - 使用

    

    - 触发器中的NEW  OLD  

      - 插入 :   数据 --- insert  -- new 记录  -- 检查约束  

      - 删除   delete  --  选中记录 --  old  

      - 修改  :   new  更新值   old   原有值 

  - 中断操作:  
    - 如果发现更新数据不符合要求 --  中断用户操作  
      - 抛出异常    signal sqlstate 'hy000' set   message=_text= 提示信息   ;

## 3.7 MYSQL 数据库的备份和恢复 

- 备份 

  - 备份数据的形式 :  

    -  物理备份      数据库三层模式 ：外模式-模式-内模式      备份数据库文件  

    -   逻辑备份 （数据对象的逻辑结构） 生成SQL语句   

  - 备份策略

    - 完全备份  >  数据库所有数据完整备份 >数据量大

    - 差异备份 > 备份上次完全备份后的变化   >  数据量小

    - 增量备份 > 备份上次完全备份和增量备份后的变化  > 数据量最小 （最优化）![img](https://gitee.com/youngstory/images/raw/master/img/a831e359-46b0-4409-8365-66e876f452ae-2519832.jpg)

  - 迁移：

    - 关闭mysql服务 

    - 将原数据库的文件直接复制到新的新的服务器的data中   

    - 数据库表访问找不到 >>> 解决方法 :  关闭服务，复制原data文件中的ibdata1文件到新的数据库data文件下

  - SQL数据的导入导出

    - 导出TXT文件
      - select * from 表名 into  outfile '磁盘文件地址'

    - 导入TXT文件
      - \> mysqlimport -uroot -p密码   端口号  数据库名   '文件路径 '

  - 数据库逻辑备份 

    - 使用MYSQL提供的备份工具 mysqldump(在bin文件夹下) 可以将数据库/数据表整个备份 

    - 备份操作:  > mysqldump  选项   数据库名 >  脚本名 （sql文件）

    - 恢复操作 : >  mysql 选项  数据库 < 脚本文件     （前提:  先创建一个空的数据库 ）

    - 完整备份所需的参数 ![img](https://gitee.com/youngstory/images/raw/master/img/518a13b1-28c8-481e-b20f-f60199ff73d6-2519832.jpg)![img](https://gitee.com/youngstory/images/raw/master/img/fb673b6c-8081-4a57-8144-3e960af78452-2519832.jpg)

    - 只备份表结构  

      - \> mysqldump  选项 --no-data --databases 数据库名 >  脚本文件

      - 时间变量

        - 获取 当前日期          %date:~0,4%

        - 获取 当前月份           %date:~5,2%

        - 如 :  mysqldump -uroot -p127 -A  --compact >d:/allc-%date:~0,4%_%date:~5,2%.sql

- 增量备份 

  - 完全备份+系统日志

    - 打开系统日志  my.ini  书写日志选项   log-bin= 日志位置 \\日志名\\MySQL-bin 

    - 日志:  记录每一步操作   

      - 参数

        - --single-transaction   事务锁定相关表；

        - --flush-logs 刷新日志

        - --master-data=2  备份记录日志信息 

        - 脚本 ---  备份点日志文件+备份点的POS 

    - 查看所有日志文件  show master logs ;

    - 查看指定日志文件内容   show  binlog events in  'MySQL-bin.000004' ; 

  - 使用增量备份恢复到还原点

    - 使用完整备份，还原到备份点  （须手动建立数据库）

    - 恢复到日志点position  :  >mymysqlbinlog --stop-position=1673（日志中commit的那行 pos）../mysql-bin.000010|mysql -uroot -p密码

    - 或者选择恢复到时间点 

      - \>mysqlbinlog -d  数据库 ../mysql-bin.000011 >d:/stu2021log.sql

      - \>mysqlbinlog --stop-datetime="2021-03-14 07:54:55" ../mysql-bin.000011| mysql -uroot -p密码  端口 
        ​