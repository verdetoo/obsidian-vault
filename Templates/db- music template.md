---
date: <% tp.file.creation_date() %>
---

# {{title}} by {{artists}}
```dataview
table without id ("![|100](" + image + ")") as "Cover", myrating as "Rating"
where contains(file.name, this.file.name) AND contains(tags, "mediaDB/music")
```

