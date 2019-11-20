---
title: Python操作ElasticSearch
copyright: true
date: 2018-08-01 10:49:15
tags:
  - ElasticSearch
  - multiprocessing
  - Pool
  - Python
categories:
---

# Python操作ElasticSearch

## Python批量向ElasticSearch插入数据

`Python 2`的多进程不能序列化类方法, 所以改为函数的形式.

直接上代码:

```
#!/usr/bin/python
# -*- coding:utf-8 -*-

import os
import re
import json
import time
import elasticsearch

from elasticsearch.helpers import bulk
from multiprocessing import Pool


def write_file(doc_type, action_list):
    """"""
    with open("/home/{}_error.json".format(doc_type), "a") as f:
        for i in action_list:
            f.write(str(i))

def add_one(file_path, doc_type, index):
    """准备插入一条"""
    print doc_type, index
    es_client = elasticsearch.Elasticsearch(hosts=[{"host": "localhost", "port": "9200"}])
    with open(file_path, "r") as f:
        for line in f:
            try:
                line = re.sub("\n", "", line)
                dict_obj = json.loads(line)
                es_client.index(index=index, doc_type=doc_type, body=dict_obj)
            except Exception as e:
                print "出错了, 错误信息: {}".format(e)

def add_bulk(doc_type, file_path, bulk_num, index):
    """"""
    es_client = elasticsearch.Elasticsearch(hosts=[{"host": "localhost", "port": "9200"}])
    action_list = []
    # 文件过大, 先插入5000万试水
    total = 50000000
    num = 0

    with open(file_path, "r") as f:

        for line in f:

            num += 0
            if num >= total:
                break
            
            # 去除每一行数据中的"\n"字符, 也可以替换为"\\n"
            line = line.replace("\n", "")
            dict_obj = json.loads(line)

            # 根据bulk_num的值发送一个批量插入请求
            # action = {
            #     "_index": index,
            #     "_type": doc_type,
            #     "_source": {
            #         "ip": dict_obj.get("ip", "None"),
            #         "data": str(dict_obj.get("data", "None"))
            #     }
            # }

            # 如果动态插入，字段过长，会报错，导致插不进去, 转为字符串就可以
            action = {
                '_op_type': 'index',
                "_index": index,
                "_type": doc_type,
                "_source": dict_obj
            }
            action_list.append(action)

            if len(action_list) >= bulk_num:

                try:
                    print "Start Bulk {}...".format(doc_type)
                    success, failed = bulk(es_client, action_list, index=index, raise_on_error=True)
                    print "End Bulk {}...".format(doc_type)
                except Exception as e:
                    print "出错了, Type:{}, 错误信息:{}".format(doc_type, e[0])
                    write_file(doc_type, action_list)
                finally:
                    del action_list[0:len(action_list)]

        # 如果不是bulk_num的等值, 那么就判断列表是否为空, 再次发送一次请求
        if len(action_list) > 0:
                try:
                    success, failed = bulk(es_client, action_list, index=index, raise_on_error=True)
                except Exception as e:
                    print "出错了, Type:{}, 错误信息:{}".format(doc_type, e[0])
                    write_file(doc_type, action_list)
                finally:
                    del action_list[0:len(action_list)]

def mulit_process(path, index, bulk_num, data):
    """"""
    # 多进程执行
    pool = Pool(10)

    results = []
    for i in data:
        doc_type = i["doc_type"]
        file_path = i["file_path"]
        result = pool.apply_async(add_bulk, args=(doc_type, file_path, bulk_num, index))
        results.append(result)
    
    pool.close()
    pool.join()

def all_info(path):
    data = []
    for i in os.listdir(path):
        file_dict = {}
        if i.endswith(".json"):
            doc_type = i.split("_")[0]
            file_path = path + i
            if doc_type == "443":
                continue
            file_dict["doc_type"] = doc_type
            file_dict["file_path"] = file_path
            data.append(file_dict)

    return data


def es_insert(process_func=None):
    """"""
    # 库
    index = "test"
    # 文件路径
    path="/home/data/"
    
    # 批量插入的数量, 如果是json整条数据插入的话, 可能会出现字段过长的问题, 导致插不进去, 适当调整bulk_num的值
    bulk_num = 5000

    if not path.endswith("/"):
        path += "/"

    data = all_info(path)

    if process_func == "bulk":
        # 插入多条, doc_type, file_path, bulk_num, index
        add_bulk("80", path + "80_result.json", bulk_num, index)
    elif process_func == "one":
        # 插入单条file_path, doc_type, index
        add_one(path + "80_result.json", "80", index)
    else:
        # 多进程
        mulit_process(path, index, bulk_num, data)


if __name__ == "__main__":
    # 计算脚本执行时间
    start_time = time.time()
    if not os.path.exists("/home/test"):
        os.makedirs("/home/test")

    # 插入数据
    es_insert()

    # 计算脚本执行时间
    end_time = time.time()
    print end_time - start_time
```

## Python搜索ElasticSearch

示例:

```
#!/usr/bin/python
# -*- coding:utf -*-

import json
import elasticsearch


def es_login(host="localhost", port="9200"):
    """连接es"""
    return elasticsearch.Elasticsearch(hosts=[{"host": host, "port": port}])

def get(es_client, _id):
    """获取一条内容"""
    # result = es_client.get(index="test", doc_type="80", id=_id)
    result = es_client.get(index="test", id=_id)
    return json.dumps(result)

def search(es_client, query, field="_all"):
    """聚合搜索内容"""

    result = es_client.search(index="test", body={
        "query": {
            "bool": {
                "must": [
                    {
                        "query_string": {
                            # 指定字段
                            "default_field": field,
                            # 查询字段
                            "query": query
                        }
                    }, 
                    {
                        "match_all": {}
                    }
                ],
                "must_not": [],
                "should": []
            }
        },
        "from": 0,
        "size": 10,
        "sort": [],
        # 聚合
        "aggs": {
            # "all_interests":{
            #     "terms":{
            #         "field":"interests"
            #     }
            # }
        }
    })

    return json.dumps(result)

def main():
    """入口"""

    # 连接es
    es_client = es_login()

    # result = search(es_client, query="123.125.115.110", field="_all")

    result = get(es_client, "AWTv-ROzCxZ1gYRliWhu")

    print result


if __name__ == "__main__":
    main()
```

## 删除ElasticSearch全部数据

`curl -X DELETE localhost:9200/test`, `test`为自己的`index`名称
