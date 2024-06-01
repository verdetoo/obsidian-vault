## [[7 Obsidian investigation/2 Plugins/Tasks]] - plugin

### Overdue
dataview
TASK
FROM "journal"
WHERE !fullyCompleted
GROUP BY file.link



### To do tasks
dataview
TASK
FROM "journal"
WHERE !fullyCompleted
GROUP BY file.link


### No due date
tasks
not done
no due date


### Done today
tasks
WHERE date is today