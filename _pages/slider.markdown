---
layout: default
title: "Pagina Slider"
header_type: hero
header_img: assets/images/sliding_dog.jpg
header_title: "Pagina Componente Slider"
subtitle: "Una Pagina di Esempio per il Componente Slider"
---

# Esempio di Componente Slider
Questa pagina dimostra l'uso del componente slider nel Template Progetto Big Data SoBigData. Lo slider consente agli utenti di navigare attraverso diverse sezioni o contenuti in modo dinamico.

{% include slider.html data="sliders" label="Scegli un carosello" %}

---

{% raw %}
```
{% include slider.html data="sliders" label="Scegli un carosello" %}
```
{% endraw %}

Attraverso questo componente, puoi caricare diversi contenuti in modo dinamico, partendo da un file yml (in questo caso: `sliders.yml`) che contiene i dati per lo slider. Lo slider può essere utilizzato per mostrare immagini, testo o qualsiasi altro contenuto che si adatti al formato carosello. Questo componente consente aggiornamenti e modifiche facili al contenuto senza modificare la struttura HTML.

Nel caso in cui tu abbia più di uno slider, devi specificare il parametro `label` per fornire un'etichetta descrittiva per il selettore dello slider, migliorando l'esperienza utente rendendo chiaro a cosa serve lo slider.

La struttura del file yml è la seguente:

```yaml 
sliders:
  - id: slider1
    title: "Slider 1"
    slides:
      - image: "assets/images/slider1_image1.jpg"
        label: "Didascalia per Immagine 1"
        description: "Nulla vitae elit libero, a pharetra augue mollis interdum."
      - image: "assets/images/slider1_image2.jpg"
        label: "Didascalia per Immagine 2"
        description: "Sed posuere consectetur est at lobortis. Aenean lacinia bibendum nulla sed consectetur."
  - id: slider2
    title: "Slider 2"
    slides:
      - image: "assets/images/slider2_image1.jpg"
        label: "Didascalia per Immagine 3"
        description: "Cras justo odio, dapibus ac facilisis in, egestas eget quam."
      - image: "assets/images/slider2_image2.jpg"
        label: "Didascalia per Immagine 4"
        description: "Morbi leo risus, porta ac consectetur ac, vestibulum at eros."
```
