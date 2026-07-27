---
published: false
layout: default
title: "Analisi"
show_sidetoc: true
header_type: hero
header_img: assets/images/roma_banner.webp
header_title: "Analisi"
#subtitle: "Una Pagina di Prova del gruppo 1"
vega: true
#plotly: true
---

Questo lavoro esplora le relazioni tra le caratteristiche socioeconomiche, demografiche, immobiliari e territoriali dei quartieri e la loro instabilità politica, definita come il cambiamento del vincitore rispetto all’elezione precedente. I modelli di machine learning sono utilizzati principalmente come strumenti esplorativi per individuare le condizioni associate a una maggiore contendibilità politica.
{: .lead }

---

![](https://placehold.co/800x200/png)

## Data cleaning e feature selection

I dati provenienti dalle diverse fonti sono stati integrati, armonizzando i nomi delle colonne e riconducendo gli intervalli temporali a singoli anni.

La variabile target è una variabile binaria:

* `0`: il vincitore rimane invariato;
* `1`: il vincitore cambia rispetto all’elezione precedente.

Sono state eliminate le osservazioni prive di un risultato elettorale precedente e le variabili che avrebbero potuto introdurre **data leakage**. Le feature fortemente correlate sono state ridotte per limitare la ridondanza.

Sono state utilizzate due strategie di validazione:

1. **temporale**, con il 2018 come training set e il 2022 come test set;
2. **geografica**, escludendo a turno una città dall’addestramento.

## Suddivisione temporale

La validazione temporale misura la capacità delle relazioni apprese nel 2018 di generalizzare all’elezione del 2022. Il dataset comprende 322 osservazioni di training e 321 di test.

<div class="asc-winners-chart">
  <vegachart schema-url="{{ site.baseurl }}/assets/charts/e_bar_winrchangd_cnt_year.json" style="width: 100%; height: 100%"></vegachart>
</div>

Data la distribuzione sbilanciata della variabile target, la valutazione si è concentrata su **balanced accuracy**, precision, recall e F1-score della classe `1`.

L’analisi è stata ripetuta utilizzando:

* il feature set completo;
* un feature set senza `e_prev_winner_margin`, per isolare il contributo delle variabili territoriali e socioeconomiche.

### Clustering

Sono stati applicati K-means con due cluster e DBSCAN.

Con il feature set completo, K-means individua due gruppi con una diversa incidenza del cambiamento politico, pari rispettivamente al 19,5% e al 31,4%. Senza `e_prev_winner_margin`, la differenza scompare quasi completamente: 27,7% contro 27,5%.

DBSCAN assegna tutte le osservazioni a un unico cluster e non produce quindi una segmentazione informativa.

Il clustering suggerisce che la competitività dell’elezione precedente contribuisce maggiormente alla distinzione tra aree politicamente stabili e instabili rispetto alle sole caratteristiche strutturali.

### Confronto tra i classificatori

Sono stati confrontati tre modelli:

* Random Forest con pesi bilanciati;
* XGBoost;
* Random Forest con `RandomOverSampler`.

Gli iperparametri sono stati selezionati mediante grid search e cross-validation stratificata. L’oversampling è stato applicato esclusivamente all’interno dei fold di training.

<div class="asc-winners-chart">
  <vegachart schema-url="{{ site.baseurl }}/assets/charts/clf_year_results.json" style="width: 100%; height: 100%"></vegachart>
</div>

| Modello                        | Balanced accuracy | Precision classe `1` | Recall classe `1` | F1 classe `1` |
| ------------------------------ | ----------------: | -------------------: | ----------------: | ------------: |
| Random Forest                  |             0,676 |                0,456 |             0,522 |         0,487 |
| XGBoost                        |             0,709 |                0,376 |             0,768 |         0,505 |
| Random Forest con oversampling |             0,677 |                0,375 |             0,652 |         0,476 |

XGBoost riconosce la quota maggiore di cambiamenti, ma produce anche più falsi positivi. L’oversampling aumenta la recall della Random Forest senza migliorarne sostanzialmente la balanced accuracy.

La Random Forest con pesi bilanciati è stata selezionata per le analisi interpretative perché presenta la precision più elevata per la classe `1` e un comportamento più conservativo.

### Ruolo del margine di vittoria

`e_prev_winner_margin` rappresenta il margine percentuale tra la prima e la seconda corrente politica nell’elezione precedente. Un valore ridotto indica una maggiore contendibilità elettorale.

La variabile emerge come il principale predittore sia nella feature importance sia nella permutation importance calcolata sul test del 2022.

Tra le altre feature rilevanti compaiono il reddito medio, la sua volatilità, alcune dinamiche immobiliari, gli indicatori relativi alle famiglie numerose e la polarità delle notizie. Il loro contributo risulta però più debole.

La rimozione della variabile riduce le prestazioni di tutti i classificatori:

| Modello                        | Con margine | Senza margine |
| ------------------------------ | ----------: | ------------: |
| Random Forest                  |       0,676 |         0,563 |
| XGBoost                        |       0,709 |         0,634 |
| Random Forest con oversampling |       0,677 |         0,556 |

Senza `e_prev_winner_margin`, l’importanza si distribuisce tra reddito medio, volatilità economica, disuguaglianza, polarità mediatica, disagio sociale e dinamiche immobiliari. 
Nessuna di queste variabili fornisce però un segnale altrettanto stabile.

### Interpretazione con SHAP

I valori SHAP sono stati utilizzati per analizzare il contributo delle feature alle previsioni della Random Forest.

![](assets/charts/shap_y_m_tp_summary.svg)
Nel modello che include `e_prev_winner_margin`, i **true positive**, cioè i casi in cui il cambiamento del vincitore viene 
correttamente identificato, sono spiegati soprattutto da questa variabile. Valori più bassi spingono infatti il modello verso la previsione della classe 1, 
indicando una maggiore contendibilità elettorale. 

Anche livelli più bassi di reddito medio e valori più negativi del sentiment delle notizie contribuiscono alla previsione del cambiamento. 

Un ulteriore segnale è rappresentato da un’elevata dinamicità dei prezzi degli immobili del settore terziario, che comprende attività commerciali e di servizio 
come uffici, istituti di credito, studi professionali e cliniche private. Questa relazione può essere interpretata come un possibile indicatore di 
maggiore pressione economica sulle attività presenti nel territorio, pur senza implicare un rapporto causale diretto.

![](assets/charts/shap_y__tp_summary.svg)
Nel modello che esclude `e_prev_winner_margin`, oltre alle variabili già individuate emerge anche l’indicatore relativo al numero di minori a carico. 
Valori più elevati di questa variabile contribuiscono a orientare correttamente il modello verso la previsione della classe associata al cambiamento 
del vincitore. Questo risultato suggerisce che una maggiore presenza di famiglie con minori possa caratterizzare contesti territoriali più esposti a condizioni 
di instabilità politica, senza tuttavia implicare una relazione causale diretta.

![](assets/charts/shap_y_m_fn_summary.svg)
Nel caso dei **false negative**, ossia quando il modello non riesce a rilevare un cambiamento effettivamente avvenuto, le variabili principali restano simili 
a quelle osservate nei true positive. Tuttavia, emerge una relazione meno lineare con il reddito medio: sia valori elevati sia valori bassi possono contribuire 
a orientare erroneamente il modello verso la previsione di continuità. 

Inoltre, alcuni cambiamenti si verificano nonostante una polarità positiva delle notizie, 
mostrando che questo segnale non è sempre sufficiente a distinguere correttamente le aree politicamente stabili da quelle in cui il vincitore cambia.

Nel modello completo, margini elettorali precedenti ridotti spingono generalmente la previsione verso la classe `1`, mentre margini elevati orientano verso la continuità politica.

Le caratteristiche socioeconomiche e territoriali modificano questa previsione, ma configurazioni simili compaiono anche nei falsi positivi e nei falsi negativi. Il modello sembra quindi riconoscere condizioni di vulnerabilità politica senza riuscire sempre a distinguere un contesto contendibile da un cambiamento effettivo.

Senza `e_prev_winner_margin`, le spiegazioni SHAP risultano più distribuite tra numerose variabili e meno nette.

### Sintesi validazione temporale

La storia elettorale recente rappresenta il segnale maggiormente trasferibile dal 2018 al 2022. 
Le variabili socioeconomiche, demografiche, mediatiche e immobiliari descrivono il contesto nel quale il cambiamento appare più plausibile, ma non mostrano autonomamente una capacità predittiva altrettanto stabile.

## Suddivisione per città

La validazione geografica misura la capacità del modello di generalizzare a una città completamente esclusa dall’addestramento.

<div class="asc-winners-chart">
  <vegachart schema-url="{{ site.baseurl }}/assets/charts/e_bar_winrchangd_cnt_city.json" style="width: 100%; height: 100%"></vegachart>
</div>

Bologna e Firenze contengono esclusivamente osservazioni della classe `0`, mentre Milano, Napoli, Roma e Torino presentano entrambe le classi. 
Utilizzarle come normali città di test avrebbe quindi prodotto metriche poco confrontabili.

### Protocollo di validazione

È stata applicata una **nested cross-validation per città**:

* Milano, Napoli, Roma e Torino sono state escluse a turno e utilizzate come test set;
* Bologna e Firenze sono state sempre mantenute nel training;
* gli iperparametri sono stati selezionati mediante una validazione interna sulle città rimanenti.

Il protocollo selezionato ha prodotto:

* balanced accuracy media: `0.563`;
* macro F1 medio: `0.539`;
* deviazione standard: `0.130`.

| Città di test | Balanced accuracy | Macro F1 |
| ------------- | ----------------: | -------: |
| Milano        |             0,652 |    0,600 |
| Napoli        |             0,370 |    0,357 |
| Roma          |             0,625 |    0,624 |
| Torino        |             0,606 |    0,574 |

Il modello conserva parte della capacità discriminante a Milano, Roma e Torino, mentre mostra prestazioni particolarmente deboli a Napoli.

L’esclusione completa di Bologna e Firenze riduce la balanced accuracy media a `0.522`, suggerendo che le due città forniscano informazioni utili per l’addestramento, nonostante non siano adatte a essere utilizzate come test set binari.

### Permutation importance e SHAP

Permutation importance e SHAP sono stati calcolati separatamente per ciascuna città esclusa.

`e_prev_winner_margin` emerge ancora come la variabile più importante. Seguono il reddito medio, le dinamiche immobiliari e alcune misure di volatilità economica e della disuguaglianza.

![](assets/charts/shap_c__fn_summary.svg)
Nella validazione basata sulle città, escludendo `e_prev_winner_margin`, i **false negative** sono associati anche a valori elevati dell’indicatore di bassa 
istruzione. In questi casi, il modello tende a interpretare tale caratteristica come un segnale di continuità politica e non riesce quindi a riconoscere il 
cambiamento effettivamente avvenuto. 

Un comportamento simile emerge per `m_Gini_volatility`: variazioni elevate della disuguaglianza dei redditi orientano il 
modello verso una previsione di stabilità politica, anche quando il vincitore cambia. Questi risultati mostrano che, nel trasferimento tra città, alcune 
caratteristiche socioeconomiche possono assumere un significato diverso da quello appreso durante l’addestramento.

Il contributo di queste variabili cambia tuttavia tra le città, indicando che il modello apprende relazioni almeno in parte specifiche dei singoli contesti urbani.

### Confronto senza `e_prev_winner_margin`

La rimozione della variabile riduce la balanced accuracy media da `0.563` a `0.512` e il macro F1 da `0.539` a `0.424`.

| Città  | Con margine | Senza margine |
| ------ | ----------: | ------------: |
| Milano |       0,652 |         0,522 |
| Napoli |       0,370 |         0,495 |
| Roma   |       0,625 |         0,500 |
| Torino |       0,606 |         0,531 |

Il miglioramento osservato a Napoli non indica una buona capacità di classificazione, poiché il risultato rimane vicino al livello casuale.

Senza il margine precedente, il reddito medio e le misure di volatilità della disuguaglianza emergono come segnali alternativi. La loro importanza varia però notevolmente tra le città e le stesse feature compaiono sia nelle previsioni corrette sia negli errori.

## Considerazioni conclusive

La capacità del modello di individuare il cambiamento del vincitore dipende principalmente da `e_prev_winner_margin`, che rappresenta direttamente la competitività della precedente elezione.
La sua importanza non deve tuttavia essere interpretata come prova di una relazione uniforme in tutti i contesti. Il vantaggio prodotto dalla variabile è evidente per Milano, Roma e Torino, mentre a Napoli 
il suo utilizzo peggiora le prestazioni. Questo risultato indica che lo stesso margine elettorale può assumere significati differenti a seconda della struttura politica e territoriale della città. 
La variabile è quindi centrale per il modello complessivo, ma non rappresenta una regola universale.

Le caratteristiche socioeconomiche, demografiche, mediatiche e immobiliari forniscono informazioni aggiuntive sul contesto territoriale, ma il loro contributo è più debole e meno stabile nel tempo e tra città differenti.
Il confronto mostra inoltre che la composizione geografica del training set interagisce con il feature set. Bologna e Firenze forniscono informazioni utili quando è presente `e_prev_winner_margin`, 
ma possono introdurre uno sbilanciamento verso la classe 0 quando il modello utilizza soltanto le variabili contestuali. La scelta delle città incluse nel training non costituisce quindi un aspetto secondario, 
ma influenza direttamente le relazioni apprese dal classificatore.

I risultati non mostrano una relazione uniforme e generalizzabile tra le caratteristiche strutturali dei quartieri e l’instabilità politica. Il principale risultato sostantivo dell’analisi è che 
la storia elettorale recente contiene una capacità predittiva superiore rispetto alle sole caratteristiche strutturali del territorio. Le variabili socioeconomiche, demografiche e immobiliari aggiungono 
informazioni contestuali, ma le loro associazioni con il cambiamento politico sono prevalentemente locali e non sufficientemente stabili da sostenere, da sole, una generalizzazione tra città. 
