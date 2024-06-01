---
cssclasses:
  - guide
tags:
  - obsidian
  - plugins
---
# Tasks linked

```dataviewjs
const name = dv.current().file.name;
dv.taskList(dv.pages().file.tasks.where(t => !t.completed && t.text.includes(name)));
```
