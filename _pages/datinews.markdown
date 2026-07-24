---
layout: default
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





<!-- step5 -->
<!-- <section class="news-pipeline-step" id="topic-modelling">
  <div class="news-pipeline-label">
    <span class="news-pipeline-number">05</span>
    <span class="news-pipeline-separator">·</span>
    <span>Temi</span>
  </div>
  <h3 class="news-pipeline-title">
    Dal tono delle notizie ai temi della città
  </h3>
  <p class="news-pipeline-text">
    Il sentiment ci dice con quale tonalità viene raccontata una notizia,
    ma non ci dice ancora quale argomento ne sia al centro. Per questo
    abbiamo applicato BERTopic ai testi formati dall’unione di titolo
    e trafiletto.
  </p>
  <p class="news-pipeline-text">
    Il modello ha raggruppato le notizie semanticamente simili, facendo
    emergere temi ricorrenti come incidenti, incendi, eventi culturali,
    criminalità, mobilità o trasformazioni urbane. Ogni notizia è stata
    associata a un topic, descritto attraverso le parole e le espressioni
    più rappresentative del gruppo.
  </p>
  <p class="news-pipeline-text">
    Incrociando i topic con la polarità ItEM possiamo distinguere il tema
    dal tono: uno stesso argomento può comprendere sia notizie positive
    sia notizie negative. Il topic modelling diventa quindi uno strumento
    interpretativo per comprendere quali aspetti della vita urbana
    contribuiscono alla rappresentazione mediatica delle diverse aree.
  </p>
  <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#topicModal">
    Come emergono i topic
  </button>
