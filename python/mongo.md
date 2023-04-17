# 1.插入数据
## 1.1 批量插入或更新数据
可以用MongoDB的更新操作符，如 "$set"、"$addToSet"、"$push"。
```python
db.collection.update(
    { name: "张三" },
    { $addToSet: { grades: 95 }, $set: {}, $push: {name: "xiao"}}
)
```

permission_point 表结构
```json
    {
        "app_id" : [ 
            "mss_default", 
            "sangfor_union-939e-40ca-9d44-6f6f7cea8a55"
        ],
        "method" : "POST",
        "page_code" : [ 
            "customer_list_event", 
            "separate_host_tool"
        ],
        "param_method" : "NONE",
        "tenant_id" : "1001",
        "value" : "workflow/order/v1/tool_box/host_separate_tool/status_manage",
        "auth" : 2,
        "is_delete" : 0,
        "page_code_old" : "customer_list_event",
        "desc" : "重新隔离",
        "url_name" : "重新隔离"
    }
```

实现功能：批量更新或插入数据
```python
def bulk_update_permission_point(urls_list, is_delete=0):
        update_list = list()
        for i in range(len(urls_list)):
            query_filter = {
                'value': urls_list[i].get("value"),
                'method': urls_list[i].get("method"),
                'is_delete': is_delete,
            }
            update_data = {
                'value': urls_list[i].get("value"),
                'method': urls_list[i].get("method"),
                'param_method': urls_list[i].get("param_method"),
                'url_name': urls_list[i].get("url_name"),
                'auth': urls_list[i].get("auth"),
                'is_delete': is_delete,
                'desc': urls_list[i].get("desc") or ""
            }
            set_data = {
                'app_id': urls_list[i].get("app_id"),
                'page_code': urls_list[i].get("page_code")
            }
            # query 查询，设置upsert=true，存在时更新，不存在时插入新的数据
            # update 更新数据时，如果字段是列表且需要去重，要用 "$addToSet"
            # 如果是列表但不去重，用 "$push"
            update_item = UpdateOne(query_filter,
                                    update={"$set": update_data, "$addToSet": set_data},
                                    upsert=True)
            update_list.append(update_item)

        return self.db.bulk_write(self.coll, update_list, ordered=False)
```


