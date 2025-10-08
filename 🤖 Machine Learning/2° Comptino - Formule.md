---
Score: 6
---

# 1 Loss Function
La funzione di loss misura la differenza tra l'output della funzione approsimata e l'output target. Tramite questa misurazione possiamo capire quanto stiamo sbagliando e muoverci tramite il negativo del gradiente verso il minimo locale.


> [!important] Loss Function
> $$Loss(h_w)=E(w)=\sum_{p=1}^{l}(y_p - h_w (x_p))^2=\sum_{p=1}^{l}(y_p - (w_1 x_p+w_0))^2	$$
> Dove $w$ sono i parametri liberi, $l$ il numero di esempi e $x_p$ il $p-$esimo esempio

# 2 Learning Algorithms - Delta Rule
> È la regola con cui riusciamo a trovare una funzione che approssima abbastanza bene la funzione target, utilizzando un approccio iterativo (Discesa del Gradiente) o diretto (Norma)
> Entrambi utilizzano LMS - Least Mean Square

**Gradiente:**
$$
\frac{\delta E(w)}{\delta w_i}
$$
Questo lo applichiamo per ogni parametro libero e lo imponiamo **uguale a 0** così da trovare il minimo locale.
## 2.1 Input Multidimensionali
$$
h(x_p)=x_pw^T
$$
Possiamo dire che $w_0$ è la **threshold**.
### 2.1.1 Classificazione
Anche per quanto riguarda la classificazione non utilizziamo il segno ($sign$) ma utilizziamo direttamente i valori così semplifichiamo i calcoli e la logica.
$$
Loss(h_w)=E(w)=\sum_{p=1}^{l}(y_p - w^Tx_p)^2
$$
Non usiamo appunto $h(x_p)$ dato che comprenderebbe il segno -> $h(x_p)=sign(w^Tx_p)$ infatti lo omettiamo completamente.
## 2.2 Normal Equation
Possiamo prendere l'**equazione normale**
$$
(X^TX)w=X^Ty
$$
Se $X^TX$ non è singolo e quindi non invertibile prendiamo come unica soluzione la pseudo inversa di Moore:
$$
w=(X^TX)^{-1}X^Ty=X^+y
$$
Altrimenti:
- Le soluzioni sono infinite che soddisfano l'equazione normale e noi possiamo scegliere la soluzione con $min\ \text{norm}(w)$
### 2.2.1 SVD - Singolar Value Decomposition
> Possiamo usarla per calcolare la matrice pseudo inversa ($X^+$)

$$
X=U\Sigma V^T\ \Rightarrow\ X^+=V\Sigma^+U^T
$$
Nella seconda sostituisco tutti i valori non zero con il loro reciproco
> Possiamo applicare direttamente SVD per calcolare $w=X^Ty$ per ottenere la soluzione con la norma minimo del problema least squares

## 2.3 Discesa del Gradiente
> Metodo iterativo basato sulle caratteristiche del gradiente -> mi sposto nella direzione opposto a quella indicata dal gradiente

$$
\frac{E(w)}{\delta w_j}=-2\sum_{P01}^{l}(y_p-x_p^Tw)(x_p)_j
$$
Dove $(x_p)_j$ è la componente $j$ del pattern $p$
![[IMG_0778.jpeg|526x312]]
- Per ogni parametro libero mi sposto sulla direzione opposta al gradiente calcolato per quel parametro
### 2.3.1 Delta Rule
> Ci spostiamo ad ogni passo iterativo basandoci su una regola per modificare i parametri in maniera proporzionale in base al gradiente locale.


> [!attention] Delta Rule - Learning Rule
> $$
> w_{new}=w+eta*\Delta w
> $$
> - $eta\ (\eta)$ -> è lo step size ossia il passo di discesa del gradiente che effettuo, è un iperparametro che setto durante la model selection -> troppo piccolo ci metto troppo ad arrivare al minimo troppo grande lo salto e rischio di non prenderlo mai


> [!important] Simple Algorithm
> ![[image-6.png]]

### 2.3.2 Batch - On-Line

> [!NOTE] Batch
> Invece di aggiornare ad ogni esempio i parametri utilizziamo tutto il dataset e alla fine aggiorniamo, per poi ripetere fino al minimo locale
> - **Mini-Batch** -> invece dell'intero dataset utilizziamo una parte e aggiorniamo quindi una via di mezzo
> - Funzione più levigata e meno variabile

> [!NOTE] On-Line
> Ad ogni esempio di training aggiorniamo i parametri -> seguiamo troppo gli esmpi di training ed anche quindi l'errore
> - Funzione più spigolosa con cambi rapidi

Questo rappresenta la differenza tra On-Line e Batch
![[image-7.png|306x248]]

## 2.4 LBE - Linear Basis Expansion
> Una funzione $\phi_k$ che rende l'equazione non lineare su gli input così da poter essere utilizzata su più problema senza avere l'assunzione di linearità negli input ma solo nei parametri liberi
> - Rischio da controllare complessità troppo elevata -> overfitting
> - Più espressivo -> più possibilità

## 2.5 Tikhonov Regolarization

> [!important] Ridge Regression
> Modello smussato -< aggiungiamo una restizione alla complessità del modello ossia alla somma di $|w_j|$ penalizzando i valori di $|w|$ alti e favorendo i modelli sparsi quindi con più parametri $w_j=0$ o vicini
> $$
> Loss(w)=\sum(y_p-x_p^Tw)^2+\boldsymbol{\lambda||w||_2^2}
> $$
> - $||w||_2^2=\sum w_j^2$

- $\boldsymbol\lambda$ -> è un iperparametro che viene scelto durante la model selection ossia nel validation set
- Deve essere calibrato bene dato che incide anche sul passo di delta rule

> $$w_{new}=w+eta*\Delta w\ -2\lambda w$$
> - $\lambda$ piccola -> rischio l'overfitting, modello troppo espressivo e complesso
> - $\lambda$ troppo grande -> rischio l'underfitting, appiattisco troppo la funzione


> [!NOTE] $\lambda$ Unico Iperparamtreo
> Grazie a $\lambda$ posso utilizzare solo questo iperparametro per modificare la funzione e quindi VC-dim
> - Implementando così un controllo della complessità del modello -> che penalizza pesi con valore alto e tende a guidare i valori di tutti i pesi a valori in media più bassi

![[image-8.png]]
