---
layout: default
title: "Dashboard Analisi Dati"
plotly: true
header_type: hero
header_img: assets/images/plotly-gallery.png
header_title: "Grafici Plotly"
subtitle: "Come incorporare grafici Plotly nelle tue pagine Jekyll"
---

Puoi aggiungere grafici interattivi alle tue pagine usando l'include `plotly-graph.html`. Per incorporare un grafico Plotly, devi esportare il grafico come file HTML e posizionarlo nella directory `assets/charts/plotly/`.
{: .lead}

<br>
---

# Da Python al Web

Plotly è una potente libreria Python per la visualizzazione interattiva dei dati. Per visualizzare un grafico Plotly sul tuo sito web, esporta il grafico come file HTML.

### Esportare un Grafico Plotly

Data qualsiasi `figura` Plotly:

```python
import plotly.graph_objects as go

# Dati di esempio
fig = go.Figure(data=[go.Bar(x=['A', 'B', 'C', 'D', 'E'], y=[28, 55, 43, 91, 81])])


# Salva il grafico come file HTML
fig.write_html("grafico1.html", include_plotlyjs=False, full_html=False, div_id="grafico1")
```

`full_html=False` esporta solo il grafico, non una pagina HTML completa.

`div_id="grafico1"` imposta l'ID per il container del grafico, che puoi referenziare nella tua pagina.

**Posiziona il file grafico1.html esportato nella directory assets/charts/plotly/.**

#### Esempio di esportazione di un Grafico Plotly

[Esempio Notebook Colab](https://colab.research.google.com/drive/18L-aGb1r3HlfTNprgBfqqlWxge4cqF4L?usp=sharing){:target="_blank"} 

<br>
---

### Incorporare il Grafico in una Pagina Jekyll
Per visualizzare il grafico, usa il seguente tag nel tuo file markdown:

{% raw %}
```markdown
{% include plotly-graph.html id="grafico1" file="grafico1.html" height="500px" %}
```
{% endraw %}
  
Parametri:

- `id`: ID del div container (obbligatorio)
- `file`: nome del file HTML con il grafico (obbligatorio)
- `path`: percorso personalizzato (default: /assets/charts/plotly/)
- `height`: altezza del grafico (default: 500px)
- `width`: larghezza del grafico (default: 100%)
- `class`: classi CSS aggiuntive (opzionale)

<br>

# Esempio di Grafico Plotly

{% include plotly-graph.html id="grafico1" file="grafico1.html" height="500px" %}
