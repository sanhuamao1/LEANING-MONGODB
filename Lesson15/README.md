恢复与备份
=========

## 知识点
- mongodump：备份
- mongorestore：恢复

## 实战演习
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
$ mongodump --help
```