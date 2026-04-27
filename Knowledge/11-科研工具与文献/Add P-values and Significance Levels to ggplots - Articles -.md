---
url: http://www.sthda.com/english/articles/24-ggpubr-publication-ready-plots/76-add-p-values-and-significance-levels-to-ggplots/
source: www.sthda.com
date_saved: 2026-04-23
tags: 科研工具
content_type: article
word_count: 8000
status: 已处理
---

# Add P-values and Significance Levels to ggplots - Articles - STHDA

> source: [www.sthda.com](http://www.sthda.com/english/articles/24-ggpubr-publication-ready-plots/76-add-p-values-and-significance-levels-to-ggplots/)

## 核心要点

1. Install and load required R packages
2. Required R package: ggpubr (version >= 0.
3. 3), for ggplot2-based publication ready plots.
4. Or, install the latest developmental version from GitHub as follow:
5. devtools::install_github("kassambara/ggpubr")
## 内容预览

In this article, we’ll describe how to easily i) compare means of two or multiple groups; ii) and to automatically add p-values and significance levels to a ggplot (such as box plots, dot plots, bar plots and line plots …).

Contents

Prerequisites
Install and load required R packages

Demo data sets

Methods for comparing means

R functions to add p-values
compare_means()

stat_compare_means()

Compare two independent groups

Compare two paired samples

Compare more than two groups

Multiple grouping variables

Other plot types

Prerequisites

Install and load required R packages

Required R package: ggpubr (version >= 0.1.3), for ggplot2-based publication ready plots.

Install from CRAN as follow:

install.packages("ggpubr")

Or, install the latest developmental version from GitHub as follow:

if(!require(devtools)) install.packages("devtools")
devtools::install_github("kassambara/ggpubr")

Load ggpubr:

library(ggpubr)

Official documentation of ggpubr is available at: https://www.sthda.com/english/rpkgs/ggpubr

Demo data sets

Data: ToothGrowth data sets.

data("ToothGrowth")
head(ToothGrowth)
## len supp dose
## 1 4.2 VC 0.5
## 2 11.5 VC 0.5
## 3 7.3 VC 0.5
## 4 5.8 VC 0.5
## 5 6.4 VC 0.5
## 6 10.0 VC 0.5

Methods for comparing means

The standard methods to compare the means of two or more groups in R, have been largely described at: comparing means in R.

The most common methods for comparing means include:

Methods
R function
Description

T-test
t.test()
Compare two g...
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