</section>
<div class="modal fade" id="topicModal" tabindex="-1" aria-labelledby="topicModalLabel" aria-hidden="true">
  <div class="modal-dialog modal-xl modal-dialog-centered">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="topicModalLabel">Topic modelling con BERTopic</h5>
        <button type="button" class="btn-close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">×</span></button>
      </div>
      <div class="modal-body">
        <h6 class="news-pipeline-modal-section-title">Il testo analizzato</h6>
        <p>
          BERTopic è stato applicato allo stesso testo utilizzato per
          l’analisi emotiva, costruito unendo il titolo e il trafiletto
          di ogni notizia.
        </p>
        <div class="news-pipeline-example">
          <strong>Titolo</strong>
          &nbsp;＋&nbsp;
          <strong>trafiletto</strong>
          &nbsp;→&nbsp;
          testo della notizia
        </div>
        <p>
          In questa fase non è stata applicata una pulizia linguistica
          aggressiva. Gli embeddings funzionano meglio quando conservano
          la struttura naturale delle frasi: sono stati quindi uniformati
          soprattutto gli spazi e gestiti i valori mancanti.
        </p>
        <h6 class="news-pipeline-modal-section-title">La pipeline di BERTopic</h6>
        <div class="news-pipeline-example">
          Testo
          &nbsp;→&nbsp;
          embedding semantico
          &nbsp;→&nbsp;
          riduzione UMAP
          &nbsp;→&nbsp;
          clustering HDBSCAN
          &nbsp;→&nbsp;
          parole rappresentative
          &nbsp;→&nbsp;
          topic
        </div>
        <div class="news-pipeline-modal-table-wrapper">
          <table class="news-pipeline-modal-table">
            <thead>
              <tr>
                <th>Passaggio</th>
                <th>Funzione</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>Sentence Transformer</td>
                <td>
                  Trasforma ogni notizia in un vettore numerico che ne
                  rappresenta il significato complessivo.
                </td>
              </tr>
              <tr>
                <td>UMAP</td>
                <td>
                  Riduce il numero di dimensioni degli embeddings,
                  conservando le principali relazioni di vicinanza
                  semantica tra le notizie.
                </td>
              </tr>
              <tr>
                <td>HDBSCAN</td>
                <td>
                  Raggruppa le notizie semanticamente vicine senza
                  richiedere di stabilire in anticipo il numero dei topic.
                </td>
              </tr>
              <tr>
                <td>CountVectorizer e c-TF-IDF</td>
                <td>
                  Individuano le parole e le espressioni che distinguono
                  ciascun topic dagli altri.
                </td>
              </tr>
            </tbody>
          </table>
        </div>
        <h6 class="news-pipeline-modal-section-title">Gli embeddings semantici</h6>
        <p>
          Per rappresentare le notizie è stato utilizzato il modello
          multilingue:
        </p>
        <div class="news-pipeline-example">
          <code>sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2</code>
        </div>
        <p>
          A differenza di un semplice conteggio delle parole, un embedding
          cerca di rappresentare il significato della frase. Due notizie
          possono quindi essere considerate simili anche quando utilizzano
          parole non perfettamente identiche.
        </p>
        <h6 class="news-pipeline-modal-section-title">Ridurre la complessità con UMAP</h6>
        <p>
          Gli embeddings originari contengono molte dimensioni. UMAP li
          proietta in uno spazio più piccolo, nel quale diventa più semplice
          individuare gruppi di notizie simili.
        </p>
        <div class="news-pipeline-modal-table-wrapper">
          <table class="news-pipeline-modal-table">
            <thead>
              <tr>
                <th>Parametro UMAP</th>
                <th>Valore utilizzato</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td><code>n_neighbors</code></td>
                <td>15</td>
              </tr>
              <tr>
                <td><code>n_components</code></td>
                <td>5</td>
              </tr>
              <tr>
                <td><code>min_dist</code></td>
                <td>0.0</td>
              </tr>
              <tr>
                <td><code>metric</code></td>
                <td>cosine</td>
              </tr>
              <tr>
                <td><code>random_state</code></td>
                <td>42</td>
              </tr>
            </tbody>
          </table>
        </div>
        <h6 class="news-pipeline-modal-section-title">Raggruppare le notizie con HDBSCAN</h6>
        <p>
          HDBSCAN identifica le zone più dense dello spazio semantico e
          forma i cluster senza imporre un numero prestabilito di temi.
          Nel modello è stata richiesta una dimensione minima di 80
          notizie per cluster.
        </p>
        <div class="news-pipeline-modal-table-wrapper">
          <table class="news-pipeline-modal-table">
            <thead>
              <tr>
                <th>Parametro HDBSCAN</th>
                <th>Valore utilizzato</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td><code>min_cluster_size</code></td>
                <td>80</td>
              </tr>
              <tr>
                <td><code>min_samples</code></td>
                <td>10</td>
              </tr>
              <tr>
                <td><code>metric</code></td>
                <td>euclidean</td>
              </tr>
              <tr>
                <td><code>cluster_selection_method</code></td>
                <td>eom</td>
              </tr>
            </tbody>
          </table>
        </div>
        <div class="news-pipeline-example">
          <strong>Topic −1</strong><br>
          Le notizie che non appartengono in modo sufficientemente chiaro
          a nessun cluster vengono assegnate al topic −1 e trattate come
          osservazioni anomale o non classificate.
        </div>
        <h6 class="news-pipeline-modal-section-title">Dare un significato ai cluster</h6>
        <p>
          Dopo il clustering, un topic è ancora soltanto un insieme di
          notizie simili. Per descriverlo vengono cercate le parole e le
          combinazioni di parole maggiormente caratteristiche del gruppo.
        </p>
        <p>
          Il vectorizer considera sia parole singole sia coppie di parole,
          così da conservare espressioni informative come
          “vigili del fuoco”, “incidente stradale” o “trasporto pubblico”.
        </p>
        <div class="news-pipeline-modal-table-wrapper">
          <table class="news-pipeline-modal-table">
            <thead>
              <tr>
                <th>Parametro del vectorizer</th>
                <th>Valore utilizzato</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td><code>ngram_range</code></td>
                <td>Da 1 a 2 parole</td>
              </tr>
              <tr>
                <td><code>min_df</code></td>
                <td>Presenza in almeno 5 testi</td>
              </tr>
              <tr>
                <td><code>max_df</code></td>
                <td>Presenza in non più dell’85% dei testi</td>
              </tr>
            </tbody>
          </table>
        </div>
        <p>
          Le stopword italiane e alcuni termini molto frequenti ma poco
          informativi, come “Milano”, “zona”, “via”, “oggi” o “articolo”,
          vengono esclusi dalla descrizione finale dei topic. Non vengono
          invece rimossi prima della costruzione degli embeddings.
        </p>
        <h6 class="news-pipeline-modal-section-title">Unire temi e sentiment</h6>
        <p>
          Dopo aver assegnato un topic a ogni notizia, la polarità ItEM
          viene trasformata in tre categorie:
        </p>
        <div class="news-pipeline-modal-table-wrapper">
          <table class="news-pipeline-modal-table">
            <thead>
              <tr>
                <th>Condizione</th>
                <th>Categoria</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>Polarità maggiore di zero</td>
                <td>Positive</td>
              </tr>
              <tr>
                <td>Polarità minore di zero</td>
                <td>Negative</td>
              </tr>
              <tr>
                <td>Polarità uguale a zero</td>
                <td>Neutral</td>
              </tr>
            </tbody>
          </table>
        </div>
        <p>
          Per ciascun topic viene quindi calcolata la percentuale di
          notizie positive, negative e neutrali. Questo permette di
          osservare, per esempio, se il tema degli incendi è associato
          soprattutto a un tono negativo oppure se un tema culturale
          presenta prevalentemente una tonalità positiva.
        </p>
        <div class="news-pipeline-example">
          <strong>Il tema non coincide con il sentiment.</strong><br>
          Un topic legato alla scuola, alla riqualificazione o agli eventi
          culturali può comprendere anche proteste, criticità o incidenti.
          Per questo il significato del cluster e il tono delle notizie
          devono essere analizzati separatamente.
        </div>
        <h6 class="news-pipeline-modal-section-title">Come interpretare i risultati</h6>
        <p>
          BERTopic propone automaticamente gruppi e parole chiave, ma il
          nome finale di un tema richiede una lettura qualitativa dei
          titoli più rappresentativi. I topic non sono categorie oggettive:
          costituiscono una sintesi esplorativa dei contenuti presenti
          nell’archivio.
        </p>
        <p>
          In questo progetto servono quindi a comprendere quali eventi,
          problemi e trasformazioni urbane accompagnano il sentiment
          osservato nelle diverse parti della città.
        </p>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">Chiudi</button>
      </div>
    </div>
  </div>
