
# 引言
MongoDB笔记。教程源于小马技术：
- 视频：https://www.youtube.com/watch?v=kmOzFqxcjEA&list=PLliocbKHJNwvYvA3paPKUg86qLKrwuNsd
- 仓库：https://gitee.com/komavideo/LearnMongoDB


该文章用于mongodb学习笔记整理。

# 目录说明
1. 什么是MongoDB
2. MongoDB安装
3. MongoDB基本概念
4. 基本操作
5. 操作集合(Collections)
6. 操作文档(Document)
7. 单个条件抽文档
8. 复杂条件抽文档
9. 抽出指定字段
10. 文档的方法
11. 文档更新
12. 操作文档的函数
13. 文档的特殊更新
14. 使用索引  


# 整理

![gc6RNd.png](https://z3.ax1x.com/2021/05/16/gc6RNd.png)

- 完整脑图：https://share.mubu.com/doc/7jeYw7aHAG
- 仓库：https://github.com/sanhuamao1/LEARN-MONGODB
（在原来的基础上小改并添加了一些注释）

# 内容汇总
## 01 什么是MongoDB
MongoDB是一个面向文档的免费数据库，多用于数据采集和分散处理（Map/Reduce），特别是在大数据处理方面比较擅长。

### 数据库分类
- 面向关系的数据库
    - Oracle
    - SQLServer
    - MySql
    - PostgreSql
- NoSql
    - MongoDB
    - Redis

## 02 MongoDB安装

- 安装地址：https://www.mongodb.com/try/download/community
- 视频教程：https://www.bilibili.com/video/BV1x541137MG


## 03 基本概念

### 与关系型数据库的区别
- 文档型数据库（nosql）
    - 数据库（Database）
    - 集合（Collection）
    - 文档（Document）
- 关系型数据库
    - 数据库（Database）
    - 数据表（Table）
    - 记录（Record）

### 数据库使用步骤
- 建立数据库（mypost）
- 建立数据集合（Posts,Categories,Tags）
- 建立数据（Post:{"id":"","title":""}）

## 04 基本操作
- mongo：进入mongo命令工具
- help：查看可执行命令
- show dbs：展示数据库
- use [database_name]：创建或切换数据库
- db.stats()：查看数据库状态
- db.dropDatabase()：删除数据库
- exit：退出

## 05 操作集合(Collections)
- db.createCollection("[collection_name]")：创建集合
- show collections：展示集合
- db.[collection_name].find()：切换集合并查看文档
- db.[collection_name1].renameCollection("[collection_name2]")：集合重命名
- db.[collection_name1].drop()：删除集合

## 06 操作文档(Document)
- db.[collection_name].insert() ——插入数据
- db.[collection_name].find() ——切换集合并查看文档
- db.[collection_name].count() ——查看文档数
- db.[collection_name].remove({}) ——删除文档

例子
```bash
> db.posts.insert({title:"怪物猎人世界评测","rank":2,"tag":"game"});
```

## 07 单个条件抽文档
- db.[collection_name].find({"":""})：find后面写筛选条件
- db.[collection_name].distinct("field_name")：取指定字段所包含的属性值

---
- $gte：>=
- $gt：>
- $lte：<=
- $lt：<
- $eq：=
- &ne：!=
- 正则表达式：/k/,/^k/

例子
```js
> db.posts.find({"tag": "game"});
> db.posts.find({"rank": {$lt: 4}});         
> db.posts.find({"title": /u/});
```
## 08 复杂条件抽文档
- db.[collection_name].find({"":"", "":""})：多个条件筛选
- db.[collection_name].find({$or:[{...},{...}]})：与或非运算
- db.[collection_name].find({"": {$in: [...]}})：在某个范围内
- db.[collection_name].find({"": {$exists: true}})：某个文档存在

例子
```js
> db.posts.find({"title": /u/, "rank":{$gte:5} });
> db.posts.find({$or: [{"title": /u/}, {"rank":{$gte:4}}] });
> db.posts.find({"rank": {$in: [3,4]} });
> db.posts.find({"istop": {$exists: true} });
```

## 09 抽出指定字段
- db.[collection_name].find({}, {field1: true, field2: 1})：抽出field1和field2。这里1相当于true

例子
```bash
> db.posts.find({}, {title:true, rank:1});
> db.posts.find({}, {title:true, rank:1, _id:0});
```

## 10 文档的方法
- sort()：按照某字段升序降序
- limit()：取指定数目条数
- skip()：跳过指定数目条数

例子
```bash
> db.posts.find({}, {_id:0}).sort({rank:1});
> db.posts.find({}, {_id:0}).limit(3);
> db.posts.find({}, {_id:0}).skip(3).limit(3);//分页
```

## 11 文档更新
- update(<filter>,<update>,<options>)

- 官方文档：https://docs.mongodb.com/manual/tutorial/update-documents/

例子
```bash
> db.posts.update({"title":"怪物猎人世界评测"}, {$set: {"rank": 10} });
> db.posts.update({"tag":"it"}, {$set: {"rank": 60}}, {multi: true});
```


## 12 操作文档的函数
- $inc:递加
- $mul:相乘
- $rename:改名
- $set:新增or修改
- $unset:字段删除

例子
```bash
> db.posts.update({title:"怪物猎人世界评测"}, {$inc: {rank: 1}});
> db.posts.update({title:"怪物猎人世界评测"}, {$rename: {"rank": "score"}});
> db.posts.update({title:"怪物猎人世界评测"}, {$unset: {"istop": true}});
```

## 13 文档的特殊更新
- upsert:有则更新，无则追加
- remove:条件删除数据

例子
```bash
> db.posts.update({title:"其实创造比大志好玩"}, {title:"其实创造比大志好玩", "rank":5,"tag":"game"}, {upsert:true});
> db.posts.remove({title:"其实创造比大志好玩"});
```

## 14 使用索引
- getIndexes()：列出当前集合的所有索引
- createIndex({...}, {...})：建立一个索引
- dropIndex({...})：删除一个索引

例子
```bash
> db.posts.getIndexes();
> db.posts.createIndex({rank:-1});
> db.posts.dropIndex({rank:-1});
```

## 15 备份与恢复
- mongodump：备份
- mongorestore：恢复
