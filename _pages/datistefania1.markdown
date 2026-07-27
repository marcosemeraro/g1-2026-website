---
published: false
layout: editorial
title: "Prova"
show_sidetoc: true
header_type: hero
header_img: assets/images/roma_banner.webp
header_title: "Analisi della Vulnerabilità Sociale e Materiale"
subtitle: "Calcolo e distribuzione dell'ISVM sui dati dei Censimenti ISTAT"
---

# L'Indice di Vulnerabilità Sociale e Materiale (ISVM)

L’**Indice di Vulnerabilità Sociale e Materiale (ISVM)** è un indicatore composito costruito attraverso la sintesi di 7 indicatori elementari, riferiti alle dimensioni della vulnerabilità ritenute più rilevanti per la caratterizzazione del territorio.
{: .lead }

Implementato dall’Istat all’interno del sistema di diffusione **“8milaCensus”**, l'indice nasce per offrire uno 
strumento di facile lettura, capace di esprimere con un unico valore sintetico un fenomeno complesso e multidimensionale.

---

### Il ruolo dell'ISVM per l'analisi territoriale

Una domanda sempre più esplicita di misure sintetiche proviene da decisori politici ed enti locali, che richiedono parametri semplici ed efficaci per pianificare e monitorare gli interventi sul territorio. 

La necessità di descrivere accuratamente le modalità di costruzione dell’ISVM nasce proprio dalla ricchezza di informazioni messe a disposizione dal Censimento della Popolazione e delle Abitazioni.

---

### Ambiti di utilizzo istituzionale

Pubblicato nel 2015, l’indice ha avuto un vasto utilizzo in importanti contesti istituzionali: **Collegi elettorali:** 
Determinazione dei collegi della Camera dei Deputati e del Senato, **Audizioni parlamentari:** Analisi dello stato di 
degrado delle città e delle loro periferie, **Finanziamenti ai Comuni:** Parametro per l'erogazione di fondi ai piccoli 
Comuni da parte del Ministero delle Infrastrutture e dei Trasporti.
 
---

### Il Quadro Concettuale: Cos'è la Vulnerabilità?

L’ISVM si fonda sul concetto di vulnerabilità intesa come l'esposizione a fattori di rischio che minacciano l'autonomia 
delle persone e la loro stabilità sociale ed economica (Ranci, 2002).

A differenza della semplice **povertà monetaria** (che misura la sola mancanza di reddito), la vulnerabilità intercetta 
una fragilità più ampia legata a:

* **Nuovi rischi sociali:** Instabilità lavorativa e difficoltà nel conciliare tempi di cura e lavoro.
* **Fasi del ciclo di vita:** Presenza di bambini piccoli, giovani in cerca di occupazione o anziani soli/non autosufficienti.
* **Indebolimento delle reti familiari:** Minore dimensione dei nuclei familiari, che riduce il supporto parentale nei momenti di difficoltà.

L'ISVM nasce quindi per misurare il **grado di esposizione al rischio dei territori**, permettendo ai decisori pubblici di programmare interventi socio-assistenziali mirati prima che la vulnerabilità si trasformi in vera e propria esclusione sociale.



---

