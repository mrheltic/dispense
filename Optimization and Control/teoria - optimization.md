# Formulario Definitivo: Modelli PL e PLI

Guida completa con spiegazione logica delle variabili, formulazione matematica ed esempi pratici tratti dalle slide del corso di Optimization and Control (Operations Research).

## Premessa: dal «problema» al «modello»

Un problema di ottimizzazione si formalizza in tre ingredienti:

- **insieme ammissibile** $F$: l'insieme delle scelte "valide";
- **funzione obiettivo** $z : F \to \mathbb{R}$: valuta ogni scelta;
- **senso di ottimizzazione**: $\min$ se voglio il costo più basso, $\max$ se voglio il profitto più alto.

In Formula: $P = (F, z; \min) \iff \min\{z(x) : x \in F\}$.

In un problema di **Programmazione Lineare (PL)** la funzione obiettivo e i vincoli che descrivono $F$ sono lineari (chiusi), le variabili sono continue e i parametri sono deterministici.

In **Programmazione Lineare Intera/PLI** alcune (o tutte) le variabili sono vincolate ad assumere valori interi o binari: la PLI permette di modellare "indivisibilità" e "scelte finite tra alternative".

Convenzione di notazione adottata in tutto il formulario:

- $x_j$ (ovvero $x_{ij}$, $x_{ijt}$, ...): livello di una decisione continua;
- $y_j, y_i \in \{0,1\}$: variabile binaria indicatrice ("interruttore");
- $c_j$ o $p_j$: coefficiente di costo/profitto nella F.O.;
- $f_j$: costo **fisso** associato a $y_j$;
- $a_{ji}$: coefficiente tecnologico (riga $=$ vincolo $j$, colonna $=$ variabile $x_i$);
- $b_i$ o $d_j$: termine noto ($b$ di RHS, $d$ di domanda);
- $\mathbb{R}$ oppure $\mathbb{R}_+$: dominio delle var. continue non-negative.

---

## PARTE 1 — Programmazione Lineare (PL)

### 1. Pianifica mix di produzione

Problema: ho $n$ prodotti che richiedono $m$ risorse limitate; voglio scegliere le quantità $x_i$ da produrre in modo da massimizzare il profitto senza esaurire le risorse.

**Variabili.** $x_i \in \mathbb{R}_+$, $i = 1, \dots, n$: quantità del bene $i$ da produrre.
Queste variabili "**consumano**" le risorse: se $x_i$ cresce, i vincoli si avvicinano al limite.

**Dati.**

- $c_i$: profitto unitario del bene $i$;
- $a_{ji}$: consumo di risorsa $j$ per unità di $i$;
- $b_j$: disponibilità massima della risorsa $j$.

**Formulazione.**

$$
\begin{aligned}
\max\ & z(x) = \sum_{i=1}^{n} c_i x_i \\
\text{s.t.}\ & \sum_{i=1}^{n} a_{ji}\, x_i \;\le\; b_j, \quad j = 1, \dots, m \\
& x_i \;\ge\; 0, \quad i = 1, \dots, n
\end{aligned}
$$

