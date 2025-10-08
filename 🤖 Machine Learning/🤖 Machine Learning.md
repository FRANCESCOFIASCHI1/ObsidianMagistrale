---
Esame: Machine Learning
CFU: 9
Voto:
dg-publish: true
---

```dataview
LIST
FROM "🤖 Machine Learning"
WHERE !CFU
SORT ASC
```


# 1 Media Compitini
```dataview
TABLE Score
FROM "🤖 Machine Learning"
WHERE Score
```

```dataviewjs
let esami = dv.pages('"🤖 Machine Learning"')
    .where(p => p.Score)
    .array();

let somma = 0;
let num = 0;

esami.forEach(p => {
    somma += p.Score;
    num += 1;
});

let media = (somma / num).toFixed(2);
dv.table(["Media"], [[media]]);

```


