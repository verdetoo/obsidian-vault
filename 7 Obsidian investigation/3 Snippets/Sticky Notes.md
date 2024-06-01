---
cssclasses:
  - guide
tags:
  - obsidian
  - snippets
---

[Sticky Notes for Obsidian : r/ObsidianMD](https://www.reddit.com/r/ObsidianMD/comments/19c0ymc/sticky_notes_for_obsidian/)

> [!STICKY|red]
> Color red

> [!STICKY|aqua right]
> Color aqua


> [!STICKY|orange left s-45]
> Color orange


> [!STICKY]
> The default sticky 
> (takes default color from Style Settings).


> [!STICKY|yellow left]
> Sticky Note
> This one floats left


> [!STICKY|green right title]
> Sticky Note
> This one floats right and has its first line bold


> [!STICKY|blue center s-45]
> Sticky Note
> This one is centered and a bit larger


> [!STICKY|aside left purple title]
> Sticky Note
> This one is put aside, purple with its first line bold


# Tasks linked

```dataviewjs
const name = dv.current().file.name;
dv.taskList(dv.pages().file.tasks.where(t => !t.completed && t.text.includes(name)));
```
