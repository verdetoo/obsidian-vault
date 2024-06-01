---
date: <% moment(tp.file.title,'DD MM YYYY').add(0,'days').format("DD MM YYYY") %>
---
# <% moment(tp.file.title,'DD.MM.YYYY').add(0,'days').format("DD MM YYYY") %>
[[— journal/daily/<% moment(tp.file.title,'DD MM YYYY').add(-1,'days').format("DD MM YYYY") %>|Yesterday]] <- [[01 HOME|01 HOME]] -> [[— journal/daily/<% moment(tp.file.title,'DD MM YYYY').add(1,'days').format("DD MM YYYY") %>|Tomorrow]] :ril_calendar: [[— journal/weekly/<% moment(tp.file.title,'DD MM YYYY').add(0,'days').format("gggg [W]WW")%>|<% moment(tp.file.title,'DD MM YYYY').add(0,'days').format("gggg [W]WW")%>]]


`INPUT[datePicker:date]`

## Journal




## Upcoming events

```dataview
TABLE WITHOUT ID
file.link as Day,
regexreplace(Tasks.text, "[⏫🔼🔽]? \[.*$", "") AS Task
FROM #event AND "journal"
FLATTEN file.tasks AS Tasks
SORT file.name ASC
```

## Today's tasks



## Past undone tasks
```dataview
TASK
FROM "journal"
WHERE !fullyCompleted and file.ctime < this.file.ctime
GROUP BY file.link
```
## References