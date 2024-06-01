---
date: 2024-04-27
tags:
  - meta
  - obsidian
  - templates
zettelkasten: permanent
---

esto es un documento grande con todos los templates que o no estoy usando o quiero conservar de alguna forma más allá de la carpeta **templates**. ordenado por carpetas o tipos de contenido. navegar desde el outline.

```table-of-contents
title: **Table of contents**
style: nestedList
minLevel: 1
maxLevel: 4
includeLinks: true
debugInConsole: false
```

# [[Meta Bind|Meta Bind]] recurrent use

`INPUT[datePicker:date]`     `INPUT[date:date]`
:ril_file_paper_2:  `INPUT[suggester(option(fleeting), option(literature), option(permanent)):zettelkasten]` 
:obs_tag_glyph:  `INPUT[inlineList:tags]` 
:obs_search:  `INPUT[suggester(option(youtube), option(obsidian forum), option(obsidian discord), option(other)):source]`

# [[Templater|Templater - plugin]] and [[Metadata menu |Metadata menu - plugin]]
## in journal

### in daily notes 
```
---
week: "[[journal/weekly/Fecha inválida]]"
class: journal, day
makebed: false
shower: false
study: false
watch: false
game: false
read: false
stream: false
month: "[[journal/monthly/Fecha inválida|Fecha inválida]]"
year: "[[journal/yearly/Fecha inválida|Fecha inválida]]"
---

# Fecha inválida
[[journal/daily/Fecha inválida|Yesterday]] <- [[home|Home]] -> [[journal/daily/Fecha inválida|Tomorrow]] :ril_calendar: [[journal/weekly/Fecha inválida|Fecha inválida]]
```

#### Today's habits
```dataviewjs
const {fieldModifier: f} = this.app.plugins.plugins["metadata-menu"].api;
dv.table(["day", "week", "🛏️", "🚿", "📚", "📺", "🕹️", "📖", "📱"],
await Promise.all(dv.pages(`"${dv.current().file.path}"`)
.map(async p => [
	p.file.link,
	p.week,
	await f(dv,p, "makebed"),
	await f(dv,p, "shower"),
	await f(dv,p, "study"),
	await f(dv,p, "watch"),
	await f(dv,p, "game"),
	await f(dv,p, "read"),
	await f(dv,p, "stream")])
))
```

### in weekly notes
#### Frontmatter, Prev Week, Next Week
```
---
tagName: [globals]
class: journal, week
tags: journal/week
month: "[[journal/monthly/Fecha inválida|Fecha inválida]]"
year: "[[journal/yearly/Fecha inválida|Fecha inválida]]"
---

# Fecha inválida
[[journal/weekly/Fecha inválida| ↶ Previous Week]] | [[home|Home]] | [[journal/weekly/Fecha inválida|Next Week ↷]] 
```


#### This week's habits
```

dataview
TABLE WITHOUT ID
	week,
	file.link as "days",
	choice(makebed, "●", "○")  AS "🛏️",
	choice(shower, "●", "○")  AS "🚿",
	choice(study, "●", "○")  AS "📚",
	choice(watch, "●", "○")  AS "📺",
	choice(game, "●", "○")  AS "🕹️",
	choice(read, "●", "○")  AS "📖",
	choice(status, "●", "○") AS "📱"
FROM "--- journal/daily" 
WHERE contains(week, this.week)

```

### in monthly notes
#### Frontmatter, Prev Month, Next month

```
---
tagName: [globals]
class: journal, month
tags: journal/month
year: "[[journal/yearly/Fecha inválida|Fecha inválida]]"
---


# Fecha inválida
[[journal/monthly/Fecha inválida| ↶ Previous Month]] | [[home|Home]] | [[journal/monthly/Fecha inválida|Next Month ↷]] 
```

### in yearly notes
#### Frontmatter
```
---
tagName: [globals]
class: journal, year
tags: journal/year
year: [[2024]]
---

# {{file.title}}
:ril_home_2: [[home|Home]]

```


## in content
### view of game , editable 

requiere [[Arrows - plugin]] y [[metadata menu - plugin]]
usa [[Icon Shortcodes - plugin]] para el checkbox, aunque se puede usar un emoji normal o un símbolo unicode o lo que sea

```
dataview
table without id 
	("![](" + background_image + ")") as banner,
file.link + " (" + string(year) + ") " + choice(cstatus, ":luc_check:" + "  ⭐ " + myrating, " ") as all
where contains(file.name, this.file.name) AND contains(tags, "mediaDB/game")
```

### view of game , non-editable AND card frontmatter

requiere [[Arrows - plugin]] para la view

para estilizarlo en cards(lo del frontmatter) requiere [[minimal theme - plugin]] con su funcionalidad de cards. creo que la funcionalidad de card se puede descargar por separado si no usas el Minimal Theme.


```ESTO ES EL FRONTMATTER
---
cssclasses: cards, cards-cover, cards-3-2, cards-cols-3
---
```


```ESTO ES EL VIEW EN SÍ, SIN ESTILIZAR
dataview
table without id 
	("![](" + background_image + ")") as banner,
	file.link as name,
	string(year) as year,
	status + "  ⭐ " + myrating  as status
where contains(file.name, this.file.name) AND contains(tags, "mediaDB/game")
```
