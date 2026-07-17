---
layout: default
title: "Elezioni"
show_sidetoc: true
header_type: hero
header_img: assets/images/roma_banner.webp
header_title: "Elezioni"
#subtitle: "Una Pagina di Prova del gruppo 1"
---


<div class="full-width-wrapper">
    <img src="{{ site.baseurl }}/assets/images/header_alt2.svg" alt="sbd-pattern" class="full-width-image">
</div>

L'idea di utilizzare i dati elettorali per analizzare i processi di segregazione all'interno delle città italiane ci 
è nata leggendo il lavoro di [Gabriele Pinto](https://www.tandfonline.com/doi/full/10.1080/2474736X.2023.2185158){:target="_blank"} pubblicato nel 2023 con il titolo _"Sezioni Elettorali Italiane (SEI): a new database of Italian electoral results geocoded at precinct level"_ che raccoglie i dati delle elezioni degli ultimi anni a livello di singole sezioni.
{: .lead }

L'aspetto che ci ha colpito di questa enorme raccolta di dati è stata la possibilità di poter guardare da vicino cosa avviene nei singoli quartieri e da lì è sorta la voglia di capire meglio come funzionano le nostre città. 
{: .lead }



---

![](https://placehold.co/800x200/png)
# Dati Elettorali
Sul portale Eligendo del Ministero dell’Interno sono riportati i dati relativi ai voti di tutte le elezioni svoltesi in Italia dall’Assemblea Costituente ai giorni nostri. Il limite di tali dati per il nostro lavoro è che i risultati qui sono aggregati per Comune. 
## Scraping dati mancanti
Una volta scelto di concentrarci sui voti alla Camera, per preservare omogeneità dei dati tra città diverse, abbiamo fatto scraping dei dati che ci mancavano direttamente dal sito delle amministrazioni comunali.
## Partiti che vanno, partiti che vengono 
Appena abbiamo cominciato a mettere insieme dati di città diverse ci siamo accorti che i nomi dei partiti erano di volta in volta riportati in maniera diversa. Il primo step è stato quindi di armonizzare introducendo un codice che identificasse in maniera univoca ogni partito. 

Il codice identificativo dei partiti è stato mantenuto da un anno all’altro per quei partiti che nel corso del tempo hanno modificato il nome ma mantenuto la struttura (e qui Wikipedia è stata d'aiuto per ricostruire la storia di scissioni e unioni, e fissare l’area di appartenenza politica). Nei casi in cui un partito fosse nato dall'unione di più di un altro partito, invece, abbiamo creato un nuovo codice.

A questo punto abbiamo raggruppato i partiti per corrente politica. Come nel paper di riferimento, alcune entità politiche spiccatamente apartitiche non sono state assegnate a nessuna corrente e anche per la loro scarsa rilevanza in termini di voti non hanno fatto parte dell’analisi.

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

## Sezioni che si muovono
I dati raccolti fino a questo punto riportano il numero della sezione, ma non il nome dell’istituto scolastico in cui è ubicata né tantomeno l’indirizzo. E da qui la domanda: “Ma la sezione X del 2013 è la stessa del 2022?” A quanto pare la risposta non è così banale, perché’ abbiamo riscontrato diversi cambi di indirizzi delle sezioni nel corso degli anni. Qui c’è da dire che la documentazione trovata è frammentata e riportata nei formati più disparati. Alcuni comuni hanno adottano il sistema OpenData da qualche anno, altri affidano a file Pdf o Doc le loro informazioni. Sulla base di quanto trovato abbiamo ricostruito una mappa dei cambi di *location* dei seggi. Eravamo ormai vicini a fissarne la posizione nello spazio, quasi.
## Coordinate
Anche in questo caso c’è stato bisogno di armonizzare gli indirizzi e far sì che tutti i “V.le” diventassero “Viale” e “A. Manzoni”, “Alessandro Manzoni”. A tutti gli indirizzi è stato aggiunto il Cap e il nome della città. Questo è stato importante per lo step successivo, il **geocoding**.

Qui non è stato necessario fare scraping, perché il sito [Nominatim](https://nominatim.org/) mette a disposizione un Api con la sola limitazione di una chiamata al secondo. Le appena acquisite coordinate Gps, insieme all’anno in cui quella sezione era presente e quell’indirizzo hanno popolato un nuovo dataset. Manualmente sono stati aggiunti tramite Google Maps quegli indirizzi che Nominatim non era riuscita a identificare, era arrivato il momento di testare i risultati.

## Aree Sub Comunali
![Roma ASC2](assets/images/asc2_roma.webp)
L’[Istat]( https://www.istat.it/notizia/basi-territoriali-e-variabili-censuarie/) fornisce i dati geografici delle partizioni del territorio italiano e in particolare le Aree sub comunali (Asc2) delle principali citta’. Poste a meta’ tra i Cap e le aree di censimento, le Asc2 forniscono una suddivisione fissata nel tempo del territorio comunale in quartieri a un livello di dettaglio idoneo al nostro studio.

Una volta scaricati gli *shapefile* delle aree, tramite **geopandas** abbiamo associato le coordinate delle sezioni elettorali alle rispettive aree nel cui perimetro sono localizzate.

