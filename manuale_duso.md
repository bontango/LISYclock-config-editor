# LISYclock Config Editor – Manuale d'uso

**Versione 1.08** | Lingua: Italiano

---

## Indice

1. [Panoramica](#1-panoramica)
2. [Requisiti & Avvio](#2-requisiti--avvio)
3. [Operazioni sui file](#3-operazioni-sui-file)
4. [Scheda: Clock](#4-scheda-clock)
5. [Scheda: General](#5-scheda-general)
6. [Scheda: Events](#6-scheda-events)
7. [Scheda: GI LEDs](#7-scheda-gi-leds)
8. [Scheda: Attract Mode](#8-scheda-attract-mode)
9. [Scheda: TTS Settings](#9-scheda-tts-settings)
10. [config.txt – Formato & Struttura](#10-configtxt--formato--struttura)
11. [Suggerimenti & Note](#11-suggerimenti--note)

---

## 1. Panoramica

Il **LISYclock Config Editor** è uno strumento per modificare comodamente il file di configurazione `config.txt` del **LISYclock**.

- Applicazione standalone: un singolo file `LISYclock_config_editor.html`, nessun server, nessun passaggio di build
- Basta aprire `LISYclock_config_editor.html` nel browser e iniziare
- Supporta il caricamento/scaricamento diretto sull'hardware LISYclock via HTTP

![Vista completa dell'editor con file di configurazione aperto](pics/Full%20view%20of%20the%20editor%20with%20an%20open%20configuration%20file.jpg)

---

## 2. Requisiti & Avvio

### Browser

| Browser | Supporto |
|---------|----------|
| **Chrome / Edge** (consigliato) | Supporto completo inclusa la File System Access API (finestra di dialogo nativa apri/salva) |
| Firefox / Safari | Funzionante, ma utilizza `<input type=file>` / link di download come fallback |

### Avvio

1. Aprire `LISYclock_config_editor.html` dalla directory del progetto **localmente** nel browser (doppio clic o `File → Apri`)
2. L'applicazione si avvia immediatamente con una configurazione predefinita vuota

> **Importante:** Per la comunicazione diretta con il LISYclock (scheda "Clock"), il file deve essere aperto tramite `file://` – **non** tramite HTTPS. Motivo: i browser bloccano le richieste HTTP (verso l'orologio) dalle pagine HTTPS per ragioni di sicurezza.

---

## 3. Operazioni sui file

I pulsanti nella **barra di navigazione** (in alto) controllano tutte le operazioni sui file:

| Pulsante | Funzione |
|----------|----------|
| **Apri** | Apre un `config.txt` esistente dal file system locale |
| **Salva** | Scrive la configurazione corrente nel file aperto (attivo solo quando un file è aperto) |
| **Salva con nome…** | Permette di scegliere un nuovo file di destinazione e vi salva la configurazione |
| **Nuova configurazione** | Ripristina tutti i campi ai valori predefiniti (attenzione: le modifiche non salvate andranno perse) |

Il **nome del file corrente** viene visualizzato al centro della barra di navigazione. Finché non è aperto alcun file, appare "(nessun file)".

---

## 4. Scheda: Clock

Questa scheda consente la comunicazione diretta con l'hardware LISYclock sulla rete locale.

### Indirizzo IP

Inserire l'indirizzo IP del proprio LISYclock (ad es. `192.168.1.42`). L'indirizzo viene salvato nel browser e viene precompilato automaticamente alla successiva apertura.

### Test di connessione

Un clic su **Testa connessione** verifica la raggiungibilità dell'orologio:

| Indicatore | Significato |
|-----------|-------------|
| Grigio | Nessun test ancora eseguito |
| Verde | Connessione riuscita |
| Rosso | Connessione fallita |

In caso di connessione riuscita vengono visualizzati la **versione del firmware** e la **versione API**. Se la versione API non corrisponde alla versione attesa, viene visualizzato un **avviso** (le funzioni potrebbero comunque essere parzialmente utilizzabili).

### Configurazione

| Pulsante | Funzione |
|----------|----------|
| **Carica config** | Trasferisce la configurazione corrente (visualizzata nell'editor) all'orologio |
| **Scarica config** | Scarica il `config.txt` dall'orologio e lo apre nell'editor |

### Riavvio

Un clic su **Riavvia** riavvia il LISYclock. Una finestra di conferma previene riavvii accidentali.

### Aggiornamento firmware

**Opzione A – File locale:**
1. Selezionare un file `.bin` con il pulsante di selezione file
2. Fare clic su **Upload & Reboot** – il file viene trasferito e l'orologio si riavvia automaticamente

**Opzione B – Dal server lisy.dev:**
1. Connettersi prima all'orologio (in modo che l'editor sappia se è in esecuzione il firmware v1 o v2)
2. Fare clic su **Fetch versions** – appare un menu a tendina con tutti i file firmware corrispondenti da lisy.dev
3. Selezionare la versione desiderata dal menu a tendina
4. Fare clic su **Upload & Reboot** – il file viene scaricato da lisy.dev e trasferito all'orologio

> Durante il caricamento viene visualizzata una barra di avanzamento. Non chiudere la scheda del browser.
> Se è selezionata una versione dal server, il menu a tendina ha la precedenza su un file selezionato localmente.

### Gestione file sulla scheda SD

- **Carica file:** Trasferire qualsiasi file (ad es. MP3, batch) sulla scheda SD dell'orologio
- **Elenco file:** Usare **Aggiorna** per caricare l'elenco file corrente dalla scheda SD
  - L'elenco mostra nome, dimensione e **data** di modifica di ogni file
  - I singoli file possono essere **scaricati** o **rinominati**

### Avviso HTTPS

Se l'editor viene aperto tramite un URL HTTPS, il browser blocca tutte le richieste HTTP verso l'orologio. Soluzione: aprire `LISYclock_config_editor.html` **localmente** tramite `file://`.

---

## 5. Scheda: General

Impostazioni generali del LISYclock.

![Scheda General](pics/General%20tab.jpg)

### Luminosità del display

Valore da **0** (scuro) a **7** (luminosità massima). Corrisponde alla chiave di configurazione `DISP_BRIGHT`.

### Server FTP

| Campo | Descrizione |
|-------|-------------|
| Abilitato (casella) | Abilita/disabilita il server FTP (`#FTP_USER=…` quando disabilitato) |
| Nome utente | Nome di accesso FTP |
| Password | Password FTP |

### Fuso orario

| Campo | Descrizione |
|-------|-------------|
| Abilitato (casella) | Abilita/disabilita la configurazione del fuso orario |
| Stringa POSIX | Fuso orario in formato POSIX |

Esempio per l'Europa centrale con ora legale:
```
CET-1CEST,M3.5.0,M10.5.0/3
```

### Stringhe dei giorni della settimana

Se abilitato, l'orologio mostra i nomi localizzati dei giorni della settimana. Ognuno dei 7 campi (domenica–sabato) può avere al massimo **6 caratteri**. Le voci più corte vengono automaticamente completate con spazi fino a 6 caratteri.

Esempio (Italiano): `Dom`, `Lun`, `Mar`, `Mer`, `Gio`, `Ven`, `Sab`

---

## 6. Scheda: Events

Gli eventi definiscono le azioni temporizzate del LISYclock.

![Scheda Events con più voci](pics/Events%20tab%20with%20multiple%20entries.jpg)

### Tipi di evento

| Tipo | Descrizione | Campo valore |
|------|-------------|--------------|
| **TTS** | Riprodurre testo tramite Text-to-Speech | Testo libero (ad es. "Buongiorno!") |
| **MP3** | Riprodurre un file MP3 dalla scheda SD | Nome file (ad es. `alarm.mp3`) |
| **BATCH** | Eseguire un file batch dalla scheda SD | Nome file (ad es. `script.bat`) |
| **DISPLAY** | Accendere o spegnere tutti i display | Dropdown: `on` / `off` |
| **SYNC_TIME** | Sincronizzare l'orario tramite NTP | Dropdown: 1–10 tentativi |
| **SAY_TIME** | Dire l'ora corrente tramite TTS | Dropdown: `german` / `english` / `italian` |
| **GI_LEDS** | Accendere o spegnere tutti i LED GI | Dropdown: `on` / `off` |
| **ATTRACT_LEDS** | Accendere o spegnere tutti i LED Attract | Dropdown: `on` / `off` |

### Formato orario

L'**orario** di un evento viene specificato come `HH:MM:W` o `HH:MM-TT.MM.JJJJ`. `*` sta per "qualsiasi" in ogni campo.

#### Modalità giorno della settimana: `HH:MM:W`

| Campo | Significato |
|-------|-------------|
| `HH` | Ora (0–23), `*` = ogni ora |
| `MM` | Minuto (0–59), `*` = ogni minuto |
| `W` | Giorno della settimana: 0 = domenica, 1 = lunedì, … 6 = sabato, `*` = ogni giorno |

#### Modalità data: `HH:MM-TT.MM.JJJJ`

Attiva quando nel campo giorno della settimana è selezionato `*` (data). Il campo data appare quindi in aggiunta. Il formato è: `TT` = giorno, `MM` = mese, `JJJJ` = anno.

| Campo | Significato |
|-------|-------------|
| `TT` | Giorno (1–31), `*` = ogni giorno |
| `MM` | Mese (1–12), `*` = ogni mese |
| `JJJJ` | Anno (quattro cifre), `*` = ogni anno |

#### Esempi

| Orario | Significato |
|--------|-------------|
| `7:30:1` | Ogni lunedì alle 07:30 |
| `*:*:*` | Ogni minuto, ogni giorno |
| `8:0:*` | Ogni giorno alle 08:00 |
| `12:0-24.12.*` | Ogni 24 dicembre alle 12:00 |
| `*:0-*.*.*` | Ogni ora intera, ogni giorno |

### Controlli

- **+ Aggiungi evento** (pulsante in basso): Aggiunge un nuovo evento in fondo alla lista
- **✕** (a destra di un evento): Rimuove quell'evento

---

## 7. Scheda: GI LEDs

Definisce i colori iniziali dei LED GI (Illuminazione Generale).

![Scheda GI LEDs](pics/GI%20LEDs%20tab.jpg)

### Campi per ogni riga LED

| Campo | Descrizione |
|-------|-------------|
| LED | Numero LED (1–64) |
| R / G / B | Valore colore per canale (0–255) |
| Selettore colore | Apre un selettore colore nativo |
| Punto di anteprima | Mostra il colore corrente come un piccolo cerchio colorato |

Il selettore colore e i campi RGB sono sincronizzati: una modifica nel selettore aggiorna i campi RGB e viceversa.

### Controlli

- **+ Aggiungi LED**: Aggiunge una nuova voce LED in fondo alla lista
- **✕**: Rimuove questa voce LED

---

## 8. Scheda: Attract Mode

Configura l'illuminazione dell'Attract Mode (luci animate in standby) in **5 gruppi** (AT1–AT5).

![Scheda Attract Mode con gruppo AT1 espanso](pics/Attract%20Mode%20tab%20with%20group%20AT1%20expanded.jpg)

Ogni gruppo è visualizzato come **pannello accordion richiudibile**.

### Impostazioni per gruppo

| Campo | Descrizione |
|-------|-------------|
| **Frequenza lampeggio** | Intervallo di lampeggio in millisecondi |
| **Attivazione casuale** | Casella: attivare il gruppo casualmente |
| **Rand** | Soglia casuale (0–255); più alto è il valore, meno frequentemente il gruppo viene attivato |
| **Lista LED** | Elenco dei LED in questo gruppo (come GI LEDs: numero, R, G, B, selettore colore) |

- **+ Aggiungi LED**: Aggiunge una nuova voce LED al gruppo
- **✕**: Rimuove una voce LED dal gruppo

---

## 9. Scheda: TTS Settings

Impostazioni per la funzione Text-to-Speech (utilizza il servizio WIT.ai).

![Scheda TTS Settings](pics/TTS%20Settings%20tab.jpg)

| Campo | Valori possibili | Predefinito |
|-------|-----------------|-------------|
| **WIT.ai Token** | Testo libero (chiave API da wit.ai) | — |
| **Voice** | 17 voci (US EN, UK EN, CA EN), ad es. `wit$Cooper` | `wit$Cooper` |
| **Style** | `default` / `soft` / `formal` / `fast` / `projected` | `default` |
| **Speed** | 0–200 | 80 |
| **Pitch** | 0–200 | 80 |
| **Gain** | 0–100 | 30 |
| **SFX Char** | `none` / `chipmunk` / `monster` / `daemon` / `robot` / `alien` | `none` |
| **SFX Env** | `none` / `reverb` / `room` / `cathedral` / `radio` / `phone` | `none` |

> Il token API di WIT.ai si ottiene dopo la registrazione gratuita su [wit.ai](https://wit.ai).

---

## 10. config.txt – Formato & Struttura

### Formato base

```
KEY=value
```

- Ogni riga contiene una chiave e un valore, separati da `=`
- Un `#` **all'inizio di una riga** disabilita la riga (senza eliminarla)
- Le maiuscole/minuscole dei nomi delle chiavi sono normalizzate nell'editor

### Regole speciali

| Regola | Descrizione |
|--------|-------------|
| Blocco FTP | Nessuno spazio dopo `#`: `#FTP_USER=lisy` |
| Fuso orario | Spazio dopo `#`: `# TIMEZONE="CET-1CEST,…"` |
| Stringhe giorni | Esattamente 6 caratteri, completati con spazi: `Lun   ` |
| `EVENT_SYNC_TIME` | Valore **senza** virgolette: `EVENT_SYNC_TIME=2:0:*,10` |
| `EVENT_SAY_TIME` | Valore **con** virgolette: `EVENT_SAY_TIME=14:12:*,"italian"` |
| Tutti gli altri `EVENT_*` | Valore **con** virgolette: `EVENT_TTS=8:0:*,"Buongiorno!"` |

### Estratto di esempio

```
DISP_BRIGHT=5
#FTP_USER=lisy
#FTP_PWD=lisy
# TIMEZONE="CET-1CEST,M3.5.0,M10.5.0/3"
EVENT_SYNC_TIME=2:0:*,10
EVENT_SAY_TIME=14:12:*,"italian"
EVENT_TTS=8:0:*,"Buongiorno!"
```

---

## 11. Suggerimenti & Note

- **Usare Chrome o Edge:** Solo questi browser supportano la File System Access API per le finestre di dialogo native apri/salva. In Firefox e Safari appaiono invece selettori file standard o link di download.

- **Aprire localmente per l'accesso Clock:** Se la comunicazione con il LISYclock non funziona, verificare che `LISYclock_config_editor.html` sia aperto tramite `file://` (localmente) e non tramite `https://`.

- **FTP come alternativa:** Il `config.txt` completato può anche essere trasferito sulla scheda SD dell'orologio tramite FTP (se il server FTP è abilitato nella configurazione).

- **Chiavi sconosciute:** Se l'editor incontra chiavi sconosciute durante il caricamento, le ignora e mostra un banner di avviso con le righe interessate. Non vengono mantenute al salvataggio. Se si hanno estensioni personalizzate nel `config.txt`, eseguirne prima un backup separato.

- **Nessuna funzione annulla:** L'editor non dispone di una funzione annulla. In caso di modifiche accidentali, riaprire il file (senza salvare prima) ripristinerà lo stato precedente.

---

*Creato per LISYclock Config Editor v1.09*
