---
title: 周报模板
author:
  - 守一
created: 2025-03-06
description: 周报模板（代码还有问题，后续找时间验证）
tags:
  - template
---
## <%moment(tp.file.title).startOf('isoWeek').format("MMM-DD")%> ~ <%moment(tp.file.title).endOf('isoWeek').format("MMM-DD")%>

[[<%moment(tp.file.title).subtract(1, 'week').format("gggg-[W]ww")%>|⬅️ 上周]] | [[<%moment(tp.file.title).subtract(-1, 'week').format("gggg-[W]ww")%>|下周 ➡️]]

## 日期

- [[日志/Daily/<%moment(tp.file.title).startOf('isoWeek').add(0,'day').format('YYYY-MM-DD')%>|周一]]
- [[日志/Daily/<%moment(tp.file.title).startOf('isoWeek').add(1,'day').format('YYYY-MM-DD')%>|周二]]
- [[日志/Daily/<%moment(tp.file.title).startOf('isoWeek').add(2,'day').format('YYYY-MM-DD')%>|周三]]
- [[日志/Daily/<%moment(tp.file.title).startOf('isoWeek').add(3,'day').format('YYYY-MM-DD')%>|周四]]
- [[日志/Daily/<%moment(tp.file.title).startOf('isoWeek').add(4,'day').format('YYYY-MM-DD')%>|周五]]
- [[日志/Daily/<%moment(tp.file.title).startOf('isoWeek').add(5,'day').format('YYYY-MM-DD')%>|周六]]
- [[日志/Daily/<%moment(tp.file.title).startOf('isoWeek').add(6,'day').format('YYYY-MM-DD')%>|周日]]

## 本周未完成的任务

```tasks
path includes Daily
not done
(scheduled on or after <%moment(tp.file.title).startOf('isoWeek').add(0,'day').format("YYYY-MM-DD")%>) OR (due on or after <%moment(tp.file.title).startOf('isoWeek').add(0,'day').format("YYYY-MM-DD")%>)
(scheduled on or before <%moment(tp.file.title).startOf('isoWeek').add(6,'day').format("YYYY-MM-DD")%>) OR (due on or before <%moment(tp.file.title).startOf('isoWeek').add(6,'day').format("YYYY-MM-DD")%>)
hide due date
hide created date
hide priority
hide edit button
#hide backlink
sort by priority
```

## 本周已完成的任务

```tasks
path includes Daily
done
(scheduled on or after <%moment(tp.file.title).startOf('isoWeek').add(0,'day').format("YYYY-MM-DD")%>) OR (due on or after <%moment(tp.file.title).startOf('isoWeek').add(0,'day').format("YYYY-MM-DD")%>)
(scheduled on or before <%moment(tp.file.title).startOf('isoWeek').add(6,'day').format("YYYY-MM-DD")%>) OR (due on or before <%moment(tp.file.title).startOf('isoWeek').add(6,'day').format("YYYY-MM-DD")%>)
hide due date
hide created date
hide priority
hide edit button
#hide backlink
hide done date
sort by priority
```

## 阅读列表

```dataview
LIST from "阅读/碎片/文章"
WHERE date >= date(<%moment(tp.file.title).startOf('isoWeek').add(0,'day').format("YYYY-MM-DD")%>) AND date <= date(<%moment(tp.file.title).startOf('isoWeek').add(6,'day').format("YYYY-MM-DD")%>)
```