# 引言
MongoDB笔记。教程源于小马技术，适合入门：
- 视频：https://www.youtube.com/watch?v=kmOzFqxcjEA&list=PLliocbKHJNwvYvA3paPKUg86qLKrwuNsd
- 仓库：https://gitee.com/komavideo/LearnMongoDB

---
**该文章用于mongodb学习笔记整理**
![gW4Itx.png](https://z3.ax1x.com/2021/05/18/gW4Itx.png)]
- 完整脑图：https://share.mubu.com/doc/7jeYw7aHAG
- 个人仓库：https://github.com/sanhuamao1/LEARN-MONGODB （包含一些操作例子）
- mongo shell 方法文档：https://docs.mongodb.com/manual/reference/method/
- nodejs驱动方法文档：https://docs.mongodb.com/drivers/node/current/usage-examples/

# 一 什么是MongoDB
MongoDB是一个**面向文档**（NoSql）的免费数据库，多用于数据采集和分散处理（Map/Reduce），特别是在大数据处理方面比较擅长。

- 数据库（Database）
- 集合（Collection）——相当于数据表（Table）
- 文档（Document）——相当于记录（Record）
---
- 官方网站：https://www.mongodb.com/
- 数据库引擎排名：https://db-engines.com/en/ranking
- 学习前准备：JavaScript基础

# 二 MongoDB安装

- 安装地址：https://www.mongodb.com/try/download/community
- 视频教程：https://www.bilibili.com/video/BV1x541137MG

# 三 基本命令

```bash
mongo	//进入mongo命令工具
help	//查看可执行命令
show dbs	//展示数据库
use [db_name]	//创建或切换数据库
db.stats()	//查看数据库状态
db.dropDatabase()	//删除数据库
exit	//退出mongo命令工具
```

**操作集合(Collections)**

```bash
db.createCollection(<col_name>)	//创建集合
show collections	//展示集合
db.[col_name].find()	//切换集合并查看文档
db.[col_name1].renameCollection(<col_name2>)	//集合重命名
db.[col_name].drop()	//删除集合
```

**操作文档(Document)**

```bash
db.[col_name].insert({}) //插入数据（正在淘汰）
db.[col_name].insertOne({}) //插入单条数据
db.[col_name].insertMany({}) //插入多条数据
db.[col_name].find() 	//切换集合并查看文档
db.[col_name].count() 	//查看文档数
db.[col_name].remove({}) //删除文档

> db.posts.insert({title:"怪物猎人世界评测","rank":2,"tag":"game"});
```

# 四 读取文档

## Find-查询文档

**比较**

```bash
db.col.find({<key1>:<value>})//等于
db.col.find({<key>:{$lt:<value>}})//小于
db.col.find({<key>:{$lte:<value>}})//小于或等于
db.col.find({<key>:{$gt:<value>}})//大于
db.col.find({<key>:{$gte:<value>}})//大于或等于
db.col.find({<key>:{$ne:<value>}})//不等于

> db.posts.find({"tag": "game"});
> db.posts.find({"rank": {$lt: 4}});   
```

**其他**

```bash
db.col.find({<key1>:<value>,<key2>:<value>})//与
db.col.find({$or:[{<key1>:<value>},{<key2>:<value>}]})//或
db.col.find({<key>:{<key>:<expression>}})	//正则表达式
db.col.find({<key>:{$in: [<value1>,<value2>]}})	//在某个范围内
db.col.find({<key>:{$exists: true}})	//存在某个字段的文档
db.col.find({<key>:{$type:<type>}})	//根据数据类型查询

> db.posts.find({"title": /u/});
> db.posts.find({"title": /u/, "rank":{$gte:5} });
> db.posts.find({$or: [{"title": /u/}, {"rank":{$gte:4}}]});
> db.posts.find({"rank": {$in: [3,4]} });
> db.posts.find({"istop": {$exists: true} });
> db.orders.find({
      name: "Lemony Snicket",
      date: {
        $gte: new Date(new Date().setHours(00, 00, 00)),
        $lt: new Date(new Date().setHours(23, 59, 59)),
      },
    });
```

**抽取**

```bash
db.col.find({}, {<key1>: true, <key2>: 1,<key3>:0}) //每条文档只选key1和key2显示,且不显示key3
db.col.findOne(<query>)//只取第一条
db.col.distinct(<key>)	//取指定字段所包含的属性值(数组)

> db.posts.find({}, {title:true, rank:1});
> db.posts.find({}, {title:true, rank:1, _id:0});
```

**方法**

```bash
db.col.find().sort({<key>:<type>});	//以key升序或降序，type可以是1和-1
db.col.find().limit(<num>);	//限制显示条数
db.col.find().skip(<num>)	//跳过指定条数

> db.posts.find({}, {_id:0}).sort({rank:1});
> db.posts.find({}, {_id:0}).limit(3);
> db.posts.find({}, {_id:0}).skip(3).limit(3);//分页
```



## Aggregate-聚合

### 前言-管道

MongoDB的聚合管道是将MongoDB文档在一个管道处理完毕后将结果传递给下一个管道处理。

- $project：修改输入文档的结构。可以用来重命名、增加或删除域，也可以用于创建计算结果以及嵌套文档。
- $match：用于过滤数据，只输出符合条件的文档。$match使用MongoDB的标准查询操作。
- $limit：用来限制MongoDB聚合管道返回的文档数。
- $skip：在聚合管道中跳过指定数量的文档，并返回余下的文档。
- $unwind：将文档中的某一个数组类型字段拆分成多条，每条包含数组中的一个值。
- $group：将集合中的文档分组，可用于统计结果。
- $sort：将输入文档排序后输出。
- $geoNear：输出接近某一地理位置的有序文档。

```bash
> db.article.aggregate(
    { $project : {
        _id : 0 ,
        title : 1 ,
        author : 1
    }});//文档只包含title和author字段，不包含_id字段
```

### 聚合操作

```bash
//计算总和
db.col.aggregate([
	{
		$group : {
			_id : <groupby-key>,
        	<new-key> : {$sum : <key>}
    	}
    }
])

db.col.aggregate([{$group:{_id : <groupby-key>,<new-key>:{$avg:<key>}}}])//平均值
db.col.aggregate([{$group:{_id : <groupby-key>,<new-key>:{$min:<key>}}}])//最小值
db.col.aggregate([{$group:{_id : <groupby-key>,<new-key>:{$max:<key>}}}])//最大值
db.col.aggregate([{$group:{_id : <groupby-key>,<new-key>:{$first:<key>}}}])//获取第一个文档数据
db.col.aggregate([{$group : {_id : <groupby-key>,<new-key> : {$last:<key>}}}])//获取最后一个文档数据

> db.orders.aggregate([
  {
    $match: {
      date: {
        $gte: new Date(new Date().getTime() - 1000 * 3600 * 24 * 7),
        $lt: new Date(),
      },
    },
  },
  {
    $group: {
      _id: "$status",
      count: {
        $sum: 1,
      },
    },
  },
]);//先匹配一周之前的数据，在其基础上以status字段分组，创建一个字段count来合计每组的数量
```

# 五 更新文档

- 官方文档：https://docs.mongodb.com/manual/reference/method/db.collection.update/

- `update(<query>,<update>,<options>)`
- `remove(<query>)`：根据条件删除


```bash
> db.posts.update({"title":"怪物猎人世界评测"}, {$set: {"rank": 10} });
> db.posts.update({"tag":"it"}, {$set: {"rank": 60}}, {multi: true});
> db.posts.remove({"title":"其实创造比大志好玩"})
```

## update

- $inc——递加
- $mul——相乘
- $rename——改名
- $set——新增or修改
- $unset——字段删除

```bash
> db.posts.update({title:"怪物猎人世界评测"}, {$inc: {rank: 1}});
> db.posts.update({title:"怪物猎人世界评测"}, {$rename: {"rank": "score"}});
```

## options

- $upsert——有则更新，无则追加
- $multi——是否开启多选

```bash
> db.posts.update({title:"其实创造比大志好玩"}, {title:"其实创造比大志好玩", "rank":5,"tag":"game"}, {upsert:true});
> db.posts.update({title:"怪物猎人世界评测"}, {$unset: {"istop": true}});
```

# 六 使用索引

使用索引可提高查询速度

```bash
db.col.getIndexes();//列出当前集合的所有索引
db.col.createIndex({<key>:<type>});//建立一个索引
db.col.createIndex({<key>:<type>,<key>:<type>});//建立复合索引（当查询需要用到多个key时）
db.col.createIndex({<key>:<type>,"unique":true});//唯一索引（key对应的value值不允许重复）
db.col.dropIndex({<key>:<type>});//删除一个索引

> db.col.createIndex({rank:-1});
> db.col.dropIndex({rank:-1});
```



# 七 权限管理

## 创建角色

先切换到数据库，再创建角色。role是角色类型，db指定数据库

```bash
> use admin
> db.createUser({
	user:"admin",
	pwd:"123456",
	roles:[{role:'root',db:admin}]
});
```

## 配置文件

`/bin/mongod.cfg`

```
security:authorization enabled
```

> 配置完文件前需重启mongoserver才生效

## 连接

```bash
mongo admin -u admin -p 123456
//or
mongo admin
db.auth("admin","123456")
```

## 其他

```bash
show users //展示角色
db.dropUser(<user_name>)//删除角色
db.updateUser(<user_name>,{pwd:<pwd>})//更改密码
```


# 八 备份与恢复

- mongodump：备份
- mongorestore：恢复

