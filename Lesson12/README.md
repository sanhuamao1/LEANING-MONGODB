操作文档的函数
============

## 知识点
- $inc:递加
- $mul:相乘
- $rename:改名
- $set:新增or修改
- $unset:字段删除

## 实战演习
```bash
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
```