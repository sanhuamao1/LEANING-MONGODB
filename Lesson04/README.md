MongoDB基本操作
===============

## 知识点
- mongo：进入mongo命令行工具
- help：查看可执行命令
- show dbs：展示数据库
- use [database_name]：创建或切换数据库
- db.createCollection("[collection_name]")：创建集合
- show collections：显示集合
- db.stats()：查看数据库状态
- db.dropDatabase()：删除数据库
- exit：退出

## 实战演习
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