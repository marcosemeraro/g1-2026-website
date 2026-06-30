---
layout: default
title: Installazione
header_title: "Installazione Locale di Jekyll"
header_type: hero #base, post, hero,image, splash
header_img: assets/images/Jekyll_Logo.png
---




# Guida all'Installazione di Jekyll

Per sviluppare il sito web del Progettone®, utilizzeremo un Static Site Generator (SSG), che ti consente di creare siti web a caricamento rapido senza la necessità di complessi sistemi backend o database. In particolare, utilizzeremo uno degli SSG più popolari, **Jekyll**. Jekyll è un SSG gratuito e open-source basato sul linguaggio di programmazione Ruby. **Non è necessario conoscere Ruby per usare Jekyll**; devi solo avere Ruby installato sul tuo computer.
<hr>

## Indice
- [Installazione su Windows](#come-installare-jekyll-su-windows)
- [Installazione su macOS](#come-installare-jekyll-su-macos)
- [Installazione su Linux](#come-installare-jekyll-su-linux)
- [Usare Jekyll](#usare-jekyll)

#### I vantaggi di Jekyll

**Facilità d'uso**: Jekyll usa file di testo semplice e sintassi markdown per creare e gestire contenuti, quindi non è necessario conoscere HTML o CSS per iniziare.

**Velocità e sicurezza**: Jekyll non interagisce con database o script lato server, riducendo il rischio di vulnerabilità e attacchi. Genera file HTML statici, rendendo il sito incredibilmente veloce e sicuro.

**Personalizzabilità**: Jekyll è altamente personalizzabile, consentendo l'uso di layout e template o la creazione di plugin per estendere le sue funzionalità.

**Facile distribuzione**: Jekyll genera file HTML statici che possono essere distribuiti su un server web o provider di hosting senza la necessità di un sistema di gestione dei contenuti dinamico.
<hr>
<br>

> **Nella seguente guida troverai i prerequisiti per far funzionare Jekyll**

<br>
# Come Installare Jekyll su Windows

Per installare Ruby e Jekyll su un computer Windows, è necessario utilizzare RubyInstaller. Questo può essere fatto scaricando e installando una versione Ruby+Devkit da [RubyInstaller Downloads](https://rubyinstaller.org/downloads/) e **utilizzando le opzioni di installazione predefinite e prendendo l'ultima versione consigliata** (lascia tutto com'è, specialmente **MSYS2**).

Questa operazione richiederà alcuni minuti.

Nell'ultima fase dell'installazione guidata, esegui <code>ridk <strong>install </strong></code> (come consigliato), che viene utilizzato per installare le gems. Per maggiori informazioni, vedi la [Documentazione RubyInstaller](https://github.com/oneclick/rubyinstaller2#using-the-installer-on-a-target-system).

Alla fine dell'installazione, vedrai questo prompt:

![Ruby Installer su windows]({{site.baseurl}}/assets/images/RubyInstaller.png "image_tooltip")

Tra le opzioni, scegli **MSYS2** e **MINGW development toolchain** (3 Enter).

Questa operazione richiede alcuni minuti ed è normale che appaiano alcuni avvisi.

Apri una nuova finestra del **prompt dei comandi** e installa Jekyll e Bundler con il seguente comando:

```
gem install jekyll bundler
```

Verifica che Jekyll sia installato correttamente:

```
jekyll -v
```

Se vedi il numero di versione, significa che Jekyll è installato e funziona correttamente sul tuo sistema. **Ora tutto è pronto per iniziare a usare Jekyll!**

<br>
# Come Installare Jekyll su macOS

Per impostazione predefinita, Ruby è preinstallato su macOS, ma non puoi usare questa versione di Ruby per installare Jekyll perché è obsoleta. Ad esempio, su Ventura, la versione Ruby preinstallata è 2.6.10, mentre l'ultima versione è 3.1.3.

Per risolvere questo problema, devi installare Ruby correttamente usando un gestore di versioni come **chruby**.

### Homebrew
Prima di tutto, devi installare **Homebrew** (nell'improbabile caso tu non l'abbia già fatto).

Per verificare se Homebrew è installato, esegui il comando

```
brew -v
```

Se è già installato, vedrai il numero di versione.

**Per installare Homebrew** sul tuo Mac, esegui il seguente comando nel tuo terminale:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Il sistema potrebbe chiederti di accettare i termini di licenza di Homebrew, segui le istruzioni che appaiono:

```bash
sudo xcodebuild -license
```

e dopo aver scorso i termini di licenza, digita

```bash
agree
```

per accettare.

Una volta completata l'installazione, configura la tua shell per usare automaticamente brew: per usare brew dal tuo terminale dovrai aggiungerlo a PATH, **il programma ti dirà esattamente come farlo** alla fine dell'installazione, è qualcosa come:

```bash
(echo; echo 'eval "$(/opt/homebrew/bin/brew shellenv)"') >> /Users/username/.zprofile
    eval "$(/opt/homebrew/bin/brew shellenv)"
```

Una volta completata l'installazione con successo, **esci e riavvia il terminale**, quindi verifica se Homebrew è pronto per l'uso eseguendo questo comando:

```bash
brew -v
```

### Chruby e Ruby-install
Se hai visto la versione di Homebrew, puoi procedere all'installazione di **chruby** e ruby-install con il seguente comando.

Continua con l'installazione di chruby e ruby-install usando il seguente comando:

```bash
brew install chruby ruby-install
```

Ora puoi installare l'ultima versione di ruby, 3.1.3, usando il pacchetto ruby-install che hai appena installato:

```bash
ruby-install 3.1.3
```

Questa operazione richiederà diversi minuti.

Una volta completata l'installazione, configura la tua shell per usare automaticamente chruby con il seguente comando:

```bash
echo "source $(brew --prefix)/opt/chruby/share/chruby/chruby.sh" >> ~/.zshrc
echo "source $(brew --prefix)/opt/chruby/share/chruby/auto.sh" >> ~/.zshrc
echo "chruby ruby-3.1.3" >> ~/.zshrc
```

<div class="alert alert-warning" role="alert">
  <strong>Attenzione!</strong> Se stai usando un terminale bash invece di zsh, il comando è lo stesso, punta solo al diverso tipo di terminale: <code>~/.bash_profile</code>

N.B. se usi bash invece di zsh, il comando è lo stesso, punta solo al diverso tipo di terminale: <code>~/.bash_profile</code>
</div>

**Chiudi e riapri** il tuo terminale e controlla che la versione corretta di ruby sia ora in uso:

```bash
ruby -v
```

### Jekyll e Bundler

Ora puoi installare gli ultimi aggiornamenti delle gems e bundle (cosa che potrebbe richiedere alcuni minuti):

```bash
gem update --system
gem update
gem cleanup
```

Infine, installa Jekyll:

```bash
gem install jekyll
```

Verifica la versione per assicurarti che l'installazione sia avvenuta correttamente. Se è installato correttamente, vedrai il numero di versione.

```bash
jekyll -v
```

# Come Installare Jekyll su Linux

L'installazione su Linux è molto simile a quella su macOS. Puoi installare Ruby usando il tuo gestore di pacchetti (apt, yum, ecc.) e poi installare Jekyll usando gem.

Per Ubuntu/Debian:

```bash
sudo apt-get install ruby-full build-essential zlib1g-dev
```

Per Fedora:

```bash
sudo dnf install ruby ruby-devel @development-tools
```

Quindi installa Jekyll:

```bash
gem install jekyll bundler
```

# Usare Jekyll

Una volta installato Jekyll, puoi creare un nuovo sito o lavorare con un sito esistente.

## Creare un nuovo sito

Per creare un nuovo sito Jekyll:

```bash
jekyll new mio-sito
cd mio-sito
bundle exec jekyll serve
```

Per maggiori informazioni, consulta la [documentazione ufficiale di Jekyll](https://jekyllrb.com/docs/).

Il tuo sito sarà disponibile all'indirizzo `http://localhost:4000/`.


<div class="alert alert-warning">
<p> Se il nuovo sito si avvia correttamente, puoi considerare Jekyll installato correttamente sul tuo sistema. <strong>Buon lavoro!</strong></p>
</div>