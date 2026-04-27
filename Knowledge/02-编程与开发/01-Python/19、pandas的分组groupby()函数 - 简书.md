---
url: https://www.jianshu.com/p/4093138ac635
source: www.jianshu.com
date_saved: 2026-04-23
tags: Python, 后端, 学习-教程
content_type: article
word_count: 2662
status: 已处理
---

# 19、pandas的分组groupby()函数 - 简书

> source: [www.jianshu.com](https://www.jianshu.com/p/4093138ac635)

## 核心要点

1. 19、pandas的分组groupby()函数
在数据处理的过程中，经常也要进行简单的运算，如果能够配合函数进行使用就会得到更好的结果
2. 源数据

1、加+， 减-，乘* ，除/ ，乘方**，取余数 %
根据Series和DataFrame的结构特征，进行运算时，结构中的每个数据都会计算
3. 也可以指定每行进行求和（指定参数axis=1进行每行求和，每行求和默认只对数值型数据进行求和）：

指定axis=1进行每行求和

还有如果DataFrame有多层索引，可以添加参数 level='索引层的名称', df.sum(level='索引层名称')就能根据索引层次的数据进行分组运算

## 内容预览

19、pandas的分组groupby()函数
在数据处理的过程中，经常也要进行简单的运算，如果能够配合函数进行使用就会得到更好的结果。

源数据

1、加+， 减-，乘* ，除/ ，乘方**，取余数 %
根据Series和DataFrame的结构特征，进行运算时，结构中的每个数据都会计算。

同一列也可以相加

除法运算

2、还有一些常用的运算函数：
count()                    计数，非空值的数量
min(), max()            计算最小值和最大值
argmin,argmax        计算能够获取到最小值和最大值的索引位置（整数）
idxmin,idxmax        计算能够获取到最小值和最大值的索引值
sum()                      求和
mean ()                    值的平均数， a.mean() 默认对每一列的数据求平均值；若加上参数  a.mean(1)则对每一行求平均值
media()                    值的算术中位数（50%分位数)
quantile(0.25)          计算样本的百分位数 
var()                          样本值的方差 
std ()                        样本值的标...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
