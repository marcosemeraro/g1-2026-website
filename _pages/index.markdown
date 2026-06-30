---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: "Home"
vega: true
---

<div class="full-width-wrapper">
    <img src="{{ site.baseurl }}/assets/images/header.svg" alt="sbd-pattern" class="full-width-image">
</div>

Per sviluppare il sito web del *Progettone* utilizzeremo un **Static Site Generator** (SSG), che consente di creare siti web a caricamento rapido senza la necessità di complessi sistemi backend o database.
In particolare, utilizzeremo uno degli SSG più popolari: Jekyll e GitHub Pages.
{: .lead }

**Jekyll** è un semplice generatore di siti statici, blog-aware, che prende i tuoi contenuti, li renderizza in un sito web statico e li serve. Combinato con GitHub Pages, consente di ospitare il tuo sito web gratuitamente, rendendolo una scelta ideale per progetti personali, portfolio e siti di documentazione.
{: .lead }

**GitHub Pages** è un servizio fornito da GitHub che consente di ospitare siti web statici direttamente da un repository GitHub. Supporta Jekyll nativamente, rendendo facile il deploy del tuo sito Jekyll con pochi click.
{: .lead }

<br>

# Cosa ti serve per iniziare

- [ ] 🐙 **Un account GitHub** - Se non ne hai uno, puoi [crearlo gratuitamente](https://github.com/)
- [ ] 📝 **Una bozza ben strutturata del tuo progetto** - Può essere un documento Word o qualsiasi altro formato tu preferisca. Dovrebbe includere le idee principali, la struttura e il contenuto che vuoi presentare sul tuo sito web.
- [ ] 📊 **Una cartella con i tuoi Grafici salvati** - Puoi usare i grafici che hai creato salvandoli come file .json nella cartella `assets/charts`.
- [ ] 📂 **Una cartella con tutte le immagini** che vuoi usare sul tuo sito web: puoi salvarle nella cartella `assets/images`.
{: .bg-color-full  .px-3 .lead}

<br>

# Come iniziare

Lo sviluppo del sito web può essere effettuato in due modi:
1. **Completamente online, utilizzando GitHub e GitHub Pages.**
2. Localmente, utilizzando Jekyll e poi caricando le modifiche su GitHub.

Nella guida, ci concentreremo sulla prima opzione, che è il modo più semplice per creare un sito web. 
Tuttavia, se vuoi sviluppare il sito web localmente, devi installare Jekyll sul tuo computer. 
Puoi trovare le istruzioni su come farlo [nella guida ufficiale](https://jekyllrb.com/docs/installation/) o nella [sezione sviluppo locale]({{ site.baseurl }}/local-development/).