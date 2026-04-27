---
url: https://chf2012.github.io/2017/05/17/%E8%BD%AF%E4%BB%B6%E5%BA%94%E7%94%A8_%E7%A8%8B%E5%BA%8F%E7%BC%96%E7%A8%8B/Python/Python_%E4%B8%93%E9%A2%98%E6%80%BB%E7%BB%93/Python_%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86_pandas_old/
source: chf2012.github.io
date_saved: 2026-04-23
tags: Python
content_type: article
word_count: 8000
status: 已处理
---

# Python_数据处理_pandas | Jimmy那些事儿

> source: [chf2012.github.io](https://chf2012.github.io/2017/05/17/%E8%BD%AF%E4%BB%B6%E5%BA%94%E7%94%A8_%E7%A8%8B%E5%BA%8F%E7%BC%96%E7%A8%8B/Python/Python_%E4%B8%93%E9%A2%98%E6%80%BB%E7%BB%93/Python_%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86_pandas_old/)

## 核心要点

1. html#function-application-groupby-window
2. Series( [], index=[], name=’’ )：创建Series
3. rand(20,5) , index=[], columns=[] )：创建20行5列的随机数组成的DataFrame对象
4. DataFrame会自动创建索引，且会被有序排列； Index索引对象是不可修改的。
5. DataFrame(obj, colums=[], index=[])
## 内容预览

pandas 指引 : http://pandas.pydata.org/pandas-docs/stable/api.html#function-application-groupby-window

DataFrame 单独取出一列是 Series 格式

[TOC]

创建与变更格式
创建对象

pd.Series( [], index=[], name=’’ )：创建Series

pd.DataFrame(np.random.rand(20,5) , index=[], columns=[] )：创建20行5列的随机数组成的DataFrame对象

DataFrame会自动创建索引，且会被有序排列； Index索引对象是不可修改的。除非在第一次写入时缺少索引列，可进行定义。

传入等长的列表或NumPy数组组成的 字典。

若传入的列在数据中查询不到，就会产生NA

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
DataFrame(obj, colums=[], index=[])
# columns 按照指定的列进行显示
# index 指定索引的名称，但并不改变行的顺序; 若对已存在的索引（eg:嵌套字典），若index中的索引不存在，则不显示

# 创建对象：运用字典--------------------
data = 'state': ['Ohi', 'Ohio', 'Ohio', 'Nevada', 'Nevada'], 'year': [2000,
2001, 2002, 2001, 2002], 'pop': [1.5, 1.7, 3.6, 2.4, 2.9]
frame = DataFrame(data, columns = ['year', 'state', 'pop'], index=[1,2,3,4,2]) # 按照指定列进行排序

# 创建对象：运用列表
pd.DataFrame([[4,7],[5,10]],columns=['a','b'],index=[1,2])

# ------嵌套字典-----------
pop = 'Nevada': 2001: 2.4, 2002: 2.9, 'Ohio': 2000: 1.5, 2001: 1.7,
2002: 3.6
frame3 = DataFrame(pop)

查看数据

df.shape()：查看行数和列数

df.head(n)：查看DataFrame对象的前n行

df.tail(n)：查看DataFrame对象的最后n行

df.describe()：查看数值型列的汇总统计

df.columns ：查看列名

s.value_counts(dropna=False)：查看Series对象的唯一值和计数

df.apply(pd.Series.value_counts)：查看DataFrame对象中每一列的唯一值和计数

http://df.info()：查看索引、数据类型和内存信息

数据整理
重命名 - rename

df.set_index(‘column_one’)：更改索引列

df.columns：获取当前列名

df.columns = [‘a’,’b’,’c’]：重命名列名

df.rename( columns = {‘a’ : ‘A’} , inplace = True )：只修改特定的列；将’a’ 改为 ‘A’
df.rename(columns=lambda x: x + 1)：...
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
