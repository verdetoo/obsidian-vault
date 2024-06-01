---
date: <% moment(tp.file.title,'YYYY').add(0,'days').format("DD MM YYYY") %>
---

# {{file.title}}
:ril_home_2: [[01 HOME|01 HOME]]
## Daily

```dataview
TABLE
FROM "journal/daily" 
WHERE file.cday.year = 2023
```

## Weekly

```dataview
TABLE
FROM "journal/weekly" 
WHERE file.cday.year = 2023
```

## Monthly

```dataview
TABLE
FROM "journal/monthly" 
WHERE file.cday.year = 2023
```