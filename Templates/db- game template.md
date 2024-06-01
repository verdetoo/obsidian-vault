---
genres:
  "{ genres }": 
platforms:
  "{ platforms }": 
publishers:
  "{ publishers }": 
developers:
  "{ developers }": 
rating:
  "{ rating }": 
playtime:
  "{ playtime }": 
short_screenshots:
  "{ short_screenshots }": 
website:
  "{ website }": 
image:
  "{ image }": 
release_date:
  "{ released }": 
release: 
tags: mediaDB/game‎‎
tba:
  "{ tba }": 
year: 
date: <% tp.file.creation_date() %>
myrating: ‎‎‎
status: not started
cstatus: false
cssclasses: cards, cards-cover, cards-3-2, cards-cols-3
---



```dataview
table without id 
	("![](" + background_image + ")") as banner,
file.link + " (" + string(year) + ") " + choice(cstatus, ":luc_check:" + "  ⭐ " + myrating, " ") as all
where contains(file.name, this.file.name) AND contains(tags, "mediaDB/game")
```

status `INPUT[suggester(option(not started), option(in progress), option(shelved), option(finished), option(dropped), option(waiting)):status]`

simplified `INPUT[toggle:cstatus]`

my rating `INPUT[suggester(option(★☆☆☆☆), option(★★☆☆☆), option(★★★☆☆), option(★★★★★)):myrating]`