---
cssclasses:
  - guide
tags:
  - obsidian
  - snippets
---
↪[Collection](Collection.md)

# Properties on hover

---

- author:: The Useful Walrus
- source:: https://discord.com/channels/686053708261228577/702656734631821413/1144209029464342538

---

cover:: ![](https://i.imgur.com/JS0E9Sz.gif)

```css
/*
author: The Useful Walrus
source: https://discord.com/channels/686053708261228577/702656734631821413/1144209029464342538
*/

.metadata-properties-title {
  transition: 500ms;
  opacity: 0.2;
}
.metadata-container:hover .metadata-properties-title {
  opacity: 1;
  color: var(--text-accent);
}

.metadata-content {
  transition: 200ms cubic-bezier(0.25, 1, 0.5, 1);
  opacity: 0;
  height: 0;
  margin-bottom: -1.8em;
}
.metadata-container:hover .metadata-content {
  opacity: 1;
  height: auto;
  margin-bottom: 0.5em;
}
```


# Tasks linked

```dataviewjs
const name = dv.current().file.name;
dv.taskList(dv.pages().file.tasks.where(t => !t.completed && t.text.includes(name)));
```
