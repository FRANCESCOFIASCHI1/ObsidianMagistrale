---
aliases:
tags:
  - schema
---
# 1 Agents

> È qualunque cosa che può interagire

Possiede:

- **Sensori** → per percepire l'ambiente
- **Attuatori** → per effettuare azioni nell'ambiente

$f:P^{*}\to A$

Dove:

- $P^{*}$ → è la storia delle percezioni
- $A$ → è l'insieme delle possibili azioni di un agent 

![[allegati/Image.png|711x394]]

+ *The First Agent*

    semplice senza valutare nessun tipo di valutazione, completa l'obbiettivo senza considerare il tempo o i cicli. Non considera i cambiamenti intermedi

```cpp
function Reflex-Vacuum-Agent( [location,status]) returns an action
  if status = Dirty then return Suck
  else if location = A then return Right
  else if location = B then return Left
```

**Rational Agents** → è un agente che sceglie le proprie azioni allo scopo di massimizzare le metriche i performance, dato lo storico delle percezioni ^RationalAgent

- Non è onnisciente
- Non prevede il futuro
- Non ha sempre successo

*AMBIENTE STATICO* → ossia completamente osservabile e prevedibile (azioni deterministiche con regole chiare) incoraggiando ad imparare comportamenti fissi.

- La soluzione ottima non cambia, stesso obbiettivo
- Soluzione fragile
    - *Esempio Vespa*: che come c'è un cambiamento riparte dall'azione che scaturisce la percezione quindi spreco di cicli inutile

## 1.1 PEAS

***P***erformance metric

***E***nviroment

***A***ctuators

***S***ensors

Più è ristretto l'ambiente → più l'agente sarà semplice da costruire

- Agent Architecture = $A + S$
- Agent = Architecture + Function
+ Example Taxi

Performance metric: safety, destination, profits, legality, comfort, ....
Environment: streets/freeways, traffic, pedestrians, weather, ...
Actuators: steering, accelerator, brake, horn, speaker/display, …
Sensors: camera, accelerometers, gauges, engine sensors, keyboard, GPS…

## 1.2 Enviroments - Ambienti

![[allegati/Image (2).png]]

> **Single-Agent** → ossia che nell'ambiente agisce un solo agente e non deve competere con altri

> Real Agent → è come il taxi non comprende nessuna delle caratteristiche, il che lo rende molto complicato da realizzare

- **Table-Driven Agent** → ossia prende decisioni consultando una **tabella di lookup** in cui, per ogni possibile percezione (o sequenza di percezioni), è già memorizzata l’azione da compiere.
    - Complessità alta per la creazione della tabella
- **Reflex Agent** → insieme di regole condizionali dirette “condizione → azione” ^ReflexAgent

![[Image (3).png|755x455]]

***Effectiveness-Efficiency Trade-off***

| **TA**                                     | **SRA**                                                            |
| ------------------------------------------ | ------------------------------------------------------------------ |
| Alti costi per gestire e creare la tabella | Riduce i costi creando solo delle condizioni di attivazione        |
| Scala esponenzialmente il tempo → $O(n^2)$ | Scala linearmente il tempo → $O(n)$                                |
|                                            | Semplifica un approccio già base e stupido → osservabile e piccolo |

## 1.3 Tipi di Agenti

### 1.3.1 Model-Based Agent

^5938be

![[Image (4).png|748x383]]

