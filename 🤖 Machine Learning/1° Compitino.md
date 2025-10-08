---
tags:
  - schema
dg-publish: true
Score: 9
---

# 1 Primo Compitino

> ML studia e propone metodi per costruire funzioni dai dati reali osservati che sono in grado di:

- **fit** → essere coerente con gli esempi
- **generalize** → ossia generalizzare su dati nuovi in modo che abbiano una ragionevole accuratezza

### 1.1.1 Dati

- **Flat** → vettori di proprietà di dimensione fissata, singola tabella di tuple
    - **Attributi**
        - Discreti → assumono un numero finito o numerabile di valori
        - Continui → assumo un numero di valori infinito o non numerabile
- **Structured** → liste, alberi e grafi, alcuni esempi sono le immagini, le stringhe di linguaggio e dati temporali

![[image_1.png|786]]

Come vediamo in questo tabella abbiamo:

- $(x)$ → ogni riga della tabella → **pattern**, example, istanza
- $(l)$ → dimensione del dataset
- $(n)$ → dimensione delle caratteristiche e quindi degli input $x$
- $(x_i)$ → è la caratteristica/attributo $i$-esimo della tabella
- $(x_p)$ → è la riga corrispondente al $p$-esimo pattern
- $(x_{p,i})$ → è l'attributo $i$ del pattern $p$

### 1.1.2 Task

- **Predire** → classificazione e regressione → approssimazione di funzioni
- **Desrivere** → cluster analysis, association rules → trova un sotto insieme da dati non classificati

#### Supervised Learning

Prende in ingresso dati di traning come `<input, output> = <x,d>`, ossia esempi etichettati per trovare una funzione sconosciuta \(f\)

**Obbiettivo** → trovare una buona approssimazione di $f$, ossia un ipotesi $h$ che verrà usata per predire dei dati non visti e quindi che sia in grado di generalizzare

#### Classificazione

Gli esempi sono visti come membri di una classe e l'obbiettivo è quello di assegnarli alla classe giusta e quindi etichettarli

- $f(x)$ restituisce la giusta classe per $x$

**Concept Learning**

$h ( x ) = sign( w^Tx   + w_0)$

#### Regressione

è il processo per definire una funzione che restituisce un valore reale basandosi su un set finito di esempi rumorosi $(x,f(x)+\text{noise})$

\$h_w(x)=w_1x+w_0$

#### Unsupervised Learning

Il training set è formato da dati non etichettati `<x>`

**Obbiettivo** → trovare raggruppamenti naturali nel dataset

- Clustering
- Riduzione della dimensione e Preprocessing
- Modelling data density

#### Semi-Supervised Learning

Combina esempi etichettati e non per generare una funzione appropriata o un classificatore

#### Reinforcement Learning

- Adattamento  autonomo del sistema
- Impara come agire in base all'osservazione, ogno azione agisce sull'ambiente e l'ambiente restituisce un feedback che guida l'apprendimento dell'algoritmo.

**Terminologia**

- **Rumore** → fattori esterni aggiuntivi nell'insieme delle informazioni
- **Valori Anomali - Outliers** → valori non usuali che non sono consistenti con la maggior parte delle osservazioni
    - Outliers detection or preprocessing
    - Robust modeling methods
- Feature Selection → piccolo numeri di caratteristiche informative che rappresentano una buona rappresentazione del problema

### 1.1.3 Learning Algorithms

Basandosi sui dati, le task e i modelli cerchiamo all'interno dello spazio delle ipotesi \(H\) la migliore ipotesi, ossia l'approssimazione migliore per la funzione target sconosciuta cercando di minimizzare l'errore.

- I parametri liberi del modello sono adattati al compito da svolgere, migliori sono i \(w\) nel modello lineare meglio saranno le regole per il modello simbolico

## 1.2 Loss Function

La loss misura quanto sto sbagliando, sono diverso dall'etichetta $d$ ossia quella con cui è stato etichettato il dato in input $x$

> **Funzione Generica LOSS**

