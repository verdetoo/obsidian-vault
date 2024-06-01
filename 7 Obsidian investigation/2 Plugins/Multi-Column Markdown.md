---
cssclasses:
  - guide
tags:
  - obsidian
  - plugins
---
Text displayed above.

```start-multi-column  
ID: ExampleRegion1  
number of columns: 2  
largest column: left  
```

Text displayed in column 1.

--- end-column ---

Text displayed in column 2.

--- end-multi-column

Text displayed below.




# Tasks linked

```dataviewjs
const name = dv.current().file.name;
dv.taskList(dv.pages().file.tasks.where(t => !t.completed && t.text.includes(name)));
```
