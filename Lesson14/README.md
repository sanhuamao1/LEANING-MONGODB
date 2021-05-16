使用索引
=======

## 知识点
- getIndexes()：列出当前集合的所有索引
- createIndex({...}, {...})：建立一个索引
- dropIndex({...})：删除一个索引

## 实战演习
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