实战演习
=======
## 01 基本命令
```bash
$ mongo
> help
> show dbs;
> use komablog;
> show collections;
> db.createCollection("posts");
> db.createCollection("categories");
> db.createCollection("tags");
> show collections;
> show dbs;
> db.stats();
> db.dropDatabase();
> show dbs;
> exit
```
## 02 操作集合
```bash
$ mongo
> show dbs;
> use komablog;
> show collections;
> db.createCollection("users");
> show collections;
> db.users.renameCollection("staff"); // users -> staff
> show collections;
> db.staff.drop();
> show collections;
```
## 03 操作文档
```bash
$ mongo
> use komablog;
> show collections;
> db.createCollection("posts");
> db.posts.insert(
... {
...     title: "我的第一篇博客",
...     content: "已经开始写博客了，太激动了。"
... }
... );
> show collections;
> db.posts.find();
> for(var i = 2; i <=10; i++ ) {
...     db.posts.insert({
...         title: "我的第" + i + "篇博客"
...     });
... }
> db.posts.find();
> db.posts.count();
> db.posts.remove({});
> db.posts.count();
> db.posts.find();
```

## 04 查询文档
```bash
$ mongo
> use komablog;
> db.posts.remove({});
> db.posts.insert({title:"怪物猎人世界评测","rank":2,"tag":"game"});
> db.posts.insert({title:"纸片马里奥试玩体验","rank":1,"tag":"game"});
> db.posts.insert({title:"Utunbu16LTS的安装","rank":3,"tag":"it"});
> db.posts.insert({title:"信长之野望大志销量突破10000","rank":4,"tag":"game"});
> db.posts.insert({title:"Ruby的开发效率真的很高吗","rank":7,"tag":"it"});
> db.posts.insert({title:"塞尔达传说最近出了DLC","rank":4,"tag":"game"});
> db.posts.find({"tag": "game"});
> db.posts.find({"rank": {$gte: 4}});        
> db.posts.find({"rank": {$gt: 4}});        
> db.posts.find({"rank": {$lte: 4}});        
> db.posts.find({"rank": {$lt: 4}});         
> db.posts.find({"title": /u/});
> db.posts.find({"title": /^R/});
> db.posts.find({"title": /^U/});
> db.posts.distinct("tag");

$ mongo
> use komablog;
> db.posts.find();
> db.posts.find({"title": /u/, "rank":{$gte:5} });               //标题带u且排名大于等于5
> db.posts.find({$or: [{"title": /u/}, {"rank":{$gte:4}}] });    //标题带u或排名大于等于4
> db.posts.find({"rank": {$in: [3,4]} });                        //排名在3到4之间
> db.posts.insert({ "title":"惊！骑士发生重大交易", "istop": true });
> db.posts.find({"istop": {$exists: true} });                    //找存在istop字段的文档
```

## 05 文档方法
```bash
$ mongo
> use komablog;
> db.posts.find();
> db.posts.find({}, {_id:0}).sort({rank:1});
> db.posts.find({}, {_id:0}).sort({rank:-1});
> db.posts.find({}, {_id:0}).limit(3);
> db.posts.find({}, {_id:0}).sort({rank:-1}).limit(3);
> db.posts.findOne({}, {_id:0});//只抽第一条，与limit(1)相似
> db.posts.find({}, {_id:0});
> db.posts.find({}, {_id:0}).limit(3);
> db.posts.find({}, {_id:0}).skip(3).limit(3);//跳过前三条，从第四条开始（分页）
```

## 06 文档更新
```bash
$ mongo
> use komablog;
> db.posts.findOne({"title":"怪物猎人世界评测"});
> db.posts.update({"title":"怪物猎人世界评测"}, {$set: {"rank": 10} }); //修改该文档的rank值
> db.posts.find();
> db.posts.update({"title":"怪物猎人世界评测"}, {"rank": 99}); //移除该文档，替换成{"rank": 99}，_id不变
> db.posts.find();
> db.posts.update({"tag":"it"}, {$set: {"rank": 50}}); //修改tag为it的第一个rank值
> db.posts.find();
> db.posts.update({"tag":"it"}, {$set: {"rank": 60}}, {multi: true});//修改tag为it的所有rank值
> db.posts.find();

$ mongo
> use komablog;
> db.posts.find({title:"怪物猎人世界评测"}, {_id:0});
> db.posts.update({title:"怪物猎人世界评测"}, {$inc: {rank: 1}});    //rank字段值+1
> db.posts.find({title:"怪物猎人世界评测"}, {_id:0});
> db.posts.update({title:"怪物猎人世界评测"}, {$mul: {rank: 2}});    //rank字段值*2
> db.posts.find({title:"怪物猎人世界评测"}, {_id:0});
> db.posts.update({title:"怪物猎人世界评测"}, {$rename: {"rank": "score"}});    //rank字段改成score
> db.posts.find({title:"怪物猎人世界评测"}, {_id:0});
> db.posts.update({title:"怪物猎人世界评测"}, {$set: {"istop": true}});
> db.posts.find({title:"怪物猎人世界评测"}, {_id:0});
> db.posts.update({title:"怪物猎人世界评测"}, {$unset: {"istop": true}});
> db.posts.find({title:"怪物猎人世界评测"}, {_id:0});

$ mongo
> use komablog;
> db.posts.find({}, {_id:0});
> db.posts.update({title:"其实创造比大志好玩"}, {title:"其实创造比大志好玩", "rank":5,"tag":"game"});    //原本没有则没变化
> db.posts.find({}, {_id:0});
> db.posts.update({title:"其实创造比大志好玩"}, {title:"其实创造比大志好玩", "rank":5,"tag":"game"}, {upsert:true});
> db.posts.find({}, {_id:0});
> db.posts.update({title:"其实创造比大志好玩"}, {title:"其实创造比大志好玩", "rank":7,"tag":"game"}, {upsert:true});
> db.posts.find({}, {_id:0});
> db.posts.remove({title:"其实创造比大志好玩"});
> db.posts.find({}, {_id:0});
```

## 07 使用索引
```bash
$ mongo
> use komablog;
> db.posts.getIndexes(); //如果没有索引，默认建立id的索引
> db.posts.createIndex({rank:-1});//添加rank降序索引
> db.posts.getIndexes();
> db.posts.dropIndex({rank:-1});
> db.posts.getIndexes();
> db.posts.createIndex({title:1}, {unique:true});//标题不可重复
> db.posts.getIndexes();
> db.posts.find({}, {_id:0});
> db.posts.insert({title:"怪物猎人世界评测"});
```

## 08 恢复与备份
```bash
$ mongo
> show dbs;
> use komablog;
> db.posts.find({}, {_id:0});
> exit
$ mkdir dbbak
$ cd dbbak
$ mongodump -d komablog //备份指定数据库
$ ls
$ mongo komablog
> db.posts.find({}, {_id:0}); 
> db.posts.remove({}); //删除数据库
> db.posts.find({}, {_id:0});
> exit
$ mongorestore --drop //恢复
$ mongo komablog
> db.posts.find({}, {_id:0});
> exit
```