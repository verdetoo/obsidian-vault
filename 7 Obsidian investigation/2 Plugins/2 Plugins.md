---
tags:
  - nooverview
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
WHERE  file.folder = this.file.folder AND !contains(tags, "nodataviewjs") AND file.link != this.file.link
SORT file.link ASC
```

# Others