---
date: "[[<%tp.file.cday("DD.MM.YYYY")%>|<%tp.file.cday("DD.MM.YYYY")%>]]"
---
# Tasks linked

```dataviewjs
const name = dv.current().file.name;
dv.taskList(dv.pages().file.tasks.where(t => !t.completed && t.text.includes(name)));
```
