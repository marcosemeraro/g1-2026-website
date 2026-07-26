---
layout: editorial
title: "Notizie"
show_sidetoc: true
header_type: hero
header_img: assets/images/news_header_AI.png
header_title: "Dalle cronache locali alla geografia del tono mediatico"
#subtitle: "Come attenzione mediatica e sentiment raccontano aree subcomunali diverse dentro la stessa città"
---

<!-- <div class="full-width-wrapper">
    <img src="{{ site.baseurl }}/assets/images/header_alt2.svg" alt="sbd-pattern" class="full-width-image">
</div> -->

<style>
/* Applica un velo scuro all'immagine di header*/
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
Prima degli algoritmi, delle mappe e dei modelli di linguaggio, il progetto è iniziato con un incontro sul campo: una visita alla sede di un notiziario regionale per esplorare le possibilità di accesso a dieci anni di edizioni locali.<br><br>

> "Per richiedere i materiali è necessario consultarli prima, uno per uno, e indicare quali sono quelli di interesse." 

<br>
Una procedura del tutto comprensibile per chi entra in un archivio alla ricerca di un contenuto preciso. Ma noi volevamo analizzare dieci anni di informazione senza conoscere preventivamente ciò che avremmo trovato.
<br><br>

La soluzione è arrivata quando abbiamo scoperto, come fonte alternativa, le edizioni locali del network [City News](https://www.citynews.it){:target="_blank"}. <br><br>
Nel suo archivio convivono le ultime notizie, la cronaca, gli eventi, le zone, le segnalazioni e molto altro. È un flusso continuo nel quale la città compare attraverso fatti eccezionali e gesti ordinari, incidenti e concerti, cantieri e mostre, problemi di quartiere e occasioni di partecipazione. <br><br>
Citynews definisce la propria forza attraverso il legame con il territorio e la capacità delle sue testate di prossimità (come [FirenzeToday](https://www.firenzetoday.it/notizie/firenze/){:target="_blank"}) di entrare nella vita delle comunità locali.

<!-- ## Pipeline
Da qui nasce l'idea di costruire un sistema per raccogliere le notizie, riconoscere i riferimenti geografici Asc2 e analizzare la tonalità del linguaggio per rispondere a diverse domande.  -->

<!-- step1 -->
<section class="news-pipeline-step" id="scraping">

  <div class="news-pipeline-label">
    <span class="news-pipeline-number">01</span>
    <span class="news-pipeline-separator">·</span>
    <span>Scraping</span>
  </div>

  <h3 class="news-pipeline-title">
    Dal flusso delle notizie ad un archivio strutturato
  </h3>

  <p class="news-pipeline-text">
    Il primo passaggio è stato raccogliere in modo sistematico le notizie
    pubblicate dalle edizioni locali di CityNews. Per ciascuna città abbiamo
    sviluppato uno scraper in Python capace di percorrere le pagine dedicate
    alle notizie di zona, procedendo dagli articoli più recenti verso quelli
    più lontani nel tempo.
  </p>

  <p class="news-pipeline-text">
    Per ogni notizia sono stati registrati la categoria (che può contenere 
    informazioni territoriali, tipo di evento o altro), il titolo,
    il link, il trafiletto, la data mostrata sul sito e la pagina di provenienza.
    Queste informazioni sono state organizzate in un archivio tabellare, che
    costituisce il punto di partenza per le successive operazioni.
  </p>
 </section>




<!-- step2 -->
<section class="news-pipeline-step" id="tempo">

  <div class="news-pipeline-label">
    <span class="news-pipeline-number">02</span>
    <span class="news-pipeline-separator">·</span>
    <span>Tempo</span>
  </div>

  <h3 class="news-pipeline-title">
    Rimettere ogni notizia al suo posto nel tempo
  </h3>

  <p class="news-pipeline-text">
    La data raccolta dal sito non corrispondeva sempre a un giorno scritto
    nel formato tradizionale. Molte notizie erano accompagnate da espressioni
    relative come “ieri”, “tre settimane fa”, “aprile scorso” oppure
    semplicemente “nel 2018”.
  </p>

  <p class="news-pipeline-text">
    Abbiamo quindi ricostruito ogni riferimento temporale a partire dal giorno
    in cui è stato eseguito lo scraping. La conversione ha prodotto una data
    standardizzata, conservando però anche il livello di precisione
    dell’informazione originale: minuto, giorno, mese oppure anno.
  </p>

<button
  type="button"
  class="btn btn-primary"
  data-toggle="modal"
  data-target="#dateModal"
>
  Dettagli tecnici
</button>

<div
  class="modal fade"
  id="dateModal"
  tabindex="-1"
  aria-labelledby="dateModalLabel"
  aria-hidden="true"
>
  <div class="modal-dialog modal-xl modal-dialog-centered">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="dateModalLabel">
          Come abbiamo ricostruito le date
        </h5>
        <button
          type="button"
          class="btn-close"
          data-dismiss="modal"
          aria-label="Close"
        >
          <span aria-hidden="true">×</span>
        </button>
      </div>
      <div class="modal-body">
         <h4>Riconoscere le diverse forme linguistiche</h4>
        <p>
          Abbiamo costruito un parser basato su espressioni regolari.
          Ogni regola riconosce una particolare struttura della data e ne
          estrae gli elementi necessari, come la quantità, l’unità temporale,
          il mese, il giorno della settimana o l’orario.
        </p>
        <div class="alert alert-light">
          <strong>Esempio:</strong><br><br>
          “3 settimane fa”<br>
          quantità: 3<br>
          unità temporale: settimane<br>
          operazione: sottrarre tre settimane dalla data dello scraping
        </div>
      <p>
        Abbiamo quindi utilizzato come riferimento la data dello scraping, fissata ad esempio all’8 luglio 2026, e abbiamo sottratto da essa l’intervallo temporale riconosciuto.
        </p>
        <hr>
        <h4>Alcuni esempi</h4>
        <div class="table-responsive">
          <table class="table table-bordered">
            <thead>
              <tr>
                <th>Espressione originale</th>
                <th>Operazione</th>
                <th>Risultato</th>
                <th>Precisione</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>“L’altro ieri, 9:31”</td>
                <td>Sottrazione di due giorni</td>
                <td>6 luglio 2026, 9:31</td>
                <td>Minuto</td>
              </tr>
              <tr>
                <td>“3 settimane fa”</td>
                <td>Sottrazione di tre settimane</td>
                <td>17 giugno 2026</td>
                <td>Giorno</td>
              </tr>
              <tr>
                <td>“5 mesi fa”</td>
                <td>Sottrazione di cinque mesi</td>
                <td>Febbraio 2026</td>
                <td>Mese</td>
              </tr>
              <tr>
                <td>“2 anni fa, a ottobre”</td>
                <td>Sottrazione di due anni e selezione del mese</td>
                <td>Ottobre 2024</td>
                <td>Mese</td>
              </tr>
              <tr>
                <td>“Aprile scorso”</td>
                <td>Ricerca dell’ultima occorrenza passata di aprile</td>
                <td>Aprile 2026</td>
                <td>Mese</td>
              </tr>
              <tr>
                <td>“Nel 2018”</td>
                <td>Estrazione diretta dell’anno</td>
                <td>2018</td>
                <td>Anno</td>
              </tr>
            </tbody>
          </table>
        </div>
      <div class="modal-footer">
        <button
          type="button"
          class="btn btn-secondary"
          data-dismiss="modal"
        >
          Chiudi
        </button>
      </div>
    </div>
  </div>
</div>
</div>
  </section>


<!-- step3 -->
<section class="news-pipeline-step" id="geografia">

  <div class="news-pipeline-label">
    <span class="news-pipeline-number">03</span>
    <span class="news-pipeline-separator">·</span>
    <span>Geografia</span>
  </div>

  <h3 class="news-pipeline-title">
    Dal linguaggio delle notizie alle aree subcomunali
  </h3>
    <p class="news-pipeline-text">I luoghi citati nelle cronache locali non seguono sempre 
    una classificazione territoriale uniforme. Una stessa zona può essere indicata attraverso 
    una via, una forma abbreviata, un quartiere, un parco oppure un toponimo 
    conosciuto dagli abitanti.</p>
    <p class="news-pipeline-text">Per collegare questi riferimenti a una geografia comune, 
    abbiamo cercato nelle categorie, nei titoli e nei trafiletti le denominazioni e le varianti 
    linguistiche delle aree subcomunali ASC2.</p>
    <p class="news-pipeline-text">Ogni corrispondenza è stata associata alla relativa area territoriale, 
    conservando anche la parola trovata e il campo della notizia nel quale compariva. Una stessa notizia 
    può quindi essere collegata a più zone quando descrive eventi che coinvolgono parti diverse della città.</p>
  
  
<button type="button" class="btn btn-primary" data-toggle="modal" data-target="#asc2Modal">
  Dettagli tecnici
  </button>
    <div class="modal fade" id="asc2Modal" tabindex="-1" aria-labelledby="asc2ModalLabel" aria-hidden="true">
    <div class="modal-dialog modal-xl modal-dialog-centered">
        <div class="modal-content">
        <div class="modal-header">
            <h5 class="modal-title" id="asc2ModalLabel">Riconoscimento delle aree ASC2</h5>
            <button type="button" class="btn-close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">×</span></button>
        </div>
        
<div class="modal-body">
  <h4>1. Normalizzazione del testo</h4>

  <p>
    Prima della ricerca, testi e denominazioni territoriali sono stati
    convertiti in minuscolo, privati degli accenti e della punteggiatura
    e uniformati negli spazi.
  </p>

  <div class="alert alert-light">
    <strong>Esempio</strong><br><br>
    <code>Notizie da NOVOLI-LIPPI</code><br>
    diventa<br>
    <code>notizie da novoli lippi</code>
  </div>

  <hr>

  <h4>2. Costruzione del dizionario geografico</h4>

  <p>
    Per ogni ASC2 abbiamo costruito una lista di parole chiave a partire
    dalle varianti presenti nel file territoriale. Le denominazioni
    separate dal simbolo <code>|</code> sono state divise, normalizzate
    e raccolte senza duplicati.
  </p>

  <div class="alert alert-light">
    <strong>Esempio: ASC2 Novoli - Lippi</strong><br><br>

    Varianti originali:<br>
    <code>novoli|lippi</code><br><br>

    Parole ricercate:<br>
    <code>novoli</code>, <code>lippi</code>
  </div>

  <p>
    Alcune parole, come <code>novoli</code>, compaiono nelle varianti
    di più ASC2: Novoli - Lippi, Novoli - Fiat, Novoli - Baracca Est
    e Novoli - Baracca Ovest.
  </p>

  <hr>

  <h4>3. Ricerca delle denominazioni</h4>

  <p>
    Le parole chiave sono state cercate nei campi normalizzati della
    categoria, del titolo e del trafiletto. Il confronto utilizza i
    confini di parola, così da riconoscere soltanto termini o espressioni
    complete.
  </p>

  <div class="alert alert-light text-center my-4">
    <strong>Pattern utilizzato</strong><br><br>
    <code>\b + denominazione + \b</code>
  </div>

  <div class="alert alert-light">
    <strong>Esempio</strong><br><br>

    Testo:<br>
    <code>Nuovi lavori nel quartiere di Novoli</code><br><br>

    Keyword riconosciuta:<br>
    <code>novoli</code>
  </div>

  <p>
    Per ogni corrispondenza vengono conservate anche la parola trovata
    e la parte della notizia in cui compare.
  </p>

  <hr>

  <h4>4. Il caso dell’ASC2 Centro</h4>

  <p>
    A Firenze l’ASC2 49 si chiama <strong>Centro</strong>. Abbiamo escluso
    questa espressione perché troppo generica e presente nella categoria
    di molte notizie riferite anche a luoghi differenti.
  </p>

  <p>
    Per recuperare le notizie del centro abbiamo sfruttato le descrizioni
    OMI, che contengono denominazioni più precise.
  </p>

  <div class="alert alert-light">
    <strong>Esempio di zona OMI</strong><br><br>

    <code>
      CENTRO STORICO (SIGNORIA - DUOMO - PITTI - SAN NICCOLO)
    </code><br><br>

    Parole utilizzate:<br>
    <code>signoria</code>,
    <code>duomo</code>,
    <code>pitti</code>,
    <code>san niccolo</code>
  </div>

  <hr>

  <h4>5. Notizie associate a più aree</h4>

  <p>
    Una notizia può citare più zone oppure contenere una denominazione
    condivisa da più ASC2. Contarla interamente in ciascuna zona
    aumenterebbe artificialmente il numero totale delle notizie.
  </p>

  <div class="alert alert-light">
    <strong>Esempio</strong><br><br>

    La parola <code>novoli</code> è associata a quattro ASC2.<br><br>

    La notizia viene quindi distribuita tra le quattro aree:<br>
    <code>0,25</code> per ciascuna ASC2.<br><br>

    Il peso complessivo rimane pari a <code>1</code>.
  </div>

  <hr>

  <h4>6. Conversione dalle zone OMI alle ASC2</h4>

  <p>
    La conversione è stata applicata soltanto alle notizie che avevano
    un riferimento OMI ma nessun match ASC2 diretto. Il peso della
    notizia è stato distribuito tra le ASC2 in proporzione alla superficie
    condivisa con la zona OMI.
  </p>

  <div class="alert alert-light">
    <strong>Esempio semplificato</strong><br><br>

    Una notizia ha peso OMI pari a <code>1</code>.<br>
    La zona OMI ricade per il 70% nell’ASC2 A e per il 30% nell’ASC2 B.
    <br><br>

    Peso ASC2 A: <code>0,70</code><br>
    Peso ASC2 B: <code>0,30</code><br>
    Peso totale: <code>1</code>
  </div>

  <p>
    Nel dataset possiamo inoltre ricostruire se la localizzazione della
    notizia deriva da un match ASC2 diretto oppure dalla conversione di
    una zona OMI.
  </p>
</div>
        <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">Chiudi</button>
        </div>
    </div>
    </div>
</div>
  </section> 


<!-- step4 -->
<section class="news-pipeline-step" id="sentiment">
  <div class="news-pipeline-label">
    <span class="news-pipeline-number">04</span>
    <span class="news-pipeline-separator">·</span>
    <span>Sentiment</span>
  </div>
  <h3 class="news-pipeline-title">Dalle emozioni alla polarità territoriale</h3>
  <p class="news-pipeline-text">Con <a href="https://github.com/Unipisa/ItEM" target="_blank" rel="noopener noreferrer">
    ItEM </a> (lessico emotivo ad alta copertura per la lingua italiana in cui a ciascun termine target viene assegnato un punteggio di associazione con le emozioni di base definite nella tassonomia di Plutchik (1994)) abbiamo analizzato titolo e trafiletto di ogni notizia, ottenendo uno score per otto emozioni: attese, disgusto, fiducia, gioia, paura, rabbia, sorpresa e tristezza.</p>
  <p class="news-pipeline-text">Da questi valori abbiamo ricavato uno score di polarità compreso tra −1 e +1 per ogni notizia. Le polarità delle singole notizie sono state poi aggregate per area ASC2 e periodo elettorale, tenendo conto del peso territoriale di ciascun articolo.</p>
  <p class="news-pipeline-text">Il risultato permette di distinguere la quantità di attenzione ricevuta da una zona dal tono, relativamente positivo o negativo, con cui viene raccontata rispetto alla propria città.</p>
  <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#sentimentModal">Dentro il calcolo della polarità</button>
  <div class="modal fade" id="sentimentModal" tabindex="-1" aria-labelledby="sentimentModalLabel" aria-hidden="true">
    <div class="modal-dialog modal-xl modal-dialog-centered">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="sentimentModalLabel">Dalle emozioni agli indicatori territoriali</h5>
          <button type="button" class="btn-close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">×</span></button>
        </div>
        <div class="modal-body">
          <h6 class="news-pipeline-modal-section-title">Dalle emozioni alla polarità</h6>
          <p>Abbiamo riunito <strong>gioia, fiducia, attese e sorpresa</strong> nella componente positiva e <strong>tristezza, rabbia, paura e disgusto</strong> nella componente negativa.</p>
          <div class="news-pipeline-formula">
            \[
            P_i=\frac{S_i^{+}-S_i^{-}}{S_i^{+}+S_i^{-}}
            \]
          </div>
          <p>La polarità varia tra −1 e +1: valori positivi indicano una prevalenza delle emozioni positive, valori negativi una prevalenza di quelle negative. Le notizie senza termini riconosciuti da ItEM vengono escluse dal calcolo, perché l’assenza di uno score non equivale a neutralità.</p>
          <h6 class="news-pipeline-modal-section-title">Polarità media della zona</h6>
          <p>Per ogni ASC2 e periodo elettorale abbiamo calcolato la media delle polarità, pesando ogni notizia per la quota attribuita alla zona. Un articolo condiviso tra più aree influenza quindi ciascuna di esse soltanto per la propria frazione.</p>
          <h6 class="news-pipeline-modal-section-title">Polarità z-normalizzata</h6>
          <p>Per confrontare territori con livelli medi differenti, la polarità della zona viene rapportata alla media e alla variabilità della propria città nello stesso periodo.</p>
          <div class="news-pipeline-formula">
            \[
            Z_{q,p}=\frac{\overline{P}_{q,p}-\mu_{c,p}}{\sigma_{c,p}}
            \]
          </div>
          <div class="news-pipeline-example"><code>Z &gt; 0</code>: tono più positivo della media cittadina.<br><code>Z &lt; 0</code>: tono più negativo della media cittadina.<br><code>Z ≈ 0</code>: tono vicino alla media della città.</div>
          <p>Lo z-score non descrive una qualità oggettiva del quartiere, ma la sua posizione relativa nella narrazione mediatica della città.</p>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-dismiss="modal">Chiudi</button>
        </div>
      </div>
    </div>
  </div>
</section>



<!--  -->

<!-- commento sui risultati.
Le notizie non costituiscono una fotografia completa della città. Raccontano una realtà selezionata attraverso criteri editoriali, routine professionali, disponibilità delle fonti ed eventi considerati notiziabili. -->

