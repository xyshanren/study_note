---
url: https://blog.csdn.net/sinat_19473755/article/details/89496138
source: blog.csdn.net
date_saved: 2026-04-23
tags: DevOps, 算法
content_type: article
word_count: 6000
status: 已处理
---

# 群晖NAS使用SMB加快传输速度（非链路聚合）_smb传输速度-CSDN博客

> source: [blog.csdn.net](https://blog.csdn.net/sinat_19473755/article/details/89496138)

## 核心要点

1. 文章标签：

 #群晖
 #NAS
 #网络

 NAS
 同时被 2 个专栏收录

 1 篇文章

 订阅专栏

 群晖

 1 篇文章

 订阅专栏

 本文介绍如何利用群晖NAS的SMB3Mutichannel功能，通过双网口实现文件传输速度翻倍，从理论上的100MB/s提升至实测的200MB/s，无需使用链路聚合交换机
2. STEP3 验证

以管理员身份启动CMD然后在里面输入,看到双通道说明成功

PowerShell
Get-SmbMultichannelConnection

看到以上显示说明已经设置成功。尝试复制NAS文件
3. 参考
 https://www.vediotalk.com/?p3261

https://forum.synology.com/enu/viewtopic.php?t128482

http://koolshare.cn/thread-96854-1-1.html

 确定要放弃本次机会

## 内容预览

群晖NAS使用SMB加快传输速度（非链路聚合）

 最新推荐文章于 2026-03-06 07:31:36 发布

 原创

 最新推荐文章于 2026-03-06 07:31:36 发布
 ·
 5.7w 阅读

 ·

 8

 ·

 43

 ·

 CC 4.0 BY-SA版权

 版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。

 文章标签：

 #群晖
 #NAS
 #网络

 NAS
 同时被 2 个专栏收录

 1 篇文章

 订阅专栏

 群晖

 1 篇文章

 订阅专栏

 本文介绍如何利用群晖NAS的SMB3Mutichannel功能，通过双网口实现文件传输速度翻倍，从理论上的100MB/s提升至实测的200MB/s，无需使用链路聚合交换机。

 群晖NAS使用SMB3 Muti channel加快传输速度

 STEP1
STEP2
STEP3 验证

 群晖NAS支持多网口链路聚合这是提升了NAS的带宽并不能提升单个文件的读写速度。对于家庭和个人用户来说链路聚合并不是他们想要的其中的瓶颈是单根千兆网线的最大传输速度100MB/s.那么如何实实在在的用到群晖多个网口带来的好处来提高文件读写速度呢本文介绍如何通过...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
