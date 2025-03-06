---
title: 日报模板
author:
  - 守一
created: 2025-03-06
description: 日报模板（代码还有问题，后续找时间验证）
tags:
  - template
---
[[日志/Daily/<% tp.date.now("YYYY-MM-DD", -1, tp.file.title, "YYYY-MM-DD")%>|⬅昨天]] | [[日志/Daily/<% tp.date.now("YYYY-MM-DD", 1, tp.file.title, "YYYY-MM-DD-dddd")%>|明天 ➡️]] | [[日志/Weekly/<%moment(tp.file.title, "YYYY-MM-DD").format("gggg-[W]ww")%>|本周 📅]]

## 截止到今天的任务

```tasks
path includes Daily
not done
due on <%moment(tp.file.title, "YYYY-MM-DD").format("YYYY-MM-DD")%>
hide due date
hide created date
hide priority
#hide backlink
hide edit button
sort by priority
```

## 进行中的任务

```tasks
path includes Daily
not done
due after <%moment(tp.file.title, "YYYY-MM-DD").format("YYYY-MM-DD")%>
#hide due date
hide created date
hide priority
#hide backlink
hide edit button
sort by priority
```

## 今天任务

## 日志

## 阅读列表

```dataview
TABLE elink(url, "🔗") as URL from "阅读/碎片/文章"
WHERE date > date(<%moment(tp.file.title).format("YYYY-MM-DD")%>) - dur(1day) and date <= date(<%moment(tp.file.title).format("YYYY-MM-DD")%>) 
```