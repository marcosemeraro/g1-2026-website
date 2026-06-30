---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: "Grafici"
vega: true

header_type: hero #base, post, hero,image, splash
header_img: assets/images/altair-gallery.png
header_title: "Grafici Altair"
subtitle: "Come incorporare grafici Altair nelle tue pagine Jekyll"
---

Puoi aggiungere grafici alle tue pagine usando il tag `vegachart`. Per incorporare un grafico devi salvare il grafico come file json nella directory `assets/charts` e quindi usare il tag `vegachart` per incorporarlo nella pagina. 

# Da Python al Web

Altair è una libreria di visualizzazione dati per Python, basata su Vega e Vega-Lite. Questo significa che Altair genera specifiche Vega-Lite, che vengono poi convertite in specifiche Vega e infine in grafici visualizzabili in un browser web. Per incorporare un grafico Altair in una pagina web, è necessario convertire la specifica Vega-Lite in un oggetto JSON e salvarla come file `.json`.

La procedura in `altair` per salvare la specifica Vega-Lite come file `.json` è la seguente:

Dato qualsiasi `chart` di `altair`:
```python
# Dati di esempio
data = pd.DataFrame({
    'a': ['A', 'B', 'C', 'D', 'E'],
    'b': [28, 55, 43, 91, 81]
})
```
Se vuoi rendere il grafico responsive, quando salvi il file `.json` puoi specificare la proprietà `width='container'`.
```python
# Crea il grafico
chart = alt.Chart(data).mark_bar().encode(
    x='a:N',
    y='b:Q'
).properties(
    width='container',
    height=300 
)

# Salva il grafico come file JSON
chart_json = chart.to_json()
with open('chart.json', 'w') as f:
    f.write(chart_json)
```
> [Link al Notebook Colab con l'esempio sopra](https://colab.research.google.com/drive/1ySTEzV2se1buHZ7X5p2fX5RlDUXyAfaX?usp=sharing)

Il file `chart.json` può quindi essere usato in una pagina web per visualizzare il grafico. Per farlo, puoi usare il tag Jekyll `vegachart`, specificando il percorso al file `.json` come valore dell'attributo `schema-url`.
{% raw %}
```html
<div style="height: 400px">
<vegachart schema-url="{{site.baseurl}}/assets/charts/chart_responsive.json" style="width: 100%; height: 100%"></vegachart>
</div>
```
{% endraw %}

Questo tema è stato personalizzato per supportare i grafici Vega senza complicazioni: se la pagina in cui vuoi visualizzare il grafico ha `vega: true` nel front matter, il tag `vegachart` verrà interpretato e il grafico verrà visualizzato correttamente.
`vega: true` è una variabile che dice alla pagina di caricare il plugin Vega.
In questo modo, il grafico verrà visualizzato in modo responsive, adattandosi alla larghezza del container in cui è posizionato.

# Esempio di incorporamento di un grafico creato con Altair

<div style="height: 400px">
<vegachart schema-url="{{site.baseurl}}/assets/charts/chart_responsive.json" style="width: 100%; height: 100%"></vegachart>
</div>

- Il grafico è stato salvato come `chart_responsive.json` e posizionato nella cartella `assets/charts`.
- Se provi a visualizzare il grafico in una pagina web senza specificare la proprietà `width='container'`, il grafico non sarà responsive e verrà visualizzato con una larghezza fissa.
- Se provi a visualizzare il grafico in un Jupyter notebook, il grafico non sarà visibile perché la proprietà `width='container'` non è supportata in quell'ambiente. In questo caso, puoi specificare una larghezza fissa in pixel e cambiare la proprietà width solo durante l'esportazione.
