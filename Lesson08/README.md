复杂条件抽文档
=============

## 知识点
- db.[collection_name].find({"":"", "":""})：多个条件筛选
- db.[collection_name].find({$or:[{...},{...}]})：与或非运算
- db.[collection_name].find({"": {$in: [...]}})：在某个范围内
- db.[collection_name].find({"": {$exists: true}})：某个文档存在



## 实战演习
```bash
$ mongo
> use komablog;
> db.posts.find();
> db.posts.find({"title": /u/, "rank":{$gte:5} });               //标题带u且排名大于等于5
> db.posts.find({$or: [{"title": /u/}, {"rank":{$gte:4}}] });    //标题带u或排名大于等于4
> db.posts.find({"rank": {$in: [3,4]} });                        //排名在3到4之间
> db.posts.insert({ "title":"惊！骑士发生重大交易", "istop": true });
> db.posts.find({"istop": {$exists: true} });                    //找存在istop字段的文档
```