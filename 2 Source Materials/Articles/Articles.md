---
tags: nooverview
banner: "https://i.imgur.com/UbNXWBm.jpeg"

cssclasses:
  - cards
  - cards-cover
  - cards-2-1
  - cards-cols-4

banner_y: 0.652
---
![|200](https://i.imgur.com/OUq9tFM.png)


# PDFs

```dataviewjs
const pdfFiles = app.vault.getFiles().filter(file => file.extension === 'pdf' && file.path.includes('Articles'))
dv.list(pdfFiles.map(file => dv.fileLink(file.path)))
```

# Annotations
```dataview
TABLE WITHOUT ID
("![](" + cover + ")") as cover,
file.link as title,
sources,
"[[" + dateformat(file.ctime,"dd.MM.y") + "]]" as created
WHERE !contains(tags, "nooverview") AND contains(type, "annotations")
SORT file.link ASC
```

# Others