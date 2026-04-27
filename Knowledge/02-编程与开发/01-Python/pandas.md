---
url: http://www.huaxiaozhuan.com/%E5%B7%A5%E5%85%B7/pandas/chapters/pandas.html
source: www.huaxiaozhuan.com
date_saved: 2026-04-23
tags: Python
content_type: article
word_count: 6000
status: 已处理
---

# pandas

> source: [www.huaxiaozhuan.com](http://www.huaxiaozhuan.com/%E5%B7%A5%E5%85%B7/pandas/chapters/pandas.html)

## 核心要点

1. pandas 0.19
一、基本数据结构
Pandas的两个主要数据结构： Series和DateFrame
1
2. index：一个array-like或者一个Index对象。它指定了label。其值必须唯一而且hashable，且长度与data一致。如果data是一个字典，则index将会使用该字典的key（此时index不起作用）。如果未提供，则使用np.arange(n)
3. Index
class pandas.Index(data=None, dtype=None, copy=False, name=None,
fastpath=False, tupleize_cols=True)：创建Index对象

## 内容预览

pandas 0.19
一、基本数据结构
Pandas的两个主要数据结构： Series和DateFrame
1. Series
创建： class pandas.Series(data=None, index=None, dtype=None, name=None, copy=False,fastpath=False):
参数：
data：它可以是一个字典、array-like、标量。表示Series包含的数据，如果是序列/数组，则它必须是一维的
如果是字典，则字典的键指定了label。如果你同时使用了index，则以index为准。
如果是标量，则结果为：该标量扩充为index长度相同的列表。
index：一个array-like或者一个Index对象。它指定了label。其值必须唯一而且hashable，且长度与data一致。如果data是一个字典，则index将会使用该字典的key（此时index不起作用）。如果未提供，则使用np.arange(n)。
name：一个字符串，为Series的名字。
dtype：指定数据类型。如果为None，则数据类型被自动推断
copy：一个布尔值。如果为True，则拷贝输入数据data

还可以通过类方法创建Series：Series.from_array(arr, index=None, name=None, dtype=None,
c...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
