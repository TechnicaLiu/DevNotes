## 特点

- 非关系型数据库NoSql (mongodb, radis) ,对象(键值对)

- 高性能、易部署、 易使用，存储数据非常方便。

- 支持的数据结构非常松散，是**类似于json**, 可以存储比较复杂的数据类型 。

- **查询语言强大**，几乎可以实现关系型数据库单表查询的绝大部分功能，还支持对数据建立索引

- 一个MongoDB 实例可以包含一组数据库，一个DataBase 可以包含一组Collection（集合），一个集合可以包含一组Document（文档）。

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

- 使用![img](https://gitee.com/youngstory/images/raw/master/img/202111131650035.png)