---
date: <% moment(tp.file.title,'MMMM YYYY').add(0,'days').format("DD MM YYYY") %>
---

# <% moment(tp.file.title,'MMMM YYYY').add(0,'days').format("MMMM YYYY") %>
[[journal/monthly/<% moment(tp.file.title,'MMMM YYYY').add(-1,'months').format("MMMM YYYY") %>| ↶ Previous Month]] | [[01 HOME|01 HOME]] | [[journal/monthly/<% moment(tp.file.title,'MMMM YYYY').add(1,'months').format("MMMM YYYY") %>|Next Month ↷]] 

## Daily notes

```dataview
TABLE WITHOUT ID
file.link as days, week
FROM "journal/daily" 
SORT file.name ASC
WHERE file.ctime > date(som) AND file.ctime < date(eom)
```

## Weekly pages

```dataview
LIST
FROM "journal/weekly"
WHERE file.ctime > date(som) AND file.ctime < date(eom)
SORT file.name ASC
```


## Uncompleted tasks

### This month

```dataview
TASK
FROM "journal"
WHERE !fullyCompleted and file.ctime = date(this.month)
GROUP BY file.link
```

### Past months
```dataview
TASK
FROM "journal"
WHERE !fullyCompleted and file.ctime < date(this.month)
GROUP BY file.link
```
