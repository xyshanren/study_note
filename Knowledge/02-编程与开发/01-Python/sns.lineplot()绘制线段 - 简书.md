---
url: https://www.jianshu.com/p/7442d1f86934
source: www.jianshu.com
date_saved: 2026-04-23
tags: Python, 算法, 学习-教程
content_type: article
word_count: 2623
status: 已处理
---

# sns.lineplot()绘制线段 - 简书

> source: [www.jianshu.com](https://www.jianshu.com/p/7442d1f86934)

## 核心要点

1. sns.lineplot()绘制线段
seaborn里的lineplot函数所传数据必须为一个pandas数组，

这一点跟matplotlib里有较大区别，并且一开始使用较为复杂，

但谁让seaborn好看呢，只有入坑了
2. 那么如何利用sns.lineplot画如matplotlib的线段呢(包含节点)
3. x: plot图的x轴label

y: plot图的y轴label

ci: 与估计器聚合时绘制的置信区间的大小

data: 所传入的pandas数组

当我们的pands数组里仅有两列数据时：

具体代码如下：(注意：1

## 内容预览

sns.lineplot()绘制线段
seaborn里的lineplot函数所传数据必须为一个pandas数组，

这一点跟matplotlib里有较大区别，并且一开始使用较为复杂，

但谁让seaborn好看呢，只有入坑了。

那么如何利用sns.lineplot画如matplotlib的线段呢(包含节点)？

首先sns.lineplot里有几个参数值得注意。

x: plot图的x轴label

y: plot图的y轴label

ci: 与估计器聚合时绘制的置信区间的大小

data: 所传入的pandas数组

当我们的pands数组里仅有两列数据时：

具体代码如下：(注意：1. matplotlib和seaborn是可以混用的; 2. seaborn画图更偏向数据统计)

import seaborn as sns
import pandas as pd
import numpy as np
from pandas import DataFrame
import matplotlib.pyplot as plt

fig = plt.figure(dpi=100)
plt.rc('font', family='STFangsong')
sns.set(style="darkgrid")

# ============ 设置xlabel及ylabel =========...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