- > dice che l’errore del modello è la **media delle loss sui singoli esempi**

    >> $Loss(h_w ) = E(w) =\frac{1}{l}\sum_{p=1}^lL(h_w(x_p),d_p)$

> $L()$ è proprio la **loss specifica sul singolo esempio** → cioè la misura dell’errore tra la predizione $h_w(x_p)$ e il target $d_p$.

> $L()$ è il cuore della Loss → a seconda del problema, si sceglie una loss diversa

### 1.2.1 Regression

> Consiste nel trovare i \(w_i\) per cui l’errore è minimizzato → sul training set con \(l\) esempi

$L(h_w(x_p),d_p ) = (d_p-h_w(x_p))^2$

$Loss(h_w ) = E(w) =\sum_{p=1}^l(y_p-h_w(x_p))^2$

dove $x_p$ è il p-esimo input e $y_p$ il p-esimo target.

| Bisogna minimizzare l'errore per avere un miglior fitting e quindi trovare i migliori \(w\)

> Per minimizzare questo errore devo porre:

$$
\frac{\mathfrak{d} E(w)}{\mathfrak{d} w_i}=0
$$

**Least Square** → non è per tutti i valori

### 1.2.2 Classificazione

- **Output** → \(\{0,1\}\)
- **H** → un set di funzioni dell'indicatore
- **Loss Function** \(L\) → misura dell'errore di classificazione

\[L(h_w(x_P),d_p)=\begin{cases}0&&h_w(x_P)=d_p\\1&&altrimenti\end{cases}\]

La media del set di dati restituisce il numero/percentuale di classificazioni errate

## 1.3 Bias Induttivo

- **Bias di Linguaggio →** è dovuto alla scelta di H come insieme di funzioni lineari (non permettendo di risolvere problemi non-lineari)
    - Nota: questo bias si può attenuare tramite la linear basis expansion permettendo modelli non-lineari rispetto alle variabili di input. Resta una assunzione di linearità sui \(w\)
- **Bias** **di Ricerca →** è dovuto all'ordine di ricerca imposti dalla Least Squares minimization con discesa di gradiente. Non è una strategia di ricerca completa (si ricordi che è una ricerca locale). Inoltre, nelle forme più articolate possiamo scegliere un metodo diverso, come ad esempio imporre nella Loss restrizioni sul valore dei pesi, che portano a diverse soluzioni con altre proprietà sul controllo della complessità (ad esempio con la ridge regression, Tikhonov) .

> **Bias di Linguaggio**

> Siccome lo abbiamo definito come insieme di funzioni lineari non permette di risolvere problemi non lineari.

> Per superare questo problema abbiamo utilizzato **LBE → Linear Basis Expansion** che permetteva di espandere rispetto alle variabili di ingresso

> **Bias di Ricerca**

> è stata fatta una scelta su come è stata fatta la ricerca Least Squares con discesa del gradiente.

> Possiamo scegliere un metodo diverso, come ad esempio imporre nella Loss restrizioni sul valore dei pesi, che portano a diverse soluzioni con altre proprietà sul controllo della complessità ad esempio con la ridge regression, Tikhonov

> Senza inductive bias non possiamo generalizzare, dato che non possiamo estrarre nessuna regola di associazione dai dati. Bisogna trovare un bias adatto che non restringa troppo e quindi tagli soluzioni.

## 1.4 Clustering and Vector Quantificazione

- Pagina 58

### 1.4.1 Stima della Densità

- Pagina 59

## 1.5 Generalizzazione

- **Learning Phase** → per costruire il modello
- **Prediction Phase** → valutare la funzione di apprendimento su nuovi dati → capacità di generalizzazione

> **Overfitting** → un apprendimento overfitta dei dati se l'ipotesi \(h\) con errore di generalizzazione \(R\) e errore empirico/training \(E\) ma esiste un'ipotesi \(h'\) tale che \(E'>E\) e \(R'<R\) così allora \(h'\) è la meglio delle due anche se fitta peggio i dati di traning.

#### Complessità

