---
banner: "https://64.media.tumblr.com/4da94d5336bcc987233b743bb6dbf407/f6c3bbe3c6ddc0cd-b8/s2048x3072/86b61731ffd88c250973bc01f2f8cf6724f16b0f.jpg"
tags: nooverview
cssclasses:
  - cards
  - cards-cover
  - cards-cols-4
  - cards-2-1
banner_y: 0.69246
---


# Overview
```dataview

TABLE WITHOUT ID
("![](" + cover + ")") as Cover,
file.link as Title,
draft-tags as Tags,
dateformat(file.ctime,"dd.MM.y") as Created
WHERE  file.folder = this.file.folder AND !contains(tags, "nooverview")
SORT file.mtime DESC
```


# Shelve
```dataview

TABLE WITHOUT ID
("![](" + cover + ")") as Cover,
file.link as Title,
dateformat(file.ctime,"dd.MM.y") as Created
WHERE  contains(file.folder, "shelve") AND !contains(tags, "nooverview")
SORT file.mtime DESC
```

# Others