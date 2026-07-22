---
layout: default
title: "Prova"
show_sidetoc: true
header_type: hero
header_img: assets/images/news_header_AI.png
header_title: "Dalle cronache locali alla geografia delle differenze urbane"
subtitle: "Come attenzione mediatica e sentiment raccontano aree subcomunali diverse dentro la stessa città"
---

<!-- <div class="full-width-wrapper">
    <img src="{{ site.baseurl }}/assets/images/header_alt2.svg" alt="sbd-pattern" class="full-width-image">
</div> -->

<style>
/* Applica un velo scuro solo all'header con questa immagine */
[style*="news_header_AI.png"] {
  background-color: rgba(0, 0, 0, 0.55) !important;
  background-blend-mode: multiply;
}

.chulapa-subtitle {
  color: #ffffff !important;
  opacity: 1 !important;
  visibility: visible !important;
  font-size: 1.25rem;
  font-weight: 400;
  text-shadow: 0 2px 6px rgba(0, 0, 0, 0.9);
}
</style>

Per capire come una città fosse stata raccontata nel corso del tempo, volevamo raccogliere molti anni di notiziari regionali.
{: .lead }
<br>
Prima degli algoritmi, delle mappe e dei modelli di linguaggio, il progetto è iniziato con un **incontro sul campo**: una visita alla sede di un notiziario regionale per esplorare le possibilità di accesso a **dieci anni** di edizioni locali.
{: .lead }
<br>
> "Per richiedere i materiali è necessario consultarli prima, uno per uno, e indicare quali sono quelli di interesse." 

Una procedura del tutto comprensibile per chi entra in un archivio alla ricerca di un contenuto preciso. Ma noi volevamo analizzare dieci anni di informazione senza conoscere preventivamente ciò che avremmo trovato.
{: .lead}
<br>

La soluzione è arrivata quando abbiamo scoperto, come fonte alternativa, le edizioni locali del network [City News](https://www.citynews.it){:target="_blank"}.
{: .lead }
<br>

## La redazione di City News
Nel suo archivio convivono le ultime notizie, la cronaca, gli eventi, le zone, le segnalazioni e molto altro. È un flusso continuo nel quale la città compare attraverso fatti eccezionali e gesti ordinari, incidenti e concerti, cantieri e mostre, problemi di quartiere e occasioni di partecipazione. Citynews definisce la propria forza attraverso il legame con il territorio e la capacità delle sue testate di prossimità (come [FirenzeToday](https://www.firenzetoday.it/notizie/firenze/){:target="_blank"}) di entrare nella vita delle comunità locali.
{: .lead}
<br>

## La pipeline
Da qui nasce l'idea di costruire un sistema per raccogliere le notizie, riconoscere i riferimenti geografici Asc2 e analizzare la tonalità del linguaggio per rispondere a diverse domande. 
{: .lead}
<br>
Le diverse parti di una città ricevono la stessa attenzione giornalistica? Quali aree sono nominate più spesso? Il linguaggio è prevalentemente positivo, negativo o neutro?
{: .lead}
<br>
Analizzare la distribuzione territoriale delle notizie permette di osservare non soltanto ciò che accade, ma anche quali parti della città diventano visibili nel discorso pubblico e attraverso quale sentimento narrativo.
{: .lead}
<br>









<!-- commento sui risultati.
Le notizie non costituiscono una fotografia completa della città. Raccontano una realtà selezionata attraverso criteri editoriali, routine professionali, disponibilità delle fonti ed eventi considerati notiziabili. -->














































<!-- # Dati Elettorali

L'idea di utilizzare i dati elettorali per analizzare i processi di segregazione all'interno delle città italiane ci 
è nata leggendo il lavoro di [Gabriele Pinto](https://www.tandfonline.com/doi/full/10.1080/2474736X.2023.2185158){:target="_blank"} pubblicato nel 2023 con il titolo _"Sezioni Elettorali Italiane (SEI): a new database of Italian electoral results geocoded at precinct level"_ che raccoglie i dati delle elezioni degli ultimi anni a livello di singole sezioni.
L'aspetto che ci ha colpito di questa enorme raccolta di dati è stata la possibilità di poter guardare da vicino cosa avviene nei singoli quartieri e da lì è sorta la voglia di capire meglio come funzionano le nostre città. 
{: .lead }


![](https://placehold.co/800x200/png)

Puoi usare titoli, paragrafi ed elenchi per strutturare il contenuto in modo efficace. Questo può essere fatto usando la sintassi Markdown, che consente una facile formattazione del testo. Ad esempio, puoi usare testo **grassetto** o *corsivo* per enfatizzare punti chiave, e puoi creare elenchi per organizzare le informazioni in modo chiaro.

Questo è un esempio di come formattare il testo in modo visivamente accattivante e facile da leggere.
In questo paragrafo usiamo una classe `.lead` per evidenziare i punti principali del progetto.
{: .lead}

<button type="button" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal">
  Esplora Correnti
</button>
<div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog modal-xl modal-dialog-centered">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Correnti politiche</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"><span aria-hidden="true">×</span></button>
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
 -->




<!-- <p class="green"> 
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
 -->
