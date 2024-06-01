---
date:  <% moment(tp.file.title,'gggg [W]WW').add(0,'days').format("DD MM YYYY") %>
---

# <% moment(tp.file.title,'gggg [W]WW').add(0,'days').format("gggg [W]WW") %>
[[— journal/weekly/<% moment(tp.file.title,'gggg [W]WW').add(-7,'days').format("gggg [W]WW") %>| ↶ Previous Week]] | [[01 HOME|01 HOME]] | [[— journal/weekly/<% moment(tp.file.title,'gggg [W]WW').add(7,'days').format("gggg [W]WW") %>|Next Week ↷]] 

# Days of the week

```dataview
TABLE WITHOUT ID
	week,
	file.link as "days"
FROM "journal/daily" 
WHERE econtains(file.outlinks, this.file.link)
SORT file.name ASC
```


# This week's tasks
```dataview
TASK
FROM "journal/daily" 
WHERE econtains(file.outlinks, this.file.link)
GROUP BY file.link
SORT file.link ASC
```
## Uncomplete past tasks
```dataview
TASK
FROM "journal"
WHERE !econtains(file.outlinks, this.file.link) AND !fullyCompleted
GROUP BY file.link
SORT ASC
```
