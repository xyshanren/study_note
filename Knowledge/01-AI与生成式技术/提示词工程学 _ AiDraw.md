---
url: https://guide.novelai.dev/guide/prompt-engineering/
source: guide.novelai.dev
date_saved: 2026-04-23
tags: Stable-Diffusion, Midjourney, LLM, AI-绘图, 学习-教程, 设计
content_type: article
word_count: 1699
status: 已处理
---

# 提示词工程学 | AiDraw

> source: [guide.novelai.dev](https://guide.novelai.dev/guide/prompt-engineering/)

## 核心要点

1. Skip to content Menu Return to top 

在这一页上

提示词工程学 #
这节会介绍绘图所需要用到的提示词，和相关的 SD-WebUI 网页应用资源。如果你会画画，那么效果会更加稳定可观
2. 迭代方式，有循环迭代和线性迭代两种，线性迭代适用于多样性测试，而 循环迭代 是优化的更好选择
3. 目前研究基本方向是：
提示词 + PS/Inpaint(微修/嫁接)
提示词 + 3D 参考
关于生成时涉及到的参数配置，见 参数介绍

## 内容预览

Skip to content Menu Return to top 

在这一页上

提示词工程学 #
这节会介绍绘图所需要用到的提示词，和相关的 SD-WebUI 网页应用资源。如果你会画画，那么效果会更加稳定可观。
基本流程 #

这幅图演示了循环迭代的流程。
迭代方式，有循环迭代和线性迭代两种，线性迭代适用于多样性测试，而 循环迭代 是优化的更好选择。
来回改提示 + 固定种子并不是好选择。
目前研究基本方向是：
提示词 + PS/Inpaint(微修/嫁接)
提示词 + 3D 参考
关于生成时涉及到的参数配置，见 参数介绍。
提示词来源 #
提示词的来源主要取决于以下三个要素：
模型所采用的自然语言处理 (NLP) 方案支持的词汇表
模型初始训练材料标记词来源
TIP
下文提供的词库、网站、Wiki 均主要适用于 NovelAI 泄露模型。使用其他模型时应当参照对应资料进行调整。

以 NovelAI 泄露模型为例，该模型主要采用 Danbooru 网站上的用户标签参与训练，因此使用这些标签进行图像生成可获得最佳效果。使用 MidJourney 时则可参照 MidJourney-Styles-and-Keywords-Reference 提供的参考数据，
Danbooru 网站的 Wiki 提供了一部分 标签分类与说明，这些文档可被视作权威来源。例如，笔触 / 绘画工具 ...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
