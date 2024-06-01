also see [[Source Materials - all list]]



## content
### movies
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

dv.table(["file", "year", "status", "my rating"],
await Promise.all(dv.pages("#mediaDB/tv/movie").map(async p => [
	p.file.link,
	p.year,
	await f(dv,p, "status"),
	await f(dv,p, "myrating")])
))
```


### shows
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

dv.table(["file", "year", "status", "my rating"],
await Promise.all(dv.pages("#mediaDB/tv/series").map(async p => [
	p.file.link,
	p.year,
	await f(dv,p, "status"),
	await f(dv,p, "myrating")])
))
```


### books
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

dv.table(["file", "year", "status", "my rating"],
await Promise.all(dv.pages("#mediaDB/book").map(async p => [
	p.file.link,
	await f(dv,p, "year"),
	await f(dv,p, "status"),
	await f(dv,p, "myrating")])
))
```


### games
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

dv.table(["file", "year", "cstatus", "rating"],
await Promise.all(dv.pages("#mediaDB/game").map(async p => [
	p.file.link,
	p.year,
	await f(dv,p, "cstatus"),
	await f(dv,p, "myrating")])
))
```


## articles
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;

dv.table(["file", "status"],
await Promise.all(dv.pages('"articles"').map(async p => [
	p.file.link,
	await f(dv,p, "status")])
))
```


## References

#### Quotes
`button-quote`

```dataview  
TABLE WITHOUT ID  
link(file.link, "Q") + " " + source as ":luc_quote:", text AS ":obs_lines_of_text:"
FROM "saves/quotes"
WHERE p.class = quote
SORT file.ctime DESC  
```

#### Compilations
```dataview  
TABLE WITHOUT ID  
link(file.link, title) as ":ril_newspaper:", date as ":luc_pencil:"
FROM "saves/compilation"
SORT file.ctime DESC  
```
