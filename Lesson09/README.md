抽出指定字段
============

## 知识点
- db.[collection_name].find({}, {field1: true, field2: 1})：抽出field1和field2。这里1相当于true

## 实战演习
```bash
$ mongo
> use komablog;
> db.posts.find();
> db.posts.find({}, {title:true, rank:1});        //默认会抽出_id
> db.posts.find({}, {title:true, rank:1, _id:0});
```