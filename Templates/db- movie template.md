---
date: <% tp.file.creation_date() %>
myrating: ‎‎‎
status: not started
cstatus: false
cssclasses: cards, cards-cover, cards-2-3, cards-cols-3
---

```dataview
table without id 
	("![](" + image + ")") as poster,
	file.link as title,
	string(year) as year, 
	"by " + producer as director,
	"⭐ " + onlineRating as rating,
	"⭐ " + myrating as rating
where contains(file.name, this.file.name) AND contains(tags, "mediaDB/tv/movie")
```


status `INPUT[suggester(option(not started), option(in progress), option(shelved), option(finished), option(dropped), option(waiting)):status]`

simplified `INPUT[toggle:cstatus]`

my rating `INPUT[suggester(option(★☆☆☆☆), option(★★☆☆☆), option(★★★☆☆), option(★★★★★)):myrating]`