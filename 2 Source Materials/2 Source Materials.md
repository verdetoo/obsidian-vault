---
tags:
  - nooverview
cssclasses:
  - cards
  - cards-cover
  - cards-2-3
  - cards-cols-6
banner: "https://i.imgur.com/5vBx0nB.png"
banner_y: 0.44578
---

## Quick saves

[Finally, We're Healing! - Therapist Plays Disco Elysium: Part 3 52:06 min](https://youtu.be/szvlgo-AOD0?list=PLBmriQSLAuRJvLNLhRBNso054qvpsqv3t&t=3126)

## Folders

```dataview
TABLE WITHOUT ID
file.link as Title
WHERE  contains(file.path, this.file.name) AND contains(tags, "nooverview") AND !contains(file.path, this.file.path)
SORT file.name ASC
```

## Overview

```dataview
LIST
WHERE  contains(file.path, this.file.name) AND !contains(tags, "nooverview")
SORT file.link ASC
```

## Books
```dataview
TABLE WITHOUT ID
("![](" + image + ")") as Cover,
file.link as Title
WHERE  contains(file.path, this.file.name) AND !contains(tags, "nooverview") AND contains(type, "book")
SORT file.link ASC
```

## Games
```dataview
TABLE WITHOUT ID
("![](" + image + ")") as Cover,
file.link as Title
WHERE  contains(file.path, this.file.name) AND !contains(tags, "nooverview") AND contains(type, "game")
SORT file.link ASC
```

## Movies
```dataview
TABLE WITHOUT ID
("![](" + image + ")") as Cover,
file.link as Title
WHERE  contains(file.path, this.file.name) AND !contains(tags, "nooverview") AND contains(type, "movie")
SORT file.link ASC
```

## Series
```dataview
TABLE WITHOUT ID
("![](" + image + ")") as Cover,
file.link as Title
WHERE  contains(file.path, this.file.name) AND !contains(tags, "nooverview") AND contains(type, "series")
SORT file.link ASC
```

# Articles
```dataviewjs
const pdfFiles = app.vault.getFiles().filter(file => file.extension === 'pdf' && file.path.includes('Articles'))
dv.list(pdfFiles.map(file => dv.fileLink(file.path)))
```


# Writings



# Others