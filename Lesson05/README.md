操作集合(Collections)
====================

## 知识点
- db.createCollection("[collection_name]")：创建集合
- show collections：展示集合
- db.[collection_name].find()：切换集合并查看文档
- db.[collection_name1].renameCollection("[collection_name2]")：集合重命名
- db.[collection_name1].drop()：删除集合

## 实战演习
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