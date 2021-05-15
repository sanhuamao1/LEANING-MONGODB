基本概念
========

## 知识点
- MongDB与关系型数据库的区别
- 数据库的基本使用步骤


## MongDB与关系型数据库的区别
**文档型数据库（nosql）**
- 数据库（Database）
- 集合（Collection）
- 文档（Document）

在nosql的数据库中，操作数据都是通过指令或者程序语言完成的，比如在mongodb中使用JavaScript和json数据结构，来操作和管理数据

**关系型数据库**
- 数据库（Database）
- 数据表（Table）
- 记录（Record）

## 数据库使用步骤
1. 建立数据库（mypost）
2. 建立数据集合（Posts,Categories,Tags）
3. 建立数据（Post:{"id":"","title":""}）

---
- mypost
    - Posts
        - {"id":"1","title":"我的第一篇博客"}
        - {"id":"2","title":"我的第二篇博客"}
        - {"id":"3","title":"我的第三篇博客"}
    - Categories
        - {"id":"1","title":"游戏"}
        - {"id":"2","title":"技术"}
    - Tags
        - {"id":"1","title":"任天堂"}
        - {"id":"2","title":"mongodb"}


> mongodb可以让每条数据的字段数不一致