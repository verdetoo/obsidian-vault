---
tags:
  - nooverview
cssclasses:
  - cards
  - cards-cols-4
  - cards-cover
---
[[Things to check out]] — [[Obsidian Settings Outline]] — [[Plugin updates]] — [[Safe Box]]


# Overview
```dataview
LIST
WHERE contains(file.path, this.file.name) AND !contains(tags, "nooverview")
SORT file.link ASC
```

# Investigator
```dataview
TABLE WITHOUT ID
file.link as Title,
"[[" + dateformat(file.ctime,"dd.M.y") + "]]" as Created
WHERE contains(file.path, this.file.name) AND contains(file.path, "Investigator") AND !contains(tags, "nooverview")
SORT file.link ASC
```

# Plugins

```dataview
TABLE WITHOUT ID
file.link as Title,
"[[" + dateformat(file.ctime,"dd.M.y") + "]]" as Created
WHERE contains(file.path, this.file.name) AND contains(file.path, "Plugins") AND !contains(tags, "nooverview")
SORT file.link ASC
```

# Snippets

```dataview
TABLE WITHOUT ID
file.link as Title,
"[[" + dateformat(file.ctime,"dd.M.y") + "]]" as Created
WHERE contains(file.path, this.file.name) AND contains(file.path, "Snippets") AND !contains(tags, "nooverview")
SORT file.link ASC
```