</div> -->

<!-- step 5 -->
<!-- <section class="news-pipeline-step" id="indicatori">

  <div class="news-pipeline-label">
    <span class="news-pipeline-number">05</span>
    <span class="news-pipeline-separator">·</span>
    <span>Indicatori</span>
  </div>

  <h3 class="news-pipeline-title">
    Dalle singole notizie ai profili territoriali
  </h3>

  <p class="news-pipeline-text">
    L’ultima fase ricompone le informazioni temporali, geografiche ed
    emotive in un unico pannello territoriale. L’unità finale di analisi
    è costituita da una zona ASC2 osservata in uno dei tre periodi
    considerati.
  </p>

  <p class="news-pipeline-text">
    Per ogni area abbiamo misurato il peso complessivo delle notizie
    associate, la continuità della copertura mediatica, la quota di
    contenuti per i quali il sentiment è risultato valido e la polarità
    media, tenendo conto dei pesi frazionari assegnati durante la
    localizzazione geografica.
  </p>

  <p class="news-pipeline-text">
    La polarità di ogni zona è stata inoltre confrontata con il tono
    medio della città nello stesso periodo. Questa normalizzazione
    permette di riconoscere le aree raccontate in modo relativamente
    più positivo o più negativo rispetto al contesto urbano generale.
  </p>

  <p class="news-pipeline-text">
    Il confronto tra periodi successivi consente infine di osservare
    se la posizione relativa di una zona è rimasta stabile oppure se
    la sua rappresentazione mediatica si è spostata nel tempo.
  </p>

  <button
    type="button"
    class="btn btn-primary"
    data-toggle="modal"
    data-target="#indicatoriModal"
  >
    Come abbiamo costruito gli indicatori
  </button>


  <div
    class="modal fade"
    id="indicatoriModal"
    tabindex="-1"
    aria-labelledby="indicatoriModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog modal-xl modal-dialog-centered">

      <div class="modal-content">

        <div class="modal-header">

          <h5
            class="modal-title"
            id="indicatoriModalLabel"
          >
            Aggregazione territoriale e costruzione degli indicatori
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

          <h4>1. I tre periodi di osservazione</h4>

          <p>
            Le notizie sono state raggruppate in tre finestre temporali,
            costruite in relazione alle elezioni politiche del 2018 e
            del 2022 e al periodo successivo.
          </p>

          <div class="table-responsive">

            <table class="table table-bordered">

              <thead>
                <tr>
                  <th>Periodo</th>
                  <th>Intervallo considerato</th>
                  <th>Riferimento</th>
                </tr>
              </thead>

              <tbody>

                <tr>
                  <td>1</td>
                  <td>Dal primo semestre 2015 al secondo semestre 2017</td>
                  <td>Elezioni del 2018</td>
                </tr>

                <tr>
                  <td>2</td>
                  <td>Dal primo semestre 2018 al primo semestre 2022</td>
                  <td>Elezioni del 2022</td>
                </tr>

                <tr>
                  <td>3</td>
                  <td>Dal secondo semestre 2022 al secondo semestre 2025</td>
                  <td>Periodo successivo al 2022</td>
                </tr>

              </tbody>

            </table>

          </div>

          <div class="alert alert-light">
            <strong>Etichette utilizzate nel dataset</strong><br>
            <code>2015-1__2017-2</code><br>
            <code>2018-1__2022-1</code><br>
            <code>2022-2__2025-2</code>
          </div>


          <hr>


          <h4>2. Una sola osservazione per notizia, zona e periodo</h4>

          <p>
            Prima dell’aggregazione abbiamo verificato che ogni
            combinazione composta da notizia, area ASC2 e periodo fosse
            rappresentata da una sola riga.
          </p>

          <div class="alert alert-light text-center my-4">
            <strong>Unità intermedia</strong><br>
            città × periodo × notizia × ASC2
          </div>

          <p>
            Quando la stessa associazione era presente più volte, i pesi
            territoriali sono stati ricomposti, mantenendo un solo valore
            di polarità e di copertura ItEM per la notizia.
          </p>

          <p>
            È stato inoltre controllato che la somma dei pesi territoriali
            di ciascuna notizia fosse pari a uno.
          </p>

          <div class="alert alert-light text-center my-4">
            \[
            \sum_q w_{iq}=1
            \]
          </div>

          <p>
            In questo modo una notizia collegata a più aree contribuisce
            all’intera città una sola volta, pur distribuendo il proprio
            peso tra le zone citate.
          </p>


          <hr>


          <h4>3. L’attenzione mediatica della zona</h4>

          <p>
            Il numero di notizie associate a una zona può essere descritto
            attraverso due misure complementari.
          </p>

          <div class="table-responsive">

            <table class="table table-bordered">

              <thead>
                <tr>
                  <th>Indicatore</th>
                  <th>Significato</th>
                </tr>
              </thead>

              <tbody>

                <tr>
                  <td><code>N_NEWS_UNICHE</code></td>
                  <td>
                    Numero di articoli distinti collegati alla zona
                    durante il periodo.
                  </td>
                </tr>

                <tr>
                  <td><code>PESO_FRAZIONARIO_TOT</code></td>
                  <td>
                    Somma dei pesi territoriali attribuiti alla zona.
                    Rappresenta il volume effettivo di attenzione mediatica,
                    tenendo conto delle notizie associate a più aree.
                  </td>
                </tr>

                <tr>
                  <td><code>N_SEMESTRI_CON_NEWS</code></td>
                  <td>
                    Numero di semestri del periodo nei quali la zona ha
                    ricevuto almeno una notizia.
                  </td>
                </tr>

              </tbody>

            </table>

          </div>

          <p>
            L’indicatore di attenzione territoriale è calcolato come:
          </p>

          <div class="alert alert-light text-center my-4">
            \[
            A_{q,p}
            =
            \sum_{i \in p} w_{iq}
            \]
          </div>

          <p>
            dove \(w_{iq}\) è il peso con cui la notizia \(i\) viene
            attribuita all’area \(q\), mentre \(p\) indica il periodo
            considerato.
          </p>

          <div class="alert alert-light">
            <strong>Esempio</strong><br><br>

            Una notizia cita soltanto Niguarda:
            peso per Niguarda <code>1</code>.<br>

            Una seconda notizia cita Niguarda e Affori:
            peso per Niguarda <code>0,5</code> e peso per Affori
            <code>0,5</code>.<br><br>

            Attenzione complessiva attribuita a Niguarda:
            <code>1 + 0,5 = 1,5</code>.
          </div>

          <p>
            Il peso complessivo è cumulativo all’interno del periodo.
            Poiché le tre finestre hanno durate differenti, deve essere
            letto insieme al numero di semestri coperti e non come un
            tasso già normalizzato per la durata.
          </p>


          <hr>


          <h4>4. Qualità e disponibilità del sentiment</h4>

          <p>
            La polarità è considerata valida soltanto quando ItEM ha
            riconosciuto almeno un termine della notizia. Una polarità
            uguale a zero derivante da un reale equilibrio tra componenti
            positive e negative viene invece conservata.
          </p>

          <div class="table-responsive">

            <table class="table table-bordered">

              <thead>
                <tr>
                  <th>Indicatore</th>
                  <th>Significato</th>
                </tr>
              </thead>

              <tbody>

                <tr>
                  <td><code>N_NEWS_SENTIMENT</code></td>
                  <td>
                    Numero di notizie della zona con una polarità valida.
                  </td>
                </tr>

                <tr>
                  <td><code>PESO_NEWS_SENTIMENT</code></td>
                  <td>
                    Somma dei pesi territoriali delle sole notizie con
                    sentiment valido.
                  </td>
                </tr>

                <tr>
                  <td><code>QUOTA_PESO_CON_SENTIMENT</code></td>
                  <td>
                    Quota dell’attenzione territoriale coperta da una
                    polarità valida.
                  </td>
                </tr>

                <tr>
                  <td><code>COVERAGE_MEDIA_PESATA</code></td>
                  <td>
                    Copertura media del lessico ItEM, pesata per il
                    contributo territoriale delle notizie.
                  </td>
                </tr>

              </tbody>

            </table>

          </div>

          <div class="alert alert-light text-center my-4">
            \[
            Q_{q,p}
            =
            \frac{
              \text{peso delle notizie con sentiment valido}
            }{
              \text{peso complessivo delle notizie}
            }
            \]
          </div>

          <p>
            Un valore vicino a uno indica che quasi tutta l’attenzione
            mediatica attribuita alla zona dispone anche di uno score
            di polarità utilizzabile.
          </p>


          <hr>


          <h4>5. Polarità media pesata della zona</h4>

          <p>
            Per ogni ASC2 e periodo abbiamo calcolato la media delle
            polarità delle notizie, utilizzando come peso il contributo
            territoriale di ciascun articolo.
          </p>

          <div class="alert alert-light text-center my-4">
            \[
            \overline{P}_{q,p}
            =
            \frac{
              \sum_{i \in V_{q,p}} w_{iq}P_i
            }{
              \sum_{i \in V_{q,p}} w_{iq}
            }
            \]
          </div>

          <p>
            \(P_i\) è la polarità ItEM della notizia, \(w_{iq}\) è il
            suo peso nell’area e \(V_{q,p}\) rappresenta l’insieme
            delle notizie con sentiment valido.
          </p>

          <p>
            La media pesata evita che una notizia collegata a molte zone
            contribuisca integralmente alla polarità di ciascuna di esse.
          </p>

          <div class="table-responsive">

            <table class="table table-bordered">

              <thead>
                <tr>
                  <th>Valore</th>
                  <th>Interpretazione</th>
                </tr>
              </thead>

              <tbody>

                <tr>
                  <td>Maggiore di 0</td>
                  <td>
                    Prevalenza della componente emotiva positiva nelle
                    notizie associate alla zona.
                  </td>
                </tr>

                <tr>
                  <td>Minore di 0</td>
                  <td>
                    Prevalenza della componente emotiva negativa.
                  </td>
                </tr>

                <tr>
                  <td>Vicino a 0</td>
                  <td>
                    Maggiore equilibrio tra le due componenti.
                  </td>
                </tr>

              </tbody>

            </table>

          </div>


          <hr>


          <h4>6. Confrontare ogni zona con la propria città</h4>

          <p>
            I livelli medi di polarità possono essere differenti tra
            città e periodi. Per questo il valore della singola area
            viene confrontato con la media cittadina calcolata nello
            stesso intervallo temporale.
          </p>

          <p>
            Nel calcolo cittadino ogni notizia viene considerata una sola
            volta, anche quando è stata associata a più zone.
          </p>

          <div class="alert alert-light text-center my-4">
            \[
            R_{q,p}
            =
            \overline{P}_{q,p}
            -
            \mu_{c,p}
            \]
          </div>

          <p>
            \(R_{q,p}\) è la polarità relativa della zona, mentre
            \(\mu_{c,p}\) è la polarità media di tutte le notizie della
            città nel periodo.
          </p>

          <div class="table-responsive">

            <table class="table table-bordered">

              <thead>
                <tr>
                  <th>Polarità relativa</th>
                  <th>Interpretazione</th>
                </tr>
              </thead>

              <tbody>

                <tr>
                  <td>Positiva</td>
                  <td>
                    La zona è raccontata con un tono più positivo rispetto
                    alla media cittadina.
                  </td>
                </tr>

                <tr>
                  <td>Negativa</td>
                  <td>
                    La zona è raccontata con un tono più negativo rispetto
                    alla media cittadina.
                  </td>
                </tr>

                <tr>
                  <td>Uguale a 0</td>
                  <td>
                    La zona è allineata alla tonalità media della città.
                  </td>
                </tr>

              </tbody>

            </table>

          </div>


          <hr>


          <h4>7. La polarità z-normalizzata</h4>

          <p>
            La semplice differenza dalla media cittadina viene
            successivamente divisa per la deviazione standard delle
            polarità della città nello stesso periodo.
          </p>

          <div class="alert alert-light text-center my-4">
            \[
            Z_{q,p}
            =
            \frac{
              \overline{P}_{q,p}-\mu_{c,p}
            }{
              \sigma_{c,p}
            }
            \]
          </div>

          <p>
            Questo indicatore esprime la distanza della zona dalla media
            cittadina in unità di deviazione standard.
          </p>

          <div class="table-responsive">

            <table class="table table-bordered">

              <thead>
                <tr>
                  <th>Valore z-normalizzato</th>
                  <th>Interpretazione</th>
                </tr>
              </thead>

              <tbody>

                <tr>
                  <td>\(Z &gt; 0\)</td>
                  <td>Tono relativamente più positivo della media.</td>
                </tr>

                <tr>
                  <td>\(Z &lt; 0\)</td>
                  <td>Tono relativamente più negativo della media.</td>
                </tr>

                <tr>
                  <td>\(Z \approx 1\)</td>
                  <td>
                    La zona si trova circa una deviazione standard sopra
                    la media cittadina.
                  </td>
                </tr>

                <tr>
                  <td>\(Z \approx -1\)</td>
                  <td>
                    La zona si trova circa una deviazione standard sotto
                    la media cittadina.
                  </td>
                </tr>

              </tbody>

            </table>

          </div>

          <div class="alert alert-light">
            La z-normalizzazione non misura quanto una zona sia
            oggettivamente positiva o negativa. Indica la sua posizione
            relativa all’interno della rappresentazione mediatica della
            propria città e del proprio periodo.
          </div>


          <hr>


          <h4>8. La variazione tra periodi</h4>

          <p>
            Dopo aver ordinato cronologicamente i tre periodi, per ogni
            ASC2 abbiamo confrontato la polarità z-normalizzata con quella
            della finestra precedente.
          </p>

          <div class="alert alert-light text-center my-4">
            \[
            \Delta Z_{q,p}
            =
            Z_{q,p}
            -
            Z_{q,p-1}
            \]
          </div>

          <div class="table-responsive">

            <table class="table table-bordered">

              <thead>
                <tr>
                  <th>Delta</th>
                  <th>Interpretazione</th>
                </tr>
              </thead>

              <tbody>

                <tr>
                  <td>Positivo</td>
                  <td>
                    La posizione relativa della zona è diventata più
                    positiva rispetto al periodo precedente.
                  </td>
                </tr>

                <tr>
                  <td>Negativo</td>
                  <td>
                    La posizione relativa della zona è diventata più
                    negativa.
                  </td>
                </tr>

                <tr>
                  <td>Vicino a 0</td>
                  <td>
                    La posizione relativa della zona è rimasta
                    sostanzialmente stabile.
                  </td>
                </tr>

              </tbody>

            </table>

          </div>

          <p>
            È stata conservata anche la variazione della polarità media
            pesata non normalizzata:
          </p>

          <div class="alert alert-light text-center my-4">
            \[
            \Delta \overline{P}_{q,p}
            =
            \overline{P}_{q,p}
            -
            \overline{P}_{q,p-1}
            \]
          </div>


          <hr>


          <h4>9. Una griglia completa di zone e periodi</h4>

          <p>
            Il dataset finale contiene tutte le combinazioni possibili
            tra aree ASC2 e periodi, comprese le zone per le quali non
            sono state raccolte notizie.
          </p>

          <div class="alert alert-light text-center my-4">
            <strong>Unità finale</strong><br>
            città × ASC2 × periodo elettorale
          </div>

          <p>
            Per le zone prive di notizie, i conteggi e il peso
            dell’attenzione sono impostati a zero. La polarità rimane
            invece mancante, perché l’assenza di notizie non equivale
            a un sentiment neutro.
          </p>

          <div class="table-responsive">

            <table class="table table-bordered">

              <thead>
                <tr>
                  <th>Indicatore di controllo</th>
                  <th>Significato</th>
                </tr>
              </thead>

              <tbody>

                <tr>
                  <td><code>HA_NEWS_ZONA</code></td>
                  <td>
                    Indica se la zona ha ricevuto almeno una quota di
                    attenzione mediatica nel periodo.
                  </td>
                </tr>

                <tr>
                  <td><code>HA_DATI_SENTIMENT</code></td>
                  <td>
                    Indica se per la zona è disponibile una polarità
                    media calcolabile.
                  </td>
                </tr>

              </tbody>

            </table>

          </div>


          <hr>


          <h4>10. Gli indicatori utilizzati nella sintesi finale</h4>

          <div class="table-responsive">

            <table class="table table-bordered">

              <thead>
                <tr>
                  <th>Indicatore</th>
                  <th>Dimensione descritta</th>
                </tr>
              </thead>

              <tbody>

                <tr>
                  <td><code>PESO_FRAZIONARIO_TOT</code></td>
                  <td>Intensità dell’attenzione mediatica.</td>
                </tr>

                <tr>
                  <td><code>POLARITY_MEDIA_PESATA</code></td>
                  <td>Tonalità media delle notizie della zona.</td>
                </tr>

                <tr>
                  <td><code>POLARITY_ZNORMALIZZATA</code></td>
                  <td>
                    Posizione della zona rispetto alla media della città.
                  </td>
                </tr>

                <tr>
                  <td><code>DELTA_POLARITY_ZNORMALIZZATA</code></td>
                  <td>
                    Cambiamento della posizione relativa rispetto al
                    periodo precedente.
                  </td>
                </tr>

                <tr>
                  <td><code>QUOTA_PESO_CON_SENTIMENT</code></td>
                  <td>
                    Quota dell’attenzione coperta dall’analisi emotiva.
                  </td>
                </tr>

                <tr>
                  <td><code>N_SEMESTRI_CON_NEWS</code></td>
                  <td>Continuità temporale della copertura.</td>
                </tr>

              </tbody>

            </table>

          </div>

          <p>
            Il risultato è un pannello nel quale ogni area subcomunale
            dispone di una misura dell’attenzione ricevuta, della tonalità
            con cui è stata raccontata e della variazione della propria
            posizione nei tre periodi.
          </p>

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

</section>
 -->

















<!-- bottone -->
  <!-- <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal">
  Esplora dettagli tecnici
  </button>
    <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog modal-xl modal-dialog-centered">
        <div class="modal-content">
        <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Scraping</h5>
            <button type="button" class="btn-close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">×</span></button>
        </div>
        <div class="modal-body">
            let's write something here
        </div>
        <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">Chiudi</button>
        </div>
    </div>
    </div>
</div> -->






















<!-- commento sui risultati.
Le notizie non costituiscono una fotografia completa della città. Raccontano una realtà selezionata attraverso criteri editoriali, routine professionali, disponibilità delle fonti ed eventi considerati notiziabili. -->

<!-- Domande a cui rispondono i grafici -->
<!-- Le diverse parti di una città ricevono la stessa attenzione giornalistica? Quali aree sono nominate più spesso? Il linguaggio è prevalentemente positivo, negativo o neutro?<br><br>

Analizzare la distribuzione territoriale delle notizie permette di osservare non soltanto ciò che accade, ma anche quali parti della città diventano visibili nel discorso pubblico e attraverso quale sentimento narrativo.<br><br>

Per trasformare questo archivio in una geografia dell’attenzione mediatica abbiamo quindi costruito una pipeline composta da passaggi successivi.  -->