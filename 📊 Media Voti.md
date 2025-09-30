---
title: Media dei voti
tags:
  - base
---

# 1 Voti Esami
```dataview
TABLE CFU, Voto
FROM ""
WHERE Voto AND CFU
```

# 2 Media Ponderata
```dataviewjs
let esami = dv.pages('""')
    .where(p => p.Voto && p.CFU)
    .array();

let sommaPonderata = esami.reduce((acc, p) => acc + p.Voto * p.CFU, 0);
let sommaCFU = esami.reduce((acc, p) => acc + p.CFU, 0);

let media = (sommaPonderata / sommaCFU).toFixed(2);

dv.table(["Media Ponderata"], [[media]]);

```