**Estensioni usuali.** Quota produttiva $x_i \le M_i$ (vincolo di mercato massimo), quote minime $x_i \ge m_i$, gestione di più impianti (variabili $x_{ij}$: quantità di bene $j$ prodotta sull'impianto $i$) → il problema si riscrive con doppio indice estendendo il mix classico.

**Esempio illustrativo (biocarburanti, esercizio 1).** Due biocarburanti con profitto $c_1 = 540$ €/t e $c_2 = 590$ €/t; tre stabilimenti (Preparazione, Purificazione, Estrazione) con disponibilità $18$, $18$, $16$ ore/giorno e matrice di consumi $\mathbf{a}$ nota. La formulazione è

$$
\max\ 540 x_1 + 590 x_2, \quad
\begin{cases}
0.72 x_1 + 0.85 x_2 \le 18 \\
1.68 x_1 + 1.42 x_2 \le 18 \\
1.92 x_1 + 2.12 x_2 \le 16 \\
x_1, x_2 \ge 0
\end{cases}
$$

**Variante 3-impianti (argilla).** Variabili $x_{iA}, x_{iB}, x_{iC}$ con $i = 1, 2, 3$. Oltre ai vincoli di capacità per ogni impianto, si può imporre una quota minima e una massima: p.es. "argilla A deve costituire tra il 50% e il 70% della produzione totale"

$$
0.5 \sum_{i, k} x_{ik} \;\le\; \sum_i x_{iA} \;\le\; 0.7 \sum_{i, k} x_{ik}
$$

Si scrivono spesso come due vincoli: uno $\ge$ e uno $\le$ (guai a dimenticarli entrambi).

---

### 2. Miscelazione / problema della dieta

Problema: ho $n$ ingredienti e $m$ requisiti minimi (nutrienti, qualità); scelgo le quantità da acquistare per **minimizzare il costo** rispettando le **soglie minime**.

**Variabili.** $x_i \in \mathbb{R}_+$: quantità acquistata dell'ingrediente $i$.

**Dati.**

- $c_i$: costo unitario dell'ingrediente $i$;
- $a_{ji}$: apporto del nutriente $j$ per unità di $i$;
- $b_j$: soglia minima di nutriente $j$.

**Formulazione.**

$$
\begin{aligned}
\min\ & z(x) = \sum_{i=1}^{n} c_i x_i \\
\text{s.t.}\ & \sum_{i=1}^{n} a_{ji}\, x_i \;\ge\; b_j, \quad j = 1, \dots, m \\
& x_i \;\ge\; 0, \quad i = 1, \dots, n
\end{aligned}
$$

**Trucco mnemonico.** Nota la simmetria con il mix di produzione: obiettivo $\min$ con vincoli $\ge$ e obiettivo $\max$ con vincoli $\le$. È l'idea "duale del mix": i vincoli sono specchiati perché qui "comprare di più" va a vantaggio della qualità (se il nutriente fa bene) ma l'obiettivo ti frena perché vuoi spendere il meno possibile.

**Estensioni.** Limiti di assunzione $x_i \le M_i$ (overdose massima), limiti massimi sui nutrienti ($\le$), vincoli su più dosaggi combinati.

---

### 3. Miscelazione con vincoli di composizione (qualità e percentuali)

Problema (tipico: acciaio): si desidera produrre una quantità totale $Q$ di prodotto finito (p.es. $100$ q di acciaio) miscelando $n$ ingredienti (rottami). Ogni ingrediente $i$ contribuisce a $m$ componenti (ferro, nichel, ...) con percentuale nota; voglio che i componenti rispettino soglie minime, esatte e massime.

**Variabili.** $x_i \in \mathbb{R}_+$: quantità del rottame $i$ nella miscela.

**Dati.**

- $c_i$: costo unitario dell'ingrediente;
- $A_{ki}$: percentuale del componente $k$ nell'ingrediente $i$ (in forma frazionaria, es. $0.93$ = 93%);
- $\underline{B}_k$, $B_k$, $\overline{B}_k$: quote minima, esatta, massima del componente $k$;
- $M_i$: disponibilità massima dell'ingrediente $i$.

**Formulazione (esempio acciaio al 65% Fe, 18% Ni esatto, 10% Cr esatto, 1% imp. max, 100 q totali).**

$$
\begin{aligned}
\min\ & z(x) = \sum_{i=1}^{n} c_i x_i \\
\text{s.t.}\ & \text{(almeno 65\% Ferro)} \quad \sum_i A_{\text{Fe},i}\, x_i \;\ge\; 65 \\
& \text{(esattamente 18\% Nichel)} \quad \sum_i A_{\text{Ni},i}\, x_i \;=\; 18 \\
& \text{(esattamente 10\% Cromo)} \quad \sum_i A_{\text{Cr},i}\, x_i \;=\; 10 \\
& \text{(al più 1 di impurità)} \quad \sum_i A_{\text{Imp},i}\, x_i \;\le\; 1 \\
& \text{(quantità totale)} \quad \sum_i x_i \;=\; 100 \\
& x_i \;\le\; M_i, \quad i = 1, \dots, n\\
& x_i \;\ge\; 0, \quad i = 1, \dots, n
\end{aligned}
$$

**Attenzione esame.** Su 100 q, "l'almeno 65% di ferro" si scrive linearmente come $\sum A_{\text{Fe},i} x_i \ge 65$ (non $\ge 65 \sum x_i$): la percentuale si traduce in quantità solo perché $\sum x_i = 100$. Se la quantità totale non fosse fissata, il vincolo percentuale diventerebbe $\sum (A_{\text{Fe},i} - 0.65) x_i \ge 0$ con coefficiente $0.65$ che filtra via la "quota di ferro dovuta".

---

### 4. Miscelazione multi-prodotto (multi-blend)

Caso uguale al precedente ma con $p$ miscele da produrre in parallelo. Si usa un secondo indice $k = 1, \dots, p$.

**Variabili.** $x_{jk} \in \mathbb{R}_+$: quantità dell'ingrediente $j$ nella miscela $k$.

**Dati.**

- $c_j$: costo unitario dell'ingrediente $j$;
- $A_{rj}$: percentuale di componente $r$ nell'ingrediente $j$;
- $\underline{B}_{rk}$, $\overline{B}_{rk}$: soglie del componente $r$ nella miscela $k$;
- $M_j$: disponibilità globale dell'ingrediente $j$ (condivisa tra le miscele);
- $D_k$: domanda minima della miscela $k$.

**Formulazione (fertilizzanti $A, B$).**

$$
\begin{aligned}
\min\ & z = \sum_{j, k} c_j x_{jk} \\
\text{s.t.}\ & \sum_j A_{rj}\, x_{jk} \;\ge\; \underline{B}_{rk}, \quad \text{(requisiti minimi)} \\
& \text{oppure } \;\ge\; \underline{B}_{rk} \;\text{ oppure } \;=\; B_{rk} \;\text{ opzionalmente} \\
& \sum_k\bigl(A_{rj}\, x_{jk}\bigr) \;\le\; \text{richieste esatte / massime} \\
& \sum_k x_{jk} \;\le\; M_j, \quad j = 1, \dots, n \quad\text{(disponibilità ingredienti)} \\
& x_{jk} \;\ge\; 0
\end{aligned}
$$

**Esempio (fertilizzanti $A, B$, 2 composti).** Composto 1 €/kg, composto 2 $4$ €/kg, $30\,000$ kg di composto 1 e $25\,000$ kg di composto 2. Viene

$$
\begin{aligned}
\min\ & 3 x_{1A} + 4 x_{2A} + 3 x_{1B} + 4 x_{2B} \\
\text{s.v.}\ & 0.5 x_{1A} + 0.7 x_{2A} \;\ge\; 10\,000 \\
& 0.4 x_{1A} + 0.06 x_{2A} \;\ge\; 4\,000 \\
& 0.5 x_{1B} + 0.7 x_{2B} \;=\; 10\,000 \\
& 0.4 x_{1B} + 0.06 x_{2B} \;\ge\; 8\,000 \\
& x_{1A} + x_{1B} \;\le\; 30\,000 \\
& x_{2A} + x_{2B} \;\le\; 25\,000 \\
& x_{1A}, x_{1B}, x_{2A}, x_{2B} \;\ge\; 0
\end{aligned}
$$

(Notare che le variabili $x_{1A}$ e $x_{1B}$ relative allo stesso ingrediente $1$ sono legate dal vincolo "somma totale $\le$ disponibilità del composto $1$". È il controllo chiave: la risorsa condivisa è il composto, non la miscela.)

---

### 5. Modello multi-periodo (produzione e stoccaggio)

Problema: sull'orizzonte $t = 1, \dots, T$ devo programmare quantità prodotte $x_t$ e quantità stoccata $I_t$. Voglio minimizzare il costo di produzione più il costo di stoccaggio.

**Variabili.**

- $x_t \in \mathbb{R}_+$: quantità prodotta nel periodo $t$;
- $I_t \in \mathbb{R}_+$: quantità stoccata a fine del periodo $t$ (Inventory).

**Dati.**

- $c_t$: costo unitario di produzione nel periodo $t$;
- $h_t$: costo unitario di stoccaggio nel periodo $t$;
- $d_t$: domanda nel periodo $t$ (da soddisfare);
- $u_t$: capacità produttiva massima;
- $S_t$: capacità massima di stoccaggio;
- $I_0$: scorta iniziale (nota).

**Formulazione.**

$$
\begin{aligned}
\min\ & z(x, I) = \sum_{t=1}^{T} \bigl(c_t x_t + h_t I_t\bigr) \\
\text{s.t.}\ & I_{t-1} + x_t \;=\; d_t + I_t, \quad t = 1, \dots, T \quad \text{(bilanciamento)}\\
& x_t \;\le\; u_t \quad \text{(capacità produttiva)}\\
& I_t \;\le\; S_t \quad \text{(capacità stoccaggio)}\\
& x_t, I_t \;\ge\; 0
\end{aligned}
$$

**Chiave mnemonica.** L'equazione di bilanciamento è quella del "secchio": l'acqua già presente all'inizio ($I_{t-1}$) più quella prodotta ($x_t$) deve eguagliare l'acqua bevuta dai clienti ($d_t$) più l'acqua che avanza a fine mese ($I_t$): $I_{t-1} + x_t = d_t + I_t$.

($I_0$ è dato; se anche $I_T$ è fissato, p.es. $I_T = 0$ per "svuotare il magazzino alla fine", si aggiunge questo vincolo.)

**Esempio numerico (lamierati di zinco, 3 mesi).** Scorta iniziale $I_0 = 100$, domande $d = (270, 290, 250)$, capacità $u = (250, 220, 280)$, costi di produzione $c = (12, 14, 16)$, costi di stoccaggio $h = (1.2, 1.1, 0.9)$. Primo vincolo di bilanciamento (scritto come $x_t - I_t = d_t - I_{t-1}$):

$$
x_1 - I_1 = 170,\quad x_2 + I_1 - I_2 = 290,\quad x_3 + I_2 - I_3 = 250
$$

---

### 6. Problema del trasporto

Problema: ho $n$ fornitori con disponibilità $a_i$ e $m$ clienti con domanda $b_j$. Spedisco $x_{ij}$ a costo unitario $c_{ij}$. Minimizzare il costo di trasporto.

**Variabili.** $x_{ij} \in \mathbb{R}_+$: quantità spedita dal fornitore $i$ al cliente $j$.

**Formulazione.**

$$
\begin{aligned}
\min\ & z(x) = \sum_{i=1}^{n} \sum_{j=1}^{m} c_{ij}\, x_{ij} \\
\text{s.t.}\ & \sum_{j=1}^{m} x_{ij} \;\le\; a_i, \quad i = 1, \dots, n\quad\text{(capacità fornitore)}\\
& \sum_{i=1}^{n} x_{ij} \;\ge\; b_j, \quad j = 1, \dots, m\quad\text{(domanda cliente)}\\
& x_{ij} \;\ge\; 0,\quad \forall (i, j)
\end{aligned}
$$

**Interpretazione mnemonica.** "La somma sulle righe è ciò che esce dalla fabbrica, la somma sulle colonne è ciò che entra in casa del cliente".

**Condizione di ammissibilità (necessaria).** $\sum_i a_i \ge \sum_j b_j$. Se tutti i fornitori sono collegati a tutti i clienti, è anche sufficiente.

**Estensioni classiche.** Capacità massime sui singoli archi $x_{ij} \le u_{ij}$; assenza di archi (vincolo $x_{ij} = 0$); presenza di centri intermedi (transshipment); perdite (rendimenti $x_{ij}^{\text{uscita}} = \eta\, x_{ij}^{\text{entrata}}$); prodotti multipli con capacità condivisa ($\sum_{(p,i,j)} \gamma_{p} x_{pij} \le u_{ij}$).

---

### 7. Schedulazione di attività (CPM / PERT)

Problema: schedulare $n$ attività rispettando relazioni di precedenza, minimizzando il tempo di completamento complessivo.

**Variabili.** $t_i \in \mathbb{R}_+$: istante di inizio dell'attività $i$.

**Dati.**

- $p_i$: durata dell'attività $i$;
- $\mathrm{prec}(i)$: insieme delle attività che devono terminare prima di $i$.

**Formulazione linearizzata.** Si introduce una variabile ausiliaria $T$ (tempo di fine progetto) e si linearizza il $\max$ dell'obiettivo.

$$
\begin{aligned}
\min\ & T \\
\text{s.t.}\ & T \;\ge\; t_i + p_i, \quad i = 1, \dots, n\quad\text{(completion time $\le T$)}\\
& t_i \;\ge\; t_j + p_j, \quad i = 1, \dots, n,\;\; j \in \mathrm{prec}(i)\\
& t_i \;\ge\; 0
\end{aligned}
$$

(Se l'unica attività senza successori è quella finale, in realtà basta porre $T \ge t_f + p_f$.)

**Earliest starting time.** Fissato $T = T^*$ (ottimo del modello sopra), si minimizza la somma degli inizi:

$$
\min \sum_i t_i \quad\text{s.v.}\;T = T^*,\; T \ge t_i + p_i,\; t_i \ge t_j + p_j,\; t_i \ge 0
$$

**Latest starting time.** Si massimizza (invece di minimizzare) partendo da $T = T^*$:

$$
\max \sum_i t_i \quad\text{s.v.}\;T = T^*,\; T \ge t_i + p_i,\; t_i \ge t_j + p_j,\; t_i \ge 0
$$

**Attività critiche e percorso critico.** Un'attività è **critica** se `tempo inizio al più presto` $=$ `tempo inizio al più tardi`. Il **percorso critico** è la catena di attività critiche che collega inizio e fine del progetto. È noto in letteratura come **CPM** (Critical Path Method) o **PERT**.

---

## PARTE 2 — Programmazione Lineare Intera (PLI)

### 8. Modelli del taglio ottimo

Problema: tagliare moduli standard (stoffa, lamiera, ...) di dimensione $D$ in moduli richiesti di dimensioni $d_i$, in modo da minimizzare lo spreco → basta minimizzare il numero di moduli standard utilizzati, perché lo spreco = moduli $\times D$ − richiesta soddisfatta.

**Variabili.** $x_j \in \mathbb{Z}_+$: numero di moduli standard tagliati con lo schema di taglio $j$, $j = 1, \dots, n$.

**Dati.**

- $d_i$, $r_i$: dimensione e domanda del pezzo $i$;
- $a_{ij}$: numero di pezzi di dimensione $d_i$ prodotti da uno standard tagliato con schema $j$;
- $n$: numero di schemi di taglio possibili.

**Formulazione.**

$$
\begin{aligned}
\min\ & z(x) = \sum_{j=1}^{n} x_j \\
\text{s.v.}\ & \sum_{j=1}^{n} a_{ij}\, x_j \;\ge\; r_i, \quad i = 1, \dots, m \\
& x_j \;\ge\; 0, \;\text{intero},\; j = 1, \dots, n
\end{aligned}
$$

**Esempio (lamiera di zinco $10 \times 1\ \mathrm{m^2}$, 5 schemi di taglio $j = 1, \dots, 5$, domande $23, 34, 28, 20, 35$).**

$$
\min\; x_1 + x_2 + x_3 + x_4 + x_5
$$

$$
\begin{aligned}
2 x_1 + x_4 + x_5 & \ge 23\\
x_1 + 2 x_2 + x_4 & \ge 34\\
x_2 + 2 x_4 & \ge 28\\
2 x_3 + x_5 & \ge 20\\
x_1 + 2 x_2 + x_3 + 2 x_5 & \ge 35
\end{aligned}
$$

$$
x_1, \dots, x_5 \;\ge\; 0, \;\text{interi}
$$

**Trucco mnemonico.** Qui il vincolo è di tipo "dieta" ($\ge$), ma l'obiettivo è $\min$ (perché si richiede di minimizzare il numero di moduli standard usati, equivalente a minimizzare lo spreco $= D \sum x_j - \sum d_i r_i$). All'esame quindi descrivi sempre "schema di taglio $j$ produce $a_{ij}$ unità del pezzo $i$", perché $a_{ij}$ è il dato più facilement scritto male.

---

### 9. Modello dello zaino (tre varianti)

Lo zaino è il modello binario archetipico: ho $n$ oggetti con "valore" $c_j$ e "peso" $p_j$, una capacità $b$; scelgo un sottoinsieme il cui peso non superi $b$ e il valore totale sia massimo.

**Variabili.** $x_j \in \{0,1\}$ (binario classico) oppure $x_j \in \mathbb{Z}_+$ (zaino intero): 1 se l'oggetto $j$ è inserito o indico il numero di pezzi $j$ da inserire.

**Ipotesi standard.**

- $c_j > 0$ (altrimenti non inseriremmo l'oggetto);
- $p_j \le b$ (altrimenti l'oggetto non entra da solo);
- $\sum_j p_j > b$ (altrimenti prendere tutto sarebbe soluzione banale).

#### 9.1 Zaino binario (0/1)

$$
\begin{aligned}
\max\ & z(x) = \sum_{j=1}^{n} c_j x_j \\
\text{s.v.}\ & \sum_{j=1}^{n} p_j x_j \;\le\; b \\
& x_j \in \{0, 1\}, \quad j = 1, \dots, n
\end{aligned}
$$

**Corrispondenza astratta** per problemi di investimento: oggetti $\to$ progetti $j$, "peso" $\to$ capitale richiesto $p_j$, "valore" $\to$ rendimento atteso $c_j$, "capacità" $\to$ budget $b$.

**Esempio investimento (Progetti $A, B, C$).**

$$
\begin{aligned}
\max\; & 100 x_A + 80 x_B + 75 x_C \\
\text{s.v.}\ & 90 x_A + 80 x_B + 70 x_C \;\le\; 200 \\
& x_A, x_B, x_C \in \{0, 1\}
\end{aligned}
$$

#### 9.2 Zaino intero

La stessa struttura ma $x_j \in \mathbb{Z}_+$: decide *quanti* oggetti di tipologia $j$ inserire.

$$
x_j \;\ge\; 0 \;\text{intero}, \quad j = 1, \dots, n
$$

(Quando il testo dice "riempire scatole con capacità $120$ g scegliendo differenti tipi di biscotti, ogni tipo caratterizzato da peso e % cioccolato pregiato" allora diventa zaino intero, perché posso prendere più biscotti dello stesso tipo.)

#### 9.3 Zaino multidimensionale

Ho più di un vincolo ($m$ diverse risorse o $m$ periodi temporali). Per ciascuna risorsa $i$ ho un limite $b_i$ e per ogni oggetto $j$ ho coefficienti $p_{ij}$ (es. peso $p_{1j}$, volume $p_{2j}$).

$$
\begin{aligned}
\max\ & z(x) = \sum_{j=1}^{n} c_j x_j \\
\text{s.v.}\ & \sum_{j=1}^{n} p_{ij}\, x_j \;\le\; b_i, \quad i = 1, \dots, m \\
& x_j \in \{0, 1\}, \quad j = 1, \dots, n
\end{aligned}
$$

**Esempio (compagnia aerea, 4 mesi).** Budget mensili $b = (1000, 750, 950, 1400)$ in migliaia di euro. Tre progetti con fabbisogno finanziamento in 4 mensilità:

$$
\begin{aligned}
\max\; & 480 x_1 + 300 x_2 + 250 x_3 \\
\text{s.v.}\ & 750 x_1 + 200 x_2 + 980 x_3 \le 1000 \\
& 550 x_1 + 150 x_2 + 70 x_3 \le 750 \\
& 820 x_1 + 80 x_2 + 600 x_3 \le 950 \\
& 500 x_1 + 105 x_2 + 203 x_3 \le 1400 \\
& x_1, x_2, x_3 \in \{0, 1\}
\end{aligned}
$$

(Notare: gli stessi oggetti compaiono in TUTTI i vincoli — segno caratteristico della multidimensionalità.)

---

### 10. Costi fissi di avviamento

Problema: per produrre un bene $j$ si sostiene un costo variabile $c_j$ (per unità) **e**, se si avvia la produzione, un costo fisso $f_j$ (indipendente dalla quantità prodotta). La funzione di costo è discontinua in $x_j = 0$: non linearizzabile con sola $x_j$ reale.

**Variabili.**

- $x_j \in \mathbb{R}_+$: quantità prodotta del bene $j$;
- $y_j \in \{0, 1\}$: variabile binaria **indicatrice**, vale $1$ se "viene attivato" il prodotto $j$, $0$ altrimenti.

**Vincolo "ponte" (key).** Sia $M_j$ un limite superiore noto per la produzione $x_j$ (grande abbastanza da non "forzare" la soluzione):

$$
x_j \;\le\; M_j\, y_j, \quad j = 1, \dots, n
$$

**Logica del ponte.** Se $y_j = 0$, allora $x_j \le 0$ (la macchina è bloccata). Se $y_j$ scatta a $1$, allora $x_j \le M_j$ (può produrre fino alla massima capacità).

Si introduce anche una condizione logica: $x_j > 0 \Rightarrow y_j = 1$ e $x_j = 0 \Rightarrow y_j = 0$. La **prima** è espressa dal vincolo ponte. La **seconda** è garantita dalla minimizzazione: se $x_j = 0$ non conviene attivare $y_j = 1$ perché pagheresti il costo fisso $f_j$ a vuoto.

**Formulazione completa.** Siano $b_i$ le domande di prodotti finiti $i$ e $a_{ij}$ la quantità del prodotto finito $i$ realizzabile con un'unità del semilavorato $j$.

$$
\begin{aligned}
\min\ & z(x, y) = \sum_{j=1}^{n} \bigl(c_j\, x_j + f_j\, y_j\bigr) \\
\text{s.v.}\ & \sum_{j=1}^{n} a_{ij}\, x_j \;\ge\; b_i, \quad i = 1, \dots, m \\
& x_j \;\le\; M_j\, y_j, \quad j = 1, \dots, n \\
& x_j \;\ge\; 0, \quad y_j \in \{0, 1\}
\end{aligned}
$$

**Esempio (4 condizionatori, 3600 m³, 15 ricambi/h).** Portate orarie $M = (44\,000, 48\,000, 36\,000, 51\,000)\ \mathrm{m^3/h}$, costi di attivazione $f = (500, 450, 600, 580)\ €/\mathrm{h}$, costi di erogazione $c = (2.42, 2.64, 1.98, 2.80)\ €/(\mathrm{m^3/h})$. Richiesta totale: $3600 \cdot 15 = 54\,000\ \mathrm{m^3/h}$.

$$
\begin{aligned}
\min\ & 2.42 x_1 + 2.64 x_2 + 1.98 x_3 + 2.80 x_4 + 500 y_1 + 450 y_2 + 600 y_3 + 580 y_4 \\
\text{s.v.}\ & x_1 + x_2 + x_3 + x_4 \;\ge\; 54\,000 \\
& x_1 \le 44\,000 y_1,\;\; x_2 \le 48\,000 y_2,\;\; x_3 \le 36\,000 y_3,\;\; x_4 \le 51\,000 y_4 \\
& x_i \ge 0,\;\; y_i \in \{0, 1\}
\end{aligned}
$$

**Pattern ("aspettative all'esame").** La tecnica del **vincolo ponte** con $x \le M y$ è il modo standard per collegare una quantità continua a un interruttore binario:  
  
> *un vincolo di tipo "$x \le M y$" ogni volta che "qualcosa avviene solo se l'interruttore y è acceso".*

---

### 11. Localizzazione di impianti (CPL, SPL, p-mediana)

Problema: decidere **dove** aprire centri di servizio (siti potenziali $N_1$) e come assegnarli ai successori ($N_2$) minimizzando il costo di trasporto più il costo di attivazione. Si usa un grafo bipartito completo $G = (N_1 \cup N_2, E)$.

**Caso di studio: CPL — Capacitated Plant Location.**

**Variabili.**

- $y_i \in \{0, 1\}$: $1$ se attivo il sito $i$, $0$ altrimenti;
- $x_{ij} \in \mathbb{R}_+$: **frazione** della domanda $d_j$ del successore $j$ servita dal sito $i$. (Equivalente: $s_{ij}$ = quantità assoluta; basta porre $x_{ij} = s_{ij} / d_j$.)

**Dati.** $k_{ij}$ = costo unitario di trasporto da $i$ a $j$; $f_i$ = costo fisso di attivazione; $q_i$ = capacità massima del sito $i$.

**Formulazione standard (variabili frazionali).**

$$
\begin{aligned}
\min\ & z(x, y) = \sum_{i \in N_1} \sum_{j \in N_2} k_{ij} d_j\, x_{ij} + \sum_{i \in N_1} f_i\, y_i \\
\text{s.v.}\ & \sum_{i \in N_1} x_{ij} \;=\; 1, \quad j \in N_2 \quad \text{(domanda del $j$ soddisfatta al $100\%$)}\\
& \sum_{j \in N_2} d_j\, x_{ij} \;\le\; q_i\, y_i, \quad i \in N_1 \quad \text{(vincolo ponte: il sito eroga solo se aperto)}\\
& x_{ij} \;\ge\; 0, \quad y_i \in \{0, 1\}
\end{aligned}
$$

Le origini dei costi nella F.O. sono duplici:

- $k_{ij} d_j x_{ij}$: $k_{ij}$ costa *per unità*, dunque il costo di trasporto sul rapporto $i \to j$ è $k_{ij}\, (\text{quantità trasportata}) = k_{ij} (d_j x_{ij})$.
- $f_i y_i$: costo di attivazione del sito $i$ se (e solo se) è acceso.

#### 11.1 CPL con limiti di attività

Aggiungo al modello CPL i vincoli su attività di ogni sito (soglia minima $q^-_i$ e soglia massima $q^+_i$):

$$
\sum_{j \in N_2} d_j\, x_{ij} \;\le\; q_i^+\, y_i, \qquad \sum_{j \in N_2} d_j\, x_{ij} \;\ge\; q_i^-\, y_i
$$

(Se un sito è "acceso" $y_i = 1$ la quantità servita dovrà essere in $[q^-, q^+]_i$).

#### 11.2 SPL — Simple Plant Location

Se i nodi logistici non hanno vincoli sul livello di attività, sostituisco il vincolo "capacità" con un vincolo che lega banalmente $x_{ij}$ a $y_i$. La scelta consueta è

$$
\sum_{j \in N_2} x_{ij} \;\le\; |N_2|\, y_i, \quad i \in N_1
$$

(Se $y_i = 0$, nessun $x_{ij}$ può essere attivo; se $y_i = 1$, tutti i $N_2$ successori possono essere assegnati al sito.)

#### 11.3 p-mediana (caso specifico di CPL)

Sotto due ipotesi particolari il modello CPL si semplifica al modello di p-mediana:

- $f_i = f$ per ogni $i \in N_1$ (costo di attivazione costante);
- $d_j = 1$ per ogni $j \in N_2$;
- vincolo aggiuntivo $\sum_{i \in N_1} y_i = p$, con $p$ numero **fissato** di nodi da attivare.

Poiché il costo di attivazione complessivo è $f \cdot p$ (costante), in F.O. resta solo il termine di trasporto (con $d_j = 1$ il termine $k_{ij} d_j$ della CPL coincide con $k_{ij}$, qui rinominato $c_{ij}$):

$$
\begin{aligned}
\min\ & z(x) = \sum_{i \in N_1} \sum_{j \in N_2} c_{ij} x_{ij} \\
\text{s.v.}\ & \sum_{i \in N_1} x_{ij} = 1, \quad j \in N_2 \\
& \sum_{j \in N_2} x_{ij} \le |N_2|\, y_i, \quad i \in N_1 \\
& \sum_{i \in N_1} y_i = p \\
& x_{ij} \ge 0, \quad y_i \in \{0, 1\}
\end{aligned}
$$

(p-mediana è tipicamente la forma "devo aprire esattamente $p$ sportelli, uffici, semafori, bancomat, ...")

#### 11.4 Altre varianti PL

**Numero prefissato di siti.** $\sum_i y_i = p$.

**Sottoinsiemi obbligatoriamente aperti.** $y_i = 1$ per $i \in N_1' \subseteq N_1$.

#### 11.5 Esempio "Sili" (CPL)

- $N_2 = $ 7 fattorie, domande $d = (36, 42, 34, 50, 27, 30, 43)$;
- $N_1 = $ 6 sili, capacità $q = (80, 90, 110, 120, 100, 120)$;
- costi fissi su 4 anni $f$ (in €): $(321\,420, 350\,640, 379\,860, 401\,775, 350\,640, 336\,030)$ — vanno divisi per 1461 giorni e combinati con il costo di stoccaggio giornaliero;
- costi di trasporto $k_{ij} = 0.06\ €/(\text{km}\cdot\text{ql})$ "andata+ritorno".

La struttura del modello è come nella CPL generica con $d_j$ e $q_i$ corretti.

**Estensione (esempio 8).** Sesto silo, se attivato, deve servire almeno $90$ ql: si aggiunge il vincolo
$$
36 x_{61} + 42 x_{62} + 34 x_{63} + 50 x_{64} + 27 x_{65} + 30 x_{66} + 43 x_{67} \;\ge\; 90 y_6
$$

---

### 12. Caricamento di contenitori (Bin Packing, 1-BP)

Problema: $m$ oggetti di peso $p_i$, $n$ contenitori identici di capacità $q$. Assegnare ogni oggetto a esattamente un contenitore rispettando la capacità; minimizzare il numero di contenitori usati.

**Variabili.**

- $y_j \in \{0, 1\}$: $1$ se il contenitore $j$ è utilizzato;
- $x_{ij} \in \{0, 1\}$: $1$ se l'oggetto $i$ è assegnato al contenitore $j$.

**Formulazione.**

$$
\begin{aligned}
\min\ & z(x, y) = \sum_{j=1}^{n} y_j \\
\text{s.v.}\ & \sum_{j=1}^{n} x_{ij} \;=\; 1, \quad i = 1, \dots, m \quad \text{(ogni oggetto finisce in UN solo cassone)}\\
& \sum_{i=1}^{m} p_i\, x_{ij} \;\le\; q\, y_j, \quad j = 1, \dots, n \quad \text{(capacità + ponte)}\\
& x_{ij} \in \{0, 1\}, \quad y_j \in \{0, 1\}
\end{aligned}
$$

**Esempio (aspirapolvere, 6 componenti su 4 linee).** Linee identiche con capacità $580$ min. Tempi di lavorazione $p = (150, 300, 250, 300, 220, 190)$ min. Voglio minimizzare le linee attivate.

$$
\begin{aligned}
\min\ & y_1 + y_2 + y_3 + y_4 \\
\text{s.v.}\ & x_{11} + x_{12} + x_{13} + x_{14} = 1, \dots, x_{61} + x_{62} + x_{63} + x_{64} = 1 \quad \text{(6 assegnazioni)}\\
& 150 x_{11} + 300 x_{21} + 250 x_{31} + 300 x_{41} + 220 x_{51} + 190 x_{61} \le 580 y_1 \\
& \text{(analoghi per $j = 2, 3, 4$)}\\
& x_{ij}, y_j \in \{0, 1\}
\end{aligned}
$$

#### 12.1 Bin Packing con contenitori non identici

Se i contenitori hanno capacità $q_j$ e costi di utilizzo $c_j$ differenti:

- F.O. diventa $\min \sum_j c_j y_j$ (costo totale, NON più numero di contenitori);
- Vincolo di capacità diventa $\sum_i p_i x_{ij} \le q_j y_j$.

**Esempio (corriere, veicoli misti).** $3$ furgoni aziendali da $800$ kg (costo $100$ €), $2$ camion a noleggio da $3000$ kg ($1.4$ €/km × $592$ km), $1$ camion con rimorchio da $3500$ kg ($1.6$ €/km × $592$ km).

---

### 13. Copertura, Riempimento, Partizionamento di insieme

Siano:

- $M = \{1, \dots, m\}$ universo di elementi;
- $S = \{S_1, \dots, S_k\}$ famiglia di sottoinsiemi di $M$;
- $F \subseteq S$ sottofamiglia scelta.

Definizioni:

| Nome | Condizione |
|------|------------|
| **Covering** (copertura) | $\bigcup_{F_i \in F} F_i = M$ |
| **Packing** (riempimento) | $F_i \cap F_j = \emptyset\ \forall i \ne j$ |
| **Partitioning** (partizionamento) | è sia copertura che packing |

**Variabili.** $x_j \in \{0, 1\}$: $1$ se $S_j$ è scelto nella famiglia soluzione.

**Matrice di incidenza $A$.** $a_{ij} = 1$ se $i \in S_j$, $0$ altrimenti (riga $=$ elemento $i$, colonna $=$ sottoinsieme $j$).

**Forme standard.**

| Problema | Vincolo | Obiettivo |
|----------|---------|-----------|
| Set Covering | $A x \;\ge\; \mathbf{1}$ | $\min c^T x$ |
| Set Packing | $A x \;\le\; \mathbf{1}$ | $\max p^T x$ |
| Set Partitioning | $A x \;=\; \mathbf{1}$ | $\min c^T x$ |

**Interpretazione del vincolo "almeno uno" in covering.** $\sum_{j: i \in S_j} x_j \ge 1$ per ogni $i$: ciascuno elemento deve essere coperto da almeno un sottoinsieme scelto. (Per packing: $\le 1$ → disgiunti; per partitioning: $= 1$ → copertura esatta, "uno e un solo".)

**Esempio mnemonico** "squadre/progetti/personale": pompieri → covering, distretti elettorali → partitioning, squadre di lavoro (una persona non può appartenere a più squadre) → packing.

#### 13.1 Applicazioni tipiche

**Localizzazione impianti.** $M$ = comunità da proteggere, $N$ = siti candidati, $S_j$ = comunità raggiungibili da $j$, $c_j$ = costo di installazione di una stazione in $j$: minimo numero/costo di stazioni necessarie a coprire tutte le comunità.

**Altre applicazioni.** Formazione di gruppi di lavoro (coprire tutte le competenze al minimo costo), routing (coprire tutti i clienti con un insieme di rotte).

**Esempio numerico (Set Covering, 11 regioni, costo unitario).**

$$
\min\;\sum_{j=1}^{11} x_j
$$

Soggetto a 11 vincoli "ogni regione sia coperta da almeno una stazione delle regioni sue o adiacenti", p.es.:

$$
\begin{cases}
x_1 + x_2 + x_3 + x_4 \;\ge\; 1 \\
x_1 + x_2 + x_3 + x_5 \;\ge\; 1 \\
\ldots \\
x_9 + x_{10} + x_{11} \;\ge\; 1
\end{cases}
\quad x_j \in \{0, 1\}
$$

#### 13.2 Esempio "macchine d'epoca" (condizionatore con logical da combinare)

Testo della traccia: 6 auto con valore $v$ e costo $c$, budget $15\,000$ €, almeno 3 auto selezionate, alcuni vincoli logici:

- "Selezionata Bianchina → inclusa anche Lancia Flavia Sport" (implicazione): $x_{\text{Bianchina}} \le x_{\text{Lancia}}$;
- "Se NON inclusa Fiat 500 → inclusa Alfa Romeo" (implicazione contraria): $1 - x_{\text{Fiat}} \le x_{\text{Alfa}}$ (ovvero $x_{\text{Fiat}} + x_{\text{Alfa}} \ge 1$);
- "almeno 3 auto tra le 6": $\sum_j x_j \;\ge\; 3$.

La struttura del modello è

$$
\begin{aligned}
\max\;& 58 x_1 + 37 x_2 + 42 x_3 + 40 x_4 + 55 x_5 + 33 x_6 \\
\text{s.v.}\ & 6000 x_1 + 4000 x_2 + 3800 x_3 + 4200 x_4 + 5500 x_5 + 3200 x_6 \;\le\; 15\,000 \\
& x_1 + x_2 + x_3 + x_4 + x_5 + x_6 \;\ge\; 3 \quad \text{(almeno 3 auto)} \\
& \text{vincoli logici}\ \ldots \\
& x_j \in \{0, 1\}
\end{aligned}
$$

---

## PARTE 3 — Variabili binarie: logica e non linearità

### 14. Condizioni logiche tra variabili binarie

Siano $\alpha$, $\beta$, $\gamma$ tre condizioni logiche ciascuna rappresentata da una variabile binaria $x_\alpha, x_\beta, x_\gamma \in \{0, 1\}$ ($1 = \text{vero}$, $0 = \text{falso}$).

| Condizione logica | Traduzione lineare |
|-------------------|--------------------|
| $\alpha \Rightarrow \beta$ (se $\alpha$ allora $\beta$) | $x_\alpha \;\le\; x_\beta$ |
| $\gamma = \alpha \land \beta$ (AND: $\gamma$ vera **sse** entrambe vere) | $x_\alpha \ge x_\gamma,\;\; x_\beta \ge x_\gamma,\;\; x_\gamma \ge x_\alpha + x_\beta - 1$ |
| $\gamma = \alpha \lor \beta$ (OR: $\gamma$ vera se almeno una vera) | $x_\alpha \le x_\gamma,\;\; x_\beta \le x_\gamma,\;\; x_\gamma \le x_\alpha + x_\beta$ |
| "Al più una tra $\alpha, \beta, \gamma$" | $x_\alpha + x_\beta + x_\gamma \;\le\; 1$ |
| "Esattamente una tra $\alpha, \beta, \gamma$" | $x_\alpha + x_\beta + x_\gamma \;=\; 1$ |

**Verifica logica delle formule AND.**

- $x_\alpha \ge x_\gamma, x_\beta \ge x_\gamma$: "se $\gamma$ è accesa, allora anche $\alpha$ e $\beta$ devono essere accese".
- $x_\gamma \ge x_\alpha + x_\beta - 1$: "se $\alpha$ e $\beta$ sono entrambe accese ($1 + 1 - 1 = 1$), allora $\gamma$ deve accendersi per forza".

**Verifica delle formule OR.**

- $x_\alpha \le x_\gamma, x_\beta \le x_\gamma$: "se $\alpha$ o $\beta$ vale $1$, allora $\gamma$ vale $1$".
- $x_\gamma \le x_\alpha + x_\beta$: "se entrambe $0$, allora $\gamma = 0$".

(Ciò è formalmente equivalente al $\gamma \Leftrightarrow \alpha \lor \beta$: "γ vera se e solo se almeno una delle due è vera".)

**Trucco mnemonico per implicazione.** "$A \Rightarrow B$" diventa $x_A \le x_B$. Ragiona: se l'ipotesi è accesa ($x_A = 1$), la tesi deve pur essere accesa ($x_B \ge 1$); se l'ipotesi è spenta ($x_A = 0$), $x_B$ resta libera.

---

### 15. Vincoli alternativi (Big-M con disgiunzione)

Problema: garantire che **almeno uno** tra due vincoli di disuguaglianza sia soddisfatto.

$$
\text{(1)}\quad \sum_{j=1}^{n} u_j x_j \;\le\; b_1 \qquad\text{OPPURE}\qquad \text{(2)}\quad \sum_{j=1}^{n} v_j x_j \;\le\; b_2
$$

**Tecnica.** Si introducono due binarie $y_1, y_2 \in \{0, 1\}$ con $y_k = 0$ se il vincolo $k$ è rispettato, $y_k = 1$ se è rilassato (disattivato). $M > 0$ sufficientemente grande.

$$
\begin{aligned}
\sum_j u_j x_j - M y_1 &\;\le\; b_1 \\
\sum_j v_j x_j - M y_2 &\;\le\; b_2 \\
y_1 + y_2 &= 1 \quad \text{(almeno un vincolo resta attivo: ESATTAMENTE uno dei due è rilassato)}\\
y_1, y_2 &\in \{0, 1\}
\end{aligned}
$$

**Logica del Big-M.** Se $y_1 = 1$, il vincolo (1) diventa $\sum u_j x_j \le b_1 + M$, che è sempre verificato (relaxed). Se invece $y_1 = 0$, il vincolo è "rigido".

**Versione compatta (una sola binaria).** Poiché $y_2 = 1 - y_1$:

$$
\sum_j u_j x_j - M y_1 \le b_1,\qquad \sum_j v_j x_j - M (1 - y_1) \le b_2,\quad y_1 \in \{0, 1\}
$$

**Estensioni.** Per "$m$ vincoli alternativi, imponendo che ALMENO / ESATTAMENTE / AL PIÙ $K$ di essi siano **rilassati**": si introducono $m$ binarie $y_k$ (con $y_k = 1$ se il vincolo $k$ è rilassato, $y_k = 0$ se è attivo) e si aggiunge un vincolo $\sum_k y_k \ge K$ (almeno $K$ rilassati), $= K$ (esattamente), $\le K$ (al più).

---

### 16. Funzione lineare a tratti (piecewise linear)

Si vuole modellare una F.O. del tipo

$$
z(x) = \begin{cases}
a_1 + b_1 x, & x \in [i_1, i_2] \\
a_2 + b_2 x, & x \in (i_2, i_3]
\end{cases}
$$

Siano $I_1 = i_2 - i_1$, $I_2 = i_3 - i_2$ le ampiezze degli intervalli.

**Variabili.**

- $y_1, y_2 \in \{0, 1\}$: $1$ se $x$ appartiene al rispettivo intervallo;
- $w_1, w_2 \in \mathbb{R}_+$: "scarto" di $x$ rispetto al limite inferiore dell'intervallo.

**Vincoli.**

$$
y_1 + y_2 = 1,\quad 0 \;\le\; w_1 \;\le\; I_1\, y_1,\quad 0 \;\le\; w_2 \;\le\; I_2\, y_2,\quad x = i_1 y_1 + w_1 + i_2 y_2 + w_2
$$

**F.O. linearizzata.**

$$
z(y_1, y_2, w_1, w_2) \;=\; a_1 y_1 + b_1 (i_1 y_1 + w_1) + a_2 y_2 + b_2 (i_2 y_2 + w_2)
$$

**Cosa succede "intuitivamente".** Scegliendo $y_1$ o $y_2$, "abilito" l'intervallo corrispondente; il Big-M sui $w_k$ impedisce a $w_k$ di esistere quando l'intervallo non è quello selezionato (quindi $w_k = 0$ se $y_k = 0$).

---

### 17. Valore assoluto in funzione obiettivo

#### 17.1 Minimizzazione di $|f(x)|$

$$
\min\ z = |f(x)| \;\;\text{s.v.}\; x \in X
\qquad\equiv\qquad
\begin{cases}
\min\ k \\
k \ge f(x) \\
k \ge -f(x) \\
x \in X
\end{cases}
$$

(Introduco una variabile fantasma $k$; poiché minimizzo $k$ e $k$ sta sopra sia $f$ che $-f$, si "schiaccia" precisamente sul valore assoluto.)

**Vincoli con modulo.** Per $|g(x)| \le b$:

$$
g(x) \le b, \qquad -g(x) \le b \;\;\iff\;\; -b \le g(x) \le b
$$

#### 17.2 Massimizzazione di $|f(x)|$

Più delicata: per "spegnere" l'altro ramo serve il Big-M e una binaria $y$ ($= 1$ se $f(x) < 0$) introdotta ad hoc.

$$
\begin{cases}
\max\ k \\
k \le f(x) + M y \\
k \le -f(x) + M (1 - y) \\
M (1 - y) \ge f(x) \\
M y \ge -f(x) \\
y \in \{0, 1\}, \; x \in X
\end{cases}
$$

(La chiave: $k$ viene "bloccato dal basso" solo sul ramo corretto. Se $f \ge 0$ → $y = 0$, $k \le f$. Se $f < 0$ → $y = 1$, $k \le -f$.)

**Esempio orale (tre profumi).** Tre profumi con domanda, qualità $Q_i$ (%), prezzo. F.O. lineare $\max |\,Q_2 - 15\,|$, con $Q_2 = \sum_j A_{j,2}\, x_{j,2}$ qualità del profumo 2.

La modellistica si riduce a:

1. Formula un modello LP con vincoli standard (domanda, qualità min/max, ricavo $\ge 95\,000$, ...);
2. Aggiungi le variabili $k$ e $y$;
3. Aggiungi i due vincoli $k$ e i due vincoli ausiliari del Big-M.

(Soluzione ottima attesa $z = 11$.)

---

## PARTE 4 — Schema riassuntivo per l'esame

| # | Modello | Variabili | F.O. | Segno vincoli principali | Variabili indicatrici "$y$" |
|---|---------|------------|------|---------------------------|----------------------------|
| 1 | Mix produzione | $x_i \in \mathbb{R}_+$ | max $c^T x$ | $\le$ | — |
| 2 | Dieta / miscela | $x_i \in \mathbb{R}_+$ | min $c^T x$ | $\ge$ | — |
| 3 | Miscela con % esatte | $x_i \in \mathbb{R}_+$ | min $c^T x$ | $\ge / = / \le$ | — |
| 4 | Multi-miscela | $x_{jk} \in \mathbb{R}_+$ | min $\sum c x$ | $\ge / = / \le$ + disponibilità | — |
| 5 | Multi-periodo | $x_t, I_t \in \mathbb{R}_+$ | min $c^T x + h^T I$ | $=$ (bilanciamento) | — |
| 6 | Trasporto | $x_{ij} \in \mathbb{R}_+$ | min $\sum c x$ | $\le$ (riga), $\ge$ (colonna) | — |
| 7 | Schedulazione (CPM) | $t_i \in \mathbb{R}_+$ | min $T$ | $\ge$ (precedenza) | — |
| 8 | Taglio ottimo | $x_j \in \mathbb{Z}_+$ | min $\sum x_j$ | $\ge$ (domanda) | — |
| 9 | Zaino (binario/intero/multi) | $x_j \in \{0, 1\}$ o $\mathbb{Z}_+$ | max $\sum c_j x_j$ | $\le$ | — |
| 10 | Costi fissi | $x_j \in \mathbb{R}_+$, $y_j \in \{0, 1\}$ | min $c^T x + f^T y$ | $\ge$ (domanda) | $x_j \le M_j y_j$ |
| 11 | CPL / SPL / p-mediana | $x_{ij} \in \mathbb{R}_+$, $y_i \in \{0, 1\}$ | min $\sum k d x + \sum f y$ | $=$ (domande), $\le$ (capacità) | $\sum_j d_j x_{ij} \le q_i y_i$ |
| 12 | Bin Packing | $x_{ij}, y_j \in \{0, 1\}$ | min $\sum y_j$ oppure $\sum c_j y_j$ | $=$ (assegnazione), $\le$ (capacità) | $\sum_i p_i x_{ij} \le q y_j$ |
| 13 | Cover / Pack / Partition | $x_j \in \{0, 1\}$ | min $c^T x$ / max $c^T x$ | $A x \; \ge / \le / = \mathbf{1}$ | — |
| 14 | Logica binaria | $x_\alpha \in \{0, 1\}$ | — | $\le / =$ | linearizza AND/OR |
| 15 | Vincoli alternativi | $x \in \mathbb{R}^n$, $y_k \in \{0, 1\}$ | linearizzare F.O. | 2 vincoli con Big-M | $y_1 + y_2 = 1$ |
| 16 | Linearizzazione a tratti | $y_k \in \{0, 1\}$, $w_k \in \mathbb{R}_+$ | $\sum_k y_k = 1$ + F.O. linearizzata | Big-M su $w_k$ | $w_k \le I_k y_k$ |
| 17 | Valore assoluto | $k \in \mathbb{R}_+$, $y \in \{0, 1\}$ (solo per max) | $\min k$ / $\max k$ | $k \ge f(x)$, $k \ge -f(x)$ | Solo per max $|f|$: Big-M |

### Trucchi mnemonici trasversali

- **"Vincolo ponte" con Big-M.** Quando una quantità $x$ può "essere attiva" solo se un interruttore binario $y$ è acceso, scrivi $x \le M y$. Se serve anche "se $x > 0$ allora $y = 1$" basta questo vincolo; la minimizzazione del F.O. si occupa del duale $x = 0 \Rightarrow y = 0$.
- **"Equazione del secchio" multi-periodo.** $I_{t-1} + x_t = d_t + I_t$ è invariante: quel che entra nel secchio (rimanenza precedente + produzione) eguaglia quel che esce (domanda + rimanenza finale).
- **Segno associativo di MAX/MIN.** MAX si sposa $\le$, MIN si sposa $\ge$, duale specchiato.
- **Cover/Packing/Partitioning.** $\ge$ per coprire, $\le$ per riempire, $=$ per partizionare.
- **CPL vs SPL.** CPL ha vincoli di capacità $\sum_j d_j x_{ij} \le q_i y_i$; SPL sostituisce con $\sum_j x_{ij} \le |N_2| y_i$.
- **Bin Packing $=$ Zaino "multi-zaino"** con $x_{ij}$ e "vincolo ponte" per le capacità del singolo cassone.