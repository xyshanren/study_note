---
url: https://juejin.im/post/5c8220d86fb9a049d4429c01
source: juejin.im
date_saved: 2026-04-23
tags: 数据库, 设计
content_type: article
word_count: 4073
status: 已处理
---

# Leaf：美团分布式ID生成服务开源Leaf是美团基础研发平台推出的一个分布式ID生成服务，名字取自德国哲学家、数学家莱 - 掘金

> source: [juejin.im](https://juejin.im/post/5c8220d86fb9a049d4429c01)

## 核心要点

1. Leaf特性

Leaf在设计之初就秉承着几点要求：

全局唯一，绝对不会出现重复的ID，且ID整体趋势递增
2. 高可用，服务完全基于分布式架构，即使MySQL宕机，也能容忍一段时间的数据库不可用
3. 高并发低延时，在CentOS 4C8G的虚拟机上，远程调用QPS可达5W+，TP99在1ms内

## 内容预览

Leaf：美团分布式ID生成服务开源

 美团技术团队 

 2019-03-08

 2,928

 阅读8分钟

 关注

 美团小编 @美团

 Leaf是美团基础研发平台推出的一个分布式ID生成服务，名字取自德国哲学家、数学家莱布尼茨的一句话：“There are no two identical leaves in the world.”Leaf具备高可靠、低延迟、全局唯一等特点。目前已经广泛应用于美团金融、美团外卖、美团酒旅等多个部门。具体的技术细节，可参考此前美团技术博客的一篇文章：《Leaf美团分布式ID生成服务》。近日，Leaf项目已经在Github上开源：github.com/Meituan-Dia…，希望能和更多的技术同行一起交流、共建。

Leaf特性

Leaf在设计之初就秉承着几点要求：

全局唯一，绝对不会出现重复的ID，且ID整体趋势递增。

高可用，服务完全基于分布式架构，即使MySQL宕机，也能容忍一段时间的数据库不可用。

高并发低延时，在CentOS 4C8G的虚拟机上，远程调用QPS可达5W+，TP99在1ms内。

接入简单，直接通过公司RPC服务或者HTTP调用即可接入。

Leaf诞生

Leaf第一个版本采用了预分发的方式生成ID，即可以在DB之上挂N个Server，每个Server启动时，都会去DB拿固定长度的ID List。这样...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
