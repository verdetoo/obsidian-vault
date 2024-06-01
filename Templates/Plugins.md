---
date: "[[<%tp.file.cday("DD.MM.YYYY")%>|<%tp.file.cday("DD.MM.YYYY")%>]]"
cssclasses:
  - guide
tags:
  - obsidian
  - plugins
  - nooverview
---
# Tasks linked

```dataviewjs
const name = dv.current().file.name;
dv.taskList(dv.pages().file.tasks.where(t => !t.completed && t.text.includes(name)));
```
