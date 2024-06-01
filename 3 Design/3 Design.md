---
tags: nooverview
cssclasses:
  - cards
  - cards-cols-3
  - cards-3-2
---


# Overview
```dataview

TABLE WITHOUT ID
("![](" + cover + ")") as Cover,
file.link as Title,
"[[" + dateformat(file.ctime,"dd.M.y") + "]]" as Created
WHERE  file.folder = this.file.folder AND !contains(tags, "nooverview") AND !contains(file.name,".excalidraw")
SORT file.link ASC
```

# Others