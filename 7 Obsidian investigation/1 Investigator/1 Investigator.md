---
banner: "https://64.media.tumblr.com/fd2c1860882d61f3b80dd5605eb36a0c/410f42f716618ea9-95/s1280x1920/66923b6bce323306b2a3b7d754cfb0a215c8e260.jpg"

cssclasses:
  - cards
  - cards-cols-3
  - cards-3-2
tags:
  - nooverview
---


# Overview
```dataview

TABLE WITHOUT ID
("![](" + cover + ")") as Cover,
file.link as Title,
"[[" + dateformat(file.ctime,"d.M.y") + "]]" as Created
WHERE  file.folder = this.file.folder AND !contains(file.path, this.file.path)
SORT file.link ASC
```

# Others

[Templater snippets](https://zachyoung.dev/posts/templater-snippets)