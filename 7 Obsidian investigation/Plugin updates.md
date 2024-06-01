---
date: 2024-04-06
zettelkasten:
  - fleeting
tags:
  - obsidian
  - updates
---

```table-of-contents
title: <font color="#7030a0"><strong>ALL DATES</strong></font>
style: inlineFirstLevel
includeLinks: true
debugInConsole: false
```

Last Check `INPUT[date:date]`

Zettelkasten `INPUT[suggester(option(fleeting), option(literature), option(permanent)):zettelkasten]`


## [[31.03.2024]]
## [[Excalidraw - plugin]]
> [!info] [[Excalidraw - plugin]] - [2.0.26](https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/2.0.26)

```vid
https://www.youtube.com/watch?v=tHUcD4rWIuY
```

- Minor updates from Excalidraw.com. The key change is text measurements that should result in consistent text sizing between desktop and mobile devices.
- Now you can embed the markdown section of an Excalidraw note to your drawing. Simply select Insert ANY file, choose the drawing, and select the relevant heading section to embed.
This also works with "back-of-the-drawing" markdown sections. Use the context menu Add back-of-note Card. The action is also available on the Command Palette and in the Excalidraw-Obsidian Tools Panel.
- Editing an embedded markdown note is now easier. Just press Enter when the element is selected. #1650
- The crosshair cursor is now hidden when the freedraw tool is active and using a pen. #1659
- Convert markdown note to Excalidraw Drawing now converts an existing markdown note (not just empty notes) into a drawing. The original markdown note will be on the "back side of the drawing".
- Introducing Annotate image in Excalidraw, which works very similarly to Crop and mask image. You can replace an image in a markdown note or on the Obsidian Canvas with an Excalidraw drawing containing that image. You will be able to annotate the image in Excalidraw.
- Now you can reference frames in images embedded in markdown and canvas with frame names e.g.
`! [[ drawing#^frame=Frame 01 ]]`
- Excalidraw file format change:
	- New frontmatter switch excalidraw-open-md: If set to true, the file by default will open as a markdown file. You can switch to Excalidraw View Mode via the command palette action or by right-clicking the tab.
	- Easter Egg: If you add a comment in front of # Text Elements, then the entire Excalidraw data: markdown and JSON will be commented out, thus invisible when exporting to the web. If you remove the comment from before # Text Elements, then only the JSON will be commented out.

# [[12.10.2023]]

## [[Tasks - plugin]]
> [!info] Tasks - [4.9.0](https://github.com/obsidian-tasks-group/obsidian-tasks/releases/tag/4.9.0)

**Add `task.priorityNameGroupText` and `task.status.typeGroupText`**

 `group by function task.priorityNameGroupText + ': ' + task.status.typeGroupText`
![|300](https://i.imgur.com/Jg58Mj3.png) 

`group by function task.due.fromNow.groupText`
![|300](https://i.imgur.com/rPcWVZj.png)

**Add custom grouping by 'date category' and 'time from now'**
 `group by function task.due.category.groupText`
 ![|300](https://i.imgur.com/MrRCV2T.png)

## [[Metadata menu - plugin]]
> [!info] metadata menu - [0.5.2](https://github.com/mdelobelle/metadatamenu/releases/tag/0.5.2)

**Improvement:** Ability to Format the field name when inserting as an inline field
**Fix:** Metadata menu form, set/update display duplicates  
Fixed input field for null value with number field, now interpreted as 0 so as to enable increment and decrement more easily.


# [[09.03.2024]]

## [[Views/Tasks]] - plugin
> [!info] Tasks - [5.6.0-6.1.1](https://github.com/mdelobelle/Tasks/releases)

### 6.1.0: Task Dependencies feature & Edit Task modal fixes

#### 🌟 Edit Task modal status-editing is fixed

Editing task statuses via the modal now correctly updates Done and Cancelled dates, and creates the next task when completing a recurring task.

![](https://i.imgur.com/gneycjw.png)

#### 🌟 Task Dependencies facility - thank you @DanielTMolloy919!

The Tasks plugin now allows for 'Finish to start (FS)' dependencies, meaning Task A needs to be finished before you start on Task B. You can learn more about this concept on Wikipedia.

User Documentation: Task Dependencies
- Below: Documentation sample: Editing Dependencies

![](https://i.imgur.com/F7w1sHd.png)


- Below: Documentation sample: Search Concepts for Dependencies
![](https://i.imgur.com/fGdKF29.png)

### 5.6.0

- Add 'today' and 'tomorrow' to Postpone context menu by [@claremacrae](https://github.com/claremacrae) in [#2566](https://github.com/obsidian-tasks-group/obsidian-tasks/pull/2566)

![](https://i.imgur.com/zJ4MfaM.png)

## [[Metadata menu|Metadata menu - plugin]]
> [!info] metadata menu - [0.8.4](https://github.com/mdelobelle/metadatamenu/releases/tag/0.8.4)

- fieldModifier (dataviewjs block, fileclass tableview, mdm code block) for nested fields. use a dotted syntax to access your nested field and get a field modifier for them in your table: fieldModifier(dv, p, "parent[1].child[3].foo") or fieldModifier(dv, p, "parent[0].child[4]") or....
- custom sorting method for File and MultiFile not broken anymore
- better navigation in modals for nested objects and objectLists
- number in Input field not broken anymore
- encapsulating strings in double quote to prevent frontmatter parsing from clearing unsupported syntax
