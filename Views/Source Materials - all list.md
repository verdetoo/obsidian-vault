# content in list view
```dataview
TABLE WITHOUT ID
class as Types,
rows.file.link as Contents
FROM "02 Source Materials"
WHERE !contains(class, empty)
GROUP BY class
SORT class ASC
```

# Articles in list view : inbox last 20
```dataview
LIST 
FROM "02 Source Materials/Articles"
WHERE p.status = inbox
SORT file.name DESC
LIMIT 20
```