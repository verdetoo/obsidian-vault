---
title: "{{title}}"
author:
  - "{ author }": 
publisher:
  "{ publisher }": 
publish:
  "{ publishDate }": 
total:
  "{ totalPage }": 
isbn:
  "{ isbn10 }": 
image:
  "{ coverUrl }": 

updated:
  "{ DATE:YYYY-MM-DD HH:mm:ss }": ‎‎
tags: []
date: <% tp.file.creation_date() %>
myrating: ‎‎‎
status: not started
cstatus: false
cssclasses:
  - cards
  - cards-cover
  - cards-2-3
  - cards-cols-3
---

```dataview
table without id 
	("![](" + cover + ")") as cover,
	file.link as title,
	"by " + author,
	status
where contains(file.name, this.file.name) AND contains(tags, "mediaDB/book")
```

status `INPUT[suggester(option(not started), option(in progress), option(shelved), option(finished), option(dropped), option(waiting)):status]`

simplified `INPUT[toggle:cstatus]`

my rating `INPUT[suggester(option(★☆☆☆☆), option(★★☆☆☆), option(★★★☆☆), option(★★★★★)):myrating]`