---
date: <% tp.file.creation_date() %>
genres: "{ genres }"
rating:
  "{ onlineRating }": 
website:
  "{ website }": 
image:
  "{ background_image }": 
year:
  "{ year }": 
tags: 
release:
  "{ released }": 
release_date:
  "{ release_date }": 
status: not started
cstatus: false
myrating: ‎‎
tba:
  "{ released }":
---

```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

dv.table(["game", "year", "genre", "cstatus"],
await Promise.all(dv.pages(`"${dv.current().file.path}"`)
.map(async p => [
	p.file.link,
	p.year,
	p.genres,
	await f(dv,p, "cstatus")])
))
```

status `INPUT[suggester(option(not started), option(in progress), option(shelved), option(finished), option(dropped), option(waiting)):status]`  

`INPUT[toggle:cstatus]` <%tp.file.name()%>

my rating `INPUT[suggester(option(★☆☆☆☆), option(★★☆☆☆), option(★★★☆☆), option(★★★★★)):myrating]`