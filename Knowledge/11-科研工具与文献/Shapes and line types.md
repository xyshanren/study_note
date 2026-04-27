---
url: http://www.cookbook-r.com/Graphs/Shapes_and_line_types/
source: www.cookbook-r.com
date_saved: 2026-04-23

content_type: article
word_count: 4132
status: 已处理
---

# Shapes and line types

> source: [www.cookbook-r.com](http://www.cookbook-r.com/Graphs/Shapes_and_line_types/)

## 核心要点

1. Problem

 Solution 
 Standard graphics

 ggplot2

 Note

Problem

You want to use different shapes and line types in your graph
2. Symbols 19 and 21-25 have an outline around the filled area, and will render with smoothed edges on most platforms
3. For symbols 21-25 to appear solid, you will also need to specify a fill (bg) color that is the same as the outline color (col); otherwise they will be hollow

## 内容预览

Problem

 Solution 
 Standard graphics

 ggplot2

 Note

Problem

You want to use different shapes and line types in your graph.

Solution

Note that with bitmap output, the filled symbols 15-18 may render without proper anti-aliasing; they can appear jagged, pixelated, and not properly centered, though this varies among platforms. Symbols 19 and 21-25 have an outline around the filled area, and will render with smoothed edges on most platforms. For symbols 21-25 to appear solid, you will also need to specify a fill (bg) color that is the same as the outline color (col); otherwise they will be...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