Aggiunge → *STATE* al [[#^ReflexAgent]

- Stima lo stato attuale dell'ambiente basandosi sullo stato interno dell'agente
    - Lo stato interno dipende dallo storico delle percezioni
- Lo stato interno serve per catturare il modello delle transizioni dell'ambiente
    - Conseguenze dell'azione dell'agente
    - Dinamiche dell'ambiente
- *Sensor Model* → come i sensori dell'agente rappresentano il mondo - rumore e limitazioni
    - Approssima l'intero stato → efficace in ambianti parzialmente osservabili

### 1.3.2 Goal-Based Agent

![[Image (5).png|702x430]]

❓Massimizzare la ricompensa immediata vs massimizzare la ricompensa a lungo termine

- **Search** → come è meglio navigare tra le possibili soluzioni? Decision Tree
- **Planning** → sfruttare la conoscenza del mondo per eseguire le migliori azioni

> I modelli goal-based rappresentano il loro obbiettivo (*goal*) e possono modificarlo

### 1.3.3 Utility-Based Agent

![[Image (6).png|727x448]]

> Invece di utilizzare un solo stato, assegna un punteggio ad ogni stato interno che indica quanto è buono

- Permette di analizzare varie traiettorie e quindi tenere in considerazione non solo se arrivo al traguardo ma anche come ci arrivo → permette di selezionare la soluzione migliore tra quelle esistenti risparmiando sulle mosse da fare per esempio

È fondamentale quando:

- ci sono stati finali **non tutti equivalenti** (es. vincere con 10 mosse vs con 100 mosse),
- l’ambiente è **stocastico** (es. backgammon),
- ci sono più obiettivi in conflitto (es. “arrivare a destinazione **velocemente ma in sicurezza**”)

### 1.3.4 Learning-Based Agent

![[Image (7).png|782x329]]

Tutti i modelli di agenti possono essere learning-based o no.

L'apprendimento può modificare qualche componente.

- **Elementi prestazionali** → i precedenti agenti ricevevano il contesto e sceglievano l'azione da effettuare
- **Critic** → guida l'apprendimento tramite la valutazione del comportamento attuale con la metrica delle performance esterne
- **Problem** → allontanarsi dal comportamento greedy provando nuove azioni (greedy = sceglie l'opzione migliore senza considerare altre soluzioni) 

## 1.4 Summary

- **Agenti** interagiscono con **l'ambiente** tramite **attuatori** e **sensori**
- La **funzione** degli agenti →  [Agent = Architecture + Function](craftdocs://open?blockId=4F5E2847-6565-43CC-A12A-EB81130E4DFF&spaceId=4b28628f-7dfd-54ce-27f9-28712867f81f) → descrive cosa fa l'agente in tutte le circostanze
- La **misurazione delle prestazioni** valuta la sequenza dell'ambiente
- Un [[#^RationalAgent]]
- [PEAS](./🔥 Schema Slide.md#peas) → descrive gli ambienti di attività
- [Enviroments - Ambienti](./🔥 Schema Slide.md#enviroments--ambienti) → Osservabile, Deterministico, Episodico, Statico, Discreto
- Molti tipi di Agenti
    - [[#^ReflexAgent]]
    - [[#1.3.1 Model-Based Agent]]
    - [[#1.3.2 Goal-Based Agent]]]
    - [[#1.3.2 Goal-Based Agent]]

---

# 2 Search

> Search Alorithm → dato un problema, ritorna una sequenza di azioni, che formano un path verso lo stato *goal* o *fail*

**Open-Loop** - *offline* → un agente cerca la sequenza ottimale di azioni e successivamente la esegue
- *Completamente osservabile*
- *Deterministico* → risposte dell'ambiente conosciute

**Closed-Loop** - *online* → l'agente valuta continuamente i feedback provenienti dall'ambiente
- *parzialmente osservabile*
- *Stocastico* → le risposte variano
	- Devo controllare il mondo per sapere dove sono, in quale stato

![[Image (8).png|752x448]]

> **Scelte di Design → Guida veloce**

- **Deterministica e completamente osservabile** → problema a stato singolo
    - l'agente sa esattamente in quel stato si troverà → la soluzione è una sequenza di stati o azioni
- > **Non osservabile** → problema conforme senza sensore
    - L'agente non percepisce lo stato → soluzione è una sequenza di azioni
- *Non deterministico e parzialmente osservabile** → problema di contingenza
    - le percezioni prendono nuove informazioni riguardo allo stato
    - la soluzione è un piano contingente o una policy
    - spesso ricerca interlacciata
- **Spazio degli Stati Sconosciuto** → problema di esplorazione (online - closed-loop)
    - l'agente deve scoprire lo spazio degli stati

![[Image (9).png|741x278]]

![[Image (10).png|738x361]]

## 2.1 Tree Search

> Inizia dagli stati già visitati e gli espande → genera gli stati successivi

- > **Frontiera** → insieme di nodi non espansi che separano i nodi espansi da quelli non raggiunti
- > **Nodi Raggiunti** → *frontiera* + *nodi espansi*
- > Ricerca **Uniforme**

```cpp
function Tree-Search(problem, strategy) return a solution or failure
  inizializza con initial state del problema
  loop do
    if non ci sono candidati return failure
    scegli un nodo da espandere in base alla strategia
    if nodo contine goal state return soluzione
    else espandi il nodo e aggiungi il nodo alla ricerca ad albero
  end
```

![[Image (11).png]]

![[Image (12).png]]

- **State** → uno stato è una rappresentazione di una configurazione fisica
- **Nodo** → è una struttura dati che costituisce una parte della ricerca ad albero
    - include genitori, figli. profondità e costo dei path (cammini)
- **La funzione di Espansione**
    - Crea nuovi nodi
    - Riempie i vari campi
    - Usa la funzione *Successore* del problema per creare gli stati corrispondenti

> La ricerca ad albero non corrisponde all'intero spazio degli stati → un insieme di satti finito può portare ad una ricerca infinita (loops, path ridondanti)

> Nodi nella ricerca ad albero = stati dello spazio

Gli stati sono tutti diversi nello spazio

I nodi non sono unici in un albero di ricerca → se abbiamo un nodo che ritorna ad un altro già esplorato devo comunque selezionarlo

### 2.1.1 Scegliere una Strategia di Ricerca

> Una strategia di ricerca è definita **in base all'ordine con cui i nodi vengono espansi**

Una **strategia** è **valutata** tramite queste dimensioni:

- **Completezza** → trova sempre una soluzione
- **Complessità di Tempo** → numero di nodi generati o espansi
- **Complessità dello Spazio** → numero massimo di nodi in memoria
- **Ottimale** → trova sempre la soluzione con meno costo

Complessità di Tempo e Spazio sono misurati in:

- $b$ → fattore massimo di ramificazione dell'albero di ricerca (quanti nodi espando)
- $d$ → profondità della soluzione migliore (minor costo)
- $m$ → massima profondità dello spazio degli stati (può essere $\infty$)

## 2.2 Uninformed Tree Search - Non Informata

> Una ricerca non informata usa solo le informazioni disponibili quando definiamo il problema, non sappiamo quanto siamo lontani dal goal (obbiettivo)

- Breadth-first search
- Uniform-cost search
- Depth-first search
- Depth-limited search
- Iterative deepening search

### 2.2.1 Breadth-First Search
- Frontiera → è una coda FIFO
- Espande prima in larghezza poi in profondità

![[Image (13).png]]

![[Image (14).png]]

| **Completo**    | **Time**                                         | **Space**                      | **Optimal**                                       |
| --------------- | ------------------------------------------------ | ------------------------------ | ------------------------------------------------- |
| **Si**          | $\mathbf{O}(b^d)$                                | $\mathbf{O}(b^d)$              | **Si**                                            |
| Se $b$ è finito | Dato che fa tutti i nodi ad una certa profondità | Prende tutti i nodi in memoria | Se il costo per step è $1$ non ottimo in generale |

#### Dijkstra's Algorithm → ricerca costo uniforme

> Le azioni hanno un costo differente → espandere i nodi con il cammino di costo minimo

> La frontiera è una coda pesata → i path con costo minore vanno per primi

| **Completo**                                          | **Time**                                                                                                                | **Space**                | **Optimal**                                                |
| ----------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- | ------------------------ | ---------------------------------------------------------- |
| **Si**                                                | $\mathbf{O}(b^{1+[\frac{C^*}{\eta}]})$                                                                                  | $\mathbf{O}(b^d)$        | **Si**                                                     |
| Se il costo minimo delle azioni è positivo ossia $>0$ | Dove $C^*$ è il costo del path ottimale → può eplorare percorsi a basso costo che non servono a niente può essere $b^d$ | **Stesso della memoria** | I nodi sono espansi in ordine in base al costo del cammino |

### 2.2.2 Depth-First Search

> Espande prima i nodi in profondità

> Coda → LIFO → mette i nodi in cima (Ultimo ad entrare primo ad uscire Last In - First Out)

![[Image (15).png]]

![[Image (16).png]]

| **Completo**                 | **Time**                                                                       | **Space**        | **Optimal** |
| ---------------------------- | ------------------------------------------------------------------------------ | ---------------- | ----------- |
| **Si**                       | $\mathbf{O}(b^m)$                                                              | $\mathbf{O}(bm)$ | No          |
| Se $b$ è finito e senza loop | Dato che vado prima in profondità esploro prima tutti i fligli fino al massimo | **Lineare**      |             |

> DFS può essere implementato in vari modi:

- > **Depth-Limited search** → con profondità di esplorazione fissata
- > **Iterative Deeping Search** → iterazioni con valore di profondità che aumenta
- > **Bidirectional Search** → iniziamo la ricerca dallo stato iniziale e dallo stato obbiettivo verso lo stato iniziale

#### Come Riconoscere uno Stato Ripetuto?

- **Ricordare gli stati raggiunti**
    - Posso riconoscere e conservare i path di costo minore
    - Se ho spazio
- Non controllo se il problema non produce path ridondanti
- **Controlla solo i loop**
    - Conserva i genitori per ogni nodo
    - Segue la catena dei parenti per vedere se ricade in un loop
    - Posso seguire la catena fino al root → puntatori al padre
    - **Non c'è bisogno di memoria in più** solo **più tempo per controllare la catena**

### 2.2.3 Sommario → Uninformed Search

![[Image (17).png]]

## 2.3 Heauristic - Informed Search

> Espandere i nodi più promettenti

> Promettenti è una valutazione euristica → propone una stima del minor costo da uno stato al nodo $n$ fino al goal

> Frontiera → è una coda pesata e ordinata in base al valore euristico

Best-First Search Algorithm:

- Greedy Search
- A* Search

### 2.3.1 Greedy Search

> Seleziono ad ogni nodo quello con il costo euristico minore

- Non ha meccanismi per trovare il cammino migliore guardando i successivi
- Potrebbe portare a cammini di costo maggiore anche se il primo passo è più conveniente

| **Completo**                                                                                                | **Time**                                       | **Space**                     | **Optimal** |
| ----------------------------------------------------------------------------------------------------------- | ---------------------------------------------- | ----------------------------- | ----------- |
| No                                                                                                          | $\mathbf{O}(b^m)$                              | $\mathbf{O}(b^m)$             | No          |
| Può rimanere incastrato in un loop.<br/>Completo in uno spazio finito con il controllo degli stati ripetuti | Una buona euristca può portare a miglioramenti | Tiene tutti i nodi in memoria |             |

### 2.3.2 A* Search

> Idea → evitare di espandere i cammini già costosi

> Controllo se un cammino è più costoso di un altro cammino che ho già esplorato se si allora continuo il cammino con il costo minore e così via

> In questo modo tengo in considerazione il cammino di costo minimo generale, in modo da poter scegliere quello minore analizzando anche cammini alternativi

**Funzione di Valutazione** → $f(n)=g(n)+h(n)$

- $g(n)$ = costo fino a questo momento per raggiungere $n$ 
- $h(n)$ = costo stimato al goal da $n$ → euristica greedy
- $f(n)$ = costo stimato del path migliore passando attraverso $n$ per raggiungere il goal
- Questa funzione valuta ad ogni nodo il costo decidendo come proseguire

**Complessità di Spazio** → la complessità scala con il numero di nodi espanso

- $A*$ espande tutti i nodi con $f(n)<C^*$
- $A*$ espande qualche nodo con $f(n)=C^*$
- $A*$ non espande nodi con $f(n)>C^*$
- Con $C^*$ costo del path ottimale 

**Complessità di Tempo** → è esponenziale nell'errore di $h$

A* utilizza un euristica *ammissibile*

- $h(n)\le h^*(n)$ dove $h^*(n)$ è il vero costo da $n$
- $h(n)\ge 0$ tale che $h(G)=0$ per ogni goal $G$
- Un euristica ammissibile non sovrastima mai la distanza dal goal

***A** è completa**

***A$^*$* è ottima se *h* è ammissibile**

> **Euristica Consistente** → $h(n)\le c(n,a,n \prime)+h(n \prime),\ \forall n\prime$

- Più forte dell'ammissibilità → il costo dei cammini è monotona crescente
    - Ossia il valore di $f(n)$ non scende mai lungo il cammino
- Quando raggiungi uno stato è già sulla la strada ottimale per arrivare ad $n$
    - Se l'euristica fosse solo ammissibile invece potrei estrarre un nodo con un cammino sub-ottimale e doverlo riconsiderare in seguito

![[Image (18).png]]

### 2.3.3 Problema
> A* visita troppi nodi → richiede che vengano esplorati ed espansi molti nodi
> → **A* Pesato**: sub-optimal ma abbastanza buono
> $f(n)=g(n)+W\ h(n),\ \ W>1$
> Trova una soluzione con costo tra $C^*$ e $W\ C^*$
> -> Spesso vicina a $C^*$

#### Contorni con A*

![[Image (19).png|714x431]]

![[Image (20).png]]

![[Image (21).png]]

#### Beam Search
La **beam search** è un algoritmo di ricerca basato su **euristiche**  che esplora un grafo espandendo il nodo più promettente in un insieme limitato di nodi.
La beam search  simile a [[#2.2.1 Breadth-First Search]] limitata. che mira a ridurre i requisiti di memoria. Nella beam search è tenuto in considerazione solo un numero predefinito di migliori soluzioni parziali. Di conseguenza, è considerato un algoritmo greedy.
- L’euristica non è necessariamente ammissibile né ottimale
- **Obiettivo tipico**: generare sequenze plausibili in problemi di decoding
- Mantiene i **B migliori cammini parziali** in un albero di ricerca.
- Ad ogni passo: espande ognuno, valuta i successori, sceglie i B migliori, scarta gli altri.
- Non garantisce la soluzione ottimale

---
# 3 Local Search
Cosa vogliamo:
- Veloce
- Economico
- Abbastanza buono
- Ricerca non sistematica
	- Non ci interessa come ci arriviamo all'obbiettivo ma vogliamo arrivarci

> Local search restituisce una soluzione non un path
> - Non salva gli stati visitati
> - Non esplora l'intero spazio degli stati

## 3.1 Hill Climbing
> Hill-climbing può rimanere bloccato
	- Ottimo locale
	- Greedy Local Search


> [!NOTE] Gradient Descent
> Se uso il gradiente per valutare e guidare la funzione di ricerca otteniamo la discesa del gradiente.

- si blocca su i massimi locali
- Si blocca su parti pari (flat)
### 3.1.1 Soluzioni
**Random Restart**
- Riesce a scapapre dall'ottimo locale
- Completo ma molto lento
**Sideway Move** -> *movimento laterale*
- Riesce ad uscire dalle parti flat se non sono il massimo
- Fisso -> Si sposta di massimo $k$ step
![[image-1.png|525x302]]

```c title="Hill Climbing"
function Hill-Climbing(problem) returns a state that is a local maximum
    inputs: problem, a problem definition
    local variables: current, a node, neighbor, a node
    
    current ← Make-Node(Initial-State(problem))
    loop do
        neighbor ← a highest-valued successor of current
        if Value(neighbor) ≤ Value(current)
        then return State(current)
        current ← neighbor
end
```

## 3.2 Simulated Annealing
> Uscire dall'ottimo locale facendo un passo random
> - un passo random non rimane mai incastrato -> ma è lento


> [!important] Simulated Annealing
> Corrisponde a -> *hill climbing $+$ random moves*
> - I movimenti random diventano meno frequenti nel tempo
> - I movimenti casuali migliorano con il tempo
> - **Temperature** -> è un parametro che controlla il trade-off tra hill-climbing e movimenti random
> 	- La temperatura cambia con il tempo permettendo all'inizio di fare molti passi casuali e mano a mano che vado avanti i passi casuali diminuiscono
> 	- Alla fine $T=0$ -> nessun passo casuale


### 3.2.1 Temperature Scheduling
$$
e^{\frac{x}{T}},\quad T>0,\quad x>0
$$
- $x$ Grande -> è una mossa molto sbagliata, c'è una bassa probabilità di accettare lo spostamento
- Aumentando $T$ -> aumento la probabilità di accettare spostamenti
Se $T\to 0$ allora anneiling diventa deterministico -> non effettua mosse casuali
- Accetta solo spostamenti buoni $e^{\frac{x}{T}}\to 0$ 
**Anneiling Scheduling** -> inizia con T grande e lo diminuisce con il passare del tempo
- Se T viene diminuite abbastanza lentamente l'algoritmo converge al *massimo globale* 
	- Se invece è troppo lento visito tutti gli stati -> lentissimo
- *Diminuzione Esponenziale* -> $T_t = k\ T_{t-1},\quad 0<k<1$
- *Power-law Decrese* -> $T_t=\frac{T_0}{t^a}$ es: $a=1$
### 3.2.2 Local Beam Search
> Invece di prendere solo la prima soluzione prendo le prime $k$
> Espando tutto e seleziono i top-$k$ stati
> - Ha senso quando la funzione non converge sicuramente


> [!important] Funzionamento
> È la versione stocastica della [[#Beam Search]]
> Gli stati interagiscono con gli altri -> abbandono i path con meno importanza, concentrandomi su quelli più promettenti
> - Non è una ricerca parallela su $k$ path
> - *Local beam -> parte da k stati, tiene i migliori k successori globalmente.*
> - **Non garantisce ottimalità**

#### Beam search decoder in NLP

NLP -> Natural Language Processing
Tutto ciò che concerne l'elaborazione del linguaggio naturale -> nell'esempio la traduzione da inglese ad italiano tramite encoder (trasforma la frase in vettori-embedding) e deconder con vari livelli di linar model e softmax per avere l'output
*Esempio:*
- *Input*: how are you?
- *Output*: come stai?
![[image-2.png]]
##### 3.3 Beam Search vs Greedy Search in NLP
**Greedy Search** -> semplicemente prende la parola più simile per ogni posizione
- Ogni parola è considerata indipendente
- Funziona ma può essere migliorato
**Beam Search**
- Trova il path migliore e tiene in considerazione cosa ha già selezionato
- Prende $N$ migliori candidati per ogni posizione
- Espande gli attuali $N$ migliori candidati e prende i migliori $N$
- Nell'ultima posizione prende il miglior risultato tra gli $N$ candidati finali
##### Beam Search Decoder in NLP
Bisogna eseguire il decoder $N$ volte per ogni posizione:
- Costa tanto
- Ma il risultato spesso è il migliore
![[image-3.png]]
## 3.3 Ambiente Non Deterministico

> Alcune azione hanno un risultato non deterministico

Belief State (Stato di Fede) -> un insieme di stati futuri per ogni stato corrente e coppia di azioni
- Non sai in anticipo dove ti porta l'azione
Conditional Plan -> la solzuione è un albero (if-then-else chain) non una sequenza
- Puoi eseguire la soluzione e in base al risultato dell'ambiente scegliere l'azione corretta
- **AND-OR Search Trees** -> è la soluzione per il conditional plan
	- **AND node** -> Fornito dall'ambiente insieme al set di tutti i possibili stati successivi
	- **OR node** -> Fornito dall'agente insieme all'azione da eseguire
### 3.3.1 Esempio Erratic Vacuum World
Non deterministic action
- Se lo stato corrente è sporco, "suck" pulisce il quadrato corrispondente ma avvolte pulisce anche il quadrato adiacente
- Se lo stato corrente è pulito, "suck" può depositare la sporcizia
"Left" e "Right" sono deterministiche

**Belief state**
- $Result(s,a)=?$
**Conditional plan**
- «suck»; `if 5 do [«right», «suck»]`
- Starting from state 1
- $Result(1, «suck»)= \{5, 7\}$
> In questo modo se "suck" pulisce entrambi io avrò che `if 5` non sarà rispettato quindi faccio un altra azione
### 3.3.2 AND-OR Search Tree
- Necessario per evitare loop
- L'algoritmo è leggermente più coprente ma l'albero si espande ancora
- Esiste una versione di $A^*$ per l'albero AND-OR

**Solzuione**
Un sotto-albero dove:
-  ogni foglia è un goal
- Ha un azione per ogni nodo OR
- Include un ramo per tutti i risultati dei nodi AND
![[image-4.png|465x412]]

## 3.4 Searching Senza Osservazioni
- Non ci sono percezioni
- Senza sensori -> *Conformant Problem*

> L'agente deve stimale lo stato
> **Soluzione** -> è una sequenza di azioni
> - Non può essere un conditional plan se non puoi ricevere un feedback dall'ambiente (non posso fare `if`)

**Belief Space** -> lo spazio delle rappresentazioni interne di ciò che credo sia vero sul mondo (non certe)

Le azioni possono portare informazioni su possibili stati futuri
- *belief state* $= \{1,2,3,4,5,6,7,8\}$, *action*$=«right»$, *next belief state* $= ?$
- $\{2, 4, 6, 8\}$
#### Search in Conformant Problem
> Idea -> cercare nel belief space (completamente osservabile) invece che nello spazio degli stati (non osservabile)

- Belief state space = powerset of states -> 2𝑛 possible beliefs instead of N
- Initial belief state = all the N states
- Le azioni *Actions(B)* che posso fare sono limitate dall'unione e l'intersezione di tutte le azioni *Actions(S)* pr ogni $S$ in $B$
- Transition model (for deterministic actions): Quando esegui un’azione, non vai in un singolo stato, ma in un **nuovo belief state**.
- **Goal Test**
	- **Possibly achieve** il goal: se almeno uno degli stati nel belief è un goal.
	- **Necessarily achieve** il goal: se _tutti_ gli stati del belief sono goal.
- **Action cost**: Se l’azione costa in modo diverso nei vari stati, allora devi trovare un criterio
	- Questo serve per decidere quanto “vale” un’azione in un belief state.

##### Check Visited States
###### Come controllare gli stati visitati?
- Esempio:
	- 𝐵1 = {5, 7}
	- 𝐵2 = {1, 3, 5, 7}
- Puoi **potare** (cioè scartare) i rami di 𝐵2, perché qualsiasi soluzione valida per $\{1, 3, 5, 7\}$ sarà anche una soluzione per $\{5, 7\}$.
👉 In generale: puoi scartare i rami che appartengono a **superset** di belief state già visitati.
###### Ragionamento inverso
- Se hai già risolto il problema per un certo belief state, allora lo hai risolto anche per qualsiasi **sottoinsieme** di esso.
###### Complessità
- Nonostante queste ottimizzazioni, la ricerca nei problemi conformant (cioè senza osservazioni, dove l’agente non sa esattamente dove si trova) rimane molto costosa, perché lo spazio dei belief state è enorme.
###### Ricerca incrementale nei belief state
1. Trova una soluzione che funziona per il **primo stato** in B.

2. Controlla se quella soluzione funziona anche per gli altri stati in B.
    - Se sì → fermati.
    - Se no → riprova.
3. **Fallire velocemente** (cioè accorgersi presto se una soluzione non funziona) può migliorare la velocità di convergenza