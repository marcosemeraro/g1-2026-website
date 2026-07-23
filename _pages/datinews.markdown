---
layout: default
title: "Notizie"
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
<!-- <section class="news-pipeline-step" id="geografia">

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
  Esplora dettagli tecnici
  </button>
    <div class="modal fade" id="asc2Modal" tabindex="-1" aria-labelledby="asc2ModalLabel" aria-hidden="true">
    <div class="modal-dialog modal-xl modal-dialog-centered">
        <div class="modal-content">
        <div class="modal-header">
            <h5 class="modal-title" id="asc2ModalLabel">Riconoscimento delle aree ASC2</h5>
            <button type="button" class="btn-close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">×</span></button>
        </div>
        <div class="modal-body">
            <h4>Dalle parole delle notizie alle aree subcomunali</h4>
            <p>Per collegare ogni notizia alla geografia della città abbiamo cercato, all’interno della categoria, del titolo e del trafiletto, le denominazioni associate alle Aree Sub-Comunali di secondo livello, indicate come ASC2.</p>
            <p>La ricerca non è stata limitata al nome ufficiale dell’area. Per ogni ASC2 è stato utilizzato un insieme di varianti, quartieri e toponimi riconducibili alla stessa zona urbana.</p>
            <hr>
            <h4>1. Normalizzazione del testo</h4>
            <p>Prima del confronto, sia il testo delle notizie sia le denominazioni territoriali sono stati trasformati in una forma comune.</p>
            <div class="table-responsive">
                <table class="table table-bordered">
                    <thead>
                        <tr>
                            <th>Operazione</th>
                            <th>Esempio</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td>Conversione in minuscolo</td>
                            <td><code>Città Studi</code> → <code>città studi</code></td>
                        </tr>
                        <tr>
                            <td>Rimozione degli accenti</td>
                            <td><code>città studi</code> → <code>citta studi</code></td>
                        </tr>
                        <tr>
                            <td>Sostituzione della punteggiatura con spazi</td>
                            <td><code>città-studi</code> → <code>citta studi</code></td>
                        </tr>
                        <tr>
                            <td>Riduzione degli spazi multipli</td>
                            <td><code>porta&nbsp;&nbsp;&nbsp;nuova</code> → <code>porta nuova</code></td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <hr>
            <h4>2. Costruzione del dizionario geografico</h4>
            <p>Per ciascuna ASC2 abbiamo costruito una lista di parole chiave a partire dalla colonna contenente le varianti territoriali. Le denominazioni separate dal simbolo <code>|</code> sono state divise, normalizzate e raccolte in un insieme privo di duplicati.</p>
            <div class="alert alert-light">
                <strong>Esempio semplificato</strong><br>
                Area ASC2: Città Studi<br>
                Varianti: <code>città studi | piola | lambrate</code><br>
                Parole ricercate: <code>citta studi</code>, <code>piola</code>, <code>lambrate</code>
            </div>
            <p>Le denominazioni sono state ordinate dalla più lunga alla più corta. In questo modo una forma più specifica, come <code>porta nuova</code>, viene controllata prima di termini più brevi eventualmente contenuti al suo interno.</p>
            <hr>
            <h4>3. Ricerca delle denominazioni</h4>
            <p>Ogni parola chiave è stata cercata nei tre campi testuali normalizzati. La corrispondenza è stata effettuata considerando la denominazione come parola o espressione completa.</p>
            <div class="alert alert-light text-center my-4">
                <code>pattern = \b + denominazione + \b</code>
            </div>
            <p>I confini di parola <code>\b</code> riducono le corrispondenze accidentali. Una denominazione viene quindi riconosciuta quando compare come unità autonoma e non semplicemente come sequenza di lettere contenuta in un’altra parola.</p>
            <div class="table-responsive">
                <table class="table table-bordered">
                    <thead>
                        <tr>
                            <th>Testo della notizia</th>
                            <th>Denominazione</th>
                            <th>Esito</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td>“Nuovi lavori nel quartiere Niguarda”</td>
                            <td><code>niguarda</code></td>
                            <td>Corrispondenza</td>
                        </tr>
                        <tr>
                            <td>“Evento organizzato a Porta Nuova”</td>
                            <td><code>porta nuova</code></td>
                            <td>Corrispondenza</td>
                        </tr>
                        <tr>
                            <td>Parola più lunga che contiene casualmente la denominazione</td>
                            <td>Termine breve</td>
                            <td>Nessuna corrispondenza parziale</td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <hr>
            <h4>4. Informazioni registrate</h4>
            <p>Per ogni coppia composta da una notizia e un’area ASC2 sono state prodotte tre informazioni.</p>
            <div class="table-responsive">
                <table class="table table-bordered">
                    <thead>
                        <tr>
                            <th>Campo</th>
                            <th>Significato</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td>Indicatore ASC2</td>
                            <td>Vale 1 quando almeno una denominazione dell’area è presente nella notizia e 0 in caso contrario.</td>
                        </tr>
                        <tr>
                            <td><code>ASC2_codice_match</code></td>
                            <td>Contiene le denominazioni che hanno prodotto la corrispondenza.</td>
                        </tr>
                        <tr>
                            <td><code>ASC2_codice_dove</code></td>
                            <td>Indica se la corrispondenza è stata trovata nella categoria, nel titolo, nel trafiletto o in più campi.</td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <div class="alert alert-light text-center my-4">
                \[
                I_{iq} =
                \begin{cases}
                1 & \text{se la notizia } i \text{ contiene una denominazione dell'area } q \\
                0 & \text{altrimenti}
                \end{cases}
                \]
            </div>
            <p>Il sistema conserva tutte le denominazioni riconosciute. Se, per esempio, <code>porta nuova</code> compare contemporaneamente nella categoria, nel titolo e nel trafiletto, i tre campi vengono registrati senza creare tre associazioni territoriali distinte.</p>
            <hr>
            <h4>5. Perché abbiamo escluso “centro storico”</h4>
            <p>La denominazione <em>centro storico</em> è stata rimossa dal dizionario di ricerca perché troppo generica. Nelle descrizioni territoriali può comparire in relazione a più aree diverse e non permette, da sola, di stabilire quale specifica ASC2 sia citata.</p>
            <div class="alert alert-light">
                <strong>Esempio:</strong><br>
                una descrizione come <code>Centro storico - Duomo, San Babila, Montenapoleone</code> conserva come riferimenti informativi <code>duomo</code>, <code>san babila</code> e <code>montenapoleone</code>, mentre la forma generica <code>centro storico</code> non viene utilizzata per attribuire la notizia.
            </div>
            <hr>
            <h4>6. Notizie associate a più aree</h4>
            <p>Una stessa notizia può citare più quartieri e quindi essere associata a più ASC2. Per evitare che venga contata interamente una volta per ogni zona, nei passaggi successivi abbiamo applicato un peso frazionario.</p>
            <div class="alert alert-light text-center my-4">
                \[
                w_{iq} = \frac{I_{iq}}{k_i}
                \]
            </div>
            <p>dove \(k_i\) è il numero di ASC2 riconosciute nella notizia \(i\). Se una notizia cita due aree, ciascuna riceve un peso pari a \(1/2\); se ne cita quattro, ciascuna riceve \(1/4\).</p>
            <div class="alert alert-light">
                <strong>Esempio:</strong><br>
                una notizia cita Niguarda e Affori.<br>
                Numero di aree riconosciute: <code>k = 2</code><br>
                Peso attribuito a Niguarda: <code>1/2</code><br>
                Peso attribuito ad Affori: <code>1/2</code><br>
                Peso complessivo della notizia: <code>1</code>
            </div>
            <hr>

<h4>7. Recupero delle notizie localizzate tramite le zone OMI</h4>

<p>
    Il riconoscimento testuale delle ASC2 non ha permesso di localizzare
    tutte le notizie. In alcuni casi il titolo, la categoria o il trafiletto
    non contenevano il nome di un’area ASC2, ma presentavano un riferimento
    a una zona dell’Osservatorio del Mercato Immobiliare, indicata come
    zona OMI.
</p>

<p>
    Per recuperare anche queste notizie abbiamo utilizzato una tabella di
    corrispondenza spaziale tra zone OMI e aree ASC2. La procedura è stata
    applicata soltanto alle notizie che non possedevano già
    un’associazione ASC2 diretta.
</p>

<div class="table-responsive">
    <table class="table table-bordered">
        <thead>
            <tr>
                <th>Informazione disponibile</th>
                <th>Operazione eseguita</th>
            </tr>
        </thead>

        <tbody>
            <tr>
                <td>La notizia contiene già una ASC2</td>
                <td>
                    Viene mantenuta l’associazione ottenuta direttamente
                    dal testo.
                </td>
            </tr>

            <tr>
                <td>La notizia non contiene ASC2, ma contiene una zona OMI</td>
                <td>
                    La zona OMI viene ricondotta alle ASC2 con cui presenta
                    una sovrapposizione territoriale.
                </td>
            </tr>

            <tr>
                <td>La notizia non contiene né ASC2 né zone OMI</td>
                <td>
                    Non viene attribuita a una specifica area subcomunale.
                </td>
            </tr>
        </tbody>
    </table>
</div>

<p>
    Nel notebook vengono quindi selezionate le notizie che possiedono
    almeno un riferimento OMI ma nessun riferimento ASC2. In questo modo
    la conversione funziona come procedura di recupero e non modifica le
    associazioni territoriali già riconosciute direttamente nel testo.
</p>

<div class="alert alert-light text-center my-4">
    <code>nessuna ASC2 diretta + almeno una zona OMI → conversione OMI–ASC2</code>
</div>

<h4>8. La corrispondenza spaziale OMI–ASC2</h4>

<p>
    Le due zonizzazioni non presentano necessariamente gli stessi confini.
    Una zona OMI può coincidere quasi interamente con una ASC2 oppure
    sovrapporsi a più aree subcomunali.
</p>

<p>
    Per ogni coppia composta da una zona OMI e una ASC2 è stato quindi
    calcolato un peso di corrispondenza, basato sulla loro intersezione
    territoriale.
</p>

<div class="table-responsive">
    <table class="table table-bordered">
        <thead>
            <tr>
                <th>Campo</th>
                <th>Significato</th>
            </tr>
        </thead>

        <tbody>
            <tr>
                <td>Codice OMI</td>
                <td>Zona immobiliare riconosciuta nella notizia.</td>
            </tr>

            <tr>
                <td>Codice ASC2</td>
                <td>Area subcomunale alla quale può essere attribuita.</td>
            </tr>

            <tr>
                <td>Area di intersezione</td>
                <td>Superficie condivisa dalle due geometrie.</td>
            </tr>

            <tr>
                <td><code>peso_omi_to_asc2</code></td>
                <td>
                    Quota con cui la zona OMI viene distribuita verso
                    la specifica ASC2.
                </td>
            </tr>
        </tbody>
    </table>
</div>

<h4>9. Il peso della notizia convertita</h4>

<p>
    Il peso con cui una notizia era stata associata alla zona OMI viene
    moltiplicato per il peso geografico della corrispondenza OMI–ASC2.
</p>

<div class="alert alert-light text-center my-4">
    \[
    w_{iq}^{OMI}
    =
    w_{io}
    \times
    w_{o \rightarrow q}
    \]
</div>

<p>
    dove \(w_{io}\) è il peso della notizia \(i\) nella zona OMI \(o\),
    mentre \(w_{o \rightarrow q}\) rappresenta la quota con cui quella
    zona OMI viene attribuita all’ASC2 \(q\).
</p>

<div class="alert alert-light">
    <strong>Esempio semplificato</strong><br><br>

    Una notizia è associata a una zona OMI con peso pari a
    <code>1</code>.<br>

    La zona OMI si sovrappone per il 70% all’ASC2 A e per il 30%
    all’ASC2 B.<br><br>

    Peso attribuito all’ASC2 A:
    <code>1 × 0,70 = 0,70</code><br>

    Peso attribuito all’ASC2 B:
    <code>1 × 0,30 = 0,30</code><br><br>

    Peso complessivo della notizia:
    <code>0,70 + 0,30 = 1</code>
</div>

<p>
    Anche in questo caso la notizia non viene duplicata integralmente:
    il suo contributo viene distribuito tra le aree interessate.
</p>

<h4>10. Origine dell’associazione geografica</h4>

<p>
    Nel dataset finale abbiamo conservato anche l’origine della
    localizzazione, distinguendo le aree riconosciute direttamente nel
    testo da quelle ricostruite attraverso la conversione OMI–ASC2.
</p>
<div class="table-responsive">
    <table class="table table-bordered">
        <thead>
            <tr>
                <th>Valore</th>
                <th>Significato</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td><code>match_testuale_asc2</code></td>
                <td>
                    La denominazione dell’ASC2 o una sua variante è stata
                    trovata direttamente nella notizia.
                </td>
            </tr>
            <tr>
                <td><code>imputata_da_omi</code></td>
                <td>
                    L’ASC2 è stata ricostruita a partire da una zona OMI
                    riconosciuta nella notizia.
                </td>
            </tr>
        </tbody>
    </table>
</div>
<p>
    Questa distinzione permette di ricostruire il percorso attraverso cui
    ogni notizia è stata localizzata e di valutare separatamente le
    attribuzioni dirette e quelle ottenute mediante conversione.
</p>
        </div>
        <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">Chiudi</button>
        </div>
    </div>
    </div>
</div>
  </section> -->

<!-- step4 -->
<!-- <section class="news-pipeline-step" id="sentiment">
  <div class="news-pipeline-label">
    <span class="news-pipeline-number">04</span>
    <span class="news-pipeline-separator">·</span>
    <span>Sentiment</span>
  </div>

  <h3 class="news-pipeline-title">
    Dalle parole alla tonalità emotiva delle notizie
  </h3>

  <p class="news-pipeline-text">
    Dopo aver collegato le notizie alle aree ASC2, abbiamo osservato non
    soltanto quali parti della città ricevessero maggiore attenzione, ma
    anche attraverso quale linguaggio venissero raccontate.
  </p>

  <p class="news-pipeline-text">
    Per farlo abbiamo utilizzato ItEM, un lessico emotivo della lingua
    italiana. Anziché assegnare immediatamente a ogni notizia un’etichetta
    positiva o negativa, ItEM permette di ricostruirne un profilo articolato
    in otto dimensioni: attese, disgusto, fiducia, gioia, paura, rabbia,
    sorpresa e tristezza.
  </p>

  <p class="news-pipeline-text">
    Per ogni articolo abbiamo analizzato insieme titolo e trafiletto,
    individuato i termini riconosciuti dal lessico e calcolato gli score
    delle diverse emozioni. Da questi valori abbiamo poi ricavato
    l’emozione dominante e un indice sintetico di polarità compreso tra
    −1 e +1.
  </p>

  <p class="news-pipeline-text">
    La polarità non descrive una caratteristica oggettiva del quartiere:
    misura il modo in cui le notizie associate a quell’area vengono
    formulate. Una zona con valori più negativi non è quindi necessariamente
    una zona “peggiore”, ma una parte della città raccontata più spesso
    attraverso un linguaggio legato a paura, rabbia, tristezza o disgusto.
  </p>
  <button
  type="button"
  class="btn btn-primary"
  data-toggle="modal"
  data-target="#sentimentModal"
>
  Dentro il calcolo del sentiment
  <span class="news-pipeline-button-icon" aria-hidden="true"></span>
</button>


<div
  class="modal fade"
  id="sentimentModal"
  tabindex="-1"
  aria-labelledby="sentimentModalLabel"
  aria-hidden="true"
>
  <div class="modal-dialog modal-xl modal-dialog-centered">
    <div class="modal-content">
      <div class="modal-header">
        <h5
          class="modal-title"
          id="sentimentModalLabel"
        >
          Sentiment analysis con ItEM
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
        <h6 class="news-pipeline-modal-section-title">
          Il testo analizzato
        </h6>
        <p>
          Per ogni notizia abbiamo unito il titolo e il trafiletto,
          costruendo un unico testo breve da sottoporre all’analisi
          emotiva.
        </p>
        <div class="news-pipeline-example">
          <strong>Titolo</strong>
          &nbsp;＋&nbsp;
          <strong>trafiletto</strong>
          &nbsp;→&nbsp;
          testo della notizia
        </div>
        <h6 class="news-pipeline-modal-section-title">
          Dalle parole ai lemmi
        </h6>
        <p>
          Il testo viene convertito in minuscolo e ripulito da link,
          numeri, punteggiatura e spazi ripetuti. Attraverso spaCy,
          ogni parola viene poi ricondotta al proprio lemma e alla
          relativa categoria grammaticale.
        </p>
        <p>
          Sono conservati sostantivi, nomi propri, aggettivi, verbi e
          ausiliari. Vengono invece escluse le stopword, le sequenze non
          alfabetiche e le parole composte da due caratteri o meno.
        </p>
        <div class="news-pipeline-example">
          <strong>Frase originale</strong><br>
          I prezzi delle case aumentano e molte famiglie povere sono
          in difficoltà.
          <br><br>
          <strong>Termini utilizzati</strong><br>
          prezzo-s · casa-s · aumentare-v · famiglia-s · povero-a ·
          difficoltà-s
        </div>
        <p>
          Le lettere finali indicano la categoria grammaticale:
          <strong>s</strong> per sostantivo,
          <strong>a</strong> per aggettivo e
          <strong>v</strong> per verbo.
        </p>
        <h6 class="news-pipeline-modal-section-title">
          Le otto emozioni
        </h6>
        <p>
          ItEM associa ciascun termine a otto dimensioni emotive:
          attese, disgusto, fiducia, gioia, paura, rabbia, sorpresa
          e tristezza.
        </p>
        <div class="news-pipeline-modal-table-wrapper">
          <table class="news-pipeline-modal-table">
            <thead>
              <tr>
                <th>Componente positiva</th>
                <th>Componente negativa</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>Gioia</td>
                <td>Tristezza</td>
              </tr>
              <tr>
                <td>Fiducia</td>
                <td>Rabbia</td>
              </tr>
              <tr>
                <td>Attese</td>
                <td>Paura</td>
              </tr>
              <tr>
                <td>Sorpresa</td>
                <td>Disgusto</td>
              </tr>
            </tbody>
          </table>
        </div>
        <h6 class="news-pipeline-modal-section-title">
          Lo score di ogni emozione
        </h6>
        <p>
          Per ciascuna emozione vengono sommati gli score dei termini
          riconosciuti da ItEM. Il risultato viene normalizzato rispetto
          al numero complessivo di termini informativi della notizia.
        </p>
        <div class="news-pipeline-formula">
          \[
          E_{i,e}
          =
          \frac{
            \sum_{j \in T_i}
            \operatorname{cosine}(j,e)
          }{
            N_i
          }
          \]
        </div>
        <p>
          L’emozione con lo score più alto viene registrata come emozione
          dominante della notizia.
        </p>
        <h6 class="news-pipeline-modal-section-title">
          Dalle emozioni alla polarità
        </h6>
        <p>
          Gli score emotivi vengono successivamente aggregati in una
          componente positiva e in una componente negativa.
        </p>
        <div class="news-pipeline-formula">
          \[
          S_i^{+}
          =
          gioia_i + fiducia_i + attese_i + sorpresa_i
          \]
        </div>
        <div class="news-pipeline-formula">
          \[
          S_i^{-}
          =
          tristezza_i + rabbia_i + paura_i + disgusto_i
          \]
        </div>
        <p>
          La polarità della notizia è calcolata come:
        </p>
        <div class="news-pipeline-formula">
          \[
          P_i
          =
          \frac{
            S_i^{+} - S_i^{-}
          }{
            S_i^{+} + S_i^{-}
          }
          \]
        </div>
        <p>
          L’indice varia tra −1 e +1. I valori positivi indicano una
          prevalenza della componente emotiva positiva, mentre quelli
          negativi indicano una prevalenza di paura, rabbia, tristezza
          e disgusto.
        </p>
        <h6 class="news-pipeline-modal-section-title">
          La copertura del lessico
        </h6>
        <p>
          Per verificare quanta parte del testo abbia contribuito al
          risultato, abbiamo calcolato anche la copertura di ItEM.
        </p>
        <div class="news-pipeline-formula">
          \[
          C_i
          =
          \frac{
            \text{termini riconosciuti da ItEM}
          }{
            \text{termini analizzati}
          }
          \]
        </div>
        <div class="news-pipeline-example">
          <strong>Polarità uguale a zero con termini riconosciuti:</strong>
          equilibrio tra componente positiva e negativa.
          <br><br>
          <strong>Polarità uguale a zero senza termini riconosciuti:</strong>
          assenza di informazioni sufficienti, non neutralità.
        </div>
        <p>
          Per questo motivo, nelle analisi successive abbiamo considerato
          valida la polarità soltanto quando almeno un termine della notizia
          aveva trovato corrispondenza nel lessico.
        </p>
        <h6 class="news-pipeline-modal-section-title">
          Come interpretare il risultato
        </h6>
        <p>
          La polarità non stabilisce se una zona urbana sia positiva o
          negativa. Descrive la tonalità emotiva del linguaggio con cui
          vengono formulate le notizie associate a quell’area.
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
  
</section> -->

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