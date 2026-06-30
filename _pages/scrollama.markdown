---
layout: default
title: "Tutorial: Visualizzazione attraverso lo Scrollytelling in Jekyll"
vega: true
scrollama: true
sidetoc: false
---

# Visualizzazione e Scrollytelling in Jekyll

La visualizzazione attraverso lo scrollytelling non si limita alla semplice esposizione di numeri e grafici, ma richiede una narrazione efficace per contestualizzare l'informazione visiva. Questo approccio permette di creare un'esperienza interattiva in cui il testo scorre in sincrono con visualizzazioni dinamiche (Vega-Lite), guidando l'utente attraverso i punti chiave della narrazione.
{: .lead}

Questo tutorial illustra come implementare e configurare il componente `scrollytelling.html` all'interno della pipeline Jekyll, partendo dalla gestione dei dataset fino al controllo dei layout di pagina.

---

## 1. Architettura dei Dati e Configurazione

Per mantenere il codice pulito e favorire la separazione tra logica narrativa e layout, il nostro componente si appoggia ai Data File di Jekyll (`_data`). 

### Il file YAML dei contenuti
Il primo passo è creare un file YAML (es. `_data/scrollama_steps.yml`) che fungerà da dizionario per i nostri step narrativi. Ogni blocco deve contenere il testo da mostrare e il riferimento al file JSON Vega-Lite associato.

<pre>
# _data/scrollama_steps.yml
- text: "Introduzione alla visualizzazione: esploriamo la prima macro-suddivisione delle performance."
  asset: "assets/json/step_1.json"
- text: "La linea tratteggiata rappresenta il benchmark medio globale."
  asset: "assets/json/step_2.json"
</pre>

### L'inclusione Liquid
Una volta strutturati i dati, il modulo viene iniettato nella pagina tramite un `include` Liquid. Questo approccio parametrico ci permette di gestire un altissimo livello di personalizzazione per ogni singola istanza sulla pagina.

<pre>
{% raw %}{% include scrollytelling.html 
  data_file="scrollama_steps" 
  chart_height="340px" 
  desktop_chart_height="27vh" 
  sticky_top="30vh" 
  desktop_steps_padding_top="6vh" 
  step_gap="100vh" 
  last_step_gap="3vh" 
  only_active_step_visible="false" 
%}{% endraw %}
</pre>

### Dizionario dei Parametri

| Parametro | Tipo | Descrizione |
| :--- | :--- | :--- |
| **`data_file`** | `String` | Il nome del file in `_data` (senza estensione `.yml`) da cui ciclare i contenuti. |
| **`layout_mode`** | `String` | Definisce la struttura visiva del componente. Valori possibili: `default` (o omesso) per il layout standard a due colonne; `fullwidth` per espandere il grafico a tutta larghezza e sovrapporre i box di testo. |
| **`chart_height`** | `CSS Value` | Altezza del grafico fisso per dispositivi mobile (es. `340px`). |
| **`desktop_chart_height`** | `CSS Value` | Altezza del grafico su viewport ampi (es. `27vh`). |
| **`sticky_top`** | `CSS Value` | Distanza dal margine superiore dello schermo a cui il grafico si "blocca" (es. `30vh`). |
| **`step_gap`** | `CSS Value` | Distanza di scorrimento (gap) tra un box testuale e il successivo (es. `100vh`). |
| **`last_step_gap`** | `CSS Value` | Margine inferiore dell'ultimo blocco di testo prima di riprendere lo scroll normale. |
| **`only_active_step_visible`**| `Boolean` | Se `true`, nasconde gli step testuali non attivi, anziché sfumarne l'opacità. |

> **Nota di produzione:** Assicurati sempre che l'inclusione avvenga in una pagina che dichiara `vega: true` e `scrollama: true` nel Front Matter, altrimenti gli script CDN non verranno caricati nell'header.
{: .notice--warning}

---

## 2. Implementazione di Base (Layout a 2 Colonne)

Il seguente blocco mostra il componente in azione configurato per un layout a due colonne. In questa modalità, lo spazio visivo viene diviso: il grafico rimane ancorato in una colonna dedicata, mentre i box di testo scorrono in modo indipendente nella colonna adiacente, garantendo che i dati siano costantemente visibili e affiancati alla narrazione durante la lettura.

{% include scrollytelling.html data_file="scrollama_steps" chart_height="400px" desktop_chart_height="35vh" sticky_top="30vh" desktop_steps_padding_top="6vh" step_gap="100vh" last_step_gap="3vh" only_active_step_visible="false" %}

Come si evince dall'interazione, lo scroller traccia la posizione del blocco testuale e, superata la soglia di attivazione della viewport, innesca il re-rendering del JSON target nella colonna del grafico senza ricaricare la pagina.

---

## 3. Gestione Avanzata: Il Layout Fullwidth

In alcuni contesti di information design, come dataset densi o grafici a dispersione complessi, il layout a due colonne potrebbe sacrificare lo spazio visivo destinato ai dati. 

Per ovviare a questo problema, il componente supporta l'attributo `layout_mode="fullwidth"`. Questa modalità estende il grafico orizzontalmente per occupare tutto lo spazio disponibile del contenitore padre, mentre i box di testo vengono spinti sul lato (o sovrapposti con margini ampi) per massimizzare l'area visiva della canvas.

Di seguito un esempio pratico utilizzando il noto dataset *Cars*:

<pre>
{% raw %}{% include scrollytelling.html 
  scrolly_id="scrolly-cars" 
  data_file="scrollama_cars" 
  layout_mode="fullwidth" 
  chart_height="400px" 
  desktop_chart_height="50vh" 
  sticky_top="15vh" 
  step_gap="80vh" 
  only_active_step_visible="true" 
%}{% endraw %}
</pre>

{% include scrollytelling.html scrolly_id="scrolly-cars" data_file="scrollama_cars" layout_mode="fullwidth" chart_height="40vh" desktop_chart_height="40vh" sticky_top="30vh" desktop_steps_padding_top="6vh" step_gap="100vh" last_step_gap="20vh" only_active_step_visible="false" %}

### Best Practice per il Fullwidth
1. **Tooltips posizionati:** Assicurati che i tooltips di Vega-Lite non vengano coperti dai box di testo in scorrimento.
2. **Dimensionamento responsivo:** Imposta `"width": "container"` nei tuoi file JSON Vega-Lite per forzare il grafico ad adattarsi al 100% della larghezza generata dalla modalità fullwidth.

---

## Conclusioni

La visualizzazione attraverso lo scrollytelling rappresenta un ponte tra l'esplorazione autonoma e la narrazione guidata. Sfruttando le potenzialità della pipeline combinata (Jekyll per il templating, Scrollama per il tracking e Vega-Lite per il rendering), è possibile generare deliverable interattivi di alta qualità tecnica e visiva, riducendo drasticamente il codice necessario per ogni nuova implementazione.

### Disclaimer

Questa implementazione è stata sviluppata per scopi didattici e dimostrativi. Non è stata progettata per essere un prodotto finito o una libreria pronta all'uso, ma piuttosto come un punto di partenza per chi desidera esplorare le potenzialità dello scrollytelling in Jekyll. Per progetti di produzione, si consiglia di valutare attentamente le esigenze specifiche e considerare eventuali personalizzazioni o ottimizzazioni del codice.