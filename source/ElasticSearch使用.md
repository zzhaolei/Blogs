---
title: ElasticSearch使用
copyright: true
date: 2018-07-27 19:38:10
tags:
categories:
---

# ElasticSearch使用

## 插入数据

示例
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
    global param

    with open(file_path, "r") as f:

        for line in f:
            # 去除每一行数据中的"\n"字符, 也可以替换为"\\n"
            line = line.replace("\n", "")
            dict_obj = json.loads(line)

            data = {}
            # 第一层
            for key_1, value_1 in dict_obj.iteritems():
                data[key_1] = {}
                if isinstance(value_1, dict):
                    # 第二层
                    for key_2, value_2 in value_1.iteritems():
                        data[key_1][key_2] = {}
                        if isinstance(value_2, dict):
                            # 第三层
                            for key_3, value_3 in value_2.iteritems():
                                data[key_1][key_2][key_3] = {}
                                if isinstance(value_3, dict):
                                    # 第四层
                                    for key_4, value_4 in value_3.iteritems():
                                        if isinstance(value_4, dict):
                                            value_4 = json.dumps(value_4)
                                        data[key_1][key_2][key_3][key_4] = value_4
                                else:
                                    data[key_1][key_3] = value_3
                        else:
                            data[key_1][key_2] = value_2
                else:
                    data[key_1] = value_1

            # 根据bulk_num的值发送一个批量插入请求
            action_list.append({
                "_op_type": "index",
                "_index": index,
                "_type": doc_type,
                "_source": data,
            })

            if len(action_list) >= bulk_num:
                try:
                    print "Start Bulk {}...".format(doc_type)
                    success, failed = bulk(es_client, action_list, index=index, raise_on_error=True, request_timeout=300)
                    print "End Bulk {}...".format(doc_type)
                except Exception as e:
                    print "出错了, Type:{}, 错误信息:{}".format(doc_type, e[0])
                finally:
                    del action_list[0:len(action_list)]

        # 如果不是bulk_num的等值, 那么就判断列表是否为空, 再次发送一次请求
        if len(action_list) > 0:
            try:
                print "Start Bulk {}...".format(doc_type)
                success, failed = bulk(es_client, action_list, index=index, raise_on_error=True, request_timeout=300)
                print "End Bulk {}...".format(doc_type)
            except Exception as e:
                print "出错了, Type:{}, 错误信息:{}".format(doc_type, e[0])
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

    # for result in results:
    #     print result.get()

def all_info(path):
    data = []
    for i in os.listdir(path):
        file_dict = {}
        if i.endswith(".json"):
            doc_type = i.split("_")[0]
            file_path = path + i
            file_dict["doc_type"] = doc_type
            file_dict["file_path"] = file_path
            data.append(file_dict)

    return data

def es_insert(process_func=None):
    """"""
    # 库
    index = "test"
    # 文件路径
    path="/home/data"
    
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

    # 插入数据
    es_insert()

    # 计算脚本执行时间
    end_time = time.time()
    print end_time - start_time
```

## 查询数据

`curl -XPOST "0.0.0.0:9200/test/_search?pretty" -d '{"query":{"bool":{"must":[{"query_string":{"default_field":"ip","query":"40.74.78.234"}}],"must_not":[],"should":[]}},"from":0,"size":10,"sort":[],"aggs":{}}'`

## 删除索引
`curl -X DELETE "localhost:9200/index"`

`index`是`索引名称`即`库名`

## 删除type
`curl -XPOST 'localhost:9200/index/type/_delete_by_query?conflicts=proceed&pretty' -H 'Content-Type: application/json' -d'{"query": {"match_all": {}}}'`

`index`是`索引名称`即`库名`, `type`是`类型`即`表`。
