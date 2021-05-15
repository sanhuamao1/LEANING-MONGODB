操作文档(Document)
=================

## 知识点
- db.[collection_name].insert() ——插入数据
- db.[collection_name].find() ——切换集合并查看文档
- db.[collection_name].count() ——查看文档数
- db.[collection_name].remove({}) ——删除文档

## 实战演习
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