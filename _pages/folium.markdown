---
layout: default
title:  "Mappa Folium"
subtitle: "Come incorporare una mappa creata con Folium"
header_type: hero #base, post, hero,image, splash
header_img: assets/images/folium_map.webp
header_title: "Mappe Folium"
vega: false
---
Questa pagina spiega come incorporare una mappa creata con Folium in una pagina web. Folium è una libreria Python che ti consente di creare mappe interattive usando Leaflet.js. Per incorporare una mappa creata con Folium in una pagina web, devi salvare la mappa in un file `.html` e includerla nella pagina web.
<br>
## Incorporare una mappa creata con Folium in una pagina web
Per incorporare una mappa creata con Folium in una pagina web, devi seguire questi passaggi:
- Creare una mappa con Folium
- Salvare la mappa in un file `.html`
- Includere il file `.html` nella pagina web
    - Usa il tag `iframe` per incorporare il file `.html` nella pagina web
    - Specifica il percorso del file `.html` come valore dell'attributo `src` del tag `iframe`
    - Specifica le dimensioni della mappa usando gli attributi `width` e `height` del tag `iframe`
    - Codice HTML di esempio per incorporare una mappa creata con Folium in una pagina web:
    
```<iframe src="{{site.baseurl}}/assets/charts/map.html" width="100%" height="500"></iframe>```

<iframe src="{{site.baseurl}}/assets/charts/usa.html" width="100%" height="500px" ></iframe>

**Nota che la mappa creata con Folium è stata esportata con larghezza e altezza impostate al 100%.**
Puoi regolare le dimensioni della mappa modificando i valori degli attributi `width` e `height` del tag `iframe`.
