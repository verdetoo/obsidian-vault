---
class: quote
day: "[[journal/daily/<%tp.file.cday("DD.MM.YYYY")%>|<%tp.file.cday("DD.MM.YYYY")%>]]"
week: "[[journal/weekly/<% moment(tp.file.cday,'DD.MM.YYYY').add(0,'days').format("gggg [W]ww") %>]]"
source: {{VALUE:source}}
fromsource: {{VALUE:fromsource}}
title: {{VALUE:title}}
text: {{VALUE:text}}
link: {{VALUE:link}}
---
> {{VALUE:text}}
> [{{VALUE:source}}]({{VALUE:link}})