- L'insieme delle funzioni sono assunte come polinomi di grado \(M\)
- La complessità di un ipotesi cresce con il grado \(M\)
- \(l\) → numero di esempi

> **Root Mean Square Errore - RMS Error**

> $E_{RMS}\sqrt{2E(w^*)/-l}$
> Dove \(E(w^*)\) è l'errore del modello di training

## 1.6 SLT - Statistical Learning Theory

> Loss Function - Cost Function

> \[L(h(x),d)=(d-h(x))^2\]

> Questo fornisce un limite superiore per l'errore di generalizzazione \(R\)

> \[R\le R_{emp}+\epsilon(1/l,\ VC,\ \delta)\]

> Con:

- > \(R_{emp}=E(x)\) → che rappresenta l'errore sui dati di traning
- > \(l\) → numero di esempi
- > \(VC=VC-dim\) → complessità della funzione di ipotesi \(h\) → flessibilità
- > \(1-\delta\) → è la probabilità che valga la relazione
- > \(\epsilon\) → è il VC-confidence
- \(\epsilon\) → è una funzione che cresce con l'aumentare di VC-dim e decresce con valori più alti di \(l\) e \(\delta\)
- \(R_{emp}\) → decresce con l'aumentare della complessità del modello e quindi \(\)VC-dim
- \(\delta\) → è la confidenza, regola la probabilità che il limite sia valido. Basso delta maggiore confidenza

![](api/attachments/q2l8UFSrRWg0/image/image.png)



- Alto \(l\) → più basso VC-confidence e il il limite si avvicina a \(R\)
- Modello troppo semplice, basso VC-dim → causa un alto \(R_{emp}\) → **Underfitting**
- Alto VC-dim fissato \(l\) → basso errore empirico-\(R_{emp}\) ma VC-confidence e quindi e \(R\) aumentano → **Overfitting**

## 1.7 Validation

> **Model Selection** → stima le performance dei diversi modelli di apprendimento per scegliere il meglio, ossia quello con più capacità di generalizzare

- > Questo significa anche scegliere i meglio iperparametri  del modello come l'ordine del polinomio

> Ritorna un Modello

> **Model Assessment** → stima o valutazione della rischio/errore di predizione testandolo su nuovi dati, è una misura delle performance dell'ultimo modello scelto

> Ritorna un Stima

### 1.7.1 Hold Out Cross Validation

Dividiamo il set \(D\) in: Training Set-\(TR\), Validation or Selection Set-\(VL\) e Test Set-\(TS\)

Tutti i set sono disgiunti

- \(TR\) → usato per il training dell'algoritmo
- \(VL\) → usato per selezionare il modello migliore, ossia la messa a punto degli iperparametri

Test Set → è usato solo per Model Assessment stimando e valutando l'algoritmo e il modello scelto su nuovi dati

![](api/attachments/bTVQnhCj5pjm/image/image.png)



#### K-Fold Cross Validation

Dividiamo il set \(D\) in \(k\) sottoinsieme mutuamente esclusivi \(D_1,D_2,...,D_k\)

Facciamo il training sull'insiemi \(D\backslash D_i\) e lo testiamo su \(D_i\) può essere applicato sia con la divisione VL che TS

*Usiamo tutti i dati per il training e validation o testing*

### 1.7.2 Accuratezza Classificatore

**Specificity** → \(TN/(FP+TN)\) dove True Negative Rate \(=1-FPR\)

**Sensitivity** → \(TP/(TP+FN)\)

- **Recall** → True Positive Rate
- **Precision** → \(TP/(TP+FP)\)

**Accuracy** → percentuale dei pattern classificati correttamente → \(TP+TN/\ total\)

> **ROC Curve**

![](api/attachments/8d00tEUoAFmh/image/image.png)

> 

- > La diagonale è il peggior classificatore possibile perché corrisponde ad avere il 50% di possibilità che il risultato sia un TP o un FP
- > La miglior curva ha un alta Area sotto al grafico - **AUC** così da avere un alto TP-Rate e un basso FP-Rate