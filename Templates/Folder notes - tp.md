
# Overview
```dataview

TABLE WITHOUT ID

file.link as Notes,
dateformat(file.mtime,"d.M.y") as Modified,
dateformat(file.ctime,"d.M.y") as Created

WHERE  file.folder = this.file.folder
SORT file.link ASC
```


