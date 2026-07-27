---
published: false
#published: false
layout: default
title: "Prova Elezioni"
show_sidetoc: true
header_type: hero
header_img: assets/images/roma_banner.webp
header_title: "Elezioni"
#subtitle: "Una Pagina di Prova del gruppo 1"
vega: true
mathjax: true
#plotly: true
---
<style>
  .asc-winners-chart {
    width: 100%;
    max-width: 100%;
    margin: 1.5rem 0 2rem;
    overflow: hidden;
  }

  .asc-winners-chart vegachart,
  .asc-winners-chart .vega-embed {
    display: block;
    width: 100%;
    max-width: 100%;
  }

  .asc-winners-chart .vega-embed > svg,
  .asc-winners-chart .vega-embed > canvas {
    display: block;
    max-width: 100%;
    height: auto !important;
  }

  .asc-winners-chart form.vega-bindings {
    margin-top: 0.5rem;
  }
</style>

<div class="full-width-wrapper">
    <img src="{{ site.baseurl }}/assets/images/header_alt2.svg" alt="sbd-pattern" class="full-width-image">
</div>

L’idea di utilizzare i dati elettorali per analizzare i processi di segregazione e differenziazione interna delle città italiane è nata dalla lettura del lavoro di [Gabriele Pinto](https://www.tandfonline.com/doi/full/10.1080/2474736X.2023.2185158){:target="_blank"}, pubblicato nel 2023 con il titolo *“Sezioni Elettorali Italiane (SEI): a new database of Italian electoral results geocoded at precinct level”*. Il lavoro raccoglie e geocodifica i risultati delle elezioni italiane a livello di singola sezione elettorale.
{: .lead }

L’aspetto che ci ha maggiormente interessato di questa ampia raccolta di dati è la possibilità di osservare da vicino le differenze elettorali tra i quartieri e di utilizzare il voto come una delle chiavi di lettura delle trasformazioni sociali e territoriali delle città.
{: .lead }

---

![](https://placehold.co/800x200/png)

# Dati elettorali

Il portale Eligendo del Ministero dell’Interno raccoglie i risultati delle elezioni svoltesi in Italia dall’Assemblea Costituente a oggi. Il principale limite di questa fonte rispetto agli obiettivi del nostro lavoro è rappresentato dal livello di aggregazione, poiché i risultati vengono generalmente forniti a livello comunale.

## Scraping dei dati mancanti

Per garantire una maggiore omogeneità tra città diverse, abbiamo scelto di concentrare l’analisi sulle elezioni della Camera dei deputati.

I dati non disponibili in formato strutturato sono stati raccolti direttamente dai portali delle amministrazioni comunali mediante procedure di web scraping. Questo passaggio ha consentito di ottenere i risultati elettorali al livello delle singole sezioni.

## Partiti che vanno, partiti che vengono

Durante l’integrazione dei dati provenienti da città e anni diversi è emerso che i nomi dei partiti non erano riportati in maniera uniforme. La prima fase di armonizzazione è quindi consistita nell’introduzione di un codice identificativo univoco per ciascun partito.

Lo stesso codice è stato mantenuto negli anni per i partiti che hanno modificato la propria denominazione senza alterare in modo sostanziale la struttura e l’area politica di appartenenza. Per ricostruire la storia di cambi di nome, scissioni e fusioni sono state consultate anche fonti documentali esterne. Nei casi in cui un nuovo partito fosse nato dall’unione di più formazioni politiche, è stato invece assegnato un nuovo codice.

I partiti sono stati successivamente raggruppati in correnti politiche più ampie. Come nel lavoro di riferimento, alcune formazioni spiccatamente apartitiche non sono state attribuite ad alcuna corrente. Considerata anche la loro limitata rilevanza in termini di voti, queste formazioni non sono state incluse nelle analisi successive.

<button type="button" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal">
  Esplora Correnti
</button>

<div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog modal-xl modal-dialog-centered">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Correnti politiche</h5>
        <button type="button" class="btn-close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">×</span></button>
      </div>
      <div class="modal-body">
        <table>
          <tr>
            <th>Corrente</th>
            <th>Partiti</th>
          </tr>
          <tr>
            <td>Centro-Sinistra</td>
            <td>Partito Democratico, +Europa, Comunisti Italiani, Rifondazione Comunista, 
                Italia dei Valori, Federazione dei Verdi, Partito Comunista dei Lavoratori, 
                Partito Socialista Italiano, Sinistra Ecologia e Libertà, Centro Democratico, 
                Rivoluzione Civile, Unione Popolare, Italia Europa Insieme, Civica Popolare Lorenzin, 
                Liberi e Uguali, Potere al Popolo!, Partito Comunista, Lista del Popolo per la Costituzione, 
                Per una Sinistra Rivoluzionaria, Partito Valore Umano, Alleanza Verdi e Sinistra, 
                Impegno Civico Luigi Di Maio</td>
          </tr>
          <tr>
            <td>Centro</td>
            <td>Unione di Centro, Scelta Civica con Monti, Fare per Fermare il Declino, 
                Partito Repubblicano Italiano, Il Popolo della Famiglia, Azione - Italia Viva, 
                Noi di Centro Mastella</td>
          </tr>
          <tr>
            <td>Centro-Destra</td>
            <td>Forza Italia, Lega, Fiamma Tricolore, 
                Partito Pensionati, Liberali per l'Italia - PLI, Forza Nuova, 
                Grande Sud - MPA, Fratelli d'Italia, La Destra, MIR - Moderati in Rivoluzione,
                Intesa Popolare, Futuro e Libertà, CasaPound Italia, Io Amo l'Italia,
                Riformisti Italiani, Liberi per una Italia Equa, Rifondazione Missina Italiana,
                Italia agli Italiani, Blocco Nazionale per le Libertà, Ilalexit per l'Italia</td>
          </tr>
          <tr>
            <td>Movimento 5 Stelle</td>
            <td>Movimento 5 Stelle</td>
          </tr>
        </table>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">Chiudi</button>
      </div>
    </div>
  </div>
</div>

## Sezioni che si muovono

I dati elettorali raccolti riportavano il numero identificativo della sezione, ma non sempre il nome dell’edificio nel quale era collocata o il relativo indirizzo. È stato quindi necessario verificare se una sezione identificata con lo stesso numero fosse rimasta nella medesima posizione nel corso delle diverse consultazioni elettorali.

L’analisi ha evidenziato numerosi cambiamenti nella localizzazione dei seggi tra un’elezione e l’altra. La ricostruzione è stata resa complessa dalla frammentarietà della documentazione disponibile e dalla varietà dei formati utilizzati dalle amministrazioni comunali. Alcuni Comuni pubblicano queste informazioni attraverso portali Open Data, mentre altri utilizzano documenti in formato PDF, Word o pagine web non strutturate.

Sulla base delle informazioni disponibili è stata quindi ricostruita una corrispondenza temporale tra numero della sezione, indirizzo del seggio e anno elettorale.

## Coordinate

Gli indirizzi raccolti sono stati sottoposti a una fase di pulizia e standardizzazione. Abbreviazioni e varianti, come “V.le” e “Viale” oppure “A. Manzoni” e “Alessandro Manzoni”, sono state ricondotte a una forma comune. A ogni indirizzo sono stati inoltre aggiunti il CAP e il nome della città.

Questa operazione è stata necessaria per procedere con il **geocoding**, ossia la conversione degli indirizzi in coordinate geografiche.

Le coordinate sono state ottenute tramite l’API di [Nominatim](https://nominatim.org/), rispettando il limite di una richiesta al secondo previsto dal servizio. Gli indirizzi non riconosciuti automaticamente sono stati verificati e geolocalizzati manualmente attraverso Google Maps.

Le coordinate ottenute, insieme all’anno di validità della sezione e al relativo indirizzo, sono state quindi utilizzate per costruire un nuovo dataset geografico.

<div class="asc-winners-chart">

  <vegachart schema-url="{{ site.baseurl }}/assets/charts/e_firenze_pins_map.json" style="width: 100%; height: 100%"></vegachart>

</div>

## Aree subcomunali

<div class="asc-winners-chart">

  <vegachart schema-url="{{ site.baseurl }}/assets/charts/chart_asc_zones.json" style="width: 100%; height: 100%"></vegachart>

</div>

L’Istat mette a disposizione i [dati](https://www.istat.it/notizia/basi-territoriali-e-variabili-censuarie/) geografici relativi alle partizioni del territorio italiano e, in particolare, alle Aree subcomunali di secondo livello, indicate come ASC2, delle principali città.

Le ASC2 rappresentano una suddivisione del territorio comunale intermedia tra i CAP e le sezioni di censimento. La loro relativa stabilità temporale e il livello di dettaglio territoriale le rendono adatte agli obiettivi del nostro studio.

Una volta acquisiti gli *shapefile* delle aree, le coordinate delle sezioni elettorali sono state associate alle rispettive ASC2 mediante un’operazione di **spatial join** realizzata con la libreria **GeoPandas**. In questo modo è stato possibile aggregare i risultati elettorali delle singole sezioni al livello dei quartieri.

<div class="asc-winners-chart">

<vegachart schema-url="{{ site.baseurl }}/assets/charts/chart_asc_winners.json" style="width: 100%; height: 100%"></vegachart>

</div>

<div class="asc-winners-chart">

  <vegachart schema-url="{{ site.baseurl }}/assets/charts/e_mean_share_chart.json" style="width: 100%; height: 100%"></vegachart>
  <vegachart schema-url="{{ site.baseurl }}/assets/charts/e_delta_share_chart.json" style="width: 100%; height: 100%"></vegachart>

</div>

## Indici

### Indice di Gini

Per descrivere il grado di concentrazione o frammentazione dei risultati elettorali è stato utilizzato l’indice di Gini, una misura comunemente impiegata per quantificare la disuguaglianza di una distribuzione.

Sono state costruite due versioni dell’indice:

* una misura calcolata a livello di singolo quartiere, sulla distribuzione dei voti tra le diverse correnti politiche;
* una misura calcolata a livello cittadino, considerando congiuntamente i quartieri presenti in una determinata elezione.

Entrambe le versioni derivano dalla seguente formulazione generale:

$$
G = \frac{\sum_{i=1}^{n}\sum_{j=1}^{n} |x_i - x_j|}{2n\sum_{i=1}^{n} x_i}
$$

Poiché il numero di correnti politiche varia tra gli anni, anche in seguito alla comparsa del Movimento 5 Stelle nel 2018, è stata utilizzata una versione normalizzata dell’indice, così da rendere maggiormente confrontabili i risultati delle diverse elezioni:

$$
G^*_{a,t}
=
\frac{
\sum_{i=1}^{n_t}\sum_{j=1}^{n_t} |x_{i,a,t} - x_{j,a,t}|
}{
2n_t \sum_{i=1}^{n_t} x_{i,a,t}
}
\cdot
\frac{n_t}{n_t - 1}
$$

dove:

* $a$ indica la zona ASC2;
* $t$ indica l’anno elettorale;
* $x_i$ rappresenta il numero di voti ricevuti dalla corrente politica $i$;
* $n_t$ rappresenta il numero di correnti politiche attive nell’anno $t$.

| Gini | Interpretazione                                                |
| ---: | -------------------------------------------------------------- |
|    0 | I voti sono perfettamente bilanciati tra le correnti politiche |
|    1 | Una sola corrente politica raccoglie tutti i voti              |

Valori bassi dell’indice indicano quindi una distribuzione più equilibrata dei voti, mentre valori elevati segnalano una maggiore concentrazione verso una o poche correnti politiche.

<div class="asc-winners-chart">

  <vegachart schema-url="{{ site.baseurl }}/assets/charts/e_gini_chart.json" style="width: 100%; height: 100%"></vegachart>

</div>

### I di Moran globale

L’indice di Gini consente di descrivere se i risultati elettorali di un quartiere siano politicamente concentrati o frammentati, ma non fornisce informazioni sulla distribuzione spaziale di tali valori.

Per valutare se quartieri con caratteristiche simili tendano a essere geograficamente vicini, è stato calcolato l’indice I di Moran globale. Si tratta di una misura di autocorrelazione spaziale che assume valori positivi quando aree con valori simili risultano spazialmente raggruppate. Al contrario, valori negativi indicano che aree vicine tendono a presentare valori differenti.

La formula utilizzata è:

$$
I_{c,t}
=
\frac{n_{c,t}}{S_{0,c,t}}
\frac{
\sum_{i=1}^{n_{c,t}} \sum_{j=1}^{n_{c,t}}
w_{ij,c,t}
(x_{i,c,t} - \bar{x}_{c,t})
(x_{j,c,t} - \bar{x}_{c,t})
}{
\sum_{i=1}^{n_{c,t}}
(x_{i,c,t} - \bar{x}_{c,t})^2
}
$$

dove:

* $c$ indica la città;
* $t$ indica l’anno;
* $x_{i,c,t}$ rappresenta il valore della variabile nell’area $i$, nella città $c$ e nell’anno $t$, ad esempio `pol_gini` o `winner_share`;
* $\bar{x}_{c,t}$ rappresenta la media della variabile nella città $c$ e nell’anno $t$;
* $w_{ij,c,t}$ rappresenta il peso spaziale tra le aree $i$ e $j$ nella città $c$ e nell’anno $t$;
* $S_{0,c,t} = \sum_i \sum_j w_{ij,c,t}$ rappresenta la somma complessiva dei pesi spaziali;
* $n_{c,t}$ rappresenta il numero di aree considerate nella città $c$ e nell’anno $t$.

|             Moran’s I | Interpretazione                                                     |
| --------------------: | ------------------------------------------------------------------- |
|              Positivo | Le aree con valori simili tendono a essere spazialmente raggruppate |
|           Intorno a 0 | Non emerge un chiaro pattern di autocorrelazione spaziale           |
|              Negativo | Le aree confinanti tendono a presentare valori differenti           |
| p-value significativo | Il pattern osservato è difficilmente attribuibile al caso           |

![](assets/images/elezioni_emptyASC.webp)

I pesi spaziali $w_{ij,c,t}$ definiscono quali aree debbano essere considerate vicine. Riprendendo l’analogia con gli scacchi, la contiguità di tipo **Rook** considera vicine le aree che condividono un lato, come i movimenti della torre, mentre la contiguità di tipo **Queen** considera anche quelle che condividono soltanto un vertice, analogamente ai movimenti della regina.

Questi metodi sono implementati nella libreria **libpysal**, che mette a disposizione strumenti per la costruzione delle matrici dei pesi e per l’analisi spaziale.

Dopo aver sperimentato entrambi i criteri di contiguità, è emersa la presenza di aree isolate. In alcuni quartieri, infatti, non erano presenti sezioni elettorali e non risultavano quindi disponibili osservazioni. Per evitare che queste discontinuità producessero unità prive di vicini, è stato adottato il metodo **K-Nearest Neighbors**, o KNN, che associa a ogni area un numero prefissato di unità geograficamente più vicine, anche quando non direttamente confinanti.

<div class="asc-winners-chart">

  <vegachart schema-url="{{ site.baseurl }}/assets/charts/e_global_moran_chart.json" style="width: 100%; height: 100%"></vegachart>

</div>

### I di Moran locale

Mentre l’indice di Moran globale consente di verificare l’esistenza complessiva di autocorrelazione spaziale all’interno di una città, l’indice di Moran locale permette di individuare le specifiche aree nelle quali si formano cluster o anomalie spaziali.

Per un’area $a$, appartenente alla città $c$ e osservata nell’anno $t$, l’indice è definito come:

$$
I_{a,c,t}
=
\frac{x_{a,c,t} - \bar{x}_{c,t}}{m_{2,c,t}}
\sum_{b=1}^{n_{c,t}}
w_{ab,c,t}
\left(x_{b,c,t} - \bar{x}_{c,t}\right)
$$

con:

$$
m_{2,c,t}
=
\frac{1}{n_{c,t}}
\sum_{k=1}^{n_{c,t}}
\left(x_{k,c,t} - \bar{x}_{c,t}\right)^2
$$

dove:

* $a$ indica l’area ASC2 analizzata;
* $b$ indicizza le aree vicine;
* $k$ indicizza tutte le aree appartenenti alla stessa combinazione città-anno ed è utilizzato per calcolarne la varianza;
* $c$ indica la città;
* $t$ indica l’anno;
* $x_{a,c,t}$ rappresenta il valore della variabile nell’area $a$, nella città $c$ e nell’anno $t$, ad esempio `pol_gini`;
* $\bar{x}_{c,t}$ rappresenta il valore medio della variabile nella città $c$ e nell’anno $t$;
* $w_{ab,c,t}$ rappresenta il peso spaziale tra l’area $a$ e l’area vicina $b`;
* $n_{c,t}$ rappresenta il numero di aree presenti nella città $c$ e nell’anno $t$;
* $m_{2,c,t}$ rappresenta il termine di normalizzazione basato sulla varianza della distribuzione città-anno.

Dopo il calcolo dell’indice di Moran locale, ogni quartiere è stato classificato mediante i **Local Indicators of Spatial Association**, o cluster LISA.

<div class="asc-winners-chart">

  <vegachart schema-url="{{ site.baseurl }}/assets/charts/chart_local_moran_maps.json" style="width: 100%; height: 100%"></vegachart>

</div>

La classificazione considera congiuntamente:

* se il valore osservato nel quartiere è superiore o inferiore alla media della relativa combinazione città-anno;
* se i valori osservati nelle aree vicine sono, a loro volta, superiori o inferiori alla stessa media.

Nel caso della variabile `pol_gini`, i cluster possono essere interpretati come segue:

| Cluster LISA    | Interpretazione                                                                                  |
| --------------- | ------------------------------------------------------------------------------------------------ |
| High-High       | Alta concentrazione politica circondata da aree caratterizzate da alta concentrazione politica   |
| Low-Low         | Bassa concentrazione politica circondata da aree caratterizzate da bassa concentrazione politica |
| High-Low        | Alta concentrazione politica circondata da aree caratterizzate da bassa concentrazione politica  |
| Low-High        | Bassa concentrazione politica circondata da aree caratterizzate da alta concentrazione politica  |
| Not significant | Il cluster locale non risulta statisticamente significativo                                      |

Le configurazioni High-High e Low-Low rappresentano cluster spaziali di valori simili, mentre High-Low e Low-High identificano potenziali anomalie spaziali, ossia quartieri che presentano valori differenti rispetto alle aree circostanti.

### Indice di Polarizzazione

La differenza tra l’80° e il 20° percentile è stata utilizzata come misura sintetica della distanza tra la parte alta e la parte bassa della distribuzione.

$$
P_{80/20} = Q_{0.80} - Q_{0.20}
$$

dove $Q_{0.80}$ e $Q_{0.20}$ rappresentano rispettivamente l’80° e il 20° percentile della distribuzione osservata.

Un valore elevato indica una maggiore distanza tra le due parti della distribuzione e può essere interpretato come un segnale di maggiore polarizzazione. Un valore contenuto indica invece una distribuzione più omogenea rispetto alla variabile analizzata.

<div class="asc-winners-chart">

  <vegachart schema-url="{{ site.baseurl }}/assets/charts/e_global_80_20_chart.json" style="width: 100%; height: 100%"></vegachart>

</div>