---
tags:
  - appunti
dg-publish: true
---

# 1 Inverted Index
30-09-2025


![[Recording 20250930144806.m4a]]
**Modelli di valutazione delle query nei motori di ricerca**, cioè come usare le **posting list** per rispondere a una query.  
I due approcci principali sono **TAAT** (_Term-At-A-Time_) e **DAAT** (_Document-At-A-Time_).

---

## 1.1 🔹 TAAT (Term-At-A-Time)

- L’algoritmo scorre **una posting list alla volta**.
- Per ogni termine della query:
    1. Prende la sua posting list.
    2. Aggiorna i punteggi dei documenti che compaiono in quella lista.
- Quando ha finito con tutti i termini, avrà i punteggi finali per ciascun documento.

👉 Vantaggio: semplice da implementare.  
👉 Svantaggio: può richiedere molte operazioni se le posting list sono grandi (si aggiornano tanti documenti più volte).

**Esempio (query: “gatto cane”):**
- Scorro la posting list di _gatto_ → aggiorno i documenti [D1, D3, D7].
- Poi quella di _cane_ → aggiorno [D2, D3].
- Alla fine calcolo il ranking con i punteggi accumulati.
---

## 1.2 🔹 DAAT (Document-At-A-Time)

- L’algoritmo scorre **tutte le posting list in parallelo**, “documento per documento”.
- Si considera un documento alla volta (il più piccolo ID corrente fra le liste) e si calcola subito il suo punteggio in base a tutti i termini della query.    
- Si passa poi al documento successivo.

👉 Vantaggio: più efficiente se voglio il **top-k ranking** (i migliori k documenti), perché posso interrompere prima usando strutture come **heap** o algoritmi di soglia.  
👉 Svantaggio: più complesso da implementare.

**Esempio (query: “gatto cane”):**
- Posting list _gatto_: [D1, D3, D7]
- Posting list _cane_: [D2, D3]
- Scorro in parallelo:
    - D1 → appare solo in _gatto_
    - D2 → appare solo in _cane_
    - D3 → appare in entrambe, calcolo subito il punteggio totale
    - D7 → appare solo in _gatto_
---

## 1.3 🔑 Riassunto rapido

- **TAAT** = scorri termine per termine, aggiorni punteggi documento per documento.
- **DAAT** = scorri documento per documento, aggiornando in parallelo tutte le posting list.