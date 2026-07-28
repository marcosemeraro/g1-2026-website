# Template per Progetti Master SoBigData
Questo repository è un template per aiutarti a costruire e pubblicare rapidamente il sito web del tuo progetto utilizzando Jekyll e GitHub Pages. Questo template è progettato per essere semplice, personalizzabile e pronto per il tuo progetto Big Data!

# Prerequisito: installazione di Jekyll per lo sviluppo locale

Nel corso **LBDAI – Laboratory of Big Data and Artificial Intelligence for Society** utilizzeremo **Jekyll**, uno *Static Site Generator* (SSG) open-source che consente di creare siti web statici, veloci e facilmente gestibili, senza la necessità di database o backend complessi.

Jekyll è scritto in **Ruby**, ma **non è necessario conoscere Ruby** per utilizzarlo: serve solo che sia installato sul proprio computer.  
Grazie a Jekyll potrai sviluppare, testare e visualizzare in locale il sito web del tuo progetto prima della pubblicazione online.

Durante il corso lavoreremo su repository **GitHub**, quindi sarà necessario disporre di un **account GitHub personale** per poter:

- clonare il template del progetto del corso;
- effettuare modifiche e commit;
- pubblicare il sito tramite **GitHub Pages**.

---

[Guida in italiano per l'installazione di Jekyll](https://danielefadda.github.io/master-projects-template-2026/local-development.html)

[English guide fot Jekyll installation](https://danielefadda.github.io/master-projects-template-2026/local-development-eng.html)


# Come iniziare

Dopo aver installato Jekyll sulla tua macchina:

1. **Crea il tuo repository:**  
   Clicca su **"Use this template"** su GitHub e assegna al tuo repo il nome `g<numerogruppo>-<annoincorso>-website` (es. `g0-2026-website`).

2. **Personalizza il tuo sito:**  
   - Modifica `_config.yml` per impostare il titolo, la descrizione e l'aspetto del progetto.
   - Aggiorna la barra di navigazione e i link del footer secondo necessità.
   - Aggiungi i membri del tuo team in `_data/members.yml`.

3. **Aggiungi i tuoi contenuti:**  
   - Crea e modifica le pagine in `_pages/` (es. `index.md`, `project.md`, `team.md`).
   - Aggiungi immagini in `assets/images/` e grafici in `assets/charts/`.

4. **Pubblica il tuo sito:**  
   - Crea il branch `gh-pages`.
   - Utilizza la action `Publish site to gh-pages branch` per pubblicare il sito su GitHub Pages.
   - Il tuo sito sarà disponibile all'indirizzo `https://<username>.github.io/<repo>/`.

Per istruzioni dettagliate, consulta i contenuti del template.

