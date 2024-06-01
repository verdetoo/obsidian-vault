---
cssclasses: cards, cards-cover, cards-2-3, cards-cols-6
---

# movies
```dataview
table without id 
	("![](" + image + ")") as poster,
	file.link as title,
	string(year) as year
from #mediaDB/tv/movie AND "02 Source Materials"
where image != null
```
# series
```dataview
table without id 
	("![](" + image + ")") as poster,
	file.link as title,
	string(year) as year
from #mediaDB/tv/series AND "02 Source Materials"
where image != null
```
# games
```dataview
table without id
	("![](" + image + ")") as cover,
	file.link as title,
	string(year) as year
from #mediaDB/game AND "02 Source Materials"
where image != null
```
# books
```dataview
table without id 
	("![](" + image + ")") as cover,
	file.link as title
from #mediaDB/book AND "02 Source Materials"
where image != null
```