![](https://placehold.co/800x200/png)
# Dati Censuari ISTAT

L'ISTAT rilascia periodicamente i dati dei **Censimenti Generali della Popolazione e delle
Abitazioni**, fonte fondamentale per analizzare le dinamiche socio-economiche del territorio. 
Per questo studio sono stati estratti i dati a livello micro-territoriale per gli anni
**2011 e 2021**, superando la dimensione comunale per accedere a una scala d'analisi 
molto più dettagliata.
{: .lead}

---

### Selezione e Pulizia dei Dati

Il processo di preparazione del dataset ha richiesto un'attenta fase di selezione:

* **Dataset di partenza:** Oltre 150 variabili censuarie grezze scaricate ed analizzate.
* **Filtering & Data Cleaning:** Selezione esclusiva delle variabili idonee al calcolo dei 7 indicatori elementari.
* **Risoluzione criticità:** Correzione dei dati incompleti o non uniformi a livello di **Area Sub-Comunale (ASC2)** per garantire la perfetta confrontabilità territoriale.

---


### Selezione delle Variabili e Costruzione degli Indicatori
A partire dalle variabili censuarie grezze, sono stati definiti e calcolati **7 indicatori elementari specifici** per intercettare le diverse dimensioni del disagio socio-economico, oltre al valore sintetico finale:

* **Carico di Minori:** incidenza della popolazione giovanissima (sotto i 14 anni) rispetto al totale delle famiglie.
* **Famiglie Numerose:** incidenza percentuale delle famiglie con sei o più componenti sul totale delle famiglie.
* **Bassa Istruzione:** incidenza della popolazione adulta con un basso livello d'istruzione, calcolata rapportando la popolazione di età compresa fra 25 e 64 anni analfabeta e alfabeta senza titolo di studio al totale della popolazione della stessa fascia d'età.
* **Disagio Assistenziale:** incidenza della popolazione anziana con più di 74 anni rapportata al totale delle famiglie dell'area.
* **Disagio Abitativo:** incidenza delle famiglie in affitto sul totale delle famiglie.
* **NEET:** incidenza dei giovani non occupati e non inseriti in percorsi di istruzione o formazione.
* **Disagio Economico:** percentuale della popolazione con età pari o superiore a 15 anni priva di reddito rispetto al totale della popolazione.

---
### Normalizzazione e Aggregazione (Metodologia AMPI)
Per garantire la comparabilità spaziale e temporale dei dati, la semplicità e trasparenza di calcolo,  
la robustezza ed immediata fruizione e interpretazione dei risultati è stata 
adottata la metodologia ufficiale ISTAT per indici compositi: "**Adjusted Mazziotta-Pareto Index – AMPI**".

### Normalizzazione dei Dati (Min-Max riscalata)
La normalizzazione dei dati, finalizzata a depurare gli indicatori elementari dalla loro variabilità e 
a consentire confronti assoluti nel tempo, è basata su una trasformazione degli
indicatori elementari rispetto ai due valori minimo e massimo che rappresentano il campo
di variazione di ciascun indicatore per l’intero periodo considerato.
Ciascun indicatore elementare $j$ dell'unità $i$ è stato normalizzato per depurarlo dalla propria unità 
di misura ed estensione, riconducendolo a un campo di variazione compreso tra **70 e 130**:

$$r_{ij} = \frac{x_{ij} - \text{Min}_{xj}}{\text{Max}_{xj} - \text{Min}_{xj}} \cdot 60 + 70$$

Dove:
* $r_{ij}$ è il valore normalizzato dell'indicatore $j$ nell'unità $i$.
* $x_{ij}$ è il valore effettivo dell'indicatore $j$ nell'unità $i$.
* $\min_{j}$ e $\max_{j}$ rappresentano i valori minimo e massimo dell'indicatore nella distribuzione considerata.

### Aggregazione con Penalità per Squilibrio ($\text{AMPI}^+$)
L’indice composito si ottiene aggregando gli indicatori normalizzati con peso uguale
mediante media aritmetica semplice, una funzione di sintesi additiva che in quanto tale
presuppone un effetto compensativo fra gli indicatori elementari. In
questa applicazione l’effetto compensativo della media aritmetica (effetto medio) è corretto
aggiungendo alla media un fattore (coefficiente di penalità) che dipende dalla variabilità dei
valori normalizzati di ciascuna unità (denominata variabilità orizzontale), ossia dalla variabilità 
degli indicatori rispetto ai valori di riferimento utilizzati per la normalizzazione.
L'indice di sintesi finale si ottiene applicando l'**AMPI con penalità positiva ($\text{AMPI}^+$)**:

$$\text{AMPI}_i^+ = M_{ri} + S_{ri} \cdot cv_i$$

Dove:
* $M_{ri}$ è la media aritmetica dei valori normalizzati degli indicatori per l'unità $i$.
* $S_{ri}$ è lo scostamento quadratico medio dei valori normalizzati per l'unità $i$.
* $cv_i = \frac{S_{ri}}{M_{ri}}$ è il **coefficiente di variazione**, che misura lo squilibrio orizzontale tra gli indicatori.

> **Perché la penalità positiva?** Trattandosi di un fenomeno "negativo" (vulnerabilità e svantaggio), 
> il coefficiente di penalità corregge la media degli indicatori normalizzati "spingendola verso 
> l'alto". Il fattore correttivo è funzione diretta del coefficiente di variazione dei valori 
> normalizzati per ogni unità ($S_{ri} \cdot cv_i$) e, a parità di media aritmetica, consente di 
> penalizzare le unità che presentano un maggiore squilibrio fra gli indicatori o la presenza anche di 
> un solo valore estremamente critico. Più è alto il valore dell'indice, maggiore è il livello di 
> vulnerabilità dell'area. Di conseguenza, l'indice sintetico valorizza e mette in evidenza i quartieri che 
> presentano anche una sola forma acuta di vulnerabilità, garantendo che le situazioni di emergenza 
> locale non vengano annullate dal calcolo statistico.
{: style="font-size: 0.85rem; line-height: 1.3;" }



---

### Georeferenziazione e Sezioni di Censimento (ASC2 / Shapefile)
Per visualizzare lo svantaggio sullo spazio urbano, i valori calcolati sono stati associati alle partizioni geografiche 
dell'ISTAT tramite i file **Shapefile**. In particolare, l'ISVM è stato mappato a livello di **Aree Sub-Comunali (ASC2)** 
per isolare i confini reali dei quartieri e mostrare le micro-disuguaglianze all'interno della città.

---

### Analisi Territoriale e Mappatura
Utilizzando la libreria **GeoPandas** per la gestione dei dati geografici e **Altair (Vega-Lite)** per la 
visualizzazione interattiva, i punteggi dell'ISVM sono stati convertiti in **mappe cromatiche (coroplete)**. 
Questo approccio permette di esplorare dinamicamente il territorio e individuare visivamente le cosiddette 
*"isole di vulnerabilità"*, ossia i quartieri in cui si concentrano le maggiori criticità materiali e sociali.

