九，python与mongodb

前提安装pymongo模块；

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
from pymongo import MongoClient


class MongoDemo:
    def __init__(self):
        client = MongoClient(
            host="127.0.0.1",
            port=27017)   # 本地连接，host和port可省略
        self.collection = client["newdatabase"]["newcollection"]  # 绑定一个集合

    # 插入数据
    def data_insert(self):
        # 插入单条数据，指定_id会返回_id的值，也可以用insert()，但是不推荐哈
        # result = self.collection.insert_one({"_id":2, "name":"jiajia","age":19})
        # print(result)
        self.collection.insert_one({"name": "小小", "age": 19})
        # 插入多条数据，
        self.collection.insert_many(
            [{"name": "jiajia" + str({i}), "age": 18} for i in range(10)])

    def data_find(self):
        # 查找单条数据，返回匹配的第一条数据
        one = self.collection.find_one({"name": "小小"})
        print(one)
        # 查找多条数据，返回一个游标对象，可以迭代，也可以list()强制转换
        many = self.collection.find({"name": "jiajia"})
        print((list(many)))
        # self.collection.find_and_modify() 已弃用
        # 找到匹配的第一个并删除
        self.collection.find_one_and_delete({"name": "jiajia"})
        # 找到匹配的第一个并替换
        self.collection.find_one_and_replace(
            {"name": "jiajia"}, {"name": "jiajia0"})
        # 找到匹配的第一个并更新，可以增加键值对
        self.collection.find_one_and_update(
            {"name": "jiajia"}, {"$set": {"gender": "female"}})

    def data_update(self):
        # 更新匹配的第一条数据
        self.collection.update_one({"name": "jiajia"}, {"$set": {"age": 18}})
        # 更新所有匹配数据
        self.collection.update_many({"name": "jiajia"}, {"$set": {"age": 18}})

    def data_delete(self):
        # 删除匹配的第一条数据，没有匹配也不报错
        # self.collection.delete_one({"name":"jiajia{0}"})
        # 删除所有匹配数据，没有匹配也不报错
        self.collection.delete_many({"name": "jiajia{1}"})
        all = self.collection.find()
        print(len(list(all)))


if __name__ == '__main__':
    c = MongoDemo()
    c.data_delete()

```

