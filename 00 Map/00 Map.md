---
tags: nooverview
cssclasses:
  - cards
  - cards-cover
  - cards-2-1
  - cards-cols-4
---
[[STICKYYY]]
# general

:fas_tasks: [[Views/Tasks]]

:luc_settings: [[Obsidian Settings Outline]]

:ril_lock: [[Private]] 

:far_map: [[Investigating Obsidian]]


# content manager

:luc_database: [[Content - Editable]]

:ril_article: [[articles list inbox]]



# vault stats
 🗄️ recent media updates
 `$=dv.list(dv.pages('"content"').sort(f=>f.file.mtime.ts,"desc").limit(5).file.link)`

🔖 Last daily notes
`$=dv.list(dv.pages('"--- journal/daily"').sort(f=>f.file.name,"desc").limit(5).file.link)`
# stats
  File Count: `$=dv.pages().length`
  Content Count: `$=dv.pages('"2 Source Materials"').length`
  ------------ 
⌛ recent pages `$=dv.list(dv.pages().sort(f=>f.file.mtime.ts,"desc").limit(5).file.link)`


# Overview
```dataview
TABLE WITHOUT ID
("![](" + banner + ")") as Cover,
file.link as Title,
"[[" + dateformat(file.ctime,"dd.M.y") + "]]" as Created
FROM "/"
WHERE contains(tags, "nooverview") AND !contains(file.path,"8 Settings") AND !contains(file.path, this.file.name)
SORT file.link ASC
```

# Others