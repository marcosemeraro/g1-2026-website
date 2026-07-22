---
layout: default
title: "Prova"
show_sidetoc: true
header_type: hero
header_img: assets/images/roma_banner.webp
header_title: "Analisi della Vulnerabilità Sociale e Materiale"
subtitle: "Calcolo e distribuzione dell'ISVM sui dati dei Censimenti ISTAT"
---

<div class="full-width-wrapper">
    <img src="{{ site.baseurl }}/assets/images/header_alt2.svg" alt="sbd-pattern" class="full-width-image">
</div>

L'idea di analizzare la **distribuzione dello svantaggio sociale e materiale** all'interno delle città italiane nasce 
dall'utilizzo delle variabili dei **Censimenti generali della popolazione** forniti dall'**ISTAT**. Attraverso il calcolo 
dell'**Indice di Svantaggio Sociale e Materiale (ISVM)**, si ha la possibilità di osservare da vicino le **disuguaglianze 
territoriali** a livello micro-locale. L'aspetto fondamentale di questo approccio è la possibilità di combinare **indicatori 
demografici, lavorativi e abitativi** per mappare e comprendere le **fragilità dei singoli quartieri**.
{: .lead }


---

![](https://placehold.co/800x200/png)
# Dati Censuari ISTAT

L'ISTAT rilascia periodicamente i dati dei **Censimenti Generali della Popolazione e delle Abitazioni**, che rappresentano 
la fonte fondamentale per analizzare le dinamiche socio-economiche del territorio. Per questo studio, sono state estratte 
le variabili di base a livello micro-territoriale, superando la sola dimensione comunale per accedere a una scala d'analisi 
molto più dettagliata. 
{: .lead}
---


### Selezione delle Variabili e Costruzione degli Indicatori
A partire dalle variabili censuarie grezze, sono stati definiti e calcolati **7 indicatori specifici** per intercettare le 
diverse dimensioni del disagio socio-economico, oltre al valore sintetico finale:

* **Carico di Minori:** incidenza della popolazione giovanile a carico.
* **Famiglie Numerose:** presenza di nuclei familiari con più di sei componenti.
* **Bassa Istruzione:** concentrazione di titoli di studio primari o assenza di titoli.
* **Disagio Assistenziale:** livello di fragilità e necessità di supporto sociale INCIDENZA POP OVER 75.
* **Disagio Abitativo:** condizioni di affollamento
* **NEET:** incidenza dei giovani non occupati e non inseriti in percorsi di istruzione o formazione.
* **Disagio Economico:** indicatori di vulnerabilità lavorativa e reddituale ??.

---

### Normalizzazione e Aggregazione (Metodologia AMPI)
Per rendere gli indicatori confrontabili e sintetizzarli in un unico indice, è stata applicata una procedura rigorosa in due fasi:

1. **Standardizzazione ISTAT (Scala 70-130):** Gli indicatori sono stati normalizzati secondo la metodologia standard ISTAT con media fissa pari a 100 e deviazione standard pari a 15, allineando i valori in un intervallo di riferimento tra 70 e 130.
2. **Aggregazione tramite AMPI Positivo (Adjusted Mazziotta-Pareto Index):**  
   L'**AMPI** è un metodo di sintesi non lineare basato su una funzione di aggregazione compensativa corretta da un fattore di penalizzazione per lo squilibrio. Nello specifico, per il calcolo dell'**ISVM Finale** è stata utilizzata la variante **AMPI Positiva**:
   * **Superamento della Media Aritmetica:** La semplice media tende a "mascherare" le criticità, poiché valori positivi in alcuni ambiti rischiano di compensare e nascondere un forte disagio presente in un altro.
   * **Enfasi sui Picchi di Vulnerabilità:** L'AMPI evita questo schiacciamento. Se anche **un solo indicatore registra un valore di disagio elevato**, l'indice rileva la disomogeneità e applica una penalità che spinge verso l'alto il valore finale dell'ISVM.
   * **Risultato:** L'indice sintetico valorizza e mette in evidenza i quartieri che presentano anche una sola forma acuta di vulnerabilità, garantendo che le situazioni di emergenza locale non vengano annullate dal calcolo statistico.

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

<p class="green"> 
    Puoi completare l'intera pagina usando solo la sintassi markdown, ma puoi anche usare tag HTML per aggiungere elementi più complessi. In questo esempio, usiamo un paragrafo con una classe "green" per evidenziare il testo in colore verde.
</p>

<p class="mt-3"><strong>La struttura del lavoro è organizzata attorno ad aree tematiche chiave:</strong></p>

<p>
Il framework del progetto è progettato per offrire un <strong>approccio interdisciplinare</strong>, combinando metodologie tecniche come <strong>analisi dei dati, machine learning e intelligenza artificiale</strong> con considerazioni più ampie. Il contenuto è strutturato per alternare <strong>guida teorica</strong> ed <strong>esplorazione pratica</strong>, incoraggiando gli utenti ad applicare concetti attraverso lo sviluppo pratico di un'applicazione o storia basata sui dati.
</p>

<hr>

<p>
Durante lo sviluppo, lavorerai con <strong>dataset del mondo reale</strong>, esplorerai <strong>narrazioni basate sui dati</strong> e rifletterai sulle <strong>implicazioni etiche e sociali</strong> delle tue scelte. La struttura è progettata per essere modulare, consentendo flessibilità pur mantenendo un flusso narrativo coerente.
</p>

<div class="full-width-wrapper">
<div class="where">
    <div class="container">
        <div class="row pt-2 ">
            <div class="col-md-6 col-sm-12">
               <img src="{{ site.baseurl }}/assets/images/Dr_Jekyll.jpg" alt="Visuale del Progetto">
            </div>
        <div class="col-md-6 col-sm-12">
            <h3>CONTESTO</h3>
            <p class="lead">Questo template è progettato per essere ospitato su GitHub Pages e supporta lo sviluppo di siti web di progetto che combinano approfondimenti tecnici con storytelling visivo. Puoi adattarlo per mostrare dataset, codice, visualizzazioni e risultati finali.
            </p>
            <h3>Per Iniziare</h3>
            <p>Usa la struttura fornita per iniziare a modificare i tuoi contenuti usando Markdown o HTML.</p>
            <p><strong>Fai riferimento alla documentazione per dettagli su come personalizzare layout, navigazione ed elementi visivi.</strong></p>
        </div>
        </div>
    </div>
</div>
</div>

<div class="aim mt-5">
    <div class="container">
        <div class="row pt-2 ">
        <div class="col-md-6 col-sm-12">
            <h3>OBIETTIVO DEL TEMPLATE</h3>
                <p class="lead">
                L'obiettivo principale di questo template è fornire agli studenti e ai ricercatori un punto di partenza per
                costruire siti web di progetto professionali che comunichino efficacemente il loro lavoro. Utilizzando strumenti moderni
                come Jekyll e GitHub Pages, questo template rende facile creare, personalizzare e pubblicare il tuo
                progetto online.
                </p>
        </div>
        <div class="col-md-6 col-sm-12">
            <img src="{{ site.baseurl }}/assets/images/Jekyll_Logo.png" alt="Logo Jekyll">
        </div>
        </div>
    </div>
</div>

<div class="container mt-5">
    <div class="row">
        <div class="col-md-12">
            <h2>Caratteristiche Principali</h2>
            <ul class="lead">
                <li>Design responsive pronto all'uso</li>
                <li>Facile da personalizzare con Markdown e HTML</li>
                <li>Supporto integrato per grafici e visualizzazioni</li>
                <li>Hosting gratuito su GitHub Pages</li>
                <li>Temi e layout multipli tra cui scegliere</li>
            </ul>
        </div>
    </div>
</div>

