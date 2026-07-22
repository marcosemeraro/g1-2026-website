---
layout: default
title: "Prova"
show_sidetoc: true
header_type: hero
header_img: assets/images/roma_banner.webp
header_title: "La Geografia della Ricchezza nelle Città"
subtitle: "Un'analisi micro-territoriale delle disuguaglianze attraverso le dichiarazioni dei redditi IRPEF"
---

<div class="full-width-wrapper">
    <img src="{{ site.baseurl }}/assets/images/header_alt2.svg" alt="sbd-pattern" class="full-width-image">
</div>

I dati fiscali sulle dichiarazioni dei redditi rappresentano la traccia più concreta per misurare le 
disuguaglianze e la capacità economica della popolazione all'interno dello spazio urbano. 
Superando la visione omogenea offerta dai dati medi comunali, abbiamo voluto guardare da vicino la struttura 
economica delle singole zone cittadine. L'obiettivo è capire quanto la polarizzazione della ricchezza 
e il peso fiscale incidano sulle dinamiche sociali e sulla trasformazione dei quartieri 
nelle sei grandi città italiane.
{: .lead }

---

![](https://placehold.co/800x200/png)
# Dati Fiscali MEF e Mappatura della Disuguaglianza

I dati sulle dichiarazioni dei redditi IRPEF, messi a disposizione dal Ministero dell'Economia e delle Finanze 
(MEF) per le annualità **2011, 2015 e dal 2019 al 2024**, rappresentano la fonte primaria per analizzare 
la distribuzione della ricchezza, la capacità economica e le dinamiche della disuguaglianza nelle sei città 
oggetto di studio (*Milano, Torino, Bologna, Firenze, Roma e Napoli*).

Poiché le rilevazioni ufficiali del MEF sono state diffuse su scala sub-comunale, per questo lavoro 
è stato effettuato un processo di elaborazione, normalizzazione e attribuzione delle informazioni fiscali 
alla griglia territoriale delle **Aree Sub-Comunali (ASC2)**. L'analisi condotta su questa scala permette 
di superare la rigidità del valore medio cittadino e di far emergere le forti polarizzazioni e le disparità 
economiche che caratterizzano i diversi quartieri all'interno dello stesso contesto urbano.
{: .lead }

---

### Documentazione e Metodologia degli Indici Statistico-Economici

Le tabelle rilasciate dal MEF organizzano i dati dei contribuenti e dei redditi complessivi suddividendoli in 
classi di frequenza e ammontare per scaglioni di reddito. A partire da questa struttura informatica, sono stati 
definiti e calcolati **4 indicatori analitici** per ciascuna Area Sub-Comunale (ASC2):

* **1. Indice di Gini (Gini Coefficient):**
  È il coefficiente standard utilizzato a livello internazionale per misurare il grado di disuguaglianza e la concentrazione della ricchezza o del reddito all'interno di una popolazione. Il suo valore varia strettamente tra **0** (*perfetta uguaglianza*, in cui tutti i contribuenti percepiscono esattamente lo stesso reddito) e **1** (*perfetta disuguaglianza*, dove un singolo contribuente possiede la totalità del reddito complessivo).
  
  Nel contesto delle Aree Sub-Comunali analizzate, un valore più alto dell'Indice di Gini segnala che in quel determinato quartiere il reddito è fortemente concentrato nelle mani di poche persone, mentre un valore più basso indica un tessuto sociale ed economicamente più omogeneo.
  
  Poiché i dati del MEF non sono disponibili per i singoli individui ma sono forniti aggregati "in classi di reddito" (es. 0–10.000 €, 10.000–15.000 €, ecc.), l'Indice di Gini è stato calcolato attraverso un'approssimazione geometrica basata sulle percentuali cumulate:
  1. Si calcola il reddito medio teorico di ogni fascia dividendo l'Ammontare complessivo per la Frequenza dei contribuenti della fascia stessa.
  2. Si ordinano le fasce dalla più povera alla più ricca in base a questo valore.
  3. Si calcola la frazione cumulata della popolazione dei contribuenti (vettore $P$).
  4. Si calcola la frazione cumulata del reddito totale generato (vettore $Q$).
  5. Geometricamente, l'insieme dei punti definiti dai vettori $P$ e $Q$ identifica la distribuzione cumulata. L'Indice di Gini viene estratto calcolando l'area sottesa a questa spezzata tramite la **regola dei trapezi** e sottraendola dall'area del triangolo di ideale uguaglianza.

* **2. Quota di Reddito Alta (Top Income Share - Fascia > 120.000 Euro):**
  È un indicatore di concentrazione della ricchezza che si focalizza unicamente sulla coda più ricca della distribuzione (*top tail*). Rappresenta una percentuale (%) pura che indica quale quota dell'intera massa di redditi di una determinata area finisce nelle tasche dei contribuenti più abbienti, ovvero coloro che dichiarano oltre 120.000 euro all'anno. 
  
  Ad esempio, se in un'Area Sub-Comunale questo valore è pari a 25.0%, significa che il gruppo di residenti che guadagna oltre 120.000 euro da solo assorbe il 25% di tutto il reddito complessivamente generato in quel quartiere. La formula mette in rapporto l'ammontare della fascia superiore a 120.000 euro con la somma totale di tutti gli ammontari del quartiere, moltiplicando il risultato per 100.

* **3. Aliquota Media Percentuale (Effettivo Peso Fiscale dell'Area):**
  È un indicatore microeconomico che misura la pressione fiscale reale media esercitata dal prelievo IRPEF sui redditi all'interno di una specifica area geografica. Esprime una percentuale (%) che indica quanti euro vengono effettivamente trattenuti dal fisco per ogni 100 euro lordi guadagnati cumulativamente dai residenti di quell'Area Sub-Comunale. 
  
  Poiché il sistema fiscale italiano è basato sul principio della progressività, questo valore funge da indicatore territoriale della ricchezza: le aree sub-comunali con un'aliquota media significativamente più alta riflettono una maggiore presenza di contribuenti posizionati su scaglioni IRPEF elevati. A differenza delle aliquote teoriche di legge, questo indicatore calcola il peso dell'**imposta NETTA**, incorporando l'impatto reale sul territorio di deduzioni e detrazioni fiscali (come spese mediche, carichi di famiglia o bonus edilizi). Il calcolo mette direttamente in rapporto l'imposta netta globale con la massa dei redditi lordi, moltiplicato per 100.

* **4. Reddito Medio:**
  È l'indicatore sintetico della capacità reddituale media per contribuente all'interno di ciascuna Area Sub-Comunale (ASC2). Offre un parametro immediato per valutare il livello di ricchezza media dichiarata ed è calcolato dividendo la somma di tutti gli ammontari dei redditi prodotti nell'area per il numero totale dei contribuenti residenti.

---

### Pipeline di Aggregazione e Costruzione del Dataset Unico Comparativo

Per analizzare l'evoluzione temporale delle dinamiche economiche, i dati storici sono stati allineati e strutturati in **3 macro-periodi di riferimento** per ciascuna area sub-comunale (quartiere):

* **2011–2015** (Primo periodo)
* **2019–2021** (Secondo periodo)
* **2022–2024** (Terzo periodo)

Per ciascuno dei 4 indici economici calcolati (Indice di Gini, Top Income Share, Aliquota Media Percentuale, 
Reddito Medio) sono state estratte e inserite nel dataset finale **6 metriche di sintesi e di trend**:

* **Struttura Base e Distance from Election:** Creazione dello scheletro dei periodi e inserimento della distanza temporale dalle elezioni di riferimento (3 per il periodo 2011–2015; 1 per i trienni 2019–2021 e 2022–2024).
* **Latest Level:** Valore puntuale dell'indice registrato nell'ultimo anno utile del periodo considerato (2015 per il primo periodo, 2021 per il secondo, 2024 per il terzo).
* **Mean Economic Condition:** Condizione economica media dell'area all'interno del periodo, calcolata tramite la media aritmetica semplice sui singoli anni compresi nel macro-periodo.
* **Percentage Change:** Variazione percentuale dell'indice rilevata da inizio a fine periodo (ad esempio, dal 2011 al 2015, dal 2019 al 2021 e dal 2022 al 2024).
* **Slope Lineare:** Pendenza (coefficiente angolare) della retta di regressione calcolata sui punti del periodo, utilizzata per identificare la direzione e l'intensità del trend evolutivo interno.
* **Volatilità Normalizzata:** Misura dell'instabilità annuale dell'indicatore all'interno del periodo, calcolata tramite Deviazione Standard e successivamente riscalata con il metodo Min-Max all'interno di un range fisso compreso tra [0, 1].

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

