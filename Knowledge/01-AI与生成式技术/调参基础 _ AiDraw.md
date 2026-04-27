---
url: https://guide.novelai.dev/guide/configuration/param-basic
source: guide.novelai.dev
date_saved: 2026-04-23
tags: Stable-Diffusion, LLM, AI-绘图, 前端, 设计, 效率工具
content_type: article
word_count: 3691
status: 已处理
---

# 调参基础 | AiDraw

> source: [guide.novelai.dev](https://guide.novelai.dev/guide/configuration/param-basic)

## 核心要点

1. Skip to content Menu Return to top 

在这一页上

调参基础 #
本章内容大多基于 Stable Diffusion WebUI 前端。NovelAI 原始界面只开放了所有设置的一小部分
2. 常用参数介绍 #
Prompt（提示词）：对你想要生成的东西进行文字描述
3. Negative prompt（反向提示词）：用文字描述你不希望在图像中出现的东西

## 内容预览

Skip to content Menu Return to top 

在这一页上

调参基础 #
本章内容大多基于 Stable Diffusion WebUI 前端。NovelAI 原始界面只开放了所有设置的一小部分。
常用参数介绍 #
Prompt（提示词）：对你想要生成的东西进行文字描述。
Negative prompt（反向提示词）：用文字描述你不希望在图像中出现的东西。
Sampling Steps（采样步数）：扩散模型的工作方式是从随机高斯噪声向符合提示的图像迈出小步。这样的步骤应该有多少个。更多的步骤意味着从噪声到图像的更小、更精确的步骤。增加这一点直接增加了生成图像所需的时间。回报递减，取决于采样器。
Sampling method（采样器）：使用哪种采样器。Euler a（ancestral 的简称）以较少的步数产生很大的多样性，但很难做小的调整。随着步数的增加，非 ancestral 采样器都会产生基本相同的图像，如果你不确定的话，可以使用 LMS。
Batch count/n_iter：每次生成图像的组数。一次运行生成图像的数量为 Batch count * Batch size。
Batch size：同时生成多少个图像。增加这个值可以提高性能，但你也需要更多的 VRAM。图像总数是这个值乘以批次数。除 4090 等高级显卡以外通常保持为 1。
CFG ...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
