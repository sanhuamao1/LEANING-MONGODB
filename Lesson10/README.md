文档的方法
=========

## 知识点
- sort()：按照某字段升序降序
- limit()：取指定数目条数
- skip()：跳过指定数目条数

## 实战演习
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
> db.posts.find({}, {_id:0}).skip(3).limit(3);//跳过前三条，从第四条开始（汾分页）
```