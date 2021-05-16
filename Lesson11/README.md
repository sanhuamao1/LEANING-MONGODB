文档更新
=========

## 知识点
- update(<filter>,<update>,<options>)

## 官方文档
https://docs.mongodb.com/manual/tutorial/update-documents/

## 实战演习
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
```