---
cssclasses: cards, cards-cover, cards-2-3, cards-cols-5
---
# movies : to be watched
```dataview
table without id 
	("![](" + image + ")") as poster,
	file.link as title,
	string(year) as year,
	"⭐ " + onlineRating as rating
from #mediaDB/tv/movie
where image != null AND contains(status, "not started")
